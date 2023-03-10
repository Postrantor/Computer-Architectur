## Basic Compiler Techniques for Exposing ILP(用于暴露 ILP 的基本编译器技术)

This section examines the use of simple compiler technology to enhance a proces- sor’s ability to exploit ILP. These techniques are crucial for processors that use static issue or static scheduling. Armed with this compiler technology, we will shortly examine the design and performance of processors using static issuing. Appendix H will investigate more sophisticated compiler and associated hardware schemes designed to enable a processor to exploit more instruction-level parallelism.

> 本节介绍了使用简单编译器技术来增强 Process-SOR 利用 ILP 的能力。这些技术对于使用静态问题或静态调度的处理器至关重要。在使用此编译器技术的情况下，我们将尽快使用静态发行研究处理器的设计和性能。附录 H 将研究更复杂的编译器和相关的硬件方案，旨在使处理器利用更多的指导级并行性。

### Basic Pipeline Scheduling and Loop Unrolling(基本管道调度和循环展开)

To keep a pipeline full, parallelism among instructions must be exploited by find- ing sequences of unrelated instructions that can be overlapped in the pipeline. To avoid a pipeline stall, the execution of a dependent instruction must be separated from the source instruction by a distance in clock cycles equal to the pipeline latency of that source instruction. A compiler’s ability to perform this scheduling depends both on the amount of ILP available in the program and on the latencies of the functional units in the pipeline. [Figure 3.2](#_bookmark98) shows the FP unit latencies we assume in this chapter, unless different latencies are explicitly stated. We assume the standard five-stage integer pipeline so that branches have a delay of one clock cycle. We assume that the functional units are fully pipelined or replicated (as many times as the pipeline depth) so that an operation of any type can be issued on every clock cycle and there are no structural hazards.

> 为了保持管道充分，必须通过发现可以在管道中重叠的无关指令的序列来利用指令之间的并行性。为了避免管道摊位，必须将依赖指令的执行与源指令分开，而时钟周期的距离等于该源指令的管道延迟。编译器执行此计划的能力既取决于程序中可用的 ILP 数量，也取决于管道中功能单元的延迟。[图 3.2](#_bookmark98) 显示了我们在本章中假设的 FP 单元延迟，除非明确说明不同的延迟。我们假设标准的五阶段整数管道，使分支的延迟为一个时钟周期。我们假设功能单元是完全管道的或复制的(与管道深度的多次)，因此可以在每个时钟周期发出任何类型的操作，并且没有结构性危害。

In this section, we look at how the compiler can increase the amount of avail- able ILP by transforming loops. This example serves both to illustrate an important technique as well as to motivate the more powerful program transformations described in Appendix H. We will rely on the following code segment, which adds a scalar to a vector:

> 在本节中，我们研究编译器如何通过转换循环来增加可用性 ILP 的量。该示例既可以说明一项重要的技术，又可以激发附录 H 中所述的更强大的程序转换。我们将依靠以下代码段，该代码段为向量增加了标量：

> ===

We can see that this loop is parallel by noticing that the body of each iteration is independent. We formalize this notion in Appendix H and describe how we can test whether loop iterations are independent at compile time. First, let’s look at the per- formance of this loop, which shows how we can use the parallelism to improve its performance for a RISC-V pipeline with the preceding latencies.

> 我们可以看到，通过注意到每次迭代的主体都是独立的，该循环是平行的。我们在附录 H 中正式化了此概念，并描述了如何在编译时测试循环迭代是否独立。首先，让我们看一下这个循环的表现，该循环的表现显示了我们如何使用并行性来改善其与前面潜伏期的 RISC-V 管道的性能。

The first step is to translate the preceding segment to RISC-V assembly language. In the following code segment, x1 is initially the address of the element in the array with the highest address, and f2 contains the scalar value _s_. Register x2 is precom- puted so that Regs [x2]+8 is the address of the last element to operate on.

> 第一步是将前面的段转换为 RISC-V 汇编语言。在以下代码段中，X1 最初是数组中具有最高地址的元素的地址，而 F2 包含标量值 _s_。寄存器 x2 是预先提出的，因此 regs [x2]+8 是最后要操作的元素的地址。

Figure 3.2 Latencies of FP operations used in this chapter. The last column is the number of intervening clock cycles needed to avoid a stall. These numbers are similar to the average latencies we would see on an FP unit. The latency of a floating-point load to a store is 0 because the result of the load can be bypassed without stalling the store. We will continue to assume an integer load latency of 1 and an integer ALU operation latency of 0 (which includes ALU operation to branch).

> 图 3.2 本章中使用的 FP 操作的潜伏期。最后一列是避免摊位所需的中间时钟周期的数量。这些数字类似于我们在 FP 单元上看到的平均潜伏期。浮点负载到存储的延迟为 0，因为可以绕过负载的结果而不会停止存储。我们将继续假设 1 个整数载荷延迟为 1，而整数 ALU 操作延迟为 0(包括 Alu 操作到分支)。

The straightforward RISC-V code, not scheduled for the pipeline, looks like this:

> 直接的 RISC-V 代码未安排用于管道，看起来像这样：

> ===

Let’s start by seeing how well this loop will run when it is scheduled on a sim- ple pipeline for RISC-V with the latencies in [Figure 3.2](#_bookmark98).

> 首先，让我们看到该循环在[图 3.2]中的 LITCISE 的 Sim-ple Pipeline 上安排在[图 3.2](#_bookmark98) 中的 sim ple Pipeline 时的运行良好。

Example Show how the loop would look on RISC-V, both scheduled and unscheduled, including any stalls or idle clock cycles. Schedule for delays from floating-point operations.

> 示例说明循环在 RISC-V 上的外观，包括计划和计划外，包括任何摊位或闲置时钟周期。浮点操作延迟的时间表。

> ===

The stalls after fadd.d are for use by the fsd, and repositioning the addi pre- vents the stall after the fld.

> FADD.D 之后的摊位供 FSD 使用，并在 FLD 之后重新定位 Addi 预定摊位。

In the previous example, we complete one loop iteration and store back one array element every seven clock cycles, but the actual work of operating on the array element takes just three (the load, add, and store) of those seven clock cycles.

> 在上一个示例中，我们每七个时钟循环完成一个循环迭代，并将一个数组元素存储回一个阵列元素，但是在数组元素上操作的实际工作仅需三个时钟周期的三个(负载，添加和存储)。

The remaining four clock cycles consist of loop overhead—the addi and bne— and two stalls. To eliminate these four clock cycles, we need to get more operations relative to the number of overhead instructions.

> 其余的四个时钟周期包括循环顶部(addi 和 bne)以及两个摊位。为了消除这四个时钟周期，我们需要相对于间接费用的数量获得更多的操作。

A simple scheme for increasing the number of instructions relative to the branch and overhead instructions is _loop unrolling_. Unrolling simply replicates the loop body multiple times, adjusting the loop termination code.

> 一个简单的方案，用于增加相对于分支机构和开销指令的指令数量是 _loop unroling_。展开简单地简单地复制循环车身多次，调整循环终止代码。

Loop unrolling can also be used to improve scheduling. Because it eliminates the branch, it allows instructions from different iterations to be scheduled together. In this case, we can eliminate the data use stalls by creating additional independent instructions within the loop body. If we simply replicated the instructions when we unrolled the loop, the resulting use of the same registers could prevent us from effectively scheduling the loop. Thus we will want to use different registers for each iteration, increasing the required number of registers.

> 循环展开也可以用于改进计划。因为它消除了分支，所以它允许将不同迭代的指令一起安排。在这种情况下，我们可以通过在循环主体内创建其他独立指令来消除数据使用摊位。如果我们简单地复制了循环时的指令，则由此产生的同一寄存器的使用可以阻止我们有效地安排循环。因此，我们将需要在每次迭代中使用不同的寄存器，从而增加所需的寄存器数量。

Example Show our loop unrolled so that there are four copies of the loop body, assuming x1 x2 (that is, the size of the array) is initially a multiple of 32, which means that the number of loop iterations is a multiple of 4. Eliminate any obviously redundant computations and do not reuse any of the registers.

> 示例显示了我们的环状曲线，因此循环主体有四个副本，假设 x1 x2(即阵列的大小)最初是 32 的倍数，这意味着循环迭代的数量为 4 的倍数。消除任何明显的冗余计算，不会重复使用任何寄存器。

_Answer_ Here is the result after merging the addi instructions and dropping the unnec- essary bne operations that are duplicated during unrolling. Note that x2 must now be set so that Regs\[x2]+32 is the starting address of the last four elements.

> _answer_ 这是合并 ADDI 指令并删除在展开期间重复的不确定的 BNE 操作后的结果。请注意，现在必须设置 X2，以便 regs  [x2]+32 是最后四个元素的起始地址。

> ===

We have eliminated three branches and three decrements of x1. The addresses on the loads and stores have been compensated to allow the addi instructions on x1 to be merged. This optimization may seem trivial, but it is not; it requires symbolic substitution and simplification. Symbolic substitution and simplification will rear- range expressions so as to allow constants to be collapsed, allowing an expression such as ((_i_ + 1) + 1) to be rewritten as (_i_ +(1 + 1)) and then simplified to (_i_ + 2).

> 我们消除了三个分支和 x1 的三个减少。负载和存储上的地址已得到补偿，以允许合并 X1 上的 ADDI 说明。这种优化似乎微不足道，但事实并非如此。它需要符号替代和简化。符号替代和简化将返回范围表达式以允许常数折叠，从而允许(((_i_i_ + 1) + 1)等表达式为(_i_ +(1 + 1))，然后简化为(_i_ + 2)。

We will see more general forms of these optimizations that eliminate dependent computations in Appendix H.

Without scheduling, every FP load or operation in the unrolled loop is followed by a dependent operation and thus will cause a stall. This unrolled loop will run in 26 clock cycles—each fld has 1 stall, each fadd.d has 2, plus 14 instruction issue cycles—or 6.5 clock cycles for each of the four elements, but it can be sched- uled to improve performance significantly. Loop unrolling is normally done early in the compilation process so that redundant computations can be exposed and eliminated by the optimizer.

> 在不调度的情况下，展开的环路中的每个 FP 负载或操作都会在依赖操作之后进行，因此会导致失速。这个展开的循环将在 26 个时钟周期中运行 - 每个 fld 有 1 个摊位，每个 fadd.d 有 2 个，再加上 14 个指令发行循环 - 或 6.5 个时钟周期，对于这四个元素中的每个元素中的每个元素，都可以安排提高性能以提高性能显著地。循环展开通常是在编译过程的早期进行的，因此优化器可以暴露和消除冗余计算。

In real programs, we do not usually know the upper bound on the loop. Sup- pose it is _n,_ and we want to unroll the loop to make _k_ copies of the body. Instead of a single unrolled loop, we generate a pair of consecutive loops. The first executes (_n_ mod _k_) times and has a body that is the original loop. The second is the unrolled body surrounded by an outer loop that iterates (_n_/_k_) times. (As we will see in [Chapter 4](#_bookmark165), this technique is similar to a technique called _strip mining_, used in com- pilers for vector processors.) For large values of _n_, most of the execution time will be spent in the unrolled loop body.

> 在实际程序中，我们通常不知道循环上的上限。支持它是 _n，_，我们想展开循环以制作 _k_ 副本的副本。我们生成一对连续的循环，而不是单个展开的循环。第一个执行(_n_ mod _k_)次，并具有原始循环的主体。第二个是被迭代(_n _/_ k_)时间的外循环包围的展开的身体。(正如我们将在[第 4 章](#* bookmark165)中看到的那样，此技术类似于一种称为\_Strip Mining*的技术，用于矢量处理器中的 compiners。在展开的环形主体中。

In the previous example, unrolling improves the performance of this loop by eliminating overhead instructions, although it increases code size substantially. How will the unrolled loop perform when it is scheduled for the pipeline described earlier?

> 在上一个示例中，展开可以通过消除间接费用指令来改善此循环的性能，尽管它大大增加了代码大小。安排在前面描述的管道安排时，展开的循环将如何执行？

Example Show the unrolled loop in the previous example after it has been scheduled for the pipeline with the latencies in [Figure 3.2](#_bookmark98).

> 示例在上一个示例中显示了在[图 3.2](#_bookmark98) 中使用潜伏期中的管道之后，在上一个示例中显示了展开的循环。

> ===

The execution time of the unrolled loop has dropped to a total of 14 clock cycles, or 3.5 clock cycles per element, compared with 8 cycles per element before any unrolling or scheduling and 6.5 cycles when unrolled but not scheduled.

> 展开环的执行时间已降至总计 14 个时钟周期，或每个元素的 3.5 时钟周期，而每个元素进行 8 个周期，在任何展开或调度之前，并且在展开但未安排时进行 6.5 个周期。

The gain from scheduling on the unrolled loop is even larger than on the original loop. This increase arises because unrolling the loop exposes more computation that can be scheduled to minimize the stalls; the preceding code has no stalls. Scheduling the loop in this fashion necessitates realizing that the loads and stores are independent and can be interchanged.

> 在展开循环上的安排比原始循环中的增益还要大。出现这一增加是因为展开循环会公开更多的计算，该计算可以安排以最大程度地减少摊位。前面的代码没有失速。以这种方式安排循环必须意识到负载和存储是独立的，并且可以互换。

### Summary of the Loop Unrolling and Scheduling(循环展开和调度的摘要)

Throughout this chapter and Appendix H, we will look at a variety of hardware and software techniques that allow us to take advantage of instruction-level parallelism to fully utilize the potential of the functional units in a processor. The key to most of these techniques is to know when and how the ordering among instructions may be changed. In our example, we made many such changes, which to us, as human beings, were obviously allowable. In practice, this process must be performed in a methodical fashion either by a compiler or by hardware. To obtain the final unrolled code, we had to make the following decisions and transformations:

> 在本章和附录 H 中，我们将研究各种硬件和软件技术，使我们能够利用指令级并行性，以充分利用处理器中功能单元的潜力。大多数这些技术的关键是要知道指令之间的订购何时以及如何更改。在我们的示例中，我们做出了许多这样的变化，对我们来说，像人类一样，显然是允许的。实际上，必须由编译器或硬件以有条不紊的方式执行此过程。为了获得最终的展开代码，我们必须做出以下决定和转换：

- Determine that unrolling the loop would be useful by finding that the loop iter- ations were independent, except for the loop maintenance code.
- Use different registers to avoid unnecessary constraints that would be forced by using the same registers for different computations (e.g., name dependences).
- Eliminate the extra test and branch instructions and adjust the loop termination and iteration code.
- Determine that the loads and stores in the unrolled loop can be interchanged by observing that the loads and stores from different iterations are independent. This transformation requires analyzing the memory addresses and finding that they do not refer to the same address.
- Schedule the code, preserving any dependences needed to yield the same result as the original code.

> - 通过发现循环迭代是独立的，除了循环维护代码外，确定循环展开将很有用。
> - 使用不同的寄存器来避免通过使用相同的寄存器进行不同计算(例如名称依赖)来强制强制的不必要约束。
> - 消除额外的测试和分支说明，并调整循环终止和迭代代码。
> - 确定可以通过观察到来自不同迭代的负载和存储是独立的，可以互换展开的环路中的载荷和存储。这种转换需要分析内存地址并发现它们没有指相同地址。
> - 安排代码，保留与原始代码相同的结果所需的任何依赖。

The key requirement underlying all of these transformations is an understanding of how one instruction depends on another and how the instructions can be changed or reordered given the dependences.

> 所有这些转换基础的关键要求是了解一个指令如何取决于另一个指令以及如何更改或重新排序指令给定依赖性。

Three different effects limit the gains from loop unrolling: (1) a decrease in the amount of overhead amortized with each unroll, (2) code size limitations, and \(3\) compiler limitations. Let’s consider the question of loop overhead first. When we unrolled the loop four times, it generated sufficient parallelism among the instructions that the loop could be scheduled with no stall cycles. In fact, in 14 clock cycles, only 2 cycles were loop overhead: the addi, which maintains the index value, and the bne, which terminates the loop. If the loop is unrolled eight times, the overhead is reduced from 1/2 cycle per element to 1/4.

> 三种不同的效果限制了循环展开的增长：(1)每次展开，(2)代码尺寸限制和\(3 \)编译器限制的开销量的量减少。让我们先考虑一下循环高架的问题。当我们四次展开循环时，它在指令中产生了足够的并行性，即可以在没有失速周期的情况下安排循环。实际上，在 14 个时钟周期中，只有 2 个循环是循环的头顶：addi，维持索引值和 bne，bne 终止了循环。如果循环是八次展开的，则将开销从每个元素的 1/2 循环减少到 1/4。

A second limit to unrolling is the resulting growth in code size. For larger loops, the code size growth may be a concern, particularly if it causes an increase in the instruction cache miss rate.

> 展开的第二个限制是代码大小的结果增长。对于较大的循环，代码尺寸的增长可能是一个问题，尤其是在导致指令缓存率增加的情况下。

Another factor often more important than code size is the potential shortfall in registers that is created by aggressive unrolling and scheduling. This secondary effect that results from instruction scheduling in large code segments is called _reg- ister pressure._ It arises because scheduling code to increase ILP causes the number of live values to increase. After aggressive instruction scheduling, it may not be possible to allocate all the live values to registers. The transformed code, while the- oretically faster, may lose some or all of its advantage because it leads to a shortage of registers. Without unrolling, aggressive scheduling is sufficiently limited by branches so that register pressure is rarely a problem. The combination of unrolling and aggressive scheduling can, however, cause this problem. The problem becomes especially challenging in multiple-issue processors that require the exposure of more independent instruction sequences whose execution can be overlapped. In general, the use of sophisticated high-level transformations, whose potential improvements are difficult to measure before detailed code generation, has led to significant increases in the complexity of modern compilers.

> 通常比代码大小更重要的另一个因素是，通过激进的展开和调度造成的寄存器的潜在不足。由大型代码段中的指令计划产生的次要效应称为\_reg- ister 压力。经过积极的指示计划后，可能无法将所有实时值分配给寄存器。转换的代码虽然更快，但可能会失去其一些或全部优势，因为它导致寄存器短缺。如果没有展开，积极的安排就足够受分支机构的限制，因此寄存器压力很少是一个问题。但是，展开和激进的调度的结合可能会导致这个问题。在需要暴露更独立的指令序列的多发处理器中，该问题变得尤其具有挑战性，这些序列的执行序列可以重叠。通常，在详细的代码生成之前，很难衡量复杂的高级转换的使用，导致现代编译器的复杂性显着提高。

Loop unrolling is a simple but useful method for increasing the size of straight- line code fragments that can be scheduled effectively. This transformation is useful in a variety of processors, from simple pipelines like those we have examined so far to the multiple-issue superscalars and VLIWs explored later in this chapter.

> 循环展开是一种简单但有用的方法，用于增加可以有效安排的直线代码片段的大小。这种转换在各种处理器中很有用，从我们迄今已研究的简单管道到本章稍后探索的多发超标和 VLIWS。
