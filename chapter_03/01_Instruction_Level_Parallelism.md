## Instruction-Level Parallelism: Concepts and Challenges(指令级并行性：概念和挑战)

All processors since about 1985 have used pipelining to overlap the execution of instructions and improve performance. This potential overlap among instructions is called _instruction-level parallelism_ (ILP), because the instructions can be eval- uated in parallel. In this chapter and Appendix H, we look at a wide range of tech- niques for extending the basic pipelining concepts by increasing the amount of parallelism exploited among instructions.

> 自 1985 年左右以来，所有处理器都使用管道来重叠指令的执行并提高性能。指令之间的这种潜在重叠称为 _instruction-plavel Parallelism_(ILP)，因为可以并行评估指令。在本章和附录 H 中，我们研究了广泛的技术，用于通过增加指令中利用的并行性数量来扩展基本的管道概念。

This chapter is at a considerably more advanced level than the material on basic pipelining in [Appendix C](#_bookmark481). If you are not thoroughly familiar with the ideas in [Appendix C](#_bookmark481), you should review that appendix before venturing into this chapter. We start this chapter by looking at the limitation imposed by data and control hazards and then turn to the topic of increasing the ability of the compiler and the processor to exploit parallelism. These sections introduce a large number of concepts, which we build on throughout this chapter and the next. While some of the more basic material in this chapter could be understood without all of the ideas in the first two sections, this basic material is important to later sections of this chapter.

> 本章比 [Appendix C](#_bookmark481) 中基本管道上的材料要高得多。如果您不熟悉 [Appendix C](#_bookmark481) 中的想法，则应在进行本章之前检查该附录。我们通过研究数据和控制危害所施加的限制开始本章，然后转向提高编译器和处理器利用并行性的能力的主题。这些部分介绍了大量概念，我们在本章和下一章中构建了这些概念。尽管如果没有前两个部分中的所有思想，就可以理解本章中一些更基本的材料，但这种基本材料对于本章的后期部分很重要。

There are two largely separable approaches to exploiting ILP: (1) an approach that relies on hardware to help discover and exploit the parallelism dynamically, and (2) an approach that relies on software technology to find parallelism statically at compile time. Processors using the dynamic, hardware-based approach, includ- ing all recent Intel and many ARM processors, dominate in the desktop and server markets. In the personal mobile device market, the same approaches are used in processors found in tablets and high-end cell phones. In the IOT space, where power and cost constraints dominate performance goals, designers exploit lower levels of instruction-level parallelism. Aggressive compiler-based approaches have been attempted numerous times beginning in the 1980s and most recently in the Intel Itanium series, introduced in 1999. Despite enormous efforts, such approaches have been successful only in domain-specific environments or in well-structured scientific applications with significant data-level parallelism.

> 利用 ILP 的两种基本可分开的方法：(1)一种依赖硬件来帮助动态发现和利用并行性的方法，(2)一种依赖软件技术在编译时静态地找到并行性的方法。使用基于动态的，基于硬件的方法的处理器，包括所有最近的英特尔和许多 ARM 处理器，在台式机和服务器市场中占主导地位。在个人移动设备市场中，在平板电脑和高端手机中发现的处理器中使用了相同的方法。在物联网空间中，功率和成本约束主导性能目标，设计师利用了较低的指导级并行性。从 1980 年代开始，最近在 1999 年推出的英特尔 Itanium 系列中尝试了多次基于积极的编译器的方法。尽管做出了巨大的努力，但这种方法仅在特定于域的环境或结构良好的科学应用中才成功。重要的数据级并行性。

In the past few years, many of the techniques developed for one approach have been exploited within a design relying primarily on the other. This chapter intro- duces the basic concepts and both approaches. A discussion of the limitations on ILP approaches is included in this chapter, and it was such limitations that directly led to the movement toward multicore. Understanding the limitations remains important in balancing the use of ILP and thread-level parallelism.

> 在过去的几年中，为一种方法开发的许多技术主要依赖于另一种方法。本章介绍了基本概念和两种方法。本章包括对 ILP 方法的局限性的讨论，正是这种局限性直接导致了多项运动的运动。了解局限性在平衡 ILP 和线程级并行性的使用方面仍然很重要。

In this section, we discuss features of both programs and processors that limit the amount of parallelism that can be exploited among instructions, as well as the critical mapping between program structure and hardware structure, which is key to understanding whether a program property will actually limit performance and under what circumstances.

> 在本节中，我们将讨论程序和处理器的功能，这些功能限制了可以在说明之间利用的并行性的数量，以及程序结构和硬件结构之间的关键映射，这是了解程序属性是否会真正限制的关键性能和在什么情况下。

The value of the CPI (cycles per instruction) for a pipelined processor is the sum of the base CPI and all contributions from stalls:

> CPI(每指令循环)对管道处理器的值是基本 CPI 的总和，也是摊位的所有贡献：

> Figure 3.1 The major techniques examined in [Appendix C](#_bookmark0), Chapter 3, and Appendix H are shown together with the component of the CPI equation that the technique affects.

The _ideal pipeline CPI_ is a measure of the maximum performance attainable by the implementation. By reducing each of the terms of the right-hand side, we decrease the overall pipeline CPI or, alternatively, increase the IPC (instructions per clock). The preceding equation allows us to characterize various techniques by what com- ponent of the overall CPI a technique reduces. [Figure 3.1](#_bookmark96) shows the techniques we examine in this chapter and in Appendix H, as well as the topics covered in the introductory material in [Appendix C](#_bookmark481). In this chapter, we will see that the tech- niques we introduce to decrease the ideal pipeline CPI can increase the importance of dealing with hazards.

> _ideal pipeline CPI_ 是可实现的最大性能的度量。通过减少右侧的每个术语，我们减少了整体管道 CPI 或增加 IPC(每个时钟指令)。前面的方程式使我们能够通过总体 CPI A 技术的哪些组合来降低各种技术来表征各种技术。[图 3.1](#_bookmark96) 显示了我们在本章和附录 H 中检查的技术，以及[附录 C](#_bookmark481) 中介绍材料中涵盖的主题。在本章中，我们将看到我们介绍的技术降低理想管道 CPI 可以增加处理危险的重要性。

### What Is Instruction-Level Parallelism?(什么是指令级并行性？)

All the techniques in this chapter exploit parallelism among instructions. The amount of parallelism available within a _basic block_—a straight-line code sequence with no branches in except to the entry and no branches out except at the exit—is quite small. For typical RISC programs, the average dynamic branch frequency is often between 15% and 25%, meaning that between three and six instructions execute between a pair of branches. Because these instructions are likely to depend upon one another, the amount of overlap we can exploit within a basic block is likely to be less than the average basic block size. To obtain substantial performance enhancements, we must exploit ILP across multiple basic blocks.

> 本章中的所有技术都利用了指令之间的并行性。_basic block_ 中可用的并行量非常小——一个直线代码序列，除了入口没有分支，除了出口没有分支。对于典型的 RISC 程序，平均动态分支频率通常在 15% 到 25% 之间，这意味着在一对分支之间执行 3 到 6 条指令。因为这些指令可能相互依赖，所以我们可以在一个基本块中利用的重叠量可能小于平均基本块大小。为了获得实质性的性能增强，我们必须跨多个基本块利用 ILP。

The simplest and most common way to increase the ILP is to exploit parallel- ism among iterations of a loop. This type of parallelism is often called _loop-level parallelism_. Here is a simple example of a loop that adds two 1000-element arrays and is completely parallel:

> 增加 ILP 的最简单和最常见的方法是利用循环迭代之间的并行性。这种类型的并行性通常称为 _循环级并行性_。下面是一个简单的循环示例，它添加了两个 1000 元素的数组并且完全并行：

> ===

Every iteration of the loop can overlap with any other iteration, although within each loop iteration, there is little or no opportunity for overlap.

> 循环的每一次迭代都可以与任何其他迭代重叠，尽管在每个循环迭代中，几乎没有重叠的机会。

We will examine a number of techniques for converting such loop-level parallelism into instruction-level parallelism. Basically, such techniques work by unrolling the loop either statically by the compiler (as in the next section) or dynamically by the hardware (as in [Sections 3.5](#dynamic-scheduling-examples-and-the-algorithm) and [3.6](#hardware-based-speculation)).

> 我们将研究一些将这种循环级并行性转换为指令级并行性的技术。基本上，此类技术通过由编译器静态展开循环(如下一节所述)或由硬件动态展开循环(如 [Sections 3.5](#dynamic-scheduling-examples-and-the-algorithm) 和 [3.6 ](#hardware-based-speculation))。

An important alternative method for exploiting loop-level parallelism is the use of SIMD in both vector processors and graphics processing units (GPUs), both of which are covered in [Chapter 4](#_bookmark165). A SIMD instruction exploits data-level parallelism by operating on a small to moderate number of data items in parallel (typically two to eight). A vector instruction exploits data-level parallelism by operating on many data items in parallel using both parallel execution units and a deep pipe- line. For example, the preceding code sequence, which in simple form requires seven instructions per iteration (two loads, an add, a store, two address updates, and a branch) for a total of 7000 instructions, might execute in one-quarter as many instructions in some SIMD architecture where four data items are processed per instruction. On some vector processors, this sequence might take only four instruc- tions: two instructions to load the vectors x and y from memory, one instruction to add the two vectors, and an instruction to store back the result vector. Of course, these instructions would be pipelined and have relatively long latencies, but these latencies may be overlapped.

> 利用循环级并行性的一种重要替代方法是在矢量处理器和图形处理单元(GPU)中使用 SIMD，这两者都涵盖在[第 4 章](#_bookmark165)中。SIMD 指令通过并行在少量到中等的数据项(通常 2 到 8)上操作数据级并行。向量指令通过使用并行执行单元和深管线并行处理许多数据项来利用数据级并行。例如，以简单的形式以每次迭代需要七个指令(两个负载，一个添加，一个存储，两个地址更新和一个分支)，总共需要 7000 个指令，可能会在四分之一中执行七个说明在某些 SIMD 体系结构中的说明，每个指令处理四个数据项。在某些矢量处理器上，此序列可能仅需四个指令：从内存中加载向量 x 和 y 的两个指令，一项指令添加两个向量，以及一份指令，以存储结果向量。当然，这些说明将是管道的，并具有相对较长的延迟，但是这些潜伏期可能会重叠。

### Data Dependences and Hazards(数据依赖和危害)

Determining how one instruction depends on another is critical to determining how much parallelism exists in a program and how that parallelism can be exploited. In particular, to exploit instruction-level parallelism, we must determine which instructions can be executed in parallel. If two instructions are _parallel_, they can execute simultaneously in a pipeline of arbitrary depth without causing any stalls, assuming the pipeline has sufficient resources (and thus no structural hazards exist). If two instructions are dependent, they are not parallel and must be executed in order, although they may often be partially overlapped. The key in both cases is to determine whether an instruction is dependent on another instruction.

> 确定一个指令如何依赖另一个指令对于确定程序中存在多少并行性以及如何利用该并行性。特别是，要利用指令级并行性，我们必须确定可以并行执行哪些指令。如果两个说明是 _Parallel_，则可以在不引起任何摊位的情况下同时执行任意深度的管道，假设管道具有足够的资源(因此不存在结构性危害)。如果两个说明取决于指示，则它们不平行，必须按顺序执行，尽管它们通常会部分重叠。两种情况下的关键是确定指令是否取决于另一个指令。

##### _Data Dependences_

There are three different types of dependences: _data dependences_ (also called true data dependences), _name dependences_, and _control dependences_. An instruction _j_ is _data-dependent_ on instruction _i_ if either of the following holds:

> 存在三种不同类型的依赖性：_数据依赖性_(也称为真实数据依赖性)、_名称依赖性_ 和 _控制依赖性_。如果满足以下任一条件，指令 _j_ _data-dependent_ 依赖于指令 _i_ ：

- Instruction _i_ produces a result that may be used by instruction _j._
- Instruction _j_ is data-dependent on instruction _k_, and instruction _k_ is data- dependent on instruction _i._

> - 指令 _i_ 产生的结果可能由指令 _J._
> - 指令 _j_ 是数据依赖于指令 _k_，而指令 _k_ 是数据取决于指令 _i._。

The second condition simply states that one instruction is dependent on another if there exists a chain of dependences of the first type between the two instructions. This dependence chain can be as long as the entire program. Note that a depen- dence within a single instruction (such as add x1,x1,x1) is not considered a dependence.

> 第二个条件只是指出，如果存在两个指令之间的第一类类型的依赖链，则一项指令取决于另一个指令。这个依赖链可以与整个程序一样长。请注意，单个指令(例如添加 x1，x1，x1)中的依赖性不被视为依赖性。

For example, consider the following RISC-V code sequence that increments a vector of values in memory (starting at 0(x1) ending with the last element at 0(x2)) by a scalar in register f2.

> 例如，请考虑以下 RISC-V 代码序列，该序列会通过寄存器 F2 中的标量递增内存中的值(从 0(x1)开始，最后一个(x1)以 0(x2)的结尾)。

> ===

![](../media/image198.png)

The data dependences in this code sequence involve both floating-point data:

In both of the preceding dependent sequences, as shown by the arrows, each instruction depends on the previous one. The arrows here and in following exam- ples show the order that must be preserved for correct execution. The arrow points from an instruction that must precede the instruction that the arrowhead points to. If two instructions are data-dependent, they must execute in order and cannot execute simultaneously or be completely overlapped. The dependence implies that there would be a chain of one or more data hazards between the two instructions. (See [Appendix C](#_bookmark481) for a brief description of data hazards, which we will define precisely in a few pages.) Executing the instructions simultaneously will cause a processor with pipeline interlocks (and a pipeline depth longer than the distance between the instructions in cycles) to detect a hazard and stall, thereby reducing or eliminating the overlap. In a processor without interlocks that relies on compiler scheduling, the compiler cannot schedule dependent instructions in such a way that they completely overlap because the program will not execute correctly. The pres- ence of a data dependence in an instruction sequence reflects a data dependence in the source code from which the instruction sequence was generated. The effect of the original data dependence must be preserved.

> 此代码序列中的数据依赖性涉及两个浮点数据：
> 如箭头所示，在前面的两个相关序列中，每个指令都取决于上一个指令。此处和以下检查中的箭头显示必须保留以正确执行的顺序。箭头从指令指向必须先于箭头指向的指令。如果两个指令取决于数据，则必须按顺序执行并且不能同时执行或完全重叠。依赖性意味着两个说明之间将存在一个或多个数据危害的链。(有关数据危害的简要说明，请参见[附录 C](#_bookmark481)，我们将在几页中精确定义。循环中的说明)以检测危险和失速，从而减少或消除重叠。在没有依赖编译器计划的无连锁店的处理器中，编译器无法以完全重叠的方式安排相关说明，因为程序无法正确执行。指令序列中数据依赖性的主张反映了产生指令序列的源代码中的数据依赖性。必须保留原始数据依赖性的效果。

Dependences are a property of _programs._ Whether a given dependence results in an actual hazard being detected and whether that hazard actually causes a stall are properties of the _pipeline organization._ This difference is critical to understand- ing how instruction-level parallelism can be exploited.

> 依赖关系是 _programs._ 给定的依赖关系是否导致检测到实际危险以及该危险是否实际导致停顿是 _pipeline organization。_ 这种差异对于理解指令级并行性如何是至关重要的剥削。

A data dependence conveys three things: (1) the possibility of a hazard, (2) the order in which results must be calculated, and (3) an upper bound on how much parallelism can possibly be exploited. Such limits are explored in a pitfall on [page 262](#_bookmark46) and in Appendix H in more detail.

> 数据依赖性传达了三件事：(1)危险的可能性，(2)必须计算结果的顺序，以及(3)上限可以利用多少平行性。在[第 262 页](#_bookmark46)和附录 H 上的陷阱中探索了此类限制。

Because a data dependence can limit the amount of instruction-level parallel- ism we can exploit, a major focus of this chapter is overcoming these limitations. A dependence can be overcome in two different ways: (1) maintaining the depen- dence but avoiding a hazard, and (2) eliminating a dependence by transforming the code. Scheduling the code is the primary method used to avoid a hazard without altering a dependence, and such scheduling can be done both by the compiler and by the hardware.

> 由于数据依赖性可以限制我们可以利用的指令级并行的数量，因此本章的主要重点是克服这些限制。可以通过两种不同的方式克服依赖性：(1)维持依赖性但避免危险，(2)通过转换代码来消除依赖性。安排代码是用于避免危害而无需更改依赖性的主要方法，并且可以由编译器和硬件完成此类调度。

A data value may flow between instructions either through registers or through memory locations. When the data flow occurs through a register, detecting the dependence is straightforward because the register names are fixed in the instruc- tions, although it gets more complicated when branches intervene and correctness concerns force a compiler or hardware to be conservative.

> 数据值可以通过寄存器或通过存储位置在指令之间流动。当数据流通过寄存器发生时，检测依赖性很简单，因为寄存器名称是固定的，尽管当分支干预和正确性涉及编译器或硬件时，该寄存器名称固定在仪器中。

Dependences that flow through memory locations are more difficult to detect because two addresses may refer to the same location but look different: For exam- ple, 100(x4) and 20(x6) may be identical memory addresses. In addition, the effective address of a load or store may change from one execution of the instruc- tion to another (so that 20(x4) and 20(x4) may be different), further compli- cating the detection of a dependence.

> 流过内存位置的依赖性更难检测到，因为两个地址可能是指同一位置，但看起来不同：对于检查，100(x4)和 20(x6)可能是相同的内存地址。另外，负载或存储的有效地址可能会从一个执行指令变为另一种指令(因此 20(x4)和 20(x4)可能不同)，进一步弥补了依赖性的检测。

In this chapter, we examine hardware for detecting data dependences that involve memory locations, but we will see that these techniques also have limita- tions. The compiler techniques for detecting such dependences are critical in unco- vering loop-level parallelism.

> 在本章中，我们检查了用于检测涉及内存位置的数据依赖的硬件，但是我们将看到这些技术也具有限制。用于检测此类依赖的编译器技术对于不遵循循环级并行性至关重要。

##### _Name Dependences_

The second type of dependence is a _name dependence._ A name dependence occurs when two instructions use the same register or memory location, called a _name_, but there is no flow of data between the instructions associated with that name. There are two types of name dependences between an instruction _i_ that _precedes_ instruc- tion _j_ in program order:

> 第二种类型的依赖是 _name dependence_。当两条指令使用相同的寄存器或内存位置(称为 _name_)时，就会发生名称依赖，但与该名称关联的指令之间没有数据流。在程序顺序中先于指令 _j_ 的指令 _i_ 之间有两种类型的名称依赖：

1. An _antidependence_ between instruction _i_ and instruction _j_ occurs when instruc- tion _j_ writes a register or memory location that instruction _i_ reads. The original ordering must be preserved to ensure that _i_ reads the correct value. In the example on page 171, there is an antidependence between fsd and addi on register x1.

> 1. 当指令 _j_ 写入指令 _i_ 读取的寄存器或内存位置时，指令 _i_ 和指令 _j_ 之间发生 _antidependence_。必须保留原始顺序以确保 _i_ 读取正确的值。在第 171 页的示例中，寄存器 x1 上的 fsd 和 addi 之间存在反依赖关系。

2. An _output dependence_ occurs when instruction _i_ and instruction _j_ write the same register or memory location. The ordering between the instructions must be preserved to ensure that the value finally written corresponds to instruction _j_.

> 2. 当指令 _i_ 和指令 _j_ 写入相同的寄存器或内存位置时，会发生 _output dependence_。必须保留指令之间的顺序，以确保最终写入的值对应于指令 _j_。

Both antidependences and output dependences are name dependences, as opposed to true data dependences, because there is no value being transmitted between the instructions. Because a name dependence is not a true dependence, instructions involved in a name dependence can execute simultaneously or be reordered, if the name (register number or memory location) used in the instructions is changed so the instructions do not conflict.

> 反依赖和输出依赖都是名称依赖，而不是真正的数据依赖，因为指令之间没有传输值。因为名称依赖不是真正的依赖，如果指令中使用的名称(寄存器号或内存位置)发生变化，则涉及名称依赖的指令可以同时执行或重新排序，因此指令不会冲突。

This renaming can be more easily done for register operands, where it is called _register renaming_. Register renaming can be done either statically by a compiler or dynamically by the hardware. Before describing dependences arising from branches, let’s examine the relationship between dependences and pipeline data hazards.

> 这种重命名可以更容易地为寄存器操作数完成，它被称为 *register renaming*。寄存器重命名可以由编译器静态完成，也可以由硬件动态完成。在描述由分支引起的依赖之前，让我们检查一下依赖和流水线数据风险之间的关系。

##### _Data Hazards_

A hazard exists whenever there is a name or data dependence between instructions, and they are close enough that the overlap during execution would change the order of access to the operand involved in the dependence. Because of the depen- dence, we must preserve what is called _program order_—that is, the order that the instructions would execute in if executed sequentially one at a time as determined by the original source program. The goal of both our software and hardware tech- niques is to exploit parallelism by preserving program order _only where it affects the outcome of the program._ Detecting and avoiding hazards ensures that neces- sary program order is preserved.

> 每当指令之间存在名称或数据依赖性时，就会存在危害，并且它们足够近，以至于执行过程中的重叠将改变依赖涉及的操作数的访问顺序。由于依赖性，我们必须保留所谓的*程序顺序*-也就是说，指令将执行的顺序是由原始源程序确定的一次依次执行的。我们的软件和硬件技术的目的是通过保存程序订单来利用并行性，只能在影响程序的结果的位置。

Data hazards, which are informally described in [Appendix C](#_bookmark481), may be classified as one of three types, depending on the order of read and write accesses in the instructions. By convention, the hazards are named by the ordering in the program that must be preserved by the pipeline. Consider two instructions _i_ and _j,_ with _i_ preceding _j_ in program order. The possible data hazards are

> [附录 C](#_bookmark481) 中非正式描述的数据冒险可分为三种类型之一，具体取决于指令中读取和写入访问的顺序。按照惯例，风险按必须由管道保留的程序中的顺序命名。考虑两条指令 _i_ 和 _j,_，其中 _i_ 在程序顺序中位于 _j_ 之前。可能的数据危害是

- RAW _(read after write)_—_j_ tries to read a source before _i_ writes it, so _j_ incor- rectly gets the _old_ value. This hazard is the most common type and corresponds to a true data dependence. Program order must be preserved to ensure that _j_ receives the value from _i_.

> - RAW _(写入后读取)_—_j_ 尝试在 _i_ 写入源之前读取它，因此 _j_ 错误地获取了 _old_ 值。这种冒险是最常见的类型，对应于真正的数据依赖。必须保留程序顺序以确保 _j_ 从 _i_ 接收值。

- WAW _(write after write)_—_j_ tries to write an operand before it is written by _i_. The writes end up being performed in the wrong order, leaving the value writ- ten by _i_ rather than the value written by _j_ in the destination. This hazard cor- responds to an output dependence. WAW hazards are present only in pipelines that write in more than one pipe stage or allow an instruction to proceed even when a previous instruction is stalled.

> - WAW _(write after write)_—_j_ 尝试在 _i_ 写入操作数之前写入操作数。写入最终以错误的顺序执行，将 _i_ 写入的值而不是 _j_ 写入的值留在目标中。这种危险对应于输出依赖性。WAW 危险仅存在于写入多个管道阶段或即使在前一条指令停滞时也允许指令继续执行的流水线中。

- WAR _(write after read)_—_j_ tries to write a destination before it is read by _i,_ so _i_ incorrectly gets the _new_ value. This hazard arises from an antidependence (or name dependence). WAR hazards cannot occur in most static issue pipelines— even deeper pipelines or floating-point pipelines—because all reads are early (in ID in the pipeline in [Appendix C](#_bookmark481)) and all writes are late (in WB in the pipe- line in [Appendix C](#_bookmark481)). A WAR hazard occurs either when there are some instruc- tions that write results early in the instruction pipeline _and_ other instructions that read a source late in the pipeline, or when instructions are reordered, as we will see in this chapter.

> - WAR _(读后写)_—_j_ 尝试在目标被 _i 读取之前写入目标，_ 因此 _i_ 错误地获取了 _new_ 值。这种危险来自反依赖(或名称依赖)。WAR 危险不会发生在大多数静态问题管道中——甚至更深的管道或浮点管道——因为所有读取都是早期的(在 [附录 C](#_bookmark481) 管道中的 ID 中)并且所有写入都是延迟的(在 WB 中 [附录 C](#_bookmark481) 中的管道)。当有一些指令在指令流水线的早期写入结果而其他指令在流水线的后期读取源时，或者当指令被重新排序时，就会发生 WAR 危险，正如我们将在本章中看到的那样。

Note that the RAR _(read after read)_ case is not a hazard.

### Control Dependences

The last type of dependence is a _control dependence._ A control dependence determines the ordering of an instruction, _i,_ with respect to a branch instruction so that instruction _i_ is executed in correct program order and only when it should be. Every instruction, except for those in the first basic block of the program, is controldependent on some set of branches, and in general, these control dependences must be preserved to preserve program order. One of the simplest examples of a control dependence is the dependence of the statements in the  "then"  part of an if statement on the branch. For example, in the code segment

> 最后一种依赖性是 _控制依赖性。_ 控制依赖性决定指令 _i,_ 相对于分支指令的顺序，以便指令 _i_ 以正确的程序顺序执行，并且仅在它应该执行的时候执行。除了程序的第一个基本块中的指令外，每条指令都依赖于某些分支集，并且通常，必须保留这些控制依赖性以保持程序顺序。控制依赖的最简单示例之一是 if 语句的 "then" 部分中的语句对分支的依赖。例如，在代码段

> ===

In general, two constraints are imposed by control dependences:

> 通常，控制依赖性施加了两个约束：

1. An instruction that is control-dependent on a branch cannot be moved _before_ the branch so that its execution _is no longer controlled_ by the branch. For example, we cannot take an instruction from the then portion of an if statement and move it before the if statement.

> 1. 不能将控制依赖于分支的指令移动到分支之前，这样它的执行就不再受分支控制。例如，我们不能从 if 语句的 then 部分获取指令并将其移动到 if 语句之前。

2. An instruction that is not control-dependent on a branch cannot be moved _after_ the branch so that its execution _is controlled_ by the branch. For example, we cannot take a statement before the if statement and move it into the then portion.

> 2. 不依赖于控制的指令不能在分支之后移动，以便其执行由分支控制。例如，我们不能将 if 语句之前的语句移到 then 部分。

When processors preserve strict program order, they ensure that control depen- dences are also preserved. We may be willing to execute instructions that should not have been executed, however, thereby violating the control dependences, _if_ we can do so without affecting the correctness of the program. Thus control depen- dence is not the critical property that must be preserved. Instead, the two properties critical to program correctness—and normally preserved by maintaining both data and control dependences—are the _exception behavior_ and the _data flow_.

> 当处理器保留严格的计划命令时，他们确保保留控制依赖性。我们可能愿意执行不应该执行的指令，但是，违反了控制依赖，_if_ 我们可以这样做而不会影响程序的正确性。因此，控制依赖不是必须保留的关键特性。取而代之的是，这两个属性对编程正确性至关重要(通常通过维护数据和控制依赖性来保存)是 _exception behavior_ 和 _data flow_。

Preserving the exception behavior means that any changes in the ordering of instruction execution must not change how exceptions are raised in the program. Often this is relaxed to mean that the reordering of instruction execution must not cause any new exceptions in the program. A simple example shows how maintain- ing the control and data dependences can prevent such situations. Consider this code sequence:

> 保留例外行为意味着指令执行的排序订购的任何更改都不得改变程序中的异常。通常，这是放松的，意味着指令执行的重新排序不得在程序中引起任何新的例外。一个简单的示例显示了如何维护控制和数据依赖性可以防止这种情况。考虑此代码顺序：

> ===

In this case, it is easy to see that if we do not maintain the data dependence involv- ing x2, we can change the result of the program. Less obvious is the fact that if we ignore the control dependence and move the load instruction before the branch, the load instruction may cause a memory protection exception. Notice that _no data dependence_ prevents us from interchanging the beqz and the ld; it is only the control dependence. To allow us to reorder these instructions (and still preserve the data dependence), we want to just ignore the exception when the branch is taken. In [Section 3.6](#hardware-based-speculation), we will look at a hardware technique, _speculation_, which allows us to overcome this exception problem. Appendix H looks at software tech- niques for supporting speculation.

> 在这种情况下，很容易看出，如果我们不维护涉及 x2 的数据依赖，我们可以改变程序的结果。不太明显的是，如果我们忽略控制依赖并将加载指令移动到分支之前，加载指令可能会导致内存保护异常。请注意，_无数据依赖性_ 阻止我们交换 beqz 和 ld；它只是控制依赖。为了允许我们重新排序这些指令(并仍然保留数据依赖性)，我们只想在采用分支时忽略异常。在[第 3.6 节](#hardware-based-speculation)中，我们将了解一种硬件技术，_speculation_，它可以让我们克服这个异常问题。附录 H 着眼于支持推测的软件技术。

The second property preserved by maintenance of data dependences and con- trol dependences is the data flow. The _data flow_ is the actual flow of data values among instructions that produce results and those that consume them. Branches make the data flow dynamic because they allow the source of data for a given instruction to come from many points. Put another way, it is insufficient to just maintain data dependences because an instruction may be data-dependent on more than one predecessor. Program order is what determines which predecessor will actually deliver a data value to an instruction. Program order is ensured by main- taining the control dependences.

> 通过维护数据依赖和控制依赖性保留的第二个属性是数据流。_data Flow_ 是产生结果的指令和消耗它们的指令之间的实际数据值。分支机构使数据流动性动态化，因为它们允许给定指令的数据源来自许多点。换句话说，仅维护数据依赖性是不足的，因为指令可能依赖于多个前身。程序顺序是确定哪个前任将实际将数据值传递给指令的原因。通过主导控制依赖性来确保程序顺序。

For example, consider the following code fragment:

> ===

In this example, the value of x1 used by the or instruction depends on whether the branch is taken or not. Data dependence alone is not sufficient to preserve correct- ness. The or instruction is data-dependent on both the add and sub instructions, but preserving that order alone is insufficient for correct execution.

> 在此示例中，或指令使用的 X1 值取决于分支是否已获取。仅数据依赖性不足以保留正确的信息。或指令均取决于添加和子指令，但仅保留该订单不足以正确执行。

Instead, when the instructions execute, the data flow must be preserved: If the branch is not taken, then the value of x1 computed by the sub should be used by the or, and if the branch is taken, the value of x1 computed by the add should be used by the or. By preserving the control dependence of the or on the branch, we prevent an illegal change to the data flow. For similar reasons, the sub instruc- tion cannot be moved above the branch. Speculation, which helps with the excep- tion problem, will also allow us to lessen the impact of the control dependence while still maintaining the data flow, as we will see in [Section 3.6](#hardware-based-speculation).

> 相反，当指令执行时，必须保留数据流：如果未采用分支，则应使用 OR 计算的 X1 值，如果采用分支，则如果获取分支，则由由添加应由 OR 使用。通过保留 OR 对分支的控制依赖性，我们可以防止对数据流的非法更改。由于类似的原因，子指导不能在分支上方移动。正如我们将在[3.6]第 3.6 节](＃基于硬件的规范)中看到的那样，猜测有助于解决问题问题，也将使我们能够减少控制依赖性的影响，同时保持数据流的影响。

Sometimes we can determine that violating the control dependence cannot affect either the exception behavior or the data flow. Consider the following code sequence:

> 有时我们可以确定违反控制依赖性不会影响异常行为或数据流。考虑以下代码序列：

> ===

Suppose we knew that the register destination of the sub instruction (x4) was unused after the instruction labeled skip. (The property of whether a value will be used by an upcoming instruction is called _liveness._) If x4 were unused, then changing the value of x4 just before the branch would not affect the data flow because x4 would be _dead_ (rather than live) in the code region after skip. Thus, if x4 were dead and the existing sub instruction could not generate an exception (other than those from which the processor resumes the same process), we could move the sub instruction before the branch because the data flow could not be affected by this change.

> 假设我们知道在标记 Skip 的指令后，子指令(X4)的寄存器目的地未使用。(即将到来的指令将使用值是否将使用值的属性称为\_lives。)在跳过之后的代码区域中。因此，如果 X4 死了并且现有的子指令无法生成异常(除了处理器恢复相同过程的那些)，我们可以在分支之前移动子指令，因为数据流不受此更改的影响。

If the branch is taken, the sub instruction will execute and will be useless, but it will not affect the program results. This type of code scheduling is also a form of speculation*,* often called software speculation, because the compiler is betting on the branch outcome; in this case, the bet is that the branch is usually not taken. More ambitious compiler speculation mechanisms are discussed in Appendix H. Normally, it will be clear when we say speculation or speculative whether the mechanism is a hardware or software mechanism; when it is not clear, it is best to say  "hardware speculation"  or  "software speculation."

> 如果跳转，sub 指令会执行，没有用，但不影响程序运行结果。这种类型的代码调度也是一种推测*，*通常称为软件推测，因为编译器将赌注押在分支结果上；在这种情况下，赌注是分支通常不会被采用。更多雄心勃勃的编译器推测机制在附录 H 中讨论。不清楚的时候，最好说 "硬件推测" 或 "软件推测" 。

Control dependence is preserved by implementing control hazard detection that causes control stalls. Control stalls can be eliminated or reduced by a variety of hardware and software techniques, which we examine in [Section 3.3](#reducing-branch-costs-with-advanced-branch-prediction).

> 通过实施导致控制失速的控制危害检测来保留控制依赖性。可以通过多种硬件和软件技术来消除或减少控制摊位，我们在[3.3](＃Reeducing-Branch-Costs-with-Advanced-Branch 预测)中进行了检查。
