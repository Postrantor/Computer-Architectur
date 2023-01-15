## Overcoming Data Hazards With Dynamic Scheduling(通过动态调度克服数据危害)

A simple statically scheduled pipeline fetches an instruction and issues it, unless there is a data dependence between an instruction already in the pipeline and the fetched instruction that cannot be hidden with bypassing or forwarding. (Forward- ing logic reduces the effective pipeline latency so that the certain dependences do not result in hazards.) If there is a data dependence that cannot be hidden, then the hazard detection hardware stalls the pipeline starting with the instruction that uses the result. No new instructions are fetched or issued until the dependence is cleared. In this section, we explore _dynamic scheduling_, a technique by which the hard- ware reorders the instruction execution to reduce the stalls while maintaining data flow and exception behavior. Dynamic scheduling offers several advantages. First, it allows code that was compiled with one pipeline in mind to run efficiently on a different pipeline, eliminating the need to have multiple binaries and recompile for a different microarchitecture. In today’s computing environment, where much of the software is from third parties and distributed in binary form, this advantage is sig- nificant. Second, it enables handling some cases when dependences are unknown at compile time; for example, they may involve a memory reference or a data- dependent branch, or they may result from a modern programming environment that uses dynamic linking or dispatching. Third, and perhaps most importantly, it allows the processor to tolerate unpredictable delays, such as cache misses, by executing other code while waiting for the miss to resolve. In [Section 3.6](#hardware-based-speculation), we explore hardware speculation, a technique with additional performance advantages, which builds on dynamic scheduling. As we will see, the advantages of dynamic scheduling are gained at the cost of a significant increase in hardware complexity. Although a dynamically scheduled processor cannot change the data flow, it tries to avoid stalling when dependences are present. In contrast, static pipeline scheduling by the compiler (covered in [Section 3.2](#basic-compiler-techniques-for-exposing-ilp)) tries to minimize stalls by sep- arating dependent instructions so that they will not lead to hazards. Of course, compiler pipeline scheduling can also be used in code destined to run on a proces- sor with a dynamically scheduled pipeline.

> 一个简单的静态调度流水线获取一条指令并发出它，除非流水线中已有的指令与获取的指令之间存在无法通过绕过或转发隐藏的数据依赖性。(转发逻辑减少了有效的流水线延迟，因此某些依赖性不会导致危险。)如果存在无法隐藏的数据依赖性，则危险检测硬件将从使用结果的指令开始停止流水线。在清除相关性之前，不会获取或发出新指令。在本节中，我们将探索*动态调度*，这是一种硬件重新排序指令执行以减少停顿同时保持数据流和异常行为的技术。
> 动态调度有几个优点。
>
> - 首先，它允许使用一个管道编译的代码在不同的管道上高效运行，从而无需拥有多个二进制文件并为不同的微体系结构重新编译。在当今的计算环境中，许多软件来自第三方并以二进制形式分发，这一优势非常显着。
> - 其次，它可以处理一些编译时依赖未知的情况；例如，它们可能涉及内存引用或数据相关分支，或者它们可能来自使用动态链接或调度的现代编程环境。
> - 第三，也许是最重要的一点，它允许处理器通过在等待未命中解决时执行其他代码来容忍不可预测的延迟，例如缓存未命中。在[第 3.6 节](#hardware-based-speculation)中，我们探讨了硬件推测，这是一种具有额外性能优势的技术，它建立在动态调度的基础上。
>   正如我们将看到的，动态调度的优势是以显着增加硬件复杂性为代价的。尽管动态调度的处理器无法更改数据流，但它会**尝试避免在存在依赖性时停顿**。相比之下，编译器的静态流水线调度(在[第 3.2 节](#basic-compiler-techniques-for-exposing-ilp) 中介绍)试图通过**分离相关指令来最大程度地减少停顿**，这样它们就不会导致危险。当然，编译器流水线调度也可以用在注定要在具有动态调度流水线的处理器上运行的代码中。

Figure 3.9 The misprediction rate for the integer SPECCPU2006 benchmarks on the Intel Core i7 920 and 6700. The misprediction rate is computed as the ratio of completed branches that are mispredicted versus all completed branches. This could understate the misprediction rate somewhat because if a branch is mispredicted and led to another mispredicted branch (which should not have been executed), it will be counted as only one misprediction. On average, the i7 920 mispredicts branches 1.3 times as often as the i7 6700.

> 图 3.9 Intel Core i7 920 和 6700 上的整数 SpecCPU2006 基准的错误预测率。计算错误预测率是根据已完成的分支机构的比率进行了错误预测的分支，而所有已完成的分支的比率。这可能会低估错误预测的率，因为如果一个分支机构被错误预测并导致另一个错误预测的分支(不应该执行)，则将仅计算为一个错误预测。平均而言，i7 920 错误预测的分支是 I7 6700 的频率 1.3 倍。

### Dynamic Scheduling: The Idea(动态调度：这个想法)

A major limitation of simple pipelining techniques is that they use in-order instruc- tion issue and execution: instructions are issued in program order, and if an instruc- tion is stalled in the pipeline, no later instructions can proceed. Thus, if there is a dependence between two closely spaced instructions in the pipeline, it will lead to a hazard, and a stall will result. If there are multiple functional units, these units could lie idle. If instruction _j_ depends on a long-running instruction _i,_ currently in execution in the pipeline, then all instructions after _j_ must be stalled until _i_ is finished and _j_ can execute. For example, consider this code:

> 简单的管道技术的一个主要局限性是他们使用固定的指导问题和执行：按计划顺序发布说明，如果在管道中停滞不前，则不得以后的说明可以继续进行。因此，如果管道中两个紧密间隔的说明之间存在依赖性，则会导致危险，并且会导致摊位。如果有多个功能单元，这些单元可能会闲置。如果指令 _j_ 取决于长期运行的指令 *i，*当前在管道中执行，则必须将* j*之后的所有指令停滞不前，直到 _i_ 完成并且 _j_ 可以执行。例如，考虑此代码：

> ===

The fsub.d instruction cannot execute because the dependence of fadd.d on fdiv.d causes the pipeline to stall; yet, fsub.d is not data-dependent on any- thing in the pipeline. This hazard creates a performance limitation that can be elim- inated by not requiring instructions to execute in program order.

> FSUB.D 指令无法执行，因为 FADD.D 对 FDIV.D 的依赖性导致管道失速；但是，fsub.d 并非数据依赖于管道中的任何事物。这种危害会产生绩效限制，可以通过不需要说明以程序顺序执行的方式来消除。

In the classic five-stage pipeline, both structural and data hazards could be checked during instruction decode (ID): when an instruction could execute without hazards, it was issued from ID, with the recognition that all data hazards had been resolved.

> 在经典的五阶段管道中，可以在指令解码(ID)期间检查结构和数据危害：当指令可以执行而无需危害时，它是从 ID 中发出的，并确认所有数据危害均已解决。

To allow us to begin executing the fsub.d in the preceding example, we must separate the issue process into two parts: checking for any structural hazards and waiting for the absence of a data hazard. Thus we still use in-order instruction issue (i.e., instructions issued in program order), but we want an instruction to begin exe- cution as soon as its data operands are available. Such a pipeline does _out-of-order execution_, which implies _out-of-order completion_.

> 为了让我们开始在上一个示例中执行 FSUB.D，我们必须将问题过程分为两个部分：检查任何结构性危害并等待缺乏数据危害。因此，我们仍然使用固定指令问题(即按程序顺序发布的说明)，但是我们希望一旦提供数据操作数，就可以开始进行指导。这样的管道执行 _out-Out corderecution_，这意味着 _out-Out of-Out of-Out contemion_。

Out-of-order execution introduces the possibility of WAR and WAW hazards, which do not exist in the five-stage integer pipeline and its logical extension to an in-order floating-point pipeline. Consider the following RISC-V floating-point code sequence:

> 排序的执行引入了战争和 WAW 危害的可能性，在五阶段的整数管道中不存在及其逻辑扩展，并将其逻辑扩展到订购的浮点管道。考虑以下 RISC-V 浮点代码序列：

> ===

There is an antidependence between the fmul.d and the fadd.d (for the register f0), and if the pipeline executes the fadd.d before the fmul.d (which is wait- ing for the fdiv.d), it will violate the antidependence, yielding a WAR hazard. Likewise, to avoid violating output dependences, such as the write of f0 by fadd.d before fdiv.d completes, WAW hazards must be handled. As we will see, both these hazards are avoided by the use of register renaming.

> FMUL.D 和 FADD.D(对于寄存器 F0)以及管道在 FMUL.D 之前执行 FADD.D(正在等待 FDIV.D)之间存在抗抑郁症(寄存器 F0)，它将违反抗抑郁症，产生战争危险。同样，为了避免违反产出依赖，例如 FDIV.D 完成之前 Fadd.D 的写入，必须处理 WAW 危害。正如我们将看到的那样，通过使用登记册重命名避免了这两种危害。

Out-of-order completion also creates major complications in handling excep- tions. Dynamic scheduling with out-of-order completion must preserve exception behavior in the sense that _exactly_ those exceptions that would arise if the program were executed in strict program order _actually_ do arise. Dynamically scheduled processors preserve exception behavior by delaying the notification of an associ- ated exception until the processor knows that the instruction should be the next one completed.

> 排序完成还会在处理外观方面产生重大并发症。如果*sactly * excrally * excrance *非常\_allyally do 会出现，则必须保留异常行为的动态调度必须保留异常行为。动态计划的处理器通过延迟关联异常的通知，直到处理器知道该指令应为下一个完成的处理器，从而保留异常行为。

Although exception behavior must be preserved, dynamically scheduled pro- cessors could generate _imprecise_ exceptions. An exception is _imprecise_ if the processor state when an exception is raised does not look exactly as if the instruc- tions were executed sequentially in strict program order. Imprecise exceptions can occur because of two possibilities:

> 尽管必须保留异常行为，但动态计划的监事可以生成 _imprecise_ 例外。例外是 _imprecise_ 如果在升级异常时的处理器状态看起来并不完全好像以严格的程序顺序依次执行指令。由于有两种可能性，可能会出现不精确的例外：

1. The pipeline may have _already completed_ instructions that are _later_ in program order than the instruction causing the exception.
2. The pipeline may have _not yet completed_ some instructions that are _earlier_ in program order than the instruction causing the exception.

> 1. 流水线可能有 *already completed* 指令，这些指令在程序顺序上比导致异常的指令*晚*。
> 2. 流水线可能*还没有完成*一些指令，这些指令在程序顺序上比导致异常的指令*早*。

Imprecise exceptions make it difficult to restart execution after an exception. Rather than address these problems in this section, we will discuss a solution that provides precise exceptions in the context of a processor with speculation in [Section 3.6](#hardware-based-speculation). For floating-point exceptions, other solutions have been used, as dis- cussed in Appendix J.

> 不精确的异常导致异常后很难重新开始执行。我们将在 [第 3.6 节](#hardware-based-speculation)中讨论一种在处理器上下文中提供精确异常的解决方案，而不是在本节中解决这些问题。对于浮点异常，已使用其他解决方案，如附录 J 中所述。

To allow out-of-order execution, we essentially split the ID pipe stage of our simple five-stage pipeline into two stages:

> 为了允许排序执行，我们本质上将简单五阶段管道的 ID 管道阶段分为两个阶段：

1. _Issue_—Decode instructions, check for structural hazards.
2. _Read operands_—Wait until no data hazards, then read operands.

> 1. _issue_-指定说明，检查结构性危害。
> 2. \_read 操作数 - 等待直到没有数据危害为止，然后阅读操作数。

An instruction fetch stage precedes the issue stage and may fetch either to an instruction register or into a queue of pending instructions; instructions are then issued from the register or queue. The execution stage follows the read operands stage, just as in the five-stage pipeline. Execution may take multiple cycles, depend- ing on the operation.

> 指令提取阶段先于发行阶段，并可以将指令登记册或待处理指令的队列获取；然后从登记册或队列发出说明。执行阶段遵循读取操作数阶段，就像在五阶段的管道中一样。执行可能取决于操作，进行多个周期。

We distinguish when an instruction _begins execution_ and when it _completes execution_; between the two times, the instruction is _in execution._ Our pipeline allows multiple instructions to be in execution at the same time; without this capa- bility, a major advantage of dynamic scheduling is lost. Having multiple instruc- tions in execution at once requires multiple functional units, pipelined functional units, or both. Because these two capabilities—pipelined functional units and multiple functional units—are essentially equivalent for the purposes of pipeline control, we will assume the processor has multiple functional units.

> 我们区分指令 _begins execution_ 及其 _completes execution_;在两次之间，指令是*in 执行。*我们的管道允许同时执行多个指令；没有这种能力，动态调度的主要优势就会丢失。在执行中具有多种指导需要多个功能单元，管道功能单元或两者兼而有之。由于这两个功能(涉及二级功能单元和多个功能单元)本质上是相当于管道控制的目的，因此我们将假设处理器具有多个功能单元。

In a dynamically scheduled pipeline, all instructions pass through the issue stage in order (in-order issue); however, they can be stalled or can bypass each other in the second stage (read operands) and thus enter execution out of order. _Scoreboarding_ is a technique for allowing instructions to execute out of order when there are sufficient resources and no data dependences; it is named after the CDC 6600 scoreboard, which developed this capability. Here we focus on a more sophis- ticated technique, called _Tomasulo_’_s algorithm._ The primary difference is that Tomasulo’s algorithm handles antidependences and output dependences by effec- tively renaming the registers dynamically. Additionally, Tomasulo’s algorithm can be extended to handle _speculation_, a technique to reduce the effect of control dependences by predicting the outcome of a branch, executing instructions at the predicted destination address, and taking corrective actions when the prediction was wrong. While the use of scoreboarding is probably sufficient to support sim- pler processors, more sophisticated, higher performance processors make use of speculation.

> 在动态安排的管道中，所有指令都以秩序(按顺序发行)通过了问题阶段；但是，它们可以在第二阶段(读取操作数)中停滞或可以绕过对方，从而输入执行。_scoreboarding_ 是一种技术，可以在有足够的资源且没有数据依赖的情况下允许指令执行订单；它以 CDC 6600 计分板的名字命名，该记分牌开发了此功能。在这里，我们专注于一种更柔和的技术，称为* tomasulo *’_ s algorithm。此外，可以扩展 tomasulo 的算法来处理\_speculation_，这是一种通过预测分支的结果，在预测的目标地址执行指令以及在预测是错误时采取纠正措施来减少控制依赖的效果的技术。虽然计分板的使用可能足以支持模拟处理器，但更复杂的高性能处理器可以利用投机。

### Dynamic Scheduling Using Tomasulo’s Approach

The IBM 360/91 floating-point unit used a sophisticated scheme to allow out-of- order execution. This scheme, invented by Robert Tomasulo, tracks when oper- ands for instructions are available to minimize RAW hazards and introduces reg- ister renaming in hardware to minimize WAW and WAR hazards. Although there are many variations of this scheme in recent processors, they all rely on two key principles: dynamically determining when an instruction is ready to execute and renaming registers to avoid unnecessary hazards.

> IBM 360/91 浮点单元使用了复杂的方案来允许订单外执行。该计划由罗伯特·托马苏洛(Robert Tomasulo)发明，可以进行操作时，可以进行操作，以最大程度地减少原始危害并引入硬件重命名的重命名，以最大程度地减少 WAW 和战争危害。尽管该方案在最近的处理器中有很多变化，但它们都依赖于两个关键原则：**动态确定何时准备执行和重命名注册表以避免不必要的危害**。

IBM’s goal was to achieve high floating-point performance from an instruction set and from compilers designed for the entire 360 computer family, rather than from specialized compilers for the high-end processors. The 360 architecture had only four double-precision floating-point registers, which limited the effective- ness of compiler scheduling; this fact was another motivation for the Tomasulo approach. In addition, the IBM 360/91 had long memory accesses and long floating-point delays, which Tomasulo’s algorithm was designed to overcome. At the end of the section, we will see that Tomasulo’s algorithm can also support the overlapped execution of multiple iterations of a loop.

> IBM 的目标是从指令集和为整个 360 计算机家族设计的编译器，而不是从高端处理器的专业编译器中实现高浮点的性能。360 体系结构只有四个双重精确的浮点寄存器，这限制了编译器计划的有效性；这个事实是 Tomasulo 方法的另一个动机。此外，IBM 360/91 具有较长的内存访问和较长的浮点延迟，Tomasulo 的算法旨在克服这些算法。在本节的末尾，我们将看到 Tomasulo 的算法还可以支持重叠的循环多次迭代。

We explain the algorithm, which focuses on the floating-point unit and load- store unit, in the context of the RISC-V instruction set. The primary difference between RISC-V and the 360 is the presence of register-memory instructions in the latter architecture. Because Tomasulo’s algorithm uses a load functional unit, no significant changes are needed to add register-memory addressing modes. The IBM 360/91 also had pipelined functional units, rather than multiple functional units, but we describe the algorithm as if there were multiple functional units. It is a simple conceptual extension to also pipeline those functional units.

> 我们在 RISC-V 指令集的背景下解释了该算法，该算法着重于浮点单元和负载存储单元。RISC-V 和 360 之间的主要区别在于后者体系结构中存在寄存器记录说明。由于 Tomasulo 的算法使用负载功能单元，因此不需要重大更改即可添加寄存器记忆地址模式。IBM 360/91 还具有管道的功能单元，而不是多个功能单元，但是我们将算法描述好像有多个功能单元一样。这是一个简单的概念扩展，也可以管道这些功能单元。

RAW hazards are avoided by executing an instruction only when its operands are available, which is exactly what the simpler scoreboarding approach provides. WAR and WAW hazards, which arise from name dependences, are eliminated by register renaming. _Register renaming_ eliminates these hazards by renaming all destination registers, including those with a pending read or write for an earlier instruction, so that the out-of-order write does not affect any instructions that depend on an earlier value of an operand. The compiler could typically implement such renaming, if there were enough registers available in the ISA. The original 360/91 had only four floating-point registers, and Tomasulo’s algorithm was cre- ated to overcome this shortage. Whereas modern processors have 32–64 floating- point and integer registers, the number of renaming registers available in recent implementations is in the hundreds.

> 仅在可用操作数时执行指令才能避免原始危害，这正是更简单的计分板方法所提供的。由名称依赖引起的战争和 WAW 危害通过登记册重命名消除。* register renaming*通过重命名所有目的地寄存器，包括较早的指令读取或写入的所有目标寄存器，从而消除了这些危害，从而使 dorter-forter debord debord nation dockister 不影响任何取决于操作数的较早价值的说明。如果 ISA 中有足够的寄存器，则编译器通常可以实施此类重命名。最初的 360/91 只有四个浮点登记册，而 Tomasulo 的算法则可以克服这一短缺。现代处理器有 32-64 个浮点和整数登记册，而最近实施中可用的重命名寄存器数量为数百。

To better understand how register renaming eliminates WAR and WAW haz- ards, consider the following example code sequence that includes potential WAR and WAW hazards:

> 为了更好地了解注册重命名如何消除战争和 WAW 危险，请考虑以下示例代码序列，其中包括潜在战争和 WAW 危害：

> ===

There are two antidependences: between the fadd.d and the fsub.d and between the fsd and the fmul.d. There is also an output dependence between the fadd.d and the fmul.d, leading to three possible hazards: WAR hazards on the use of f8 by fadd.d and its use by the fsub.d, as well as a WAW hazard because the fadd.d may finish later than the fmul.d. There are also three true data dependences: between the fdiv.d and the fadd.d, between the fsub.d and the fmul.d, and between the fadd.d and the fsd.

> 有两种抗想者：在 FADD.D 和 FSUB.D 之间以及 FSD 和 FMUL.D 之间。FADD.D 和 FMUL.D 之间也存在输出依赖性，导致了三种可能的危害：FADD.D 使用 F8 的战争危害及其使用 FSUB.D，以及 WAW 危险，因为 FADD.D 可能比 FMUL.D 完成。还有三个真实的数据依赖性：在 fdiv.d 和 fadd.d 之间，fsub.d 和 fmul.d 之间以及 FADD.D 和 FSD 之间。

These three name dependences can all be eliminated by register renaming. For simplicity, assume the existence of two temporary registers, S and T. Using S and T, the sequence can be rewritten without any dependences as

> 这三个名称依赖都可以通过登记命名来消除。为简单起见，假设存在两个临时寄存器 S 和 T。使用 S 和 T，可以在没有任何依赖的情况下重写该序列

> ===

In addition, any subsequent uses of f8 must be replaced by the register T. In this example, the renaming process can be done statically by the compiler. Finding any uses of f8 that are later in the code requires either sophisticated compiler analysis or hardware support because there may be intervening branches between the pre- ceding code segment and a later use of f8. As we will see, Tomasulo’s algorithm can handle renaming across branches.

> 此外，必须将 F8 的任何后续用途替换为寄存器 T。在此示例中，编译器可以静态地进行重命名过程。查找较晚代码中的 F8 的任何用途都需要复杂的编译器分析或硬件支持，因为预审代码段与后来的 F8 之间可能存在中间分支。正如我们将看到的那样，Tomasulo 的算法可以处理各个分支的重命名。

In Tomasulo’s scheme, register renaming is provided by _reservation stations_, which buffer the operands of instructions waiting to issue and are associated with the functional units. The basic idea is that a reservation station fetches and buffers an operand as soon as it is available, eliminating the need to get the operand from a register. In addition, pending instructions designate the reservation station that will provide their input. Finally, when successive writes to a register overlap in execution, only the last one is actually used to update the register. As instructions are issued, the register specifiers for pending operands are renamed to the names of the reservation station, which provides register renaming.

> 在 Tomasulo 的方案中，登记量重命名由*Reservation Stations *提供，它可以缓解等待发行并与功能单元关联的说明操作数。基本想法是，预订站一旦可用，就可以从操作数获取并缓冲操作数，从而消除了从寄存器中获取操作数的需求。此外，待处理指示指定将提供其输入的预订站。最后，当连续写入寄存器的执行重叠时，只有最后一个实际上用于更新寄存器。在发布说明时，待处理操作数的登记册指定符被更名为预订站的名称，该预订站提供了登记册的重命名。

Because there can be more reservation stations than real registers, the technique can even eliminate hazards arising from name dependences that could not be elim- inated by a compiler. As we explore the components of Tomasulo’s scheme, we will return to the topic of register renaming and see exactly how the renaming occurs and how it eliminates WAR and WAW hazards.

> 因为比实际注册者可能有更多的预订站，所以该技术甚至可以消除由编译器无法消除的名称依赖引起的危害。当我们探索 Tomasulo 计划的组成部分时，我们将返回注册重命名的主题，并确切地查看重命名的方式以及它如何消除战争和 WAW 危害。

The use of reservation stations, rather than a centralized register file, leads to two other important properties. First, hazard detection and execution control are distributed: the information held in the reservation stations at each functional unit determines when an instruction can begin execution at that unit. Second, results are passed directly to functional units from the reservation stations where they are buffered, rather than going through the registers. This bypassing is done with a common result bus that allows all units waiting for an operand to be loaded simul- taneously (on the 360/91, this is called the _common data bus_, or CDB). In pipelines that issue multiple instructions per clock and also have multiple execution units, more than one result bus will be needed.

> 保留站的使用，而不是集中式寄存器文件，导致了另外两个重要属性。首先，分配了危险检测和执行控制：在每个功能单元的预订站中保存的信息确定指令何时可以在该单元处执行。其次，结果将直接传递给预订站的功能单元，而这些单元被缓冲而不是浏览寄存器。此旁路是使用通用结果总线完成的，该总线允许所有等待操作数的单位模拟加载(在 360/91 上，这称为 _Common Data Bus_ 或 CDB)。在每个时钟发出多个说明并具有多个执行单元的管道中，将需要多个结果总线。

[Figure 3.10](#_bookmark108) shows the basic structure of a Tomasulo-based processor, includ- ing both the floating-point unit and the load/store unit; none of the execution con- trol tables is shown. Each reservation station holds an instruction that has been issued and is awaiting execution at a functional unit. If the operand values for that instruction have been computed, they are also stored in that entry; otherwise, the reservation station entry keeps the names of the reservation stations that will pro- vide the operand values.

> [图 3.10](#_bookmark108) 显示了基于 tomasulo 的处理器的基本结构，其中包括浮点单元和负载/存储单元；没有显示执行控制表。每个预订站都有已发行的指令，并正在等待功能部门执行。如果计算了该指令的操作数值，则它们也存储在该条目中；否则，预订站条目将保留预订站的名称，这些预订站将为操作数值提供。

The load buffers and store buffers hold data or addresses coming from and going to memory and behave almost exactly like reservation stations, so we dis- tinguish them only when necessary. The floating-point registers are connected by a pair of buses to the functional units and by a single bus to the store buffers. All results from the functional units and from memory are sent on the common data bus, which goes everywhere except to the load buffer. All reservation stations have tag fields, employed by the pipeline control.

> 负载缓冲区和存储缓冲区保留了来自和进入内存的数据或地址几乎完全像预订站一样，因此我们只有在必要时才分辨它们。浮点寄存器通过一对公共汽车连接到功能单元，并通过单个总线连接到存储缓冲区。来自功能单元和内存的所有结果均在公共数据总线上发送，除了负载缓冲区外，该总线无处不在。所有预订站都有管道控制使用的标签字段。

Before we describe the details of the reservation stations and the algorithm, let’s look at the steps an instruction goes through. There are only three steps, although each one can now take an arbitrary number of clock cycles:

> 在描述预订站和算法的详细信息之前，让我们看一下指令通过的步骤。只有三个步骤，尽管现在每个步骤都可以进行任意数量的时钟周期：

1. _Issue_—Get the next instruction from the head of the instruction queue, which is maintained in FIFO order to ensure the maintenance of correct data flow. If there is a matching reservation station that is empty, issue the instruction to the station with the operand values, if they are currently in the registers. If there is not an empty reservation station, then there is a structural hazard, and the instruction issue stalls until a station or buffer is freed. If the operands are not in the reg- isters, keep track of the functional units that will produce the operands. This step renames registers, eliminating WAR and WAW hazards. (This stage is some- times called _dispatch_ in a dynamically scheduled processor.)

> 1. _issue_-从指令队列的头部获取下一个指令，该指令按 FIFO 顺序维护，以确保维护正确的数据流。如果有一个空的匹配预订站，如果目前在寄存器中，则将指令发送给车站。如果没有空的预订站，则会存在结构性危害，并且指令问题停滞不前，直到释放站或缓冲区为止。如果操作数不在重新计算机中，请跟踪会产生操作数的功能单元。这一步骤重命名为注册，消除了战争和 WAW 危害。(此阶段在动态计划的处理器中有时称为 _disPatch_。)

![](../media/image202.png)

Figure 3.10 The basic structure of a RISC-V floating-point unit using Tomasulo’s algorithm. Instructions are sent from the instruction unit into the instruction queue from which they are issued in first-in, first-out (FIFO) order. The reservation stations include the operation and the actual operands, as well as information used for detecting and resolving hazards. Load buffers have three functions: (1) hold the components of the effective address until it is com- puted, (2) track outstanding loads that are waiting on the memory, and (3) hold the results of completed loads that are waiting for the CDB. Similarly, store buffers have three functions: (1) hold the components of the effective address until it is computed, (2) hold the destination memory addresses of outstanding stores that are waiting for the data value to store, and (3) hold the address and value to store until the memory unit is available. All results from either the FP units or the load unit are put on the CDB, which goes to the FP register file as well as to the reservation stations and store buffers. The FP adders implement addition and subtraction, and the FP multipliers do multiplication and division.

> 图 3.10 使用 Tomasulo 的算法，RISC-V 浮点单元的基本结构。说明从指令单元发送到以第一票，第一(FIFO)订单发出的指令队列。预订站包括操作和实际操作数，以及用于检测和解决危害的信息。负载缓冲区具有三个功能：
> (1)保留有效地址的组件，直到被调整为止，
> (2)正在等待内存的未偿还负载；
> (3)保留正在等待的完整负载的结果 CDB。
> 同样，存储缓冲区具有三个功能：
> (1)保留有效地址的组件，直到计算出来，
> (2)保留正在等待数据值存储的未销售存储的目标存储器地址，并且
> (3)保持地址和值存储，直到可用内存单元为止。
> FP 单元或负载单元的所有结果都放在 CDB 上，该 CDB 转到 FP 寄存器文件以及预订站和存储缓冲区。FP 加法器实现加法和减法，FP 乘数进行乘法和分裂。

1. _Execute_—If one or more of the operands is not yet available, monitor the com- mon data bus while waiting for it to be computed. When an operand becomes available, it is placed into any reservation station awaiting it. When all the oper- ands are available, the operation can be executed at the corresponding functional unit. By delaying instruction execution until the operands are available, RAW hazards are avoided. (Some dynamically scheduled processors call this step "issue," but we use the name "execute," which was used in the first dynamically scheduled processor, the CDC 6600.)

> 1. _execute_ - 如果尚未使用一个或多个操作数，请在等待其计算时监视 common 数据总线。当操作数可用时，将其放置在等待它的任何预订站中。当所有操作都可用时，可以在相应的功能单元处执行操作。通过延迟指令执行，直到操作数可用，可以避免原始危害。(一些动态计划的处理器称此步骤为 "问题" ，但我们使用名称为 "执行" ，该名称已在第一个动态计划的处理器 CDC 6600 中使用。)

Notice that several instructions could become ready in the same clock cycle for the same functional unit. Although independent functional units could begin execution in the same clock cycle for different instructions, if more than one instruction is ready for a single functional unit, the unit will have to choose among them. For the floating-point reservation stations, this choice may be made arbitrarily; loads and stores, however, present an additional complication.

> 请注意，对于同一功能单元，可以在同一时钟周期中准备好几个说明。尽管独立的功能单元可以在相同的时钟周期中开始执行不同的指令，但如果为单个功能单元准备了多个指令，则该单元必须在其中选择。对于浮点预订站，可以任意做出此选择；但是，负载和存储会带来额外的并发症。

Loads and stores require a two-step execution process. The first step com- putes the effective address when the base register is available, and the effective address is then placed in the load or store buffer. Loads in the load buffer exe- cute as soon as the memory unit is available. Stores in the store buffer wait for the value to be stored before being sent to the memory unit. Loads and stores are maintained in program order through the effective address calculation, which will help to prevent hazards through memory.

> 负载和存储需要两个步骤的执行过程。第一步是在可用的基本寄存器时将有效地址汇总的，然后将有效地址放置在负载或存储缓冲区中。可用内存单元后，将在负载缓冲区的负载中加载。存储在存储缓冲区中等待将值存储在发送到存储单元之前。通过有效的地址计算以程序顺序维护负载和存储，这将有助于防止通过记忆危害。

To preserve exception behavior, no instruction is allowed to initiate execu- tion until a branch that precedes the instruction in program order has completed. This restriction guarantees that an instruction that causes an exception during execution really would have been executed. In a processor using branch predic- tion (as all dynamically scheduled processors do), this means that the processor must know that the branch prediction was correct before allowing an instruction after the branch to begin execution. If the processor records the occurrence of the exception, but does not actually raise it, an instruction can start execution but not stall until it enters Write Result.

> 为了保留异常行为，不允许启动执行指令，直到计划顺序中的指令之前的分支已经完成。这种限制确保了在执行过程中导致例外的指令确实将被执行。在使用分支预测的处理器中(如所有动态计划的处理器所做的那样)，这意味着处理器必须知道分支预测是正确的，然后才能在分支开始执行后允许使用指令。如果处理器记录了异常的出现，但实际上并未提高该异常，则指令可以开始执行，但直到输入写入结果之前就不会失速。

Speculation provides a more flexible and more complete method to handle exceptions, so we will delay making this enhancement and show how specula- tion handles this problem later.

> 猜测提供了一种更灵活，更完整的方法来处理异常，因此我们将延迟进行此增强，并显示何时猜测如何处理此问题。

1. _Write result_—When the result is available, write it on the CDB and from there into the registers and into any reservation stations (including store buffers) wait- ing for this result. Stores are buffered in the store buffer until both the value to be stored and the store address are available; then the result is written as soon as the memory unit is free.

> 1. _write 结果_-当结果可用时，将其写在 CDB 上，然后从那里写入寄存器，然后将其写入任何预订站(包括存储缓冲区)等待此结果。存储在存储缓冲区中进行缓冲，直到两个要存储的价值都可以使用为止。然后，在内存单元免费后立即写入结果。

The data structures that detect and eliminate hazards are attached to the reserva- tion stations, to the register file, and to the load and store buffers with slightly dif- ferent information attached to different objects. These tags are essentially names for an extended set of virtual registers used for renaming. In our example, the tag field is a 4-bit quantity that denotes one of the five reservation stations or one of the five load buffers. This combination produces the equivalent of 10 registers (5 reservation sta- tions+ 5 load buffers) that can be designated as result registers (as opposed to the four double-precision registers that the 360 architecture contains). In a processor with more real registers, we want renaming to provide an even larger set of virtual registers, often numbering in the hundreds. The tag field describes which reservation station contains the instruction that will produce a result needed as a source operand.

> 检测和消除危害的数据结构附在保留站，寄存器文件以及与不同对象附加的信息略有不同的信息上的负载和存储缓冲区。这些标签本质上是用于重命名的一组扩展的虚拟寄存器集的名称。在我们的示例中，标签字段是一个 4 位数量，表示五个预订站之一或五个负载缓冲区之一。这种组合产生的相当于 10 个寄存器(5 个预订式 + 5 个负载缓冲区)，可以将其指定为结果寄存器(与 360 架构包含的四个双重精确寄存器相反)。在具有更多真实寄存器的处理器中，我们希望重命名提供更大的虚拟寄存器集，通常是数百个编号。标签字段描述了哪个预订站包含的指令，该指令将产生作为源操作数所需的结果。

Once an instruction has issued and is waiting for a source operand, it refers to the operand by the reservation station number where the instruction that will write the register has been assigned. Unused values, such as zero, indicate that the oper- and is already available in the registers. Because there are more reservation stations than actual register numbers, WAW and WAR hazards are eliminated by renaming results using reservation station numbers. Although in Tomasulo’s scheme the res- ervation stations are used as the extended virtual registers, other approaches could use a register set with additional registers or a structure like the reorder buffer, which we will see in [Section 3.6](#hardware-based-speculation).

> 一旦发出了指令并正在等待源操作数，它是指保留站编号的操作数，在该编号中，将撰写寄存器的指令已分配。未使用的值(例如零)表明寄存器中的操作和已经可用。由于保留站的预订站多于实际寄存器编号，因此通过使用预订站编号重命名结果来消除 WAW 和 WAR 危险。尽管在 Tomasulo 的方案中，重新介绍站被用作扩展的虚拟寄存器，但其他方法可以使用带有其他寄存器的寄存器集或诸如重新订购缓冲区之类的结构，我们将在[3.6](＃基于 Hartware-的第 3.6 节)中看到猜测)。

In Tomasulo’s scheme, as well as the subsequent methods we look at for sup- porting speculation, results are broadcast on a bus (the CDB), which is monitored by the reservation stations. The combination of the common result bus and the retrieval of results from the bus by the reservation stations implements the forward- ing and bypassing mechanisms used in a statically scheduled pipeline. In doing so, however, a dynamically scheduled scheme, such as Tomasulo’s algorithm, intro- duces one cycle of latency between source and result because the matching of a result and its use cannot be done until the end of the Write Result stage, as opposed to the end of the Execute stage for a simpler pipeline. Thus, in a dynamically sched- uled pipeline, the effective latency between a producing instruction and a consum- ing instruction is at least one cycle longer than the latency of the functional unit producing the result.

> 在 Tomasulo 的方案以及我们为支持投机的后续方法中，结果在总线(CDB)上广播，该结果由预订站监视。预订站的共同结果总线和从总线取回结果的结合，实现了静态计划管道中使用的向前和绕过机制。但是，这样做的动态计划方案，例如 tomasulo 的算法，介绍了源和结果之间的一个延迟周期，因为结果及其使用及其使用才能在写入结果阶段结束之前进行，以相反到执行阶段的结尾，以进行更简单的管道。因此，在动态的计划管道中，生产指令和消费指令之间的有效延迟至少比产生结果的功能单元的延迟时间更长。

It is important to remember that the tags in the Tomasulo scheme refer to the buffer or unit that will produce a result; the register names are discarded when an instruction issues to a reservation station. (This is a key difference between Toma- sulo’s scheme and scoreboarding: in scoreboarding, operands stay in the registers and are read only after the producing instruction completes and the consuming instruction is ready to execute.)

> 重要的是要记住，tomasulo 方案中的标签是指将产生结果的缓冲区或单元。当指令向预订站发出指令时，寄存器名称将被丢弃。(这是 Toma-Sulo 的方案和记分牌之间的关键区别：在记分板中，操作数留在寄存器中，并且仅在生产指令完成并准备执行消耗指令后才阅读。)
