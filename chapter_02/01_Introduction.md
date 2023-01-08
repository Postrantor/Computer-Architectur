## Introduction

> ## 介绍

Computer pioneers correctly predicted that programmers would want unlimited amounts of fast memory. An economical solution to that desire is a memory hierarchy, which takes advantage of locality and trade-offs in the cost-performance of memory technologies. The principle of locality, presented in the first chapter, says that most programs do not access all code or data uniformly. Locality occurs in time (temporal locality) and in space (spatial locality). This principle plus the guideline that for a given implementation technology and power budget, smaller hardware can be made faster led to hierarchies based on memories of different speeds and sizes. [Figure 2.1](#_bookmark49) shows several different multilevel memory hierarchies, including typical sizes and speeds of access. As Flash and next generation memory technologies continue to close the gap with disks in cost per bit, such technologies are likely to increasingly replace magnetic disks for secondary storage. As [Figure 2.1](#_bookmark49) shows, these technologies are already used in many personal computers and increasingly in servers, where the advantages in performance, power, and density are significant.

> 计算机先驱者正确地预测了程序员需要无限量的快速内存。对该欲望的经济解决方案是记忆层次结构，它利用了记忆技术成本绩效的地方和权衡。第一章中提出的本地性原则说，大多数程序并未统一访问所有代码或数据。局部性发生在时间（时间区域）和空间（空间位置）。该原则加上一个指南，即对于给定的实施技术和功率预算，可以使较小的硬件更快地导致基于不同速度和尺寸的记忆，从而导致层次结构。[图 2.1]（#_ bookmark49）显示了几个不同的多级内存层次结构，包括典型的尺寸和访问速度。随着闪光灯和下一代记忆技术继续使用磁盘的每位成本来缩小差距，此类技术可能会越来越替换磁盘以进行辅助存储。如[图 2.1]（#_ bookmark49）所示，这些技术已经在许多个人计算机中使用，并且在服务器中越来越多地使用，在该服务器中，性能，功率和密度的优势很重要。

Because fast memory is more expensive, a memory hierarchy is organized into several levels—each smaller, faster, and more expensive per byte than the next lower level, which is farther from the processor. The goal is to provide a memory system with a cost per byte that is almost as low as the cheapest level of memory and a speed almost as fast as the fastest level. In most cases (but not all), the data contained in a lower level are a superset of the next higher level. This property, called the _inclusion property_, is always required for the lowest level of the hierarchy, which consists of main memory in the case of caches and secondary storage (disk or Flash) in the case of virtual memory.

> 由于快速内存更昂贵，因此将内存层次结构分为几个级别 - 比下一个较低级别的较小，更快，更昂贵的每个字节，该级别离处理器更远。目的是提供一个以每个字节成本的内存系统，几乎与最便宜的内存水平和速度一样低，几乎与最快的水平一样快。在大多数情况下（但不是全部），较低级别中包含的数据是下一个更高级别的超集。该属性称为* Inclusion 属性*，始终需要层次结构的最低级别，在虚拟内存的情况下，在缓存和辅助存储（磁盘或闪存）的情况下，该属性由主内存组成。

The importance of the memory hierarchy has increased with advances in performance of processors. [Figure 2.2](#_bookmark50) plots single processor performance projections against the historical performance improvement in time to access main memory. The processor line shows the increase in memory requests per second on average (i.e., the inverse of the latency between memory references), while the memory line shows the increase in DRAM accesses per second (i.e., the inverse of the DRAM access latency), assuming a single DRAM and a single memory bank. The reality is more complex because the processor request rate is not uniform, and the memory system typically has multiple banks of DRAMs and channels. Although the gap in access time increased significantly for many years, the lack of significant performance improvement in single processors has led to a slowdown in the growth of the gap between processors and DRAM.

> 随着处理器性能的进步，内存层次结构的重要性增加了。[图 2.2]（#\_ bookmark50）绘制单个处理器性能预测，以改善访问主内存的历史绩效。处理器行显示了平均每秒内存请求的增加（即，内存引用之间的延迟倒数），而内存行显示了每秒 DRAM 访问的增加（即，DRAM 访问延迟的倒数），假设一个 DRAM 和一个单一的存储器库。现实更为复杂，因为处理器请求率不统一，并且内存系统通常具有多个 DRAM 和渠道。尽管多年的访问时间差距显着增加，但单个处理器的性能缺乏显着提高，导致处理器和 DRAM 之间差距的增长放缓。

Because high-end processors have multiple cores, the bandwidth requirements are greater than for single cores. Although single-core bandwidth has grown more slowly in recent years, the gap between CPU memory demand and DRAM bandwidth continues to grow as the numbers of cores grow. A modern high-end desktop processor such as the Intel Core i7 6700 can generate two data memory references per core each clock cycle. With four cores and a 4.2 GHz clock rate, the i7 can generate a peak of 32.8 billion 64-bit data memory references per second, in addition to a peak instruction demand of about 12.8 billion 128-bit instruction

> 由于高端处理器具有多个核心，因此带宽要求大于单核。尽管近年来单核带宽的增长越来越缓慢，但随着核心数量的增长，CPU 记忆需求和 DRAM 带宽之间的差距继续增长。现代的高端桌面处理器（例如英特尔核心 i7 6700）可以在每个核心周期中生成两个数据存储器引用。i7 具有四个内核和 4.2 GHz 时钟速率，除了约 128 亿 128 位指令的峰值指令需求外，每秒可产生 328 亿位 64 位数据记忆参考的峰值

![](./media/image99.png)Register reference
![](./media/image104.png)(B) Memory hierarchy for a laptop or a desktop

> Figure 2.1 The levels in a typical memory hierarchy in a personal mobile device (PMD), such as a cell phone or tablet (A), in a laptop or desktop computer (B), and in a server (C). As we move farther away from the processor, the memory in the level below becomes slower and larger. Note that the time units change by a factor of 10<sup>9</sup> from picoseconds to milliseconds in the case of magnetic disks and that the size units change by a factor of 10<sup>10</sup> from thousands of bytes to tens of terabytes. If we were to add warehouse-sized computers, as opposed to just servers, the capacity scale would increase by three to six orders of magnitude. Solid-state drives (SSDs) composed of Flash are used exclusively in PMDs, and heavily in both laptops and desktops. In many desktops, the primary storage system is SSD, and expansion disks are primarily hard disk drives (HDDs). Likewise, many servers mix SSDs and HDDs.

> 图 2.1 个人移动设备（PMD）（例如手机或平板电脑（a）），笔记本电脑或台式计算机（B）和服务器（C）中的典型内存层次结构中的级别。随着我们远离处理器的移动，下面的级别中的内存变得越来越大。请注意，在磁盘的情况下，时间单元的变化为 10 <sup> 9 </sup>从 picseconds 变为毫秒，并且大小单位从数千个变化 10 <sup> 10 </sup>。字节至数十吨的字节。如果我们要添加仓库大小的计算机，而不是仅服务器，那么容量量表将增加三到六个数量级。由 Flash 组成的固态驱动器（SSD）仅在 PMD 中使用，并在笔记本电脑和台式机中大量使用。在许多台式机中，主要存储系统是 SSD，扩展磁盘主要是硬盘驱动器（HDDS）。同样，许多服务器混合 SSD 和 HDD。

![](./media/image110.png)<span id="_bookmark50" class="anchor"></span>100,000

> Figure 2.2 Starting with 1980 performance as a baseline, the gap in performance, measured as the difference in the time between processor memory requests (for a single processor or core) and the latency of a DRAM access, is plotted over time. In mid-2017, AMD, Intel and Nvidia all announced chip sets using versions of HBM technology. Note that the vertical axis must be on a logarithmic scale to record the size of the processor-DRAM performance gap. The memory baseline is 64 KiB DRAM in 1980, with a 1.07 per year performance improvement in latency (see [Figure 2.4](#_bookmark53) on page 88). The processor line assumes a 1.25 improvement per year until 1986, a 1.52 improvement until 2000, a 1.20 improvement between 2000 and 2005, and only small improvements in processor performance (on a per-core basis) between 2005 and 2015. As you can see, until 2010 memory access times in DRAM improved slowly but consistently; since 2010 the improvement in access time has reduced, as compared with the earlier periods, although there have been continued improvements in bandwidth. See Figure 1.1 in [Chapter 1](#_bookmark2) for more information.

> 图 2.2 从 1980 年的性能开始，作为基线的差距，以处理器内存请求（用于单个处理器或核心）和 DRAM 访问的延迟之间的时间差异，以衡量。在 2017 年中，AMD，Intel 和 Nvidia 都使用 HBM 技术版本宣布了芯片套件。请注意，垂直轴必须处于对数尺度上，才能记录处理器-DRAM 性能差距的大小。记忆基线在 1980 年为 64 KIB DRAM，延迟的每年 1.07 绩效提高（请参见第 88 页的[图 2.4]（#_ bookmark53））。该处理器生产线每年的改善为 1.25，直到 1986 年为 1.52，直到 2000 年，在 2000 年至 2005 年之间有 1.20 的改善，并且在 2005 年至 2015 年之间，处理器性能（每核）的改善只有很小的改善。，直到 2010 年，DRAM 中的记忆访问时间都缓慢但始终如一。自 2010 年以来，与较早时期相比，访问时间的改善有所减少，尽管带宽持续有所改善。有关更多信息，请参见[第 1 章]（第 1 章]（#_ Bookmark2）中的图 1.1。

references; this is a total peak demand bandwidth of 409.6 GiB/s! This incredible bandwidth is achieved by multiporting and pipelining the caches; by using three levels of caches, with two private levels per core and a shared L3; and by using a separate instruction and data cache at the first level. In contrast, the peak bandwidth for DRAM main memory, using two memory channels, is only 8% of the demand bandwidth (34.1 GiB/s). Upcoming versions are expected to have an L4 DRAM cache using embedded or stacked DRAM (see [Sections 2.2](#memory-technology-and-optimizations) and [2.3](#ten-advanced-optimizations-of-cache-performance)). Traditionally, designers of memory hierarchies focused on optimizing average memory access time, which is determined by the cache access time, miss rate, and miss penalty. More recently, however, power has become a major consideration. In high-end microprocessors, there may be 60 MiB or more of on-chip cache, and a large secondor third-level cache will consume significant power both as leakage when not operating (called _static power_) and as active power, as when performing a read or write (called _dynamic power_), as described in [Section 2.3](#ten-advanced-optimizations-of-cache-performance). The problem is even more acute in processors in PMDs where the CPU is less aggressive and the power budget may be 20 to 50 times smaller. In such cases, the caches can account for 25% to 50% of the total power consumption. Thus more designs must consider both performance and power trade-offs, and we will examine both in this chapter.

> 参考;这是 409.6 GIB/S 的总峰值需求带宽！这种令人难以置信的带宽是通过将缓存物多用和管道供电来实现的。通过使用三个级别的缓存，每个核心有两个私人级别和一个共享的 L3；并在第一级使用单独的指令和数据缓存。相反，使用两个内存通道的 DRAM 主内存的峰带宽仅是需求带宽的 8％（34.1 GIB/s）。预计即将到来的版本使用嵌入式或堆叠的 DRAM 具有 L4 DRAM CACHE（请参阅[第 2.2 节]（#MOMEMION-TECHNOLOCY and-OPTIMIZATIONS）和[2.3]（#ten-Ad-Ad-Advanced-Optigrization of-Cache-Performance-formformance of-Cache-Performance-formallance）。传统上，内存层次结构的设计师致力于优化平均内存访问时间，这取决于缓存访问时间，错过率和罚款。然而，最近，权力已成为主要考虑因素。在高端微处理器中，可能有 60 个或更多的芯片缓存，并且大型二级高速缓存在不工作时会消耗大量功率（称为 *STATIC Power*）和有效力量，例如执行时如[第 2.3 节]中所述（#ten-Advanced-Optimization of-Cache-Performance）中所述的读或写入（称为 *DYNAMIC POWER*）。问题在 PMD 中的处理器中，CPU 的侵略性较低，并且功率预算可能小的 20 至 50 倍。在这种情况下，缓存可以占总功耗的 25％至 50％。因此，更多的设计必须同时考虑性能和权力权衡，我们将在本章中进行研究。

### Basics of Memory Hierarchies: A Quick Review

> ###内存层次结构的基础知识：快速评论

The increasing size and thus importance of this gap led to the migration of the basics of memory hierarchy into undergraduate courses in computer architecture, and even to courses in operating systems and compilers. Thus we’ll start with a quick review of caches and their operation. The bulk of the chapter, however, describes more advanced innovations that attack the processor—memory performance gap.

> 这种差距的规模不断增加，因此导致记忆层次结构的基础知识迁移到计算机架构中的本科课程，甚至是操作系统和编译器中的课程。因此，我们将从快速审查缓存及其操作开始。然而，本章的大部分描述了攻击处理器的更高级创新 - 记忆性能差距。

When a word is not found in the cache, the word must be fetched from a lower level in the hierarchy (which may be another cache or the main memory) and placed in the cache before continuing. Multiple words, called a _block_ (or _line_), are moved for efficiency reasons, and because they are likely to be needed soon due to spatial locality. Each cache block includes a _tag_ to indicate which memory address it corresponds to.

> 当缓存中找不到单词时，必须从层次结构中的较低级别（可能是另一个缓存或主内存）中获取单词，然后将其放在缓存中。出于效率原因，多个称为 *block*（或 *line*）的单词被移动，并且由于空间位置可能很快需要它们。每个缓存块都包含一个 *tag*，以指示其对应的内存地址。

A key design decision is where blocks (or lines) can be placed in a cache. The most popular scheme is _set associative_, where a _set_ is a group of blocks in the cache. A block is first mapped onto a set, and then the block can be placed anywhere within that set. Finding a block consists of first mapping the block address to the set and then searching the set—usually in parallel—to find the block. The set is chosen by the address of the data:

> 一个关键的设计决策是可以将块（或行）放在缓存中的位置。最受欢迎的方案是 *SET Associative*，其中 *set* 是缓存中的一组块。首先将块映射到集合上，然后可以将块放置在该集合中的任何地方。查找块包括首先将块地址映射到集合，然后并行搜索集合以找到块。该集由数据地址选择：

(_Block address_) MOD (_Number of sets in cache_)

If there are _n_ blocks in a set, the cache placement is called _n-way set associative_. The end points of set associativity have their own names. A _direct-mapped_ cache has just one block per set (so a block is always placed in the same location), and a _fully associative_ cache has just one set (so a block can be placed anywhere).

> 如果集合中有 *n* 块，则缓存位置称为 *n-way set coopiative*。集合关联的终点有自己的名字。* direct-mapped *缓存每组仅一个块（因此始终将一个块放置在同一位置），并且一个*相关的 cassiative* cache 只有一组（因此可以将块放置在任何地方）。

Caching data that is only read is easy because the copy in the cache and memory will be identical. Caching writes is more difficult; for example, how can the copy in the cache and memory be kept consistent? There are two main strategies. A _write-through_ cache updates the item in the cache _and_ writes through to update main memory. A _write-back_ cache only updates the copy in the cache. When the block is about to be replaced, it is copied back to memory. Both write strategies can use a _write buffer_ to allow the cache to proceed as soon as the data are placed in the buffer rather than wait for full latency to write the data into memory.

> 仅读取的缓存数据很容易，因为缓存和内存中的副本将是相同的。缓存写作更加困难。例如，如何保持缓存和内存中的副本保持一致？有两种主要策略。*write-through* 缓存更新缓存中的项目*和*在更新主内存中。*write-back* 缓存仅更新缓存中的副本。当要替换块时，将其复制回存储器。两种写策略都可以使用*write buffer *允许将数据放置在缓冲区中，而不是等待完整的延迟将数据写入内存中。

One measure of the benefits of different cache organizations is miss rate. _Miss rate_ is simply the fraction of cache accesses that result in a miss—that is, the number of accesses that miss divided by the number of accesses.

> 衡量不同缓存组织的好处的一种衡量标准是错率。* MISS RATE*只是导致错过的缓存访问的部分，即错过访问数量的访问数量。

To gain insights into the causes of high miss rates, which can inspire better cache designs, the three Cs model sorts all misses into three simple categories:

> 为了洞悉高率的原因，这可以激发更好的缓存设计，这三个 CS 模型都将所有遗漏分为三个简单类别：

_Compulsory_—The very first access to a block _cannot_ be in the cache, so the block must be brought into the cache. Compulsory misses are those that occur even if you were to have an infinite-sized cache.

> _COMPULSORY_ - 第一个访问块 *cannot* 位于缓存中，因此必须将块带入缓存中。强制性错过是即使您拥有无限大小的缓存，也是如此。

_Capacity_—If the cache cannot contain all the blocks needed during execution of a program, capacity misses (in addition to compulsory misses) will occur because of blocks being discarded and later retrieved.

> _ capacity_-如果缓存无法包含程序执行过程中所需的所有块，则由于丢弃并检索到的块，将发生容量失误（除强制性错过）。

_Conflict_—If the block placement strategy is not fully associative, conflict misses (in addition to compulsory and capacity misses) will occur because a block may be discarded and later retrieved if multiple blocks map to its set and accesses to the different blocks are intermingled.

> _conflict_ - 如果块放置策略不是完全关联的，则冲突错过（除强制性和容量失误外），因为可能会丢弃一个块，并且如果多个块映射到其集合，并且访问对不同块的访问被丢弃。

> Figure B.8 on page 24 shows the relative frequency of cache misses broken down by the three Cs. As mentioned in [Appendix B](#_bookmark436), the three C’s model is conceptual, and although its insights usually hold, it is not a definitive model for explaining the cache behavior of individual references.

> 第 24 页的图 B.8 显示了三个 CS 分解的高速缓存的相对频率。如[附录 B]（#\_ bookmark436）中提到的，这三个 C 的模型是概念性的，尽管它的见解通常可以保持，但它不是解释单个参考的缓存行为的确定模型。

As we will see in Chapters [3](#_bookmark93) and [5](#_bookmark213), multithreading and multiple cores add complications for caches, both increasing the potential for capacity misses as well as adding a fourth C, for _coherency_ misses due to cache flushes to keep multiple caches coherent in a multiprocessor; we will consider these issues in [Chapter 5](#_bookmark213). However, miss rate can be a misleading measure for several reasons. Therefore some designers prefer measuring _misses per instruction_ rather than misses per memory reference (miss rate). These two are related:

> 正如我们将在第[3]（#_ bookmark93）和[5]（#_ bookmark213）中看到的那样缓存冲洗以使多个缓存在多处理器中相干；我们将在[第 5 章]（#* bookmark213）中考虑这些问题。但是，由于几个原因，错过的费率可能是一个误导性的措施。因此，一些设计师喜欢测量*每个指令*的仪式*，而不是每个内存参考（错过率）的错过。这两个相关：

> ===

where _hit time_ is the time to hit in the cache and _miss penalty_ is the time to replace the block from memory (that is, the cost of a miss). Average memory access time is still an indirect measure of performance; although it is a better measure than miss rate, it is not a substitute for execution time. In [Chapter 3](#_bookmark93) we will see that speculative processors may execute other instructions during a miss, thereby reducing the effective miss penalty. The use of multithreading (introduced in [Chapter 3](#_bookmark93)) also allows a processor to tolerate misses without being forced to idle. As we will examine shortly, to take advantage of such latency tolerating techniques, we need caches that can service requests while handling an outstanding miss.

> 其中 *hit Time* 是在缓存中击中的时间，而 MISS 惩罚是从内存中替换块的时间（即失误的成本）。平均内存访问时间仍然是绩效的间接度量。尽管这比错过率更好，但它不能代替执行时间。在[第 3 章]（#_ bookmark93）中，我们将看到投机性处理器可以在错过期间执行其他说明，从而减少有效的罚款。多线程的使用（在[第 3 章]（#_ bookmark93）中引入）还允许处理器宽容而不会被迫闲置。正如我们将在不久的将来，要利用这种延迟耐受性技术，我们需要在处理出色的错过时可以服务请求的缓存。

If this material is new to you, or if this quick review moves too quickly, see [Appendix B](#_bookmark436). It covers the same introductory material in more depth and includes examples of caches from real computers and quantitative evaluations of their effectiveness.

> 如果此材料对您来说是新的，或者此快速评论移动得太快，请参见[附录 B]（#\_ bookmark436）。它涵盖了更深入的相同介绍材料，并包括来自真实计算机的缓存示例以及对其有效性的定量评估。

Section B.3 in [Appendix B](#_bookmark436) presents six basic cache optimizations, which we quickly review here. The appendix also gives quantitative examples of the benefits of these optimizations. We also comment briefly on the power implications of these trade-offs.

> B.3 节[附录 B]（#\_ bookmark436）介绍了六个基本的缓存优化，我们在此处快速审查。附录还给出了这些优化的好处的定量示例。我们还简要评论了这些权衡的权力影响。

1. _Larger block size to reduce miss rate_—The simplest way to reduce the miss rate is to take advantage of spatial locality and increase the block size. Larger blocks reduce compulsory misses, but they also increase the miss penalty. Because larger blocks lower the number of tags, they can slightly reduce static power. Larger block sizes can also increase capacity or conflict misses, especially in smaller caches. Choosing the right block size is a complex trade-off that depends on the size of cache and the miss penalty.

> 1. \_larger 块大小以降低错过率 - 降低错过率的最简单方法是利用空间位置并增加块大小。较大的块减少了强制性的失误，但也增加了罚款。由于较大的块会降低标签的数量，因此它们可以稍微降低静态功率。较大的块尺寸也会增加容量或冲突的误差，尤其是在较小的缓存中。选择合适的块大小是一个复杂的权衡，取决于缓存的大小和罚款。

2. _Bigger caches to reduce miss rate_—The obvious way to reduce capacity misses is to increase cache capacity. Drawbacks include potentially longer hit time of the larger cache memory and higher cost and power. Larger caches increase both static and dynamic power.

> 2. _bigger 缓存以降低错过率_-减少容量失误的明显方法是增加缓存能力。缺点包括较大的缓存内存的可能更长的命中时间以及更高的成本和功率。较大的缓存增加了静态和动态功率。

3. _Higher associativity to reduce miss rate_—Obviously, increasing associativity reduces conflict misses. Greater associativity can come at the cost of increased hit time. As we will see shortly, associativity also increases power consumption.

> 3. _更紧密的关联性降低失误率_-明显地增加关联会减少冲突的失误。更大的关联性可以以增加的命中时间为代价。正如我们将不久的将来，关联也会增加功耗。

4. _Multilevel caches to reduce miss penalty_—A difficult decision is whether to make the cache hit time fast, to keep pace with the high clock rate of processors, or to make the cache large to reduce the gap between the processor accesses and main memory accesses. Adding another level of cache between the original cache and memory simplifies the decision. The first-level cache can be small enough to match a fast clock cycle time, yet the second-level (or third-level) cache can be large enough to capture many accesses that would go to main memory. The focus on misses in second-level caches leads to larger blocks, bigger capacity, and higher associativity. Multilevel caches are more power-efficient than a single aggregate cache. If L1 and L2 refer, respectively, to firstand second-level caches, we can redefine the average memory access time:

> 4. \_ Multilevel 缓存以减少罚款小姐 - 一个艰难的决定是使缓存时间快速播放时间，以保持高时钟的加快处理器速率，还是使高速缓存较大以减少处理器访问和主内存之间的差距访问。在原始缓存和内存之间添加另一个级别的缓存层，简化了决策。第一级缓存可能足够小，可以匹配快速的时钟周期时间，但是第二级（或第三级）缓存可能足够大，可以捕获许多可以进入主内存的访问。对二级缓存的失误的重点会导致更大的块，更大的容量和更高的关联性。多级缓存比单个聚合缓存更强大。如果 L1 和 L2 分别指第一和第二级缓存，我们可以重新定义平均内存访问时间：

5. _Giving priority to read misses over writes to reduce miss penalty_—A write buffer is a good place to implement this optimization. Write buffers create hazards because they hold the updated value of a location needed on a read miss— that is, a read-after-write hazard through memory. One solution is to check the contents of the write buffer on a read miss. If there are no conflicts, and if the memory system is available, sending the read before the writes reduces the miss penalty. Most processors give reads priority over writes. This choice has little effect on power consumption.

> 5. _ giving 优先级阅读《错过》写作以减少罚款_-写作缓冲区是实施此优化的好地方。写缓冲区会造成危险，因为它们具有读取失误所需的位置的更新价值，即通过记忆来读取后的危险。一种解决方案是检查读取失误上的写缓冲区的内容。如果没有冲突，并且如果有内存系统可用，则在撰写书写之前发送读取量会减少罚款。大多数处理器都将读取优先级优于写入。这种选择对功耗几乎没有影响。

6. _Avoiding address translation during indexing of the cache to reduce hit time_— Caches must cope with the translation of a virtual address from the processor to a physical address to access memory. (Virtual memory is covered in [Sections 2.4](#virtual-memory-and-virtual-machines) and B.4.) A common optimization is to use the page offset—the part that is identical in both virtual and physical addresses—to index the cache, as described in [Appendix B](#_bookmark436), page B.38. This virtual index/physical tag method introduces some system complications and/or limitations on the size and structure of the L1 cache, but the advantages of removing the translation lookaside buffer (TLB) access from the critical path outweigh the disadvantages.

> 6. _ vavoiding 地址翻译在缓存索引期间以减少命中时间_-缓存必须应对从处理器到物理地址的虚拟地址转换到访问内存。（虚拟内存涵盖[第 2.4 节]（#Virtual-Memory and-virtual-Machines）和 B.4。）一个常见的优化是使用页面偏移量，该部分在虚拟和物理地址中相同的部分 - 如[附录 B]中所述（#\_ Bookmark436），第 B.38 页。此虚拟索引/物理标签方法引入了一些系统并发症和/或限制 L1 缓存的大小和结构，但是从关键路径删除翻译 LookAside Buffer（TLB）访问的优势超过了缺点。

Note that each of the preceding six optimizations has a potential disadvantage that can lead to increased, rather than decreased, average memory access time.

> 请注意，前面的六个优化中的每个优化都有潜在的劣势，可能导致增加而不是减少平均内存访问时间。

The rest of this chapter assumes familiarity with the preceding material and the details in [Appendix B](#_bookmark436). In the “Putting It All Together” section, we examine the memory hierarchy for a microprocessor designed for a high-end desktop or smaller server, the Intel Core i7 6700, as well as one designed for use in a PMD, the Arm Cortex-53, which is the basis for the processor used in several tablets and smartphones. Within each of these classes, there is a significant diversity in approach because of the intended use of the computer.

> 本章的其余部分假设了[附录 B]中的前面材料和细节（#\_ bookmark436）。在“将所有内容放在一起”部分中，我们检查了专为高端台式机或较小服务器设计的微处理器的内存层次结构，即 Intel Core i7 6700，以及用于在 PMD 中使用的一个，ARM Cortex-53，这是几片和智能手机中使用的处理器的基础。在这些类别的每个类别中，由于计算机的预定用途，方法都有很大的多样性。

Although the i7 6700 has more cores and bigger caches than the Intel processors designed for mobile uses, the processors have similar architectures. A processor designed for small servers, such as the i7 6700, or larger servers, such as the Intel Xeon processors, typically is running a large number of concurrent processes, often for different users. Thus memory bandwidth becomes more important, and these processors offer larger caches and more aggressive memory systems to boost that bandwidth.

> 尽管 i7 6700 比为移动用途设计的英特尔处理器具有更多的内核和更大的缓存，但处理器具有相似的体系结构。专为小型服务器设计的处理器，例如 i7 6700 或较大的服务器，例如英特尔 Xeon 处理器，通常是针对不同用户的大量并发流程。因此，内存带宽变得越来越重要，这些处理器提供了更大的缓存和更具侵略性的内存系统，以增强带宽。

In contrast, PMDs not only serve one user but generally also have smaller operating systems, usually less multitasking (running of several applications simultaneously), and simpler applications. PMDs must consider both performance and energy consumption, which determines battery life. Before we dive into more advanced cache organizations and optimizations, one needs to understand the various memory technologies and how they are evolving.

> 相比之下，PMD 不仅为一个用户服务，而且通常具有较小的操作系统，通常多任务处理（同时运行多个应用程序）和更简单的应用程序。PMD 必须考虑性能和能耗，这决定电池寿命。在我们深入研究更高级的缓存组织和优化之前，人们需要了解各种记忆技术及其发展方式。
