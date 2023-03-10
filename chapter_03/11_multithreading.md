## Multithreading: Exploiting Thread-Level Parallelism to Improve Uniprocessor Throughput

The topic we cover in this section, multithreading, is truly a cross-cutting topic, because it has relevance to pipelining and superscalars, to graphics processing units ([Chapter 4](#_bookmark165)), and to multiprocessors ([Chapter 5](#_bookmark213)). A _thread_ is like a process in that it has state and a current program counter, but threads typically share the address space of a single process, allowing a thread to easily access data of other threads within the same process. Multithreading is a technique whereby multiple threads share a processor without requiring an intervening process switch. The ability to switch between threads rapidly is what enables multithreading to be used to hide pipeline and memory latencies.

> 我们在本节中介绍的主题是多线程，这确实是一个跨切割主题，因为它与管道和超级标准，图形处理单元([[4 章](#_bookmark165))和多处理器([[第 5 章]]具有相关性([第 4 章 Bookmark165))(＃*Bookmark213))。thread 就像一个过程，因为它具有状态和当前程序计数器，但是线程通常共享单个进程的地址空间，从而使线程可以轻松访问同一进程中其他线程的数据。多线程是一种技术，多个线程共享处理器而无需中间过程开关。迅速在线程之间切换的能力使多线程可以用来隐藏管道和内存延迟。

In the next chapter, we will see how multithreading provides the same advantages in GPUs. Finally, [Chapter 5](#_bookmark213) will explore the combination of multithreading and multiprocessing. These topics are closely interwoven because multithreading is a primary technique for exposing more parallelism to the hardware. In a strict sense, multithreading uses thread-level parallelism, and thus is properly the subject of [Chapter 5](#_bookmark213), but its role in both improving pipeline utilization and in GPUs motivates us to introduce the concept here.

> 在下一章中，我们将看到多线程如何在 GPU 中提供相同的优势。最后，[第 5 章](#_bookmark213)将探索多线程和多处理的组合。这些主题是紧密相互交织的，因为多线程是公开更多并行性与硬件的主要技术。从严格的意义上讲，**多线程使用线程级并行性**，因此是[第 5 章](#_bookmark213)的适当主题，但是它在改善管道利用率和 GPU 中的作用都可以激励我们在此处介绍该概念。

Although increasing performance by using ILP has the great advantage that it is reasonably transparent to the programmer, as we have seen, ILP can be quite limited or difficult to exploit in some applications. In particular, with reasonable instruction issue rates, cache misses that go to memory or off-chip caches are unlikely to be hidden by available ILP. Of course, when the processor is stalled waiting on a cache miss, the utilization of the functional units drops dramatically.

> 尽管通过使用 ILP 提高性能具有很大的优势，即对程序员合理地透明，但正如我们已经看到的，ILP 在某些应用中可能非常有限或难以利用。特别是，对于合理的指令发行率，可用的 ILP 不太可能隐藏进入内存或芯片之外缓存的缓存。当然，当处理器停滞不前等待缓存失误时，功能单元的利用率会大大降低。

Because attempts to cover long memory stalls with more ILP have limited effectiveness, it is natural to ask whether other forms of parallelism in an application could be used to hide memory delays. For example, an online transaction processing system has natural parallelism among the multiple queries and updates that are presented by requests. Of course, many scientific applications contain natural parallelism because they often model the three-dimensional, parallel structure of nature, and that structure can be exploited by using separate threads. Even desktop applications that use modern Windows-based operating systems often have multiple active applications running, providing a source of parallelism.

> 由于尝试使用更多 ILP 的长期记忆摊位的尝试具有有限的有效性，因此自然要询问应用程序中是否可以使用其他形式的并行性来隐藏内存延迟。例如，在线交易处理系统在请求提出的多个查询和更新中具有自然的并行性。当然，许多科学应用都包含自然的并行性，因为它们通常对自然的三维平行结构进行建模，并且可以通过使用单独的线程来利用该结构。即使使用现代基于 Windows 的操作系统的桌面应用程序也经常运行多个活动应用程序，从而提供了并行性的来源。

_Multithreading_ allows multiple threads to share the functional units of a single processor in an overlapping fashion. In contrast, a more general method to exploit _thread-level parallelism_ (TLP) is with a multiprocessor that has multiple independent threads operating at once and in parallel. Multithreading, however, does not duplicate the entire processor as a multiprocessor does. Instead, multithreading shares most of the processor core among a set of threads, duplicating only private state, such as the registers and program counter. As we will see in [Chapter 5](#_bookmark213), many recent processors incorporate both multiple processor cores on a single chip and provide multithreading within each core.

> *multithReading* 允许多个线程以重叠的方式共享单个处理器的功能单元。相比之下，一种更通用的方法来利用*thread-level parallelism *(TLP)是与一个多个独立线程一起工作和并行运行的多处理器。但是，多线程不会像多处理器那样复制整个处理器。相反，**多线程在一组线程中共享大多数处理器核心，仅复制私人状态，例如寄存器和程序计数器**。正如我们将在[第 5 章](#_bookmark213)中看到的那样，许多最近的处理器在单个芯片上都包含了两个处理器内核，并在每个核心内提供多线程。

Duplicating the per-thread state of a processor core means creating a separate register file and a separate PC for each thread. The memory itself can be shared through the virtual memory mechanisms, which already support multiprogramming. In addition, the hardware must support the ability to change to a different thread relatively quickly; in particular, a thread switch should be much more efficient than a process switch, which typically requires hundreds to thousands of processor cycles. Of course, for multithreading hardware to achieve performance improvements, a program must contain multiple threads (we sometimes say that the application is multithreaded) that could execute in concurrent fashion. These threads are identified either by a compiler (typically from a language with parallelism constructs) or by the programmer.

> 复制处理器核心的每线程状态意味着为每个线程创建单独的寄存器文件和单独的 PC。可以通过虚拟内存机制共享内存本身，该机制已经支持多编程。此外，硬件必须支持相对快速更改为其他线程的能力。特别是，线程开关应该比过程开关要高得多，该过程开关通常需要数百至数千个处理器周期。当然，要获得多线程硬件以实现性能改进，程序必须包含多个线程(有时我们说应用程序是多线程)，这些线程可能以并发方式执行。这些线程由编译器(通常来自具有并行构造的语言)或程序员标识。

There are three main hardware approaches to multithreading: fine-grained, coarse-grained, and simultaneous. _Fine-grained multithreading_ switches between threads on each clock cycle, causing the execution of instructions from multiple threads to be interleaved. This interleaving is often done in a round-robin fashion, skipping any threads that are stalled at that time. One key advantage of finegrained multithreading is that it can hide the throughput losses that arise from both short and long stalls because instructions from other threads can be executed when one thread stalls, even if the stall is only for a few cycles. The primary disadvantage of fine-grained multithreading is that it slows down the execution of an individual thread because a thread that is ready to execute without stalls will be delayed by instructions from other threads. It trades an increase in multithreaded throughput for a loss in the performance (as measured by latency) of a single thread.

> 多线程有三种主要的硬件方法：细粒度，粗粒和同时进行。* fine 粒度的多线程*在每个时钟周期的线程之间之间的开关，从而导致从多个线程进行交错的指令执行。这种交错通常以圆形旋转方式进行，跳过当时停滞不前的任何线程。细化多线程的一个关键优点是，它可以隐藏由短路和长摊位造成的吞吐量损失，因为当一个线程摊位时，即使摊位仅用于几个周期，也可以执行其他线程的指令。细粒度多线程的主要缺点是它会减慢单个线程的执行，因为准备在没有失速的情况下执行的线程将被其他线程从其他线程延迟。它交易了多线程吞吐量的增加，以造成单个线程的性能(按延迟衡量)的损失。

The SPARC T1 through T5 processors (originally made by Sun, now made by Oracle and Fujitsu) use fine-grained multithreading. These processors were targeted at multithreaded workloads such as transaction processing and web services. The T1 supported 8 cores per processor and 4 threads per core, while the T5 supports 16 cores and 128 threads per core. Later versions (T2–T5) also supported 4–8 processors. The NVIDIA GPUs, which we look at in the next chapter, also make use of fine-grained multithreading.

> SPARC T1 通过 T5 处理器(最初由 Sun 制造，现在由 Oracle 和 Fujitsu 制造)使用细粒的多线程。这些处理器针对多线程工作负载，例如交易处理和 Web 服务。T1 每个处理器支持 8 个内核，每个核心 4 个线程，而 T5 则支持 16 个内核和 128 个线程。后来的版本(T2 -T5)也支持 4-8 个处理器。我们在下一章中查看的 NVIDIA GPU 也利用了**细粒度的多线程**。

_Coarse-grained multithreading_ was invented as an alternative to fine-grained multithreading. Coarse-grained multithreading switches threads only on costly stalls, such as level two or three cache misses. Because instructions from other threads will be issued only when a thread encounters a costly stall, coarse-grained multithreading relieves the need to have thread-switching be essentially free and is much less likely to slow down the execution of any one thread.

> * coarse grained 多线程*是发明的，是细粒多线程的替代方法。**粗粒度的多线程**开关仅在昂贵的摊位上(例如第二或三级缓存错过)上的线程。因为只有当线程遇到昂贵的摊位时，才会发布来自其他线程的说明，而粗粒的多线程可缓解实质上免费的线程开关的需求，并且减慢任何一个线程的执行的可能性要小得多。

Coarse-grained multithreading suffers, however, from a major drawback: it is limited in its ability to overcome throughput losses, especially from shorter stalls. This limitation arises from the pipeline start-up costs of coarse-grained multithreading. Because a processor with coarse-grained multithreading issues instructions from a single thread, when a stall occurs, the pipeline will see a bubble before the new thread begins executing. Because of this start-up overhead, coarse-grained multithreading is much more useful for reducing the penalty of very high-cost stalls, where pipeline refill is negligible compared to the stall time. Several research projects have explored coarse-grained multithreading, but no major current processors use this technique.

> 然而，粗粒度的多线程受到了主要缺点：它克服吞吐量损失的能力有限，尤其是从较短的摊位中。这种限制来自粗粒多线程的管道启动成本。由于具有粗粒度多线程的处理器会从单个线程中发出指令，因此当发生摊位时，管道将在新线程开始执行之前看到气泡。由于这个启动开销，粗粒粒的多线程对于减少非常高成本摊位的惩罚更为有用，在该摊位中，与失速时间相比，管道补充可忽略不计。一些研究项目探索了粗粒的多线程，但是当前没有主要的处理器使用此技术。

The most common implementation of multithreading is called _simultaneous multithreading_ (SMT). Simultaneous multithreading is a variation on fine-grained multithreading that arises naturally when fine-grained multithreading is implemented on top of a multiple-issue, dynamically scheduled processor. As with other forms of multithreading, SMT uses thread-level parallelism to hide long-latency events in a processor, thereby increasing the usage of the functional units. The key insight in SMT is that register renaming and dynamic scheduling allow multiple instructions from independent threads to be executed without regard to the dependences among them; the resolution of the dependences can be handled by the dynamic scheduling capability.

> 多线程的最常见实现称为* simultanes 多线程*(SMT)。同时多线程是对细粒多线程的一种变体，当在多发，动态计划的处理器之上实现细颗粒的多线程时，自然会产生。与其他形式的多线程一样，SMT 使用线程级并行性将长期事件隐藏在处理器中，从而增加了功能单元的使用情况。SMT 中的关键见解是，寄存器重命名和动态调度允许执行来自独立线程的多个指令，而无需考虑它们之间的依赖性；依赖的分辨率可以通过动态调度能力来处理。

[Figure 3.31](#_bookmark136) conceptually illustrates the differences in a processor’s ability to exploit the resources of a superscalar for the following processor configurations:

> [图 3.31](#_bookmark136) 从概念上说明了处理器利用超量表资源进行以下处理器配置的能力的差异：

Figure 3.31 How four different approaches use the functional unit execution slots of a superscalar processor. The horizontal dimension represents the instruction execution capability in each clock cycle. The vertical dimension represents a sequence of clock cycles. An empty (white) box indicates that the corresponding execution slot is unused in that clock cycle. The shades of gray and black correspond to four different threads in the multithreading processors. Black is also used to indicate the occupied issue slots in the case of the superscalar without multithreading support. The Sun T1 and T2 (aka Niagara) processors are fine-grained, multithreaded processors, while the Intel Core i7 and IBM Power7 processors use SMT. The T2 has 8 threads, the Power7 has 4, and the Intel i7 has 2. In all existing SMTs, instructions issue from only one thread at a time. The difference in SMT is that the subsequent decision to execute an instruction is decoupled and could execute the operations coming from several different instructions in the same clock cycle.

> 图 3.31 如何使用 SuperScalar 处理器的功能单元执行插槽。水平尺寸表示每个时钟周期中的指令执行能力。垂直尺寸代表一系列时钟循环。一个空的(白色)框，指示相应的执行插槽在该时钟周期中未使用。灰色和黑色的阴影对应于多线程处理器中的四个不同线程。黑色还用于指示在超级标准的情况下，没有多线程支持。Sun T1 和 T2(又名 Niagara)处理器是细粒度的多线程处理器，而 Intel Core i7 和 IBM Power7 处理器则使用 SMT。T2 具有 8 个线程，Power7 具有 4 个线程，并且 Intel I7 具有 2。在所有现有的 SMT 中，指令一次从一个线程出发。SMT 的区别在于，执行指令的后续决定将被脱钩，并且可以在同一时钟周期中执行来自几个不同指令的操作。

A superscalar with simultaneous multithreading

> 具有同时多线程的超级标准

In the superscalar without multithreading support, the use of issue slots is limited by a lack of ILP, including ILP to hide memory latency. Because of the length of L2 and L3 cache misses, much of the processor can be left idle.

> 在没有多线程支持的超级标准中，问题插槽的使用受到 ILP 缺乏的限制，包括 ILP 隐藏记忆延迟。由于 L2 和 L3 高速缓存的长度，因此大部分处理器可以闲置。

In the coarse-grained multithreaded superscalar, the long stalls are partially hidden by switching to another thread that uses the resources of the processor. This switching reduces the number of completely idle clock cycles. In a coarse-grained multithreaded processor, however, thread switching occurs only when there is a stall. Because the new thread has a start-up period, there are likely to be some fully idle cycles remaining.

> 在粗粒的多线程超级标准中，通过切换到使用处理器资源的另一个线程来部分隐藏长摊位。此切换减少了完全闲置时钟周期的数量。但是，在粗粒的多线程处理器中，仅在有失速时才发生线程开关。由于新线程有一个启动期，因此可能还会剩下一些完全闲置的周期。

In the fine-grained case, the interleaving of threads can eliminate fully empty slots. In addition, because the issuing thread is changed on every clock cycle, longer latency operations can be hidden. Because instruction issue and execution are connected, a thread can issue only as many instructions as are ready. With a narrow issue width, this is not a problem (a cycle is either occupied or not), which is why finegrained multithreading works perfectly for a single issue processor, and SMT would make no sense. Indeed, in the Sun T2, there are two issues per clock, but they are from different threads. This eliminates the need to implement the complex dynamic scheduling approach and relies instead on hiding latency with more threads.

> 在细颗粒的情况下，线程的交织可以消除完全空的插槽。此外，由于在每个时钟周期中更改了发行线程，因此可以隐藏更长的延迟操作。由于指令问题和执行是连接的，因此线程只能发布尽可能多的说明。对于狭窄的问题宽度，这不是一个问题(周期是占用或不占用的)，这就是为什么细化的多线程适用于单个问题处理器的原因，而 SMT 毫无意义。确实，在太阳 T2 中，每个时钟有两个问题，但它们来自不同的线程。这消除了实现复杂的动态调度方法的需求，而是依靠更多线程隐藏延迟。

If one implements fine-grained threading on top of a multiple-issue, dynamically schedule processor, the result is SMT. In all existing SMT implementations, all issues come from one thread, although instructions from different threads can initiate execution in the same cycle, using the dynamic scheduling hardware to determine what instructions are ready. Although [Figure 3.31](#_bookmark136) greatly simplifies the real operation of these processors, it does illustrate the potential performance advantages of multithreading in general and SMT in wider issue, dynamically scheduled processors.

> 如果一个人在多发的，动态安排处理器的顶部实现细颗粒的线程，则结果是 SMT。在所有现有的 SMT 实现中，所有问题均来自一个线程，尽管来自不同线程的指令可以使用动态调度硬件在同一周期内启动执行，以确定已准备就绪的指令。尽管[图 3.31](#_bookmark136) 大大简化了这些处理器的真实操作，但它确实说明了多线程的潜在性能优势，而在更广泛的问题(动态计划的处理器)中，多线程和 SMT 的潜在性能优势。

Simultaneous multithreading uses the insight that a dynamically scheduled processor already has many of the hardware mechanisms needed to support the mechanism, including a large virtual register set. Multithreading can be built on top of an out-of-order processor by adding a per-thread renaming table, keeping separate PCs, and providing the capability for instructions from multiple threads to commit.

> 同时多线程使用了一个动态计划的处理器已经具有支持该机制所需的许多硬件机制，包括大型虚拟寄存器集所需的许多硬件机制。多线程可以通过添加每个线程重命名表，保留单独的 PC 并提供从多个线程进行提交的说明的功能来构建多线程。

### Effectiveness of Simultaneous Multithreading on Superscalar Processors

A key question is, how much performance can be gained by implementing SMT? When this question was explored in 2000–2001, researchers assumed that dynamic superscalars would get much wider in the next five years, supporting six to eight issues per clock with speculative dynamic scheduling, many simultaneous loads and stores, large primary caches, and four to eight contexts with simultaneous issue and retirement from multiple contexts. No processor has gotten close to this combination.

> 一个关键问题是，实施 SMT 可以获得多少绩效？当在 2000 - 2001 年探索这个问题时，研究人员认为，动态超量表将在未来五年内变得更加宽广，通过投机性的动态调度，每个时钟都支持六到八期，许多同时负载和存储，大型的主要卡车和四个，而四个则为从多个上下文中同时发行和退休的八个上下文。没有处理器接近这种组合。

As a result, simulation research results that showed gains for multiprogrammed workloads of two or more times are unrealistic. In practice, the existing implementations of SMT offer only two to four contexts with fetching and issue from only one, and up to four issues per clock. The result is that the gain from SMT is also more modest.

> 结果，模拟研究结果显示，两次或更多次多编程工作负载的增长是不现实的。实际上，现有的 SMT 实现仅提供两到四个上下文，并提供一个从一个时钟起，并且每个时钟最多可发出四个问题。结果是 SMT 的增益也更为适中。

[Esmaeilzadeh et al. (2011)](#_bookmark945) did an extensive and insightful set of measurements that examined both the performance and energy benefits of using SMT in a single i7 920 core running a set of multithreaded applications. The Intel i7 920 supported SMT with two threads per core, as does the recent i7 6700. The changes between the i7 920 and the 6700 are relatively small and are unlikely to significantly change the results as shown in this section.

> [Esmaeilzadeh 等。(2011)](#_bookmark945)进行了一系列广泛而有见地的测量值，研究了在单个 i7 920 核心中使用 SMT 的性能和能源好处，该核心运行了一组多线程应用程序。英特尔 i7 920 和最近的 i7 6700 一样，为 SMT 提供了两个线程。i7 920 和 6700 之间的变化相对较小，不太可能显着改变结果，如本节所示。

The benchmarks used consist of a collection of parallel scientific applications and a set of multithreaded Java programs from the DaCapo and SPEC Java suite, as summarized in [Figure 3.32](#_bookmark137). [Figure 3.31](#_bookmark139) shows the ratios of performance and energy efficiency for these benchmarks when run on one core of a i7 920 with SMT turned off and on. (We plot the energy efficiency ratio, which is the inverse of energy consumption, so that, like speedup, a higher ratio is better.)

> 所使用的基准由平行科学应用集合以及 Dacapo 和 Spec Java Suite 的一组多线 Java 程序组成，如[图 3.32](#_bookmark137) 中所述。[图 3.31](#_bookmark139) 显示了这些基准测试的性能和能源效率的比率，当时在 SMT 关闭并开启的 i7 920 的一个核心上运行。(我们绘制能源效率比率，这是能源消耗的倒数，因此，像加速一样，更高的比率也更好。)

The harmonic mean of the speedup for the Java benchmarks is 1.28, despite the two benchmarks that see small gains. These two benchmarks, pjbb2005 and tradebeans, while multithreaded, have limited parallelism. They are included because they are typical of a multithreaded benchmark that might be run on an SMT processor with the hope of extracting some performance, which they find in limited amounts. The PARSEC benchmarks obtain somewhat better speedups than the full set of Java benchmarks (harmonic mean of 1.31). If tradebeans and pjbb2005 were omitted, the Java workload would actually have significantly better speedup (1.39) than the PARSEC benchmarks. (See the discussion of the implication of using harmonic mean to summarize the results in the caption of [Figure 3.33](#_bookmark139).)

> Java 基准测试的加速度的谐波平均值为 1.28，尽管有两个基准有很小的收益。这两个基准测试是 PJBB2005 和贸易 Beans，虽然多线程却有有限的并行性。它们之所以包括在内，是因为它们是多线程基准的典型代表，该基准可能会在 SMT 处理器上运行，希望提取一些性能，并以有限的数量找到。与完整的 Java 基准测试相比，PARSEC 基准获得的加速度(谐波平均值为 1.31)更好。如果省略了贸易 BEAN 和 PJBB2005，那么 Java 的工作量实际上将比 PARSEC 基准高得多(1.39)。(请参阅关于使用谐波均值来总结[图 3.33](#_bookmark139) 的结果的含义的讨论。

Energy consumption is determined by the combination of speedup and increase in power consumption. For the Java benchmarks, on average, SMT delivers the same energy efficiency as non-SMT (average of 1.0), but it is brought down by the two poor performing benchmarks; without pjbb2005 and tradebeans, the average energy efficiency for the Java benchmarks is 1.06, which is almost as good as the PARSEC benchmarks. In the PARSEC benchmarks, SMT reduces energy by

> 能源消耗取决于加速和功耗增加的结合。对于 Java 基准，平均而言，SMT 提供与非 SMT 相同的能源效率(平均 1.0)，但由两个表现不佳的基准测试。没有 PJBB2005 和贸易爆炸，Java 基准的平均能源效率为 1.06，几乎与 PARSEC 基准一样好。在 parsec 基准中，SMT 减少了能量

1 (1/1.08) 7%. Such energy-reducing performance enhancements are _very difficult_ to find. Of course, the static power associated with SMT is paid in both cases, thus the results probably slightly overstate the energy gains.

> 1(1/1.08)7％。这种降低能源的性能提高很难找到。当然，在两种情况下都支付了与 SMT 相关的静电功率，因此结果可能略微夸大了能源增长。

These results clearly show that SMT with extensive support in an aggressive speculative processor can improve performance in an energy-efficient fashion. In 2011, the balance between offering multiple simpler cores and fewer more sophisticated cores has shifted in favor of more cores, with each core typically being a threeto four-issue superscalar with SMT supporting two to four threads. Indeed, [Esmaeilzadeh et al. (2011)](#_bookmark945) show that the energy improvements from SMT are even larger on the Intel i5 (a processor similar to the i7, but with smaller caches and a lower clock rate) and the Intel Atom (an 80 x 86 processor originally designed for the netbook and PMD market, now focused on low-end PCs, and described in [Section 3.13](#_bookmark148)).

> 这些结果清楚地表明，在积极的投机处理器中获得广泛支持的 SMT 可以以节能方式提高性能。在 2011 年，提供多个简单的核心和更少复杂的核心之间的平衡已经转移了更多的核心，每个核心通常都是三局的三局 Superscalar，SMT 支撑了两到四个线程。的确，[Esmaeilzadeh 等。(2011)](#_bookmark945)表明，Intel i5(类似于 i7 的处理器，但较小的缓存和较低的时钟速率)和 Intel Atom(80 x 86 处理器)中 SMT 的能量改进甚至更大最初是为上网本和 PMD 市场设计的，现在专注于低端 PC，并在[第 3.13 节](#_bookmark148)中进行了描述。

Figure 3.32 The parallel benchmarks used here to examine multithreading, as well as in [Chapter 5](#_bookmark0) to examine multiprocessing with an i7. The top half of the chart consists of PARSEC benchmarks collected by Bienia et al. (2008). The PARSEC benchmarks are meant to be indicative of compute-intensive, parallel applications that would be appropriate for multicore processors. The lower half consists of multithreaded Java benchmarks from the DaCapo collection (see Blackburn et al., 2006) and pjbb2005 from SPEC. All of these benchmarks contain some parallelism; other Java benchmarks in the DaCapo and SPEC Java workloads use multiple threads but have little or no true parallelism and, hence, are not used here. See Esmaeilzadeh et al. (2011) for additional information on the characteristics of these benchmarks, relative to the measurements here and in [Chapter 5](#_bookmark0).

> 图 3.32 此处用于检查多线程的平行基准以及[第 5 章](#_bookmark0)在使用 i7 检查多处理。图表的上半部分由 Bienia 等人收集的 PARSEC 基准组成。(2008)。PARSEC 基准旨在指示适用于多层处理器的计算密集型，并行应用。下半部分由 DACAPO 系列的多线程 Java 基准组成(参见 Blackburn 等，2006)和 Spec 的 PJBB2005。所有这些基准都包含一些并行性。Dacapo 和 Spec Java 工作负载中的其他 Java 基准测试使用多个线程，但几乎没有真正的并行性，因此在这里不使用。参见 Esmaeilzadeh 等。(2011)有关这些基准测试特征的其他信息，相对于此处和[第 5 章](#_bookmark0)的测量值。
