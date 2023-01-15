---
data: 2023-01-15
---
> [!note] 内存层次结构基础

## Cross-Cutting Issues: The Design of Memory Hierarchies(交叉切割问题：内存层次结构的设计)

This section describes four topics discussed in other chapters that are fundamental to memory hierarchies.

> 本节介绍了其他章节中讨论的四个主题，这些主题是内存层次结构的基础。

### Protection, Virtualization, and Instruction Set Architecture(保护，虚拟化和指令集体系结构)

Protection is a joint effort of architecture and operating systems, but architects had to modify some awkward details of existing instruction set architectures when vir- tual memory became popular. For example, to support virtual memory in the IBM 370, architects had to change the successful IBM 360 instruction set architecture that had been announced just 6 years before. Similar adjustments are being made today to accommodate virtual machines.

> 保护是建筑和操作系统的共同努力，但是当纯记忆变得流行时，建筑师必须修改现有指令集架构的一些尴尬细节。例如，为了支持 IBM 370 中的虚拟内存，建筑师必须更改仅 6 年前宣布的成功的 IBM 360 指令集架构。今天正在进行类似的调整以容纳虚拟机。

For example, the 80x86 instruction POPF loads the flag registers from the top of the stack in memory. One of the flags is the Interrupt Enable (IE) flag. Until recent changes to support virtualization, running the POPF instruction in user mode, rather than trapping it, simply changed all the flags except IE. In system mode, it does change the IE flag. Because a guest OS runs in user mode inside a VM, this was a problem, as the OS would expect to see a changed IE. Extensions of the 80x86 architecture to support virtualization eliminated this problem.

> 例如，80x86 指令 POPF 从内存中的堆栈顶部加载标志寄存器。旗帜之一是中断启用(即)标志。直到最新更改以支持虚拟化，以用户模式运行 POPF 指令，而不是捕获它，只是更改除了 IE 之外的所有标志。在系统模式下，它确实更改了 IE 标志。由于访客操作系统在 VM 内部以用户模式运行，因此这是一个问题，因为 OS 希望看到更改 IE。支持虚拟化的 80x86 体系结构的扩展消除了此问题。

Historically, IBM mainframe hardware and VMM took three steps to improve performance of virtual machines:

> 从历史上看，IBM 大型机硬件和 VMM 采取了三个步骤来提高虚拟机的性能：

1. Reduce the cost of processor virtualization.
2. Reduce interrupt overhead cost due to the virtualization.
3. Reduce interrupt cost by steering interrupts to the proper VM without invoking VMM.

> 1. 降低处理器虚拟化的成本。
> 2. 减少由于虚拟化而导致的中断开销成本。
> 3. 通过将中断转向正确的 VM 而无需调用 VMM 来降低中断成本。

