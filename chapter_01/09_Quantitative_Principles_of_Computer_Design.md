## Quantitative Principles of Computer Design

> ##计算机设计的定量原理

Now that we have seen how to define, measure, and summarize performance, cost, dependability, energy, and power, we can explore guidelines and principles that are useful in the design and analysis of computers. This section introduces important observations about design, as well as two equations to evaluate alternatives.

> 既然我们已经看到了如何定义，衡量和总结性能，成本，可靠性，能源和权力，我们可以探索在计算机设计和分析中有用的准则和原理。本节介绍了有关设计的重要观察以及两个方程式以评估替代方案。

### Take Advantage of Parallelism

> ###利用并行性

Using parallelism is one of the most important methods for improving perfor- mance. Every chapter in this book has an example of how performance is enhanced through the exploitation of parallelism. We give three brief examples here, which are expounded on in later chapters.

> 使用并行性是改善穿孔的最重要方法之一。本书中的每一章都有一个示例，说明如何通过对并行性的利用来增强性能。我们在此提供了三个简短的示例，这些示例在以后的章节中进行了阐述。

Our first example is the use of parallelism at the system level. To improve the throughput performance on a typical server benchmark, such as SPECSFS or TPC- C, multiple processors and multiple storage devices can be used. The workload of handling requests can then be spread among the processors and storage devices, resulting in improved throughput. Being able to expand memory and the number of processors and storage devices is called _scalability_, and it is a valuable asset for servers. Spreading of data across many storage devices for parallel reads and writes enables data-level parallelism. SPECSFS also relies on request-level parallelism to use many processors, whereas TPC-C uses thread-level parallelism for faster pro- cessing of database queries.

> 我们的第一个示例是在系统级别上使用并行性。为了改善典型服务器基准上的吞吐性能，例如 SPECSFS 或 TPC-C，可以使用多个处理器和多个存储设备。然后可以在处理器和存储设备之间扩展处理请求的工作量，从而改善吞吐量。能够扩展内存，并且处理器和存储设备的数量称为 *Scalibality*，它是服务器的宝贵资产。在许多存储设备上传播数据以进行并行读取和写入启用数据级并行性。SPECSF 还依靠请求级并行性来使用许多处理器，而 TPC-C 则使用线程级并行性来更快地进行数据库查询。