IBM is still the gold standard of virtual machine technology. For example, an IBM mainframe ran thousands of Linux VMs in 2000, while Xen ran 25 VMs in 2004 ([Clark et al., 2004](#_bookmark937)). Recent versions of Intel and AMD chipsets have added special instructions to support devices in a VM to mask interrupts at lower levels from each VM and to steer interrupts to the appropriate VM.

> IBM 仍然是虚拟机技术的黄金标准。例如，IBM 大型机在 2000 年运行了数千个 Linux VM，而 Xen 在 2004 年运行 25 VM([[Clark 等，2004](#_bookmark937))。Intel 和 AMD 芯片组的最新版本添加了特殊说明，以支持 VM 中的设备，以掩盖每个 VM 较低级别的中断，并将中断转向适当的 VM。

### Autonomous Instruction Fetch Units(自主指令获取单元)

Many processors with out-of-order execution and even some with simply deep pipelines decouple the instruction fetch (and sometimes initial decode), using a separate instruction fetch unit (see [Chapter 3](#_bookmark93)). Typically, the instruction fetch unit accesses the instruction cache to fetch an entire block before decoding it into indi- vidual instructions; such a technique is particularly useful when the instruction length varies. Because the instruction cache is accessed in blocks, it no longer makes sense to compare miss rates to processors that access the instruction cache once per instruction. In addition, the instruction fetch unit may prefetch blocks into the L1 cache; these prefetches may generate additional misses, but may actually reduce the total miss penalty incurred. Many processors also include data prefetch- ing, which may increase the data cache miss rate, even while decreasing the total data cache miss penalty.

> 许多带有排序执行的处理器，甚至有些具有简单的深层管道的处理器将指令摘要(有时是初始解码)解除，使用单独的指令获取单元(请参阅[[第 3 章](#_bookmark93))。通常，指令获取单元访问指令缓存以获取整个块，然后再将其解码为独立说明；当指令长度变化时，这种技术特别有用。由于指令缓存是在块中访问的，因此将失误率与每次指令一次访问指令缓存的处理器进行比较不再有意义。此外，指令提取单元可能会将块预取块进入 L1 缓存；这些预购可能会产生更多的失误，但实际上可能会减少所产生的总罚款。许多处理器还包括数据预取，即使减少总数据缓存损失惩罚，也可能会增加数据缓存率。

### Speculation and Memory Access(猜测和内存访问)

One of the major techniques used in advanced pipelines is speculation, whereby an instruction is tentatively executed before the processor knows whether it is really needed. Such techniques rely on branch prediction, which if incorrect requires that the speculated instructions are flushed from the pipeline. There are two separate issues in a memory system supporting speculation: protection and performance. With speculation, the processor may generate memory references, which will never be used because the instructions were the result of incorrect speculation. Those references, if executed, could generate protection exceptions. Obviously, such faults should occur only if the instruction is actually executed. In the next chapter, we will see how such “speculative exceptions” are resolved. Because a speculative processor may generate accesses to both the instruction and data caches, and subsequently not use the results of those accesses, speculation may increase the cache miss rates. As with prefetching, however, such speculation may actually lower the total cache miss penalty. The use of speculation, like the use of prefetching, makes it misleading to compare miss rates to those seen in pro- cessors without speculation, even when the ISA and cache structures are otherwise identical.

> 高级管道中使用的主要技术之一是猜测，在处理器知道是否真的需要之前，暂时执行指令。这样的技术依赖于分支预测，如果不正确，则要求将推测指令从管道中冲洗。内存系统中有两个单独的问题支持猜测：保护和性能。通过猜测，处理器可能会生成内存引用，因为指令是错误猜测的结果，因此永远不会使用。这些引用(如果执行)可以生成保护异常。显然，只有在实际执行指令时才会发生此类故障。在下一章中，我们将看到如何解决这种“投机性异常”。由于投机处理器可以同时生成指令和数据缓存的访问权限，然后不使用这些访问的结果，因此推测可能会增加缓存率。然而，与预摘要一样，这种猜测实际上可能会降低总的缓存错过罚款。猜测的使用，例如使用预摘要，也使人们误导了将失误率与没有猜测的情况下的未经猜测进行比较，即使 ISA 和缓存结构否则也是相同的。

### Special Instruction Caches(特殊说明缓存)

One of the biggest challenges in superscalar processors is to supply the instruc- tion bandwidth. For designs that translate the instructions into micro-operations, such as most recent Arm and i7 processors, instruction bandwidth demands and branch misprediction penalties can be reduced by keeping a small cache of recently translated instructions. We explore this technique in greater depth in the next chapter.

> 超级处理器中最大的挑战之一是提供指导带宽。对于将说明转化为微功能的设计，例如最近的 ARM 和 i7 处理器，可以通过保留最近翻译的指令的少量缓存来减少指导带宽需求和分支错误预测的惩罚。我们在下一章中更深入地探讨了这一技术。

### Coherency of Cached Data(缓存数据的连贯性)

Data can be found in memory and in the cache. As long as the processor is the sole component changing or reading the data and the cache stands between the proces- sor and memory, there is little danger in the processor seeing the old or _stale_ copy. As we will see, multiple processors and I/O devices raise the opportunity for copies to be inconsistent and to read the wrong copy.

> 数据可以在内存和缓存中找到。只要处理器是更改或读取数据的唯一组件，并且缓存位于程序和内存之间，则在处理器中，看到旧或 _Stale_ 复制几乎没有危险。正如我们将看到的那样，多个处理器和 I/O 设备为副本不一致并阅读错误的副本提供了机会。

The frequency of the cache coherency problem is different for multiprocessors than for I/O. Multiple data copies are a rare event for I/O—one to be avoided when- ever possible—but a program running on multiple processors will _want_ to have copies of the same data in several caches. Performance of a multiprocessor pro- gram depends on the performance of the system when sharing data.

> 对于多处理器而言，缓存相干性问题的频率与 I/O 不同。多个数据副本对于 I/O 来说是罕见的事件 - 在可能的情况下避免了一个数据副本，但是在多个处理器上运行的程序将 _WANT_ 在几个缓存中具有相同数据的副本。多处理器处理的性能取决于共享数据时系统的性能。

The _I/O cache coherency_ question is this: where does the I/O occur in the com- puter—between the I/O device and the cache or between the I/O device and main memory? If input puts data into the cache and output reads data from the cache, both I/O and the processor see the same data. The difficulty in this approach is that it interferes with the processor and can cause the processor to stall for I/O. Input may also interfere with the cache by displacing some information with new data that are unlikely to be accessed soon.

> *i/o 缓存相干*问题是：I/O 在 I/O 设备与缓存之间或 I/O 设备与主内存之间发生 I/O 在哪里发生？如果输入将数据放入缓存并输出读取来自缓存的数据，则 I/O 和处理器都会看到相同的数据。这种方法的困难在于它会干扰处理器，并可能导致处理器停止 I/O。输入也可能通过将一些信息输送到不太可能很快访问的新数据中来干扰缓存。

The goal for the I/O system in a computer with a cache is to prevent the stale data problem while interfering as little as possible. Many systems therefore prefer that I/O occur directly to main memory, with main memory acting as an I/O buffer. If a write-through cache were used, then memory would have an up-to-date copy of the information, and there would be no stale data issue for output. (This benefit is a reason processors used write through.) However, today write through is usually found only in first-level data caches backed by an L2 cache that uses write back. Input requires some extra work. The software solution is to guarantee that no blocks of the input buffer are in the cache. A page containing the buffer can be marked as noncachable, and the operating system can always input to such a page. Alternatively, the operating system can flush the buffer addresses from the cache before the input occurs. A hardware solution is to check the I/O addresses on input to see if they are in the cache. If there is a match of I/O addresses in the cache, the cache entries are invalidated to avoid stale data. All of these approaches can also be used for output with write-back caches.

> 具有缓存的计算机中 I/O 系统的目标是防止陈旧的数据问题，同时尽可能少地干扰。因此，许多系统更喜欢直接发生 I/O，而主内存则充当 I/O 缓冲区。如果使用写入缓存，则内存将具有最新信息的最新副本，并且不会出现陈旧的数据问题。(这个好处是处理器写入的原因。)但是，今天写的通常仅在由使用回报的 L2 缓存支持的第一级数据缓存中找到。输入需要一些额外的工作。软件解决方案是为了确保没有输入缓冲区的块在缓存中。包含缓冲区的页面可以标记为不可访问的页面，并且操作系统始终可以输入此页面。另外，操作系统可以在输入发生之前从缓存中冲洗缓冲区。硬件解决方案是检查输入上的 I/O 地址，以查看它们是否在缓存中。如果缓存中有 I/O 地址的匹配，则缓存条目将无效以避免过时的数据。所有这些方法也可以用于带有写下缓存的输出。

Processor cache coherency is a critical subject in the age of multicore proces- sors, and we will examine it in detail in [Chapter 5](#_bookmark213).

> 处理器高速缓存相干性是多核心过程时代的关键主题，我们将在[第 5 章](#_bookmark213)中详细检查它。