At the level of an individual processor, taking advantage of parallelism among instructions is critical to achieving high performance. One of the simplest ways to do this is through pipelining. (Pipelining is explained in more detail in [Appendix C](#_bookmark481) and is a major focus of [Chapter 3](#_bookmark93).) The basic idea behind pipelining is to overlap instruction execution to reduce the total time to complete an instruction sequence. A key insight into pipelining is that not every instruction depends on its immediate predecessor, so executing the instructions completely or partially in parallel may be possible. Pipelining is the best-known example of ILP.

> 在单个处理器的层面上，在指示中利用并行性对于实现高性能至关重要。做到这一点的最简单方法之一是通过管道进行。（在[附录 C]（#_ bookmark481）中更详细地解释管道，并且是[第 3 章]（#_ bookmark93）的主要重点。）管道上的基本思想是重叠指令执行，以减少总时间来完成一个完成的时间指令序列。对管道的关键洞察力是，并非每个指令都取决于其直接的前身，因此可以完全或部分地并行执行指令。管道是 ILP 的最著名示例。

Parallelism can also be exploited at the level of detailed digital design. For example, set-associative caches use multiple banks of memory that are typically searched in parallel to find a desired item. Arithmetic-logical units use carry- lookahead, which uses parallelism to speed the process of computing sums from linear to logarithmic in the number of bits per operand. These are more examples of _data-level parallelism_.

> 并行性也可以在详细数字设计层面上被利用。例如，设置缔合缓存使用多个内存库，这些内存通常并行搜索以找到所需的项目。算术逻辑单元使用 carry-lookahead，它使用并行性来加快计算总和从线性到对数的过程中的每个操作数量的数量。这些是 *data 级并联*的更多示例。

### Principle of Locality

> ###地区原则

Important fundamental observations have come from properties of programs. The most important program property that we regularly exploit is the _principle of local- ity_: programs tend to reuse data and instructions they have used recently. A widely held rule of thumb is that a program spends 90% of its execution time in only 10% of the code. An implication of locality is that we can predict with reasonable accuracy what instructions and data a program will use in the near future based on its accesses in the recent past. The principle of locality also applies to data accesses, though not as strongly as to code accesses.

> 重要的基本观察来自计划的属性。我们定期利用的最重要的程序属性是本地 - ity\_的原理：程序倾向于重复使用他们最近使用的数据和指令。一项广泛持有的经验法则是，一个程序仅在该代码的 10％上花费其执行时间的 90％。当地的一个暗示是，我们可以准确地预测程序在不久的将来根据最近的访问将使用哪些指令和数据。局部原理也适用于数据访问，尽管不像代码访问那样强。

Two different types of locality have been observed. _Temporal locality_ states that recently accessed items are likely to be accessed soon. _Spatial locality_ says that items whose addresses are near one another tend to be referenced close together in time. We will see these principles applied in [Chapter 2](#_bookmark46).

> 已经观察到两种不同类型的地方。*temporal locality* 状态*最近很快就会访问最近访问的项目。* -Spatial locality *说，地址彼此接近的项目往往会及时地关闭。我们将在[第 2 章]（#* bookmark46）中看到这些原理。

### Focus on the Common Case

> ###关注常见案例

Perhaps the most important and pervasive principle of computer design is to focus on the common case: in making a design trade-off, favor the frequent case over the infrequent case. This principle applies when determining how to spend resources, because the impact of the improvement is higher if the occurrence is commonplace. Focusing on the common case works for energy as well as for resource allo- cation and performance. The instruction fetch and decode unit of a processor may be used much more frequently than a multiplier, so optimize it first. It works on dependability as well. If a database server has 50 storage devices for every processor, storage dependability will dominate system dependability.

> 计算机设计的最重要和普遍的原则也许是关注常见案例：在设计权衡方面，倾向于频繁的情况，而不是不经常情况。该原则在确定如何花费资源时适用，因为如果发生很常见，改进的影响会更高。专注于共同的案例以及资源分配和性能。处理器的指令获取和解码单元可能比乘数更频繁地使用，因此先对其进行优化。它也可以可靠性。如果数据库服务器为每个处理器都有 50 个存储设备，则存储可靠性将主导系统可靠性。

In addition, the common case is often simpler and can be done faster than the infrequent case. For example, when adding two numbers in the processor, we can expect overflow to be a rare circumstance and can therefore improve performance by optimizing the more common case of no overflow. This emphasis may slow down the case when overflow occurs, but if that is rare, then overall performance will be improved by optimizing for the normal case.

> 此外，常见情况通常更简单，并且可以比不经常情况更快地完成。例如，当在处理器中添加两个数字时，我们可以期望溢出是一种罕见的情况，因此可以通过优化更常见的无溢出情况来提高性能。这种重点可能会在发生溢出时放慢速度，但是如果很少见，那么通过优化正常情况，将改善整体性能。

We will see many cases of this principle throughout this text. In applying this simple principle, we have to decide what the frequent case is and how much per- formance can be improved by making that case faster. A fundamental law, called _Amdahl_’_s Law_, can be used to quantify this principle.

> 在本文中，我们将看到许多这一原则的情况。在应用此简单原则时，我们必须确定什么是频繁的情况以及通过使该案例更快地改善多少情况。一个称为 *amdahl *’* s Law*的基本定律可用于量化这一原则。

### Amdahl’s Law

> ### Amdahl 的定律

The performance gain that can be obtained by improving some portion of a com- puter can be calculated using Amdahl’s Law. Amdahl’s Law states that the perfor- mance improvement to be gained from using some faster mode of execution is limited by the fraction of the time the faster mode can be used.

> 可以使用 Amdahl 的法律来计算可以通过改进计算机的一部分来获得的绩效增长。Amdahl 的定律指出，使用某种更快的执行模式将获得的绩效改进受到使用更快模式的时间的限制。

Amdahl’s Law defines the _speedup_ that can be gained by using a particular feature. What is speedup? Suppose that we can make an enhancement to a com- puter that will improve performance when it is used. Speedup is the ratio

> Amdahl 的定律定义了可以使用特定功能获得的 *speedup*。什么是加速？假设我们可以对一个使用者进行增强，从而在使用时会提高性能。加速是比率

Speedup Performance for entire task using the enhancement when possible Performance for entire task without using the enhancement

> 在可能的整个任务时，在不使用增强的情况下，使用增强性能加速整个任务的加速性能

Alternatively,

> 或者，

Speedup Execution time for entire task without using the enhancement Execution time for entire task using the enhancement when possible

> 加速执行时间为整个任务执行时间，而无需在可能的情况下使用增强任务的增强执行时间

Speedup tells us how much faster a task will run using the computer with the enhance- ment contrary to the original computer.

> Speedup 告诉我们，与原始计算机相反的增强功能，任务将运行的速度更快。

Amdahl’s Law gives us a quick way to find the speedup from some enhance- ment, which depends on two factors:

> Amdahl 的定律为我们提供了一种快速的方法来从某些增强中找到加速，这取决于两个因素：

1. _The fraction of the computation time in the original computer that can be con- verted to take advantage of the enhancement_—For example, if 40 seconds of the execution time of a program that takes 100 seconds in total can use an enhancement, the fraction is 40/100. This value, which we call Fraction<sub>enhanced</sub>, is always less than or equal to 1.

> 1. \_原始计算机中计算时间的分数可以使用以利用增强功能，例如，如果程序的执行时间的 40 秒（总计需要 100 秒）可以使用增强功能，则分数为 40/100。我们称之为分数<sub>增强</sub>的值始终小于或等于 1。

2. _The improvement gained by the enhanced execution mode, that is, how much faster the task would run if the enhanced mode were used for the entire pro- gram_—This value is the time of the original mode over the time of the enhanced mode. If the enhanced mode takes, say, 4 seconds for a portion of the program, while it is 40 seconds in the original mode, the improvement is 40/4 or 10. We call this value, which is always greater than 1, Speedup<sub>enhanced</sub>.

> 2. \_通过增强的执行模式获得的改进，即，如果用于整个程序的增强模式使用，则任务将运行的速度更快 - 该值是增强模式的原始模式的时间。如果增强模式为程序的一部分需要 4 秒，而在原始模式下为 40 秒，则改进为 40/4 或 10。我们称此值始终大于 1，Speedup <sub>增强</sub>。

The execution time using the original computer with the enhanced mode will be the time spent using the unenhanced portion of the computer plus the time spent using the enhancement:

> 使用原始计算机具有增强模式的执行时间将是使用计算机的未增强部分所花费的时间，以及使用增强功能所花费的时间：

> ===

Example Suppose that we want to enhance the processor used for web serving. The new processor is 10 times faster on computation in the web serving application than the old processor. Assuming that the original processor is busy with computation 40% of the time and is waiting for I/O 60% of the time, what is the overall speedup gained by incorporating the enhancement?

> 示例假设我们要增强用于 Web 服务的处理器。新处理器在 Web 服务应用程序中的计算速度比旧处理器快 10 倍。假设原始处理器有 40％的时间忙于计算，并且正在等待 I/O 60％的时间，那么通过合并增强功能获得了什么总体加速？

> ===

Amdahl’s Law expresses the law of diminishing returns: The incremental improve- ment in speedup gained by an improvement of just a portion of the computation diminishes as improvements are added. An important corollary of Amdahl’s Law is that if an enhancement is usable only for a fraction of a task, then we can’t speed up the task by more than the reciprocal of 1 minus that fraction.

> Amdahl 的定律表达了回报的定律：随着改进的添加，仅一部分计算的改进而获得的加速改善，随着改进的添加。Amdahl 定律的重要推论是，如果增强功能仅适用于任务的一小部分，那么我们将无法将任务加快比 1 减，而不是该分数的 1 减。

A common mistake in applying Amdahl’s Law is to confuse “fraction of time con- verted _to use an enhancement_” and “fraction of time _after enhancement is in use_.” If, instead of measuring the time that we _could use_ the enhancement in a compu- tation, we measure the time _after_ the enhancement is in use, the results will be incorrect!

> 应用 AMDAHL 定律的一个常见错误是混淆“时间的分数*使用增强*”和“时间的一部分*在使用*”。如果我们没有测量我们在构成中使用增强功能的时间，而是测量时间 *after* 的时间，则结果将是不正确的！

Amdahl’s Law can serve as a guide to how much an enhancement will improve performance and how to distribute resources to improve cost-performance. The goal, clearly, is to spend resources proportional to where time is spent. Amdahl’s Law is particularly useful for comparing the overall system performance of two alternatives, but it can also be applied to compare two processor design alterna- tives, as the following example shows.

> Amdahl 的定律可以为提高绩效以及如何分配资源以提高成本绩效的方式提供指导。显然，目标是将资源与时间花费的时间成正比。Amdahl 的定律对于比较两种替代方案的整体系统性能特别有用，但也可以应用于比较两种处理器设计替代方案，如以下示例所示。

Example A common transformation required in graphics processors is square root. Imple- mentations of floating-point (FP) square root vary significantly in performance, especially among processors designed for graphics. Suppose FP square root (FSQRT) is responsible for 20% of the execution time of a critical graphics bench- mark. One proposal is to enhance the FSQRT hardware and speed up this operation by a factor of 10. The other alternative is just to try to make all FP instructions in the graphics processor run faster by a factor of 1.6; FP instructions are responsible for half of the execution time for the application. The design team believes that they can make all FP instructions run 1.6 times faster with the same effort as required for the fast square root. Compare these two design alternatives.

> 示例图形处理器中需要的常见转换是平方根。浮点（FP）平方根的凸出的性能差异很大，尤其是在为图形设计的处理器中。假设 FP 平方根（FSQRT）负责关键图形基准的 20％的执行时间。一个建议是增强 FSQRT 硬件并加快此操作的速度 10 倍。另一种选择只是试图使图形处理器中的所有 FP 指令更快地运行 1.6 倍；FP 指令负责该应用程序的一半执行时间。设计团队认为，他们可以以与快速平方根所需的相同努力来使所有 FP 指令运行的速度更快 1.6 倍。比较这两个设计替代方案。

> ===

Improving the performance of the FP operations overall is slightly better because of the higher frequency.

> 由于频率较高，整体上提高 FP 操作的性能要好得多。

Amdahl’s Law is applicable beyond performance. Let’s redo the reliability example from page 39 after improving the reliability of the power supply via redundancy from 200,000-hour to 830,000,000-hour MTTF, or 4150 × better.

> Amdahl 的定律适用于绩效。让我们重做第 39 页的可靠性示例，因为通过冗余从 20 万小时提高了电源的可靠性，将电源的可靠性从 20 万小时到 830,000,000 小时，或更好的 4150×。

Example The calculation of the failure rates of the disk subsystem was

> 示例磁盘子系统的故障率的计算是

> ===

Therefore the fraction of the failure rate that could be improved is 5 per million hours out of 23 for the whole system, or 0.22.

> 因此，可以提高的故障率的比例为整个系统中 23 个小时或 0.22。

> ===

Despite an impressive 4150 improvement in reliability of one module, from the system’s perspective, the change has a measurable but small benefit.

> 尽管从系统的角度来看，尽管一个模块的可靠性有 4150 个可靠性，但该变化具有可衡量但很小的好处。

In the preceding examples, we needed the fraction consumed by the new and improved version; often it is difficult to measure these times directly. In the next section, we will see another way of doing such comparisons based on the use of an equation that decomposes the CPU execution time into three separate components. If we know how an alternative affects these three components, we can determine its overall performance. Furthermore, it is often possible to build simulators that measure these components before the hardware is actually designed.

> 在前面的示例中，我们需要新版本和改进版本所消耗的分数；通常很难直接测量这些时间。在下一部分中，我们将根据将 CPU 执行时间分解为三个单独的组件的方程式的使用方程式进行另一种进行比较的方法。如果我们知道替代方案如何影响这三个组件，我们可以确定其整体性能。此外，通常可以构建在硬件实际设计之前测量这些组件的模拟器。

### The Processor Performance Equation

> ###处理器性能方程式

Essentially all computers are constructed using a clock running at a constant rate. These discrete time events are called _clock periods_, _clocks_, _cycles_, or _clock cycles_. Computer designers refer to the time of a clock period by its duration (e.g., 1 ns) or by its rate (e.g., 1 GHz). CPU time for a program can then be expressed two ways:

> 本质上，所有计算机都是使用以恒定速率运行的时钟构造的。这些离散的时间事件称为 *clock ofers*，_clocks_，*cycles* 或 *Clock Cycles*。计算机设计人员是指时钟时间的时间（例如 1 ns）或速率（例如 1 GHz）。然后可以用两种方式表达 CPU 的时间：

CPU time = CPU clock cycles for a program ×Clock cycle time

> CPU 时间=程序 × 时钟周期时间的 CPU 时钟周期

or

> 或者

CPU time CPU clock cycles for a program

> CPU 时间 CPU 时钟周期

Clock rate

> 时钟速率

In addition to the number of clock cycles needed to execute a program, we can also count the number of instructions executed—the _instruction path length_ or _instruction count_ (IC). If we know the number of clock cycles and the instruction count, we can calculate the average number of _clock cycles per instruction_ (CPI). Because it is easier to work with, and because we will deal with simple processors in this chapter, we use CPI. Designers sometimes also use _instructions per clock_ (IPC), which is the inverse of CPI.

> 除了执行程序所需的时钟周期数外，我们还可以计算执行的指令数 - *instruction 路径长度*或 *instruction count*（ic）。如果我们知道时钟周期的数量和指令数量，则可以计算每个指令*（CPI）的平均\_Clock 循环数。因为它更容易使用，并且由于我们将在本章中处理简单的处理器，因此我们使用 CPI。设计人员有时还会使用*每个时钟\_（IPC），这是 CPI 的倒数。

> ===

This processor figure of merit provides insight into different styles of instruction sets and implementations, and we will use it extensively in the next four chapters.

> 这个功绩的处理器形象提供了对不同风格的指令集和实现方式的见解，我们将在接下来的四章中广泛使用它。

By transposing the instruction count in the preceding formula, clock cycles can be defined as IC CPI. This allows us to use CPI in the execution time formula:

> 通过在上述公式中转换指令数，可以将时钟周期定义为 IC CPI。这使我们可以在执行时间公式中使用 CPI：

CPU time = Instruction count ×Cycles per instruction ×Clock cycle time Expanding the first formula into the units of measurement shows how the pieces fit together:

> CPU 时间=指令计数 × 每指令 × 时钟周期时间，将第一个公式扩展到测量单元中显示了零件如何结合在一起：

Instructions Clock cycles Seconds Seconds

> 说明时钟周期秒秒

Program × Instruction ×Clock cycle = Program = CPU time

> 程序 × 指令 × 时钟周期=程序= CPU 时间

As this formula demonstrates, processor performance is dependent upon three characteristics: clock cycle (or rate), clock cycles per instruction, and instruction count. Furthermore, CPU time is _equally_ dependent on these three characteristics; for example, a 10% improvement in any one of them leads to a 10% improvement in CPU time.

> 正如该公式所证明的那样，处理器性能取决于三个特征：时钟周期（或速率），每指令时钟周期和指令计数。此外，CPU 时间是\_等于这三个特征。例如，其中任何一个的提高 10％会导致 CPU 时间提高 10％。

Unfortunately, it is difficult to change one parameter in complete isolation from others because the basic technologies involved in changing each characteristic are interdependent:

> 不幸的是，很难在与其他参数完全隔离的情况下更改一个参数，因为改变每个特征的基本技术是相互依存的：

- _Clock cycle time_—Hardware technology and organization

> - _clock 循环时间_-硬件技术和组织

- _CPI_—Organization and instruction set architecture

> - _cpi_ - 组织和指令集体系结构

- _Instruction count_—Instruction set architecture and compiler technology

> - _ Instruction count_ - Instructuction Set 架构和编译器技术

Luckily, many potential performance improvement techniques primarily enhance one component of processor performance with small or predictable impacts on the other two.

> 幸运的是，许多潜在的性能改进技术主要增强了处理器性能的一个组成部分，对其他两种的影响很小或可预测的影响。

In designing the processor, sometimes it is useful to calculate the number of total processor clock cycles as

> 在设计处理器时，有时将总处理器时钟周期的数量计算为

> ===

where IC*<sub>i</sub>* represents the number of times instruction _i_ is executed in a program and CPI*<sub>i</sub>* represents the average number of clocks per instruction for instruction _i_. This form can be used to express CPU time as

> 其中 ic* <ub> i </sub>*表示在程序中执行的次数* i *的次数，而 cpi* <sub> i </sub>*表示指令* i*的每个指令的平均时钟数。该表格可用于表达 CPU 时间为

> ===

The latter form of the CPI calculation uses each individual CPI*<sub>i</sub>* and the fraction of occurrences of that instruction in a program (i.e., IC*<sub>i</sub>* Instruction count). Because it must include pipeline effects, cache misses, and any other memory system inefficiencies, CPI*<sub>i</sub>* should be measured and not just calculated from a table in the back of a reference manual.

> CPI 计算的后一种形式使用每个单独的 CPI* <sub> i </sub>*，以及该指令在程序中的出现分数（即，IC* <sub> i </sub>*指令计数）。因为它必须包括管道效应，缓存误差和任何其他内存系统效率低下，因此应测量 CPI* <sub> i </sub>*，而不仅仅是从参考手册背面的表中计算出来。

Consider our performance example on page 52, here modified to use measure- ments of the frequency of the instructions and of the instruction CPI values, which, in practice, are obtained by simulation or by hardware instrumentation.

> 考虑我们在第 52 页上的性能示例，此处进行了修改，以使用指令频率和指令 CPI 值的测量值，实际上，该值是通过仿真或硬件仪器获得的。

Example Suppose we made the following measurements: Frequency of FP operations = 25%

> 示例假设我们进行了以下测量：FP 操作的频率= 25％

Average CPI of FP operations = 4.0 Average CPI of other instructions = 1.33 Frequency of FSQRT= 2%

> FP 操作的平均 CPI = 4.0 其他指令的平均 CPI = 1.33 FSQRT 频率= 2％

CPI of FSQRT = 20

> FSQRT 的 CPI = 20

Assume that the two design alternatives are to decrease the CPI of FSQRT to 2 or to decrease the average CPI of all FP operations to 2.5. Compare these two design alternatives using the processor performance equation.

> 假设两个设计替代方法是将 FSQRT 的 CPI 降低到 2，或将所有 FP 操作的平均 CPI 降低到 2.5。使用处理器性能方程比较这两个设计替代方案。

_Answer_ First, observe that only the CPI changes; the clock rate and instruction count remain identical. We start by finding the original CPI with neither enhancement:

> *answer* 首先，观察只有 CPI 改变；时钟率和指令数量保持相同。我们首先找到最初的 CPI，都没有增强：

> ===

We can compute the CPI for the enhanced FSQRT by subtracting the cycles saved from the original CPI:

> 我们可以通过减去从原始 CPI 节省的周期来计算增强的 FSQRT 的 CPI：

We can compute the CPI for the enhancement of all FP instructions the same way or by summing the FP and non-FP CPIs. Using the latter gives us

> 我们可以以相同的方式计算 CPI 以增强所有 FP 指令，也可以通过求和 FP 和非 FP CPI 来计算 CPI。使用后者给了我们

Since the CPI of the overall FP enhancement is slightly lower, its performance will be marginally better. Specifically, the speedup for the overall FP enhancement is

> 由于整体 FP 增强的 CPI 略低，因此其性能将略有差。具体而言，总体 FP 增强的加速是

> ===

Happily, we obtained this same speedup using Amdahl’s Law on page 51.

> 令人高兴的是，我们在第 51 页上使用 Amdahl 定律获得了同样的加速。

It is often possible to measure the constituent parts of the processor performance equation. Such isolated measurements are a key advantage of using the processor performance equation versus Amdahl’s Law in the previous example. In particular, it may be difficult to measure things such as the fraction of execution time for which a set of instructions is responsible. In practice, this would probably be computed by summing the product of the instruction count and the CPI for each of the instruc- tions in the set. Since the starting point is often individual instruction count and CPI measurements, the processor performance equation is incredibly useful.

> 通常可以测量处理器性能方程的组成部分。这种孤立的测量是在上一个示例中使用处理器性能方程与 Amdahl 定律的关键优势。特别是，可能很难衡量诸如一组指令负责的执行时间的比例。实际上，这可能是通过将指令计数的乘积和集合中每种指令的 CPI 求和来计算的。由于起点通常是单个指令计数和 CPI 测量值，因此处理器性能方程非常有用。

To use the processor performance equation as a design tool, we need to be able to measure the various factors. For an existing processor, it is easy to obtain the exe- cution time by measurement, and we know the default clock speed. The challenge lies in discovering the instruction count or the CPI. Most processors include counters for both instructions executed and clock cycles. By periodically monitoring these counters, it is also possible to attach execution time and instruction count to seg- ments of the code, which can be helpful to programmers trying to understand and tune the performance of an application. Often designers or programmers will want to understand performance at a more fine-grained level than what is available from the hardware counters. For example, they may want to know why the CPI is what it is. In such cases, the simulation techniques used are like those for processors that are being designed.

> 要将处理器性能方程式用作设计工具，我们需要能够衡量各种因素。对于现有的处理器，很容易通过测量获得摄取时间，我们知道默认时钟速度。挑战在于发现指令数量或 CPI。大多数处理器都包括执行指令和时钟周期的计数器。通过定期监视这些计数器，还可以将执行时间和指令计数附加到代码的范围内，这可能对试图理解和调整应用程序性能的程序员有帮助。通常，设计师或程序员会希望比硬件计数器可用的水平更细粒度地了解性能。例如，他们可能想知道为什么 CPI 是什么。在这种情况下，所使用的仿真技术就像是为正在设计的处理器的技术。

Techniques that help with energy efficiency, such as dynamic voltage fre- quency scaling and overclocking (see [Section 1.5](#trends-in-power-and-energy-in-integrated-circuits)), make this equation harder to use, because the clock speed may vary while we measure the program. A simple approach is to turn off those features to make the results reproducible. Fortunately, as performance and energy efficiency are often highly correlated—taking less time to run a program generally saves energy—it’s probably safe to consider perfor- mance without worrying about the impact of DVFS or overclocking on the results.

> 有助于能源效率的技术，例如动态电压频率缩放和超频（请参阅[1.5]（第 1.5 节）（＃趋势中的趋势和能量融合电路）），使此方程式更难使用，更难使用，因为在我们测量程序时，时钟速度可能会有所不同。一种简单的方法是关闭这些功能以使结果可重现。幸运的是，由于性能和能源效率通常高度相关 - 花更少的时间运行程序通常可以节省能源 - 因此可以安全地考虑穿孔，而不必担心 DVFS 的影响或超频对结果的影响。
