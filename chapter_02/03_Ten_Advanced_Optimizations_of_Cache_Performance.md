## Ten Advanced Optimizations of Cache Performance

> ##缓存性能的十个高级优化

The preceding average memory access time formula gives us three metrics for cache optimizations: hit time, miss rate, and miss penalty. Given the recent trends, we add cache bandwidth and power consumption to this list. We can classify the 10 advanced cache optimizations we examine into five categories based on these metrics:

> 前面的平均内存访问时间公式为我们提供了三个用于缓存优化的指标：命中时间，错过率和罚款。鉴于最近的趋势，我们将缓存带宽和功耗添加到此列表中。我们可以根据这些指标对我们研究的 10 种高级缓存优化进行分类：

1. _Reducing the hit time_—Small and simple first-level caches and way-prediction. Both techniques also generally decrease power consumption.

> 1. _重新记录了命中时间_-小且简单的第一级缓存和出路预测。这两种技术通常还会降低功耗。

2. _Increasing cache bandwidth_—Pipelined caches, multibanked caches, and non- blocking caches. These techniques have varying impacts on power consumption.

> 2. _increasing Cache Bandwidth_-铺设缓存，多轴车牌和非阻止缓存。这些技术对功耗有不同的影响。

3. _Reducing the miss penalty_—Critical word first and merging write buffers. These optimizations have little impact on power.

> 3. _ Quall the Punnty_-首先关键单词并合并写缓冲区。这些优化对功率几乎没有影响。

4. _Reducing the miss rate_—Compiler optimizations. Obviously any improvement at compile time improves power consumption.

> 4. _重新降低了率_-计算机优化。显然，在编译时间的任何改进都可以改善功耗。

5. _Reducing the miss penalty or miss rate via parallelism_—Hardware prefetching and compiler prefetching. These optimizations generally increase power con- sumption, primarily because of prefetched data that are unused.

> 5. *通过 ParalleleSism *降低了罚款或错过的费率 - 预取预约和编译器预取。这些优化通常会增加功率吸引力，这主要是由于未使用的预取数据。

In general, the hardware complexity increases as we go through these optimi- zations. In addition, several of the optimizations require sophisticated compiler technology, and the final one depends on HBM. We will conclude with a summary of the implementation complexity and the performance benefits of the 10 tech- niques presented in [Figure 2.18](#_bookmark68) on page 113. Because some of these are straight- forward, we cover them briefly; others require more description.

> 通常，随着我们进行这些优化，硬件复杂性会增加。此外，一些优化需要复杂的编译器技术，最后一个优化取决于 HBM。我们将以第 113 页的[图 2.18](#\_ bookmark68)中提出的 10 个技术的绩效好处的摘要以及第 113 页中提出的 10 个技术的性能好处。其他需要更多描述。

### First Optimization: Small and Simple First-Level Caches to Reduce Hit Time and Power

> ###首次优化：小而简单的第一级缓存，以减少命中时间和力量

The pressure of both a fast clock cycle and power limitations encourages limited size for first-level caches. Similarly, use of lower levels of associativity can reduce both hit time and power, although such trade-offs are more complex than those involving size.

> 快速时钟周期和功率限制的压力促进了一级缓存的尺寸有限。同样，使用较低的关联性可以减少命中时间和功率，尽管这种权衡比涉及大小的权衡更为复杂。

The critical timing path in a cache hit is the three-step process of addressing the tag memory using the index portion of the address, comparing the read tag value to the address, and setting the multiplexor to choose the correct data item if the cache is set associative. Direct-mapped caches can overlap the tag check with the transmis- sion of the data, effectively reducing hit time. Furthermore, lower levels of associa- tivity will usually reduce power because fewer cache lines must be accessed.

> 缓存点击中的关键时序路径是使用地址的索引部分解决标签内存的三步过程，将读取标签值与地址进行比较，并设置多路复用器以选择正确的数据项，如果缓存为设置协会。直接映射的缓存可以与数据的传输重叠，从而有效地减少了命中时间。此外，由于必须访问更少的高速缓存线，因此较低的关联率通常会降低功率。

Although the total amount of on-chip cache has increased dramatically with new generations of microprocessors, because of the clock rate impact arising from a larger L1 cache, the size of the L1 caches has recently increased either slightly or not at all. In many recent processors, designers have opted for more associativity rather than larger caches. An additional consideration in choosing the associativity is the possibility of eliminating address aliases; we discuss this topic shortly.

> 尽管新一代的微处理器的片上缓存总量急剧增加，但由于较大的 L1 缓存产生的时钟速率影响，L1 缓存的大小最近略有或根本没有增加。在许多最近的处理器中，设计师选择了更多的关联性，而不是更大的缓存。选择关联性的另一个考虑因素是消除地址别名的可能性。我们很快讨论了这个话题。

One approach to determining the impact on hit time and power consumption in advance of building a chip is to use CAD tools. CACTI is a program to estimate the access time and energy consumption of alternative cache structures on CMOS microprocessors within 10% of more detailed CAD tools. For a given minimum feature size, CACTI estimates the hit time of caches as a function of cache size, associativity, number of read/write ports, and more complex parameters. [Figure 2.8](#_bookmark58) shows the estimated impact on hit time as cache size and associativity are varied. Depending on cache size, for these parameters, the model suggests that the hit time for direct mapped is slightly faster than two-way set associative and that two-way set associative is 1.2 times as fast as four-way and four-way is 1.4 times as fast as eight-way. Of course, these estimates depend on technology as well as the size of the cache, and CACTI must be carefully aligned with the technology; [Figure 2.8](#_bookmark58) shows the relative tradeoffs for one technology.

> 在构建芯片之前，确定对命中时间和功耗的影响的一种方法是使用 CAD 工具。仙人掌是一项旨在估算 CMOS 微处理器上替代缓存结构的访问时间和能源消耗的程序，占更详细的 CAD 工具的 10％。对于给定的最小特征大小，仙人掌估计缓存时间的命中时间是缓存大小，关联性，读取/写入端口的数量和更复杂的参数。[图 2.8](#_ bookmark58)显示了对命中时间的估计影响，因为缓存大小和关联性变化。根据缓存大小，对于这些参数，该模型表明，直接映射的命中时间略高于双向设置的关联，并且双向集合的关联速度为 1.2 倍，是四向，四向为 1.4 次数快到八路。当然，这些估计取决于技术以及缓存的大小，仙人掌必须与技术仔细合并。[图 2.8](#_ bookmark58)显示了一项技术的相对权衡。

> Figure 2.8 Relative access times generally increase as cache size and associativity are increased. These data come from the CACTI model 6.5 by [Tarjan et al. (2005)](#_bookmark1010). The data assume typical embedded SRAM technology, a single bank, and 64-byte blocks. The assumptions about cache layout and the complex trade-offs between inter- connect delays (that depend on the size of a cache block being accessed) and the cost of tag checks and multiplexing lead to results that are occasionally surprising, such as the lower access time for a 64 KiB with two-way set associativity versus direct mapping. Sim- ilarly, the results with eight-way set associativity generate unusual behavior as cache size is increased. Because such observations are highly dependent on technology and detailed design assumptions, tools such as CACTI serve to reduce the search space. These results are relative; nonetheless, they are likely to shift as we move to more recent and denser semiconductor technologies.

>> 图 2.8 相对访问时间通常会随着缓存大小和关联的增加而增加。这些数据来自[Tarjan 等人的 CACTI Model 6.5。(2005)](#\_ Bookmark1010)。该数据假设典型的嵌入式 SRAM 技术，一个银行和 64 个字节块。有关缓存布局和相互连接延迟之间的复杂权衡的假设(这取决于访问缓存块的大小)和标签检查和多重型的成本导致结果偶尔令人惊讶的结果，例如较低的访问带有双向设置关联性与直接映射的 64 KIB 的时间。同样，随着缓存大小的增加，具有八个路关联的结果会产生异常的行为。由于这些观察结果高度依赖于技术和详细的设计假设，因此诸如仙人掌之类的工具可以减少搜索空间。这些结果是相对的。尽管如此，随着我们转向最近和更密集的半导体技术，它们可能会转移。
>>

Example Using the data in Figure B.8 in [Appendix B](#_bookmark436) and [Figure 2.8](#_bookmark58), determine whether a 32 KiB four-way set associative L1 cache has a faster memory access time than a 32 KiB two-way set associative L1 cache. Assume the miss penalty to L2 is 15 times the access time for the faster L1 cache. Ignore misses beyond L2. Which has the faster average memory access time?

> 示例使用图 B.8 中的数据[附录 B](#_ bookmark436)和[图 2.8](#_ bookmark58)，确定 32 kiB 四向套件的关联 L1 缓存是否比 32 KIB 更快双向套件关联 L1 缓存。假设对 L2 的罚款是更快的 L1 缓存的访问时间的 15 倍。忽略超越 L2 的错过。哪个平均内存访问时间更快？

> ===

>> ===
>>

Clearly, the higher associativity looks like a bad trade-off; however, because cache access in modern processors is often pipelined, the exact impact on the clock cycle time is difficult to assess.

> 显然，更高的关联性看起来像是一个不良的权衡。但是，由于现代处理器中的缓存访问通常是管道的，因此很难评估对时钟周期时间的确切影响。

Energy consumption is also a consideration in choosing both the cache size and associativity, as [Figure 2.9](#_bookmark59) shows. The energy cost of higher associativity ranges from more than a factor of 2 to negligible in caches of 128 or 256 KiB when going from direct mapped to two-way set associative.

> 如[图 2.9](#\_ bookmark59)所示，能源消耗也是选择缓存大小和关联性的考虑因素。从直接映射到双向套件的关联时，较高的关联性的能源成本范围从超过 2 倍到 128 或 256 KIB 的缓存。

As energy consumption has become critical, designers have focused on ways to reduce the energy needed for cache access. In addition to associativity, the other key factor in determining the energy used in a cache access is the number of blocks in the cache because it determines the number of “rows” that are accessed. A designer could reduce the number of rows by increasing the block size (holding total cache size constant), but this could increase the miss rate, especially in smaller L1 caches.

> 随着能源消耗变得至关重要，设计师专注于减少缓存访问所需的能量的方法。除了关联性外，确定缓存访问中使用的能量的另一个关键因素是缓存中的块数量，因为它确定了访问的“行”数量。设计师可以通过增加块大小(保持总缓存大小恒定)来减少行数，但这可能会增加错过率，尤其是在较小的 L1 缓存中。

> Figure 2.9 Energy consumption per read increases as cache size and associativity are increased. As in the previous figure, CACTI is used for the modeling with the same technology parameters. The large penalty for eight-way set associative caches is due to the cost of reading out eight tags and the corresponding data in parallel.

>> 图 2.9，随着缓存大小和关联的增加，每读的能耗增加。与上图一样，仙人掌用于具有相同技术参数的建模。八方设置的关联缓存的巨大惩罚是由于读取八个标签的成本以及并行的相应数据。
>>

An alternative is to organize the cache in banks so that an access activates only a portion of the cache, namely the bank where the desired block resides. The primary use of multibanked caches is to increase the bandwidth of the cache, an optimization we consider shortly. Multibanking also reduces energy because less of the cache is accessed. The L3 caches in many multicores are logically uni- fied, but physically distributed, and effectively act as a multibanked cache. Based on the address of a request, only one of the physical L3 caches (a bank) is actually accessed. We discuss this organization further in [Chapter 5](#_bookmark213).

> 另一种选择是组织银行中的缓存，以便访问仅激活缓存的一部分，即所需块所在的银行。多轴线缓存的主要用途是增加缓存的带宽，这是我们不久考虑的优化。多键也可以减少能源，因为访问较少的缓存。许多多具的 L3 缓存在逻辑上是独立的，但物理分布，并有效地充当多型轴的缓存。根据请求的地址，实际上只访问了一个物理 L3 缓存(银行)。我们在[第 5 章](#\_ bookmark213)中进一步讨论该组织。

In recent designs, there are three other factors that have led to the use of higher associativity in first-level caches despite the energy and access time costs. First, many processors take at least 2 clock cycles to access the cache and thus the impact of a longer hit time may not be critical. Second, to keep the TLB out of the critical path (a delay that would be larger than that associated with increased associativity), almost all L1 caches should be virtually indexed. This limits the size of the cache to the page size times the associativity because then only the bits within the page are used for the index. There are other solutions to the problem of indexing the cache before address translation is completed, but increasing the associativity, which also has other benefits, is the most attractive. Third, with the introduction of multi- threading (see [Chapter 3](#_bookmark93)), conflict misses can increase, making higher associativity more attractive.

> 在最近的设计中，尽管有精力和访问时间成本，但还有其他三个因素导致在第一级缓存中使用更高的关联性。首先，许多处理器至少需要 2 个时钟循环来访问缓存，因此更长的命中时间的影响可能并不重要。其次，为了将 TLB 排除在临界路径之外(延迟大于与关联性增加相关的延迟)，几乎所有 L1 缓存都应实际上被索引。这将缓存的大小限制为页面大小乘以关联性，因为这样的索引只有页面中的位。在地址翻译完成之前，还有其他解决缓存问题的解决方案，但是增加的关联性(也具有其他好处)是最有吸引力的。第三，随着多线程的引入(请参阅[第 3 章](#\_ bookmark93))，冲突错过可以增加，使更高的关联性更具吸引力。

### Second Optimization: Way Prediction to Reduce Hit Time

> ###第二优化：减少命中时间的预测

Another approach reduces conflict misses and yet maintains the hit speed of direct- mapped cache. In _way prediction_, extra bits are kept in the cache to predict the way (or block within the set) of the _next_ cache access. This prediction means the mul- tiplexor is set early to select the desired block, and in that clock cycle, only a single tag comparison is performed in parallel with reading the cache data. A miss results in checking the other blocks for matches in the next clock cycle.

> 另一种方法减少了冲突的失误，但仍保持直接映射缓存的命中速度。在 *way 预测*中，将额外的位保留在缓存中，以预测 *next* 缓存访问的方式(或在集合内)。该预测意味着要尽早设置 mul-tiplexor 以选择所需的块，在该时钟周期中，仅与读取缓存数据并行执行单个标签比较。错过会导致检查其他块中的其他块中的匹配项。

Added to each block of a cache are block predictor bits. The bits select which of the blocks to try on the next cache access. If the predictor is correct, the cache access latency is the fast hit time. If not, it tries the other block, changes the way predictor, and has a latency of one extra clock cycle. Simulations suggest that set prediction accuracy is in excess of 90% for a two-way set associative cache and 80% for a four-way set associative cache, with better accuracy on I-caches than D-caches. Way prediction yields lower average memory access time for a two- way set associative cache if it is at least 10% faster, which is quite likely. Way prediction was first used in the MIPS R10000 in the mid-1990s. It is popular in processors that use two-way set associativity and was used in several ARM pro- cessors, which have four-way set associative caches. For very fast processors, it may be challenging to implement the one-cycle stall that is critical to keeping the way prediction penalty small.

> 在缓存的每个块中添加的是块预测器位。钻头选择下一个缓存访问中要尝试的块中的哪个块。如果预测变量是正确的，则缓存访问延迟是快速命中时间。如果没有，它会尝试另一个块，更改预测变量，并具有一个额外的时钟周期的延迟。模拟表明，对于双向集合缓存，设置的预测准确性超过 90％，对于四向组合的关联缓存，I-Caches 的精度比 D-Caches 更好。方式预测会产生较低的平均内存访问时间，如果速度至少要速度至少 10％，则两种相关缓存的时间很可能。1990 年代中期的 MIPS R10000 首次使用 Way 预测。它在使用双向设置关联性的处理器中很受欢迎，并在具有四向套件的关联缓存中使用。对于非常快速的处理器而言，实施单周期摊位对于保持预测罚款至关重要，这可能是一项挑战。

An extended form of way prediction can also be used to reduce power con- sumption by using the way prediction bits to decide which cache block to actually

> 通过使用预测位来确定实际上要哪个缓存块的方式，也可以使用一种扩展方式预测的方式来减少功率。

access (the way prediction bits are essentially extra address bits); this approach, which might be called way selection, saves power when the way prediction is cor- rect but adds significant time on a way misprediction, because the access, not just the tag match and selection, must be repeated. Such an optimization is likely to make sense only in low-power processors. Inoue et al. (1999) estimated that using the way selection approach with a four-way set associative cache increases the average access time for the I-cache by 1.04 and for the D-cache by 1.13 on the SPEC95 benchmarks, but it yields an average cache power consumption relative to a normal four-way set associative cache that is 0.28 for the I-cache and 0.35 for the D-cache. One significant drawback for way selection is that it makes it difficult to pipeline the cache access; however, as energy concerns have mounted, schemes that do not require powering up the entire cache make increasing sense.

> 访问(预测位本质上是额外的地址位)；这种方法可能被称为“选择”，可以在预测的方式上节省电源，但在错误预测的方式上增加了大量时间，因为必须重复访问，而不仅仅是标签匹配和选择。这样的优化只有在低功率处理器中才有意义。Inoue 等。(1999 年)估计，使用四个方向的选择方法使用方法方法可以将 I-CACHE 的平均访问时间增加 1.04，而 D-CACHE 的平均访问时间则在 SPEC95 基准上提高 1.13，但它产生了平均缓存功率相对于正常的四向集合缓存的消耗量为 I-CACHE 0.28，D-CACHE 为 0.35。选择方法的一个重要缺点是，它使得很难管道缓存访问。但是，随着能源的关注，不需要为整个缓存提供电源的方案具有越来越多的意义。

Example Assume that there are half as many D-cache accesses as I-cache accesses and that the I-cache and D-cache are responsible for 25% and 15% of the processor’s power consumption in a normal four-way set associative implementation. Determine if way selection improves performance per watt based on the estimates from the preceding study.

> 例如，假设在正常的四向集合实现中，i-CACHE 和 D-CACHE 的 D-CACHE 访问的一半与 I-CACHE 访问一样多，而 I-Cache 和 D-Cache 则负责处理器的 25％和 15％。确定根据上一项研究的估计值确定选择方法是否改善每瓦的性能。

_Answer_ For the I-cache, the savings in power is 25 0.28 0.07 of the total power, while for the D-cache it is 15 0.35 0.05 for a total savings of 0.12. The way prediction version requires 0.88 of the power requirement of the standard four-way cache. The increase in cache access time is the increase in I-cache average access time plus one-half the increase in D-cache access time, or 1.04 + 0.5 0.13 1.11 times lon- ger. This result means that way selection has 0.90 of the performance of a standard four-way cache. Thus way selection improves performance per joule very slightly by a ratio of 0.90/0.88 1.02. This optimization is best used where power rather than performance is the key objective.

> *answer* 对于 I-CACHE，节省的功率为 25 0.28 0.07，而对于 D-CACHE，其总节省为 0.12，为 15 0.35 0.05。预测版本需要标准四向缓存的功率要求的 0.88。高速缓存访问时间的增加是 I-CACHE 平均访问时间的增加以及 D-CACHE 访问时间的增加一半，即 1.04 + 0.5 0.13 1.11 1.11 倍 longer。该结果意味着选择的选择具有标准四向缓存的 0.90 个性能。因此，选择的方式可通过 0.90/0.88 1.02 的比率略微提高每个焦耳的性能。在功率而不是性能的情况下，最好使用这种优化。

### Third Optimization: Pipelined Access and Multibanked Caches to Increase Bandwidth

> ###第三优化：管道访问和多型轴的缓存以增加带宽

These optimizations increase cache bandwidth either by pipelining the cache access or by widening the cache with multiple banks to allow multiple accesses per clock; these optimizations are the dual to the superpipelined and superscalar approaches to increasing instruction throughput. These optimizations are primarily targeted at L1, where access bandwidth constrains instruction throughput. Multiple banks are also used in L2 and L3 caches, but primarily as a power-management technique.

> 这些优化可以通过管道访问缓存访问或使用多个银行宽扩大缓存以允许每个时钟允许多次访问来增加缓存带宽；这些优化是超级公开和超大标准方法的双重方法，以增加指导吞吐量。这些优化主要针对的是 L1，其中访问带宽会约束指令吞吐量。L2 和 L3 缓存中还使用了多个银行，但主要用作一种电力管理技术。

> Figure 2.10 Four-way interleaved cache banks using block addressing. Assuming 64 bytes per block, each of these addresses would be multiplied by 64 to get byte addressing.

>> 图 2.10 使用块地址的四向交织的缓存库。假设每个块有 64 个字节，这些地址中的每个地址都将乘以 64 个字节以获取字节地址。
>>

Pipelining L1 allows a higher clock cycle, at the cost of increased latency. For example, the pipeline for the instruction cache access for Intel Pentium processors in the mid-1990s took 1 clock cycle; for the Pentium Pro through Pentium III in the mid-1990s through 2000, it took 2 clock cycles; and for the Pentium 4, which became available in 2000, and the current Intel Core i7, it takes 4 clock cycles. Pipelining the instruction cache effectively increases the number of pipeline stages, leading to a greater penalty on mispredicted branches. Correspondingly, pipelining the data cache leads to more clock cycles between issuing the load and using the data (see [Chapter 3](#_bookmark93)). Today, all processors use some pipelining of L1, if only for the simple case of separating the access and hit detection, and many high-speed processors have three or more levels of cache pipelining.

> 管道 L1 允许更高的时钟周期，而延迟的成本增加。例如，1990 年代中期，英特尔奔腾处理器的指令缓存访问的管道访问了 1 个时钟周期；对于 1990 年代中期至 2000 年的奔驰 III 的奔腾者来说，它花了 2 个时钟周期。对于 2000 年可用的奔腾 4 和当前的英特尔核心 i7，它需要 4 个时钟周期。管道说明缓存有效地增加了管道阶段的数量，从而导致对错误预测的分支机构的罚款更大。相应地，管道数据缓存会导致发出负载和使用数据之间的更多时钟周期(请参阅[第 3 章](#\_ bookmark93))。如今，所有处理器都使用 L1 的管道衬里，如果仅用于分开访问和命中检测的简单情况，并且许多高速处理器具有三个或更多级别的高速缓存管道。

It is easier to pipeline the instruction cache than the data cache because the pro- cessor can rely on high performance branch prediction to limit the latency effects. Many superscalar processors can issue and execute more than one memory refer- ence per clock (allowing a load or store is common, and some processors allow multiple loads). To handle multiple data cache accesses per clock, we can divide the cache into independent banks, each supporting an independent access. Banks were originally used to improve performance of main memory and are now used inside modern DRAM chips as well as with caches. The Intel Core i7 has four banks in L1 (to support up to 2 memory accesses per clock).

> 管道比数据缓存更容易，因为专业人士可以依靠高性能分支预测来限制延迟效应。许多超量表处理器每个时钟都可以发出并执行多个内存参考(允许负载或存储是常见的，并且某些处理器允许多个负载)。要处理每个时钟的多个数据缓存访问，我们可以将缓存分为独立的银行，每个银行都支持独立访问。银行最初是用于提高主内存性能的，现在在现代 DRAM 芯片和缓存中使用。Intel Core i7 在 L1 中有四个银行(每个时钟最多支持 2 个内存访问)。

Clearly, banking works best when the accesses naturally spread themselves across the banks, so the mapping of addresses to banks affects the behavior of the memory system. A simple mapping that works well is to spread the addresses of the block sequentially across the banks, which is called _sequential interleaving_. For example, if there are four banks, bank 0 has all blocks whose address modulo 4 is 0, bank 1 has all blocks whose address modulo 4 is 1, and so on. [Figure 2.10](#_bookmark60) shows this interleaving. Multiple banks also are a way to reduce power consump- tion in both caches and DRAM.

> 显然，当访问自然地遍布银行时，银行业务效果最好，因此地址映射到银行会影响内存系统的行为。一个简单的映射效果很好，是依次跨银行传播块的地址，这称为*序列交流*。例如，如果有四个银行，则银行 0 的地址 MODULO 4 为 0，银行 1 的地址 MODULO 4 为 1，依此类推。[图 2.10](#\_ bookmark60)显示了这个交错。多个银行也是减少缓存和 DRAM 中功率构成的一种方式。

Multiple banks are also useful in L2 or L3 caches, but for a different reason. With multiple banks in L2, we can handle more than one outstanding L1 miss, if the banks do not conflict. This is a key capability to support nonblocking caches, our next optimization. The L2 in the Intel Core i7 has eight banks, while Arm Cortex processors have used L2 caches with 1–4 banks. As mentioned earlier, multibanking can also reduce energy consumption.

> 多个银行在 L2 或 L3 缓存中也很有用，但出于不同的原因。如果银行不冲突，我们可以在 L2 中的多家银行中处理多个未出色的 L1 失误。这是支持非阻止缓存的关键功能，这是我们的下一个优化。Intel Core i7 中的 L2 有八家银行，而 ARM Cortex 处理器使用了 1-4 个银行的 L2 缓存。如前所述，多现在也可以减少能源消耗。

### Fourth Optimization: Nonblocking Caches to Increase Cache Bandwidth

> ###第四优化：非块缓存以增加缓存带宽

For pipelined computers that allow out-of-order execution (discussed in [Chapter 3](#_bookmark93)), the processor need not stall on a data cache miss. For example, the processor could continue fetching instructions from the instruction cache while waiting for the data cache to return the missing data. A _nonblocking cache_ or _lockup-free cache_ esca- lates the potential benefits of such a scheme by allowing the data cache to continue to supply cache hits during a miss. This “hit under miss” optimization reduces the effective miss penalty by being helpful during a miss instead of ignoring the requests of the processor. A subtle and complex option is that the cache may further lower the effective miss penalty if it can overlap multiple misses: a “hit under multiple miss” or “miss under miss” optimization. The second option is beneficial only if the memory system can service multiple misses; most high-performance pro- cessors (such as the Intel Core processors) usually support both, whereas many lower-end processors provide only limited nonblocking support in L2.

> 对于允许排序执行的管道计算机(在[第 3 章](#* bookmark93)中进行了讨论)，处理器不需要停滞在数据缓存失误上。例如，处理器可以在等待数据缓存返回丢失的数据时继续从指令缓存获取指令。\_nonblocking cache *或* lockup free Cache*-通过允许数据缓存在错过期间继续提供缓存命中，从而带来了这种方案的潜在好处。这种“击中小姐”的优化可以通过在错过期间提供帮助而不是忽略处理器的要求来减少有效的罚款。一个微妙而复杂的选择是，如果缓存可以重叠多个错过，则可以进一步降低有效的罚款：“击中多个失误”或“小姐小姐”优化。仅当存储系统能够为多个失误服务时，第二个选项才是有益的。大多数高性能的专作(例如英特尔核心处理器)通常都支持两者，而许多低端处理器仅在 L2 中提供有限的非封锁支持。

To examine the effectiveness of nonblocking caches in reducing the cache miss penalty, [Farkas and Jouppi (1994)](#_bookmark947) did a study assuming 8 KiB caches with a 14-cycle miss penalty (appropriate for the early 1990s). They observed a reduction in the effective miss penalty of 20% for the SPECINT92 benchmarks and 30% for the SPECFP92 benchmarks when allowing one hit under miss.

> 为了检查非阻止缓存在减少缓存罚款中的有效性，[Farkas and Jouppi(1994)](#\_ bookmark947)进行了一项研究，假设有 8 个 KIB 缓存和 14 周期的罚款(适用于 1990 年代初)。他们观察到 SpecInt92 基准的有效罚款减少了 20％，而 SpecFP92 基准的 30％允许一击中一击。

[Li et al. (2011)](#_bookmark978) updated this study to use a multilevel cache, more modern assumptions about miss penalties, and the larger and more demanding SPECCPU2006 benchmarks. The study was done assuming a model based on a single core of an Intel i7 (see [Section 2.6](#_bookmark71)) running the SPECCPU2006 benchmarks. [Figure 2.11](#_bookmark61) shows the reduction in data cache access latency when allowing 1, 2, and 64 hits under a miss; the caption describes further details of the memory system. The larger caches and the addition of an L3 cache since the earlier study have reduced the benefits with the SPECINT2006 benchmarks showing an average reduction in cache latency of about 9% and the SPECFP2006 bench- marks about 12.5%.

> [Li 等。(2011)](#_ bookmark978)更新了这项研究，以使用多级缓存，关于罚款的更现代假设以及更大，更苛刻的 SpecCPU2006 基准。该研究是根据基于 Intel I7 的单个核心(请参阅[2.6](#_ bookmark71))的模型完成的。[图 2.11](#\_ bookmark61)显示了允许 1、2 和 64 次命中时数据缓存访问延迟的减少；标题描述了内存系统的更多详细信息。自早期研究以来，较大的缓存和添加了 L3 缓存，从而降低了 SpecInt2006 基准测试的好处，显示缓存潜伏期的平均降低约为 9％，SpecFP2006 基准标记约为 12.5％。

Example Which is more important for floating-point programs: two-way set associativity or hit under one miss for the primary data caches? What about integer programs? Assume the following average miss rates for 32 KiB data caches: 5.2% for floating-point programs with a direct-mapped cache, 4.9% for the programs with a two-way set associative cache, 3.5% for integer programs with a direct-mapped cache, and 3.2% for integer programs with a two-way set associative cache. Assume the miss penalty to L2 is 10 cycles, and the L2 misses and penalties are the same.

> 示例哪个对于浮点程序更重要：双向集合的关联性或在一个主要数据缓存下击中一个失误？整数程序呢？假设 32 KIB 数据缓存的以下平均损失率：带有直接映射缓存的浮点程序的 5.2％，具有双向设置的关联缓存的程序为 4.9％，整数程序的 3.5％，带直接映射缓存，用于带有双向套件的关联缓存的整数程序 3.2％。假设对 L2 的罚款是 10 个周期，而 L2 的错过和罚款是相同的。

> ===

>> ===
>>

> Figure 2.11 The effectiveness of a nonblocking cache is evaluated by allowing 1, 2, or 64 hits under a cache miss with 9 SPECINT (on the left) and 9 SPECFP (on the right) benchmarks. The data memory system modeled after the Intel i7 consists of a 32 KiB L1 cache with a four-cycle access latency. The L2 cache (shared with instructions) is 256 KiB with a 10-clock cycle access latency. The L3 is 2 MiB and a 36-cycle access latency. All the caches are eight-way set associative and have a 64-byte block size. Allowing one hit under miss reduces the miss penalty by 9% for the integer benchmarks and 12.5% for the floating point. Allowing a second hit improves these results to 10% and 16%, and allowing 64 results in little additional improvement.

>> 图 2.11 通过允许 1、2 或 64 命中在缓存误差下(左侧)(左侧)和 9 个 SpecFP(右侧)基准测试中的 1、2 或 64 命中来评估非阻止缓存的有效性。以 Intel I7 建模的数据存储系统由具有四周期访问延迟的 32 KIB L1 缓存组成。L2 缓存(与指令共享)为 256 KIB，具有 10 个锁定周期访问延迟。L3 是 2 个 MIB 和 36 周期访问延迟。所有的缓存都是八方集的关联，并具有 64 字节的块大小。允许在小姐身上进行一次命中，将小姐的罚款减少了 9％的整数基准，而浮点数为 12.5％。允许第二次命中将这些结果提高到 10％和 16％，而 64 个结果几乎没有进步。
>>

The cache access latency (including stalls) for two-way associativity is 0.49/0.52 or 94% of direct-mapped cache. [Figure 2.11](#_bookmark61) caption indicates that a hit under one miss reduces the average data cache access latency for floating-point programs to 87.5% of a blocking cache. Therefore, for floating-point programs, the direct-mapped data cache supporting one hit under one miss gives better perfor- mance than a two-way set-associative cache that blocks on a miss.

> 双向关联性的缓存访问延迟(包括摊位)为直接映射缓存的 0.49/0.52 或 94％。[图 2.11](#\_ bookmark61)标题表明，在一个遗漏下的命中率将浮点程序的平均数据缓存访问延迟减少到阻塞缓存的 87.5％。因此，对于浮点程序，与一个双向设置缔合性高速缓存相比，直接映射的数据缓存支撑一个命中率是一个命中率的击中，它可以更好地策略。

> ===

>> ===
>>

The data cache access latency of a two-way set associative cache is thus 0.32/0.35 or 91% of direct-mapped cache, while the reduction in access latency when allow- ing a hit under one miss is 9%, making the two choices about equal.

> 因此，双向集合缓存的数据缓存访问延迟为 0.32/0.35 或直接映射缓存的 91％，而允许在一个失误下允许命中率的访问延迟减少为 9％，做出了两种选择大约相等。

The real difficulty with performance evaluation of nonblocking caches is that a cache miss does not necessarily stall the processor. In this case, it is difficult to judge the impact of any single miss and thus to calculate the average memory access time. The effective miss penalty is not the sum of the misses but the nonoverlapped time that the processor is stalled. The benefit of nonblocking caches is complex, as it depends upon the miss penalty when there are multiple misses, the memory reference pattern, and how many instructions the processor can execute with a miss outstanding.

> 对非封锁缓存的性能评估的真正困难是，缓存失误不一定会拖延处理器。在这种情况下，很难判断任何单个失误的影响，从而计算平均内存访问时间。有效的罚款不是错过的总和，而是处理器停滞不前的未经封闭时间。非封锁缓存的好处很复杂，因为这取决于有多个错过，内存参考模式以及处理器可以用未偿还的失误执行的指令时，这取决于罚款。

In general, out-of-order processors are capable of hiding much of the miss penalty of an L1 data cache miss that hits in the L2 cache but are not capable of hiding a significant fraction of a lower-level cache miss. Deciding how many outstanding misses to support depends on a variety of factors:

> 通常，越常处理器能够掩盖撞击 L2 缓存的 L1 数据缓存失误的大部分罚款，但不能隐藏大量的低级缓存失误。确定支持多少未偿还的未来取决于各种因素：

- The temporal and spatial locality in the miss stream, which determines whether a miss can initiate a new access to a lower-level cache or to memory.

> - Miss 流中的时间和空间位置，该局部性确定错过是否可以启动对低级缓存或内存的新访问。

- The bandwidth of the responding memory or cache.

> - 响应内存或缓存的带宽。

- To allow more outstanding misses at the lowest level of the cache (where the miss time is the longest) requires supporting at least that many misses at a higher level, because the miss must initiate at the highest level cache.

> - 要允许在缓存的最低级别(错过时间是最长的地方)的更多未偿还的失误，需要至少支持许多较高级别的失误，因为失踪必须在最高级别的缓存中启动。

- The latency of the memory system.

> - 内存系统的延迟。

The following simplified example illustrates the key idea.

> 以下简化的示例说明了关键想法。

Example Assume a main memory access time of 36 ns and a memory system capable of a sustained transfer rate of 16 GiB/s. If the block size is 64 bytes, what is the maximum number of outstanding misses we need to support assuming that we can maintain the peak bandwidth given the request stream and that accesses never conflict. If the prob- ability of a reference colliding with one of the previous four is 50%, and we assume that the access has to wait until the earlier access completes, estimate the number of maximum outstanding references. For simplicity, ignore the time between misses.

> 示例假设一个 36 NS 的主内存访问时间和能够持续传输速率为 16 GIB/s 的内存系统。如果块大小为 64 个字节，则假设我们可以维持峰值带宽，并且访问永远不会发生冲突，那么我们需要支持的最大未命中率是多少。如果引用与前四个的碰撞的概率为 50％，并且我们假设访问必须等到较早的访问完成，请估算最大未偿还参考的数量。为简单起见，请忽略错过之间的时间。

_Answer_ In the first case, assuming that we can maintain the peak bandwidth, the memory system can support (16 10)<sup>9</sup>/64 250 million references per second. Because each reference takes 36 ns, we can support 250 10<sup>6</sup> 36 10<sup>—9</sup> 9 references.

> *answer* 在第一种情况下，假设我们可以维护峰带宽，则内存系统可以支持(16 10)<SUP> 9 </sup>/64 25000 万参考。因为每个参考需要 36 ns，所以我们可以支持 250 10 <sup> 6 </sup> 36 10 <sup> —9 </sup> 9 参考。

If the probability of a collision is greater than 0, then we need more outstanding ref-

> 如果碰撞的概率大于 0，那么我们需要更多出色的参考

erences, because we cannot start work on those colliding references; the memory system needs more independent references, not fewer! To approximate, we can sim- ply assume that half the memory references do not have to be issued to the memory. This means that we must support twice as many outstanding references, or 18.

> Erences，因为我们无法开始在那些冲突的参考资料上工作；内存系统需要更多独立的引用，而不是更少！大约要说，我们可以假设不必向内存发出一半的内存引用。这意味着我们必须支持两倍的杰出参考文献或 18 倍。

In Li, Chen, Brockman, and Jouppi’s study, they found that the reduction in CPI for the integer programs was about 7% for one hit under miss and about 12.7% for 64. For the floating-point programs, the reductions were 12.7% for one hit under miss and 17.8% for 64. These reductions track fairly closely the reductions in the data cache access latency shown in [Figure 2.11](#_bookmark61).

> 在 Li，Chen，Brockman 和 Jouppi 的研究中，他们发现 Integer 计划的 CPI 的减少约为 7％，一人受失误的命中率约为 12.7％。对于浮点计划，浮点计划的减少为 12.7％。对于一个命中率的命中率是一击，为 17.8％，为 64。这些减少轨迹相当接近[图 2.11](#\_ bookmark61)所示的数据缓存访问延迟中的减少。

##### _Implementing a Nonblocking Cache_

> ##### _实施非阻止缓存_

Although nonblocking caches have the potential to improve performance, they are nontrivial to implement. Two initial types of challenges arise: arbitrating conten- tion between hits and misses, and tracking outstanding misses so that we know when loads or stores can proceed. Consider the first problem. In a blocking cache, misses cause the processor to stall and no further accesses to the cache will occur until the miss is handled. In a nonblocking cache, however, hits can collide with misses returning from the next level of the memory hierarchy. If we allow multiple outstanding misses, which almost all recent processors do, it is even possible for misses to collide. These collisions must be resolved, usually by first giving priority to hits over misses, and second by ordering colliding misses (if they can occur). The second problem arises because we need to track multiple outstanding mis- ses. In a blocking cache, we always know which miss is returning, because only one can be outstanding. In a nonblocking cache, this is rarely true. At first glance, you might think that misses always return in order, so that a simple queue could be kept to match a returning miss with the longest outstanding request. Consider, however, a miss that occurs in L1. It may generate either a hit or miss in L2; if L2 is also nonblocking, then the order in which misses are returned to L1 will not necessarily be the same as the order in which they originally occurred. Multi- core and other multiprocessor systems that have nonuniform cache access times also introduce this complication.

> 尽管非封锁缓存有可能提高绩效，但它们是不平凡的。出现了两种初始类型的挑战：命中和错过之间的仲裁竞争，以及跟踪未偿还的错过，以便我们知道何时可以进行负载或商店。考虑第一个问题。在阻止缓存中，错过会导致处理器失速，并且在处理失误之前，将不再访问缓存。但是，在非阻止缓存中，命中可能会与从下一个内存层次结构级别返回的错过相撞。如果我们允许几乎所有最近的处理器都允许多次出色的失误，那么即使是失踪人员也可能会发生碰撞。这些碰撞必须得到解决，通常是首先优先考虑击中误差，其次是订购碰撞的错过(如果可能发生的话)。出现第二个问题是因为我们需要跟踪多个出色的错误。在阻止缓存中，我们始终知道哪个小姐正在返回，因为只有一个可以出色。在非封锁缓存中，这很少是正确的。乍一看，您可能会认为失踪总是按顺序返回，因此可以保留一个简单的队列，以与最长的未出色要求相匹配。但是，请考虑在 L1 中发生的错过。它可能会在 L2 中产生打击或错过。如果 L2 也是不封锁的，则返回 L1 的命令不一定与它们最初发生的顺序相同。具有不均匀缓存访问时间的多核和其他多处理器系统也引入了这种并发症。

When a miss returns, the processor must know which load or store caused the miss, so that instruction can now go forward; and it must know where in the cache the data should be placed (as well as the setting of tags for that block). In recent processors, this information is kept in a set of registers, typically called the _Miss Status Handling Registers (MSHRs)_. If we allow _n_ outstanding misses, there will be _n_ MSHRs, each holding the information about where a miss goes in the cache and the value of any tag bits for that miss, as well as the information indicating which load or store caused the miss (in the next chapter, you will see how this is tracked). Thus, when a miss occurs, we allocate an MSHR for handling that miss, enter the appropriate information about the miss, and tag the memory request with the index of the MSHR. The memory system uses that tag when it returns the data, allowing the cache system to transfer the data and tag information to the appropri- ate cache block and “notify” the load or store that generated the miss that the data is now available and that it can resume operation. Nonblocking caches clearly require extra logic and thus have some cost in energy. It is difficult, however, to assess their energy costs exactly because they may reduce stall time, thereby decreasing execution time and resulting energy consumption.

> 当错过返回时，处理器必须知道哪个负载或存储会导致错过，以便该指令现在可以继续前进；并且必须知道应将数据放置在哪里(以及该块的标签设置)。在最近的处理器中，此信息保存在一组寄存器中，通常称为* MISS 状态处理寄存器(MSHRS)*。如果我们允许* n *未偿命中率，将会有* n* mshrs，每个人都保留有关遗漏在缓存中的位置以及该错过的任何标签位的信息，以及指示哪个负载或存储的信息导致失误(在下一章中，您将看到如何跟踪)。因此，当错过发生时，我们将 MSHR 分配用于处理的 MSHR，输入有关错过的适当信息，并使用 MSHR 索引标记内存请求。内存系统在返回数据时使用该标签，允许缓存系统将数据和标记信息传输到适用于适当的缓存块，并“通知”生成失误的负载或存储数据，该数据现已可用，并且它可以恢复操作。非阻止库显然需要额外的逻辑，因此能源成本一定。但是，很难完全评估他们的能源成本，因为它们可能会减少失速时间，从而减少执行时间和产生的能耗。

In addition to the preceding issues, multiprocessor memory systems, whether within a single chip or on multiple chips, must also deal with complex implemen- tation issues related to memory coherency and consistency. Also, because cache mis- ses are no longer atomic (because the request and response are split and may be interleaved among multiple requests), there are possibilities for deadlock. For the interested reader, Section I.7 in online Appendix I deals with these issues in detail.

> 除了前面的问题外，多处理器内存系统(无论是在单个芯片中还是在多个芯片中)还必须处理与内存相干性和一致性有关的复杂插入问题。同样，由于缓存错误不再是原子质(因为请求和响应是分裂的，并且可能在多个请求之间交织)，因此有可能发生僵局。对于有兴趣的读者，在线附录 I 中的 I.7 节详细处理了这些问题。

### Fifth Optimization: Critical Word First and Early Restart to Reduce Miss Penalty

> ###第五优化：关键词首先和早期重新启动以减少罚款

This technique is based on the observation that the processor normally needs just one word of the block at a time. This strategy is impatience: don’t wait for the full

> 该技术基于这样的观察，即处理器通常一次只需要一个单词。这种策略是不耐烦的：不要等待

block to be loaded before sending the requested word and restarting the processor. Here are two specific strategies:

> 在发送请求的单词并重新启动处理器之前，要加载要加载的块。这是两个具体策略：

- _Critical word first_—Request the missed word first from memory and send it to the processor as soon as it arrives; let the processor continue execution while filling the rest of the words in the block.

> - _ critalital Word first _-首先从内存中重新征用错过的单词，并在其到达后立即将其发送给处理器；让处理器继续执行，同时填充块中的其余单词。

- _Early restart_—Fetch the words in normal order, but as soon as the requested word of the block arrives, send it to the processor and let the processor continue execution.

> - _arly restart _-以正常顺序获取单词，但是一旦块的请求单词到达，就将其发送给处理器，然后让处理器继续执行。

Generally, these techniques only benefit designs with large cache blocks because the benefit is low unless blocks are large. Note that caches normally continue to satisfy accesses to other blocks while the rest of the block is being filled.

> 通常，这些技术仅具有大量缓存块的设计，因为除非块很大，否则收益较低。请注意，在填充其余部分时，缓存通常会继续满足对其他块的访问。

However, given spatial locality, there is a good chance that the next reference is to the rest of the block. Just as with nonblocking caches, the miss penalty is not simple to calculate. When there is a second request in critical word first, the effec- tive miss penalty is the nonoverlapped time from the reference until the second piece arrives. The benefits of critical word first and early restart depend on the size of the block and the likelihood of another access to the portion of the block that has not yet been fetched. For example, for SPECint2006 running on the i7 6700, which uses early restart and critical word first, there is more than one reference made to a block with an outstanding miss (1.23 references on average with a range from 0.5 to 3.0). We explore the performance of the i7 memory hierarchy in more detail in [Section 2.6](#_bookmark71).

> 但是，给定空间位置，下一个参考很有可能是其余部分。就像非封锁缓存一样，错过的罚款也不容易计算。当关键单词首先是第二个请求时，有效的罚款是从参考文献到第二件作品到达的无封闭时间。临界单词的好处首先和早期重新启动取决于块的大小以及尚未获取块的另一部分访问部分的可能性。例如，对于在 i7 6700 上运行的 Specint2006，首先使用早期重新启动和关键单词，有多个引用对一个出色的失误的块(平均为 1.23 引用，范围为 0.5 至 3.0)。我们在[第 2.6 节](#\_ bookmark71)中更详细地探讨了 i7 内存层次结构的性能。

### Sixth Optimization: Merging Write Buffer to Reduce Miss Penalty

> ###第六个优化：合并写缓冲区以减少罚款

Write-through caches rely on write buffers, as all stores must be sent to the next lower level of the hierarchy. Even write-back caches use a simple buffer when a block is replaced. If the write buffer is empty, the data and the full address are written in the buffer, and the write is finished from the processor’s perspective; the processor continues working while the write buffer prepares to write the word to memory. If the buffer contains other modified blocks, the addresses can be checked to see if the address of the new data matches the address of a valid write buffer entry. If so, the new data are combined with that entry. _Write merging_ is the name of this optimization. The Intel Core i7, among many others, uses write merging.

> 写入缓存依赖写缓冲区，因为所有商店都必须发送到层次结构的下一个较低级别。当更换块时，甚至写下后的缓存也会使用简单的缓冲区。如果写缓冲区为空，则数据和完整地址写在缓冲区中，并且写入从处理器的角度完成；处理器在写缓冲区准备将单词写入内存时继续工作。如果缓冲区包含其他修改的块，则可以检查地址以查看新数据的地址是否与有效的写缓冲区条目的地址匹配。如果是这样，则将新数据与该条目结合使用。*write 合并*是此优化的名称。Intel Core i7，除其他等等》中使用写入合并。

If the buffer is full and there is no address match, the cache (and processor) must wait until the buffer has an empty entry. This optimization uses the memory more efficiently because multiword writes are usually faster than writes performed one word at a time. [Skadron and Clark (1997)](#_bookmark1004) found that even a merging four-entry write buffer generated stalls that led to a 5%–10% performance loss.

> 如果缓冲区已满并且没有地址匹配，则缓存(和处理器)必须等到缓冲区的空条目为止。该优化使用内存更有效地使用内存，因为多字写作通常比一次执行一个单词的写入要快。[Skadron 和 Clark(1997)](#\_ Bookmark1004)发现，即使是合并的四项写入缓冲液产生的摊位，导致了 5％–10％的性能损失。

> Figure 2.12 In this illustration of write merging, the write buffer on top does not use write merging while the write buffer on the bottom does. The four writes are merged into a single buffer entry with write merging; without it, the buffer is full even though three-fourths of each entry is wasted. The buffer has four entries, and each entry holds four 64-bit words. The address for each entry is on the left, with a valid bit (V) indicating whether the next sequential 8 bytes in this entry are occupied. (Without write merging, the words to the right in the upper part of the figure would be used only for instructions that wrote multiple words at the same time.)

>> 图 2.12 在此写入合并的说明中，顶部的写缓冲区在底部的写缓冲区时不使用写入合并。与写入合并，将四个写入合并为单个缓冲区条目。没有它，即使浪费了每个条目的四分之三，缓冲区也已满。缓冲区有四个条目，每个条目都包含四个 64 位单词。每个条目的地址在左侧，有效的位(v)指示该条目中的下一个顺序 8 字节是否被占据。(没有写入合并，该图上部的右侧单词仅用于同时写多个单词的说明。)
>>

The optimization also reduces stalls because of the write buffer being full. [Figure 2.12](#_bookmark62) shows a write buffer with and without write merging. Assume we had four entries in the write buffer, and each entry could hold four 64-bit words. Without this optimization, four stores to sequential addresses would fill the buffer at one word per entry, even though these four words when merged fit exactly within a single entry of the write buffer.

> 由于写缓冲区已满，因此优化还会减少摊位。[图 2.12](#\_ bookmark62)显示了有或没有写入合并的写缓冲区。假设我们在写缓冲区中有四个条目，每个条目都可以容纳四个 64 位单词。如果没有这种优化，即使合并合并完全适合写缓冲区的单个条目时，四个商店到顺序地址将以每个条目的一个字填充缓冲区。

Note that input/output device registers are often mapped into the physical address space. These I/O addresses _cannot_ allow write merging because separate I/O registers may not act like an array of words in memory. For example, they may require one address and data word per I/O register rather than use multiword writes using a single address. These side effects are typically implemented by marking the pages as requiring nonmerging write through by the caches.

> 请注意，输入/输出设备寄存器通常被映射到物理地址空间中。这些 I/O 地址 *cannot* 允许写入合并，因为单独的 I/O 寄存器可能不像内存中的单词数组。例如，他们可能需要一个地址和数据字，每个地址和数据单词，而不是使用单个地址使用多词写作。这些副作用通常是通过标记页面来实现的，因为这些页面需要通过缓存的非标准写入。

### Seventh Optimization: Compiler Optimizations to Reduce Miss Rate

> ###第七优化：编译器优化以降低错过费率

Thus far, our techniques have required changing the hardware. This next technique reduces miss rates without any hardware changes.

> 到目前为止，我们的技术需要更改硬件。下一项技术会降低未经任何硬件更改的情况下的错率。

This magical reduction comes from optimized software—the hardware designer’s favorite solution! The increasing performance gap between processors and main memory has inspired compiler writers to scrutinize the memory hierarchy to see if compile time optimizations can improve performance. Once again, research is split between improvements in instruction misses and improvements in data mis- ses. The optimizations presented next are found in many modern compilers.

> 这种神奇的减少来自优化的软件 - 硬件设计师最喜欢的解决方案！处理器和主内存之间的性能差距不断增加，激发了编译器作家仔细检查内存层次结构，以查看编译时间优化是否可以提高性能。再一次，研究的改进教学错过和数据误差的改进之间进行了划分。接下来提出的优化是在许多现代编译器中找到的。

##### _Loop Interchange_

> ##### _loop Interchange_

Some programs have nested loops that access data in memory in nonsequential order. Simply exchanging the nesting of the loops can make the code access the data in the order in which they are stored. Assuming the arrays do not fit in the cache, this technique reduces misses by improving spatial locality; reordering max- imizes use of data in a cache block before they are discarded. For example, if x is a two-dimensional array of size \[5000,100\] allocated so that x\[i,j\] and x\[i, j + 1\] are adjacent (an order called row major because the array is laid out by rows), then the two pieces of the following code show how the accesses can be optimized:

> 一些程序具有嵌套的循环，可以按内存以非顺序访问数据。简单地交换循环的嵌套可以使代码以存储的顺序访问数据。假设阵列不适合缓存，则该技术通过改善空间位置来减少错过。在丢弃缓存块中最大化数据的最大使用。例如，如果 x 是分配的二维阵列\ [5000,100 \]，以便 x \ [i，j \]和 x \ [i，j + 1 \]相邻(称为行的订单，由于数组是按行布置的)，因此以下代码的两个部分显示如何优化访问：

> ===

>> ===
>>

The original code would skip through memory in strides of 100 words, while the revised version accesses all the words in one cache block before going to the next block. This optimization improves cache performance without affecting the num- ber of instructions executed.

> 原始代码将以 100 个单词的大步跳过内存，而修订版的版本在进入下一个块之前访问一个缓存块中的所有单词。这种优化可改善缓存性能，而不会影响执行的指令数量。

##### _Blocking_

> ##### _ blocking_

This optimization improves temporal locality to reduce misses. We are again deal- ing with multiple arrays, with some arrays accessed by rows and some by columns. Storing the arrays row by row (_row major order_) or column by column (_column major order_) does not solve the problem because both rows and columns are used in every loop iteration. Such orthogonal accesses mean that transformations such as loop interchange still leave plenty of room for improvement.

> 这种优化改善了时间的位置，以减少错过。我们再次处理多个数组，其中一些阵列由行访问，有些列来访问。通过行(_ROW MARIAD ORDER_)或列来存储阵列(_Column Major Order_)无法解决问题，因为每个环路迭代中都使用行和列。这种正交访问意味着诸如环路互换之类的转换仍然留出足够的改进空间。

> Figure 2.13 A snapshot of the three arrays x, y, and z when _N_ 56 and i 51. The age of accesses to the array elements is indicated by shade: white means not yet touched, light means older accesses, and dark means newer accesses. The elements of y and z are read repeatedly to calculate new elements of x. The variables i, j, and k are shown along the rows or columns used to access the arrays.

>> 图 2.13 _n_ 56 和 i 51 时三个阵列 x，y 和 z 的快照。访问阵列元素的访问年龄由阴影指示：白色表示尚未触摸，光表示较旧的访问，深色表示更新访问。Y 和 Z 的元素重复读取以计算 X 的新元素。变量 I，J 和 K 沿着用于访问数组的行或列显示。
>>

Instead of operating on entire rows or columns of an array, blocked algorithms operate on submatrices or _blocks_. The goal is to maximize accesses to the data loaded into the cache before the data are replaced. The following code example, which performs matrix multiplication, helps motivate the optimization:

> 阻塞算法无需在整个行或列上操作，而是在子膜或 *Blocks* 上操作。目的是在更换数据之前最大程度地访问加载到缓存中的数据。执行矩阵乘法的以下代码示例有助于激励优化：

> ===

>> ===
>>

The two inner loops read all N-by-N elements of z, read the same N elements in a row of y repeatedly, and write one row of N elements of x. [Figure 2.13](#_bookmark63) gives a snapshot of the accesses to the three arrays. A dark shade indicates a recent access, a light shade indicates an older access, and white means not yet accessed. The number of capacity misses clearly depends on N and the size of the cache.

> 两个内部循环读取 z 的所有 nby-n 元素，反复读取相同的 n 元素，然后写入 x 的 n 元素的一行。[图 2.13](#\_ bookmark63)给出了三个阵列的访问的快照。深色阴影表示最近的访问，浅色阴影表示较旧的访问，而白色表示尚未访问。容量遗漏的数量显然取决于 n 和缓存的大小。

If it can hold all three N-by-N matrices, then all is well, provided there are no cache conflicts. If the cache can hold one N-by-N matrix and one row of N, then at least the ith row of y and the array z may stay in the cache. Less than that and misses may occur for both x and z. In the worst case, there would be 2N<sup>3</sup> + N<sup>2</sup> memory words accessed for N<sup>3</sup> operations.

> 如果它可以容纳所有三个 n by-n 矩阵，那么只要没有缓存冲突，一切都很好。如果缓存可以容纳一个 n-n n 矩阵和一行 N，则至少 Y 的 ITH 行和数组 Z 可以保留在缓存中。少于 X 和 Z 可能会发生错过。在最坏的情况下，将有 2n <sup> 3 </sup> + n <sup> 2 </sup>访问 n <sup> 3 </sup>操作的内存单词。

To ensure that the elements being accessed can fit in the cache, the original code is changed to compute on a submatrix of size B by B. Two inner loops now compute in steps of size B rather than the full length of x and z. B is called the _blocking factor_. (Assume x is initialized to zero.)

> 为了确保所访问的元素可以适合缓存，更改了原始代码以计算 B 大小 B 上的 B 上 B 上 B 上 B 上的 B。两个内部循环现在以大小 B 的步骤而不是 X 和 Z 的全长为单位。b 称为* blocking 因子*。(假设 X 初始化为零。)

[Figure 2.14](#_bookmark64) illustrates the accesses to the three arrays using blocking. Looking only at capacity misses, the total number of memory words accessed is 2N<sup>3</sup>/B+N<sup>2</sup>. This total is an improvement by an approximate factor of B. Therefore blocking exploits a combination of spatial and temporal locality, because y benefits from spatial locality and z benefits from temporal locality. Although our example uses a square block (BxB), we could also use a rectangular block, which would be nec- essary if the matrix were not square.

> [图 2.14](#\_ bookmark64)说明了使用阻塞对三个数组的访问。仅查看容量失误，访问的内存单词总数为 2n <sup> 3 </sup>/b+n <Sup> 2 </sup>。该总数大约是 B 的大约改进。因此，阻止空间和时间位置的组合，因为 Y 受益于空间位置，而 Z 受益于时间位置。尽管我们的示例使用了方形块(BXB)，但我们也可以使用矩形块，如果矩阵不是正方形，这将是必需的。

Although we have aimed at reducing cache misses, blocking can also be used to help register allocation. By taking a small blocking size such that the block can be held in registers, we can minimize the number of loads and stores in the program. As we shall see in Section 4.8 of [Chapter 4](#_bookmark165), cache blocking is absolutely nec- essary to get good performance from cache-based processors running applications using matrices as the primary data structure.

> 尽管我们旨在减少缓存失误，但也可以使用阻止来帮助注册分配。通过摄入一个小的阻塞大小以使块可以保存在寄存器中，我们可以最大程度地减少程序中的负载和商店数量。正如我们将在[第 4 章](#\_ bookmark165)的第 4.8 节中看到的那样，缓存阻塞绝对是必不可少的，可以从基于缓存的处理器使用矩阵作为主要数据结构的基于缓存的处理器中获得良好的性能。

### Eighth Optimization: Hardware Prefetching of Instructions and Data to Reduce Miss Penalty or Miss Rate

> ###第八次优化：指令和数据的硬件预摘要，以减少罚款或错过率

Nonblocking caches effectively reduce the miss penalty by overlapping execution with memory access. Another approach is to prefetch items before the processor requests them. Both instructions and data can be prefetched, either directly into the caches or into an external buffer that can be more quickly accessed than main memory.

> 非阻止缓存可以通过与内存访问重叠的执行有效地减少罚款。另一种方法是在处理器请求之前预取项目。指令和数据都可以直接拿到缓存或外部缓冲区中，该缓冲区可以比主内存更快地访问。

> Figure 2.14 The age of accesses to the arrays x, y, and z when _B_ 53. Note that, in contrast to [Figure 2.13](#_bookmark63), a smaller number of elements is accessed.

>> 图 2.14 _b_ 53 时访问数组 x，y 和 z 的年龄。请注意，与[图 2.13](#\_ bookmark63)相比，访问较少数量的元素。
>>

Instruction prefetch is frequently done in hardware outside of the cache. Typically, the processor fetches two blocks on a miss: the requested block and the next consec- utive block. The requested block is placed in the instruction cache when it returns, and the prefetched block is placed in the instruction stream buffer. If the requested block is present in the instruction stream buffer, the original cache request is canceled, the block is read from the stream buffer, and the next prefetch request is issued.

> 指令预取通常在缓存外部的硬件中进行。通常，处理器在错过上获取两个块：请求的块和下一个 contive 块。当请求的块返回时，将其放置在指令缓存中，并将预取的块放在指令流缓冲区中。如果指令流缓冲区中存在请求的块，则取消原始缓存请求，从流缓冲区读取块，并发出下一个预取请求。

A similar approach can be applied to data accesses ([Jouppi, 1990](#_bookmark965)). [Palacharla](#_bookmark986) [and Kessler (1994)](#_bookmark986) looked at a set of scientific programs and considered multiple stream buffers that could handle either instructions or data. They found that eight stream buffers could capture 50%–70% of all misses from a processor with two 64 KiB four-way set associative caches, one for instructions and the other for data. The Intel Core i7 supports hardware prefetching into both L1 and L2 with the most common case of prefetching being accessing the next line. Some earlier Intel processors used more aggressive hardware prefetching, but that resulted in reduced performance for some applications, causing some sophisticated users to turn off the capability.

> 类似的方法可以应用于数据访问([Jouppi，1990](#_ bookmark965))。[palacharla](#_ bookmark986)[and Kessler(1994)](#\_ bookmark986)研究了一组科学程序，并考虑了可以处理指令或数据的多个流缓冲区。他们发现，八个流缓冲区可以捕获带有两个 64 KIB 四向套件的关联缓存的处理器中所有失误的 50％–70％，一个用于说明，另一个用于数据。Intel Core i7 支持将硬件预拿到 L1 和 L2，最常见的是预取访问下一行的最常见情况。一些较早的英特尔处理器使用了更具侵略性的硬件预取，但这导致某些应用程序的性能降低，从而导致一些复杂的用户关闭功能。

[Figure 2.15](#_bookmark65) shows the overall performance improvement for a subset of SPEC2000 programs when hardware prefetching is turned on. Note that this figure includes only 2 of 12 integer programs, while it includes the majority of the SPECCPU floating-point programs. We will return to our evaluation of prefetch- ing on the i7 in [Section 2.6](#_bookmark71).

> [图 2.15](#_ bookmark65)在打开硬件预取件时，显示了 Spec2000 程序子集的总体性能改进。请注意，该图仅包含 12 个整数程序中的 2 个，而其中包括大多数 SpecCPU 浮点程序。我们将返回[2.6](#_ bookmark71)中对 i7 预取的评估。

> Figure 2.15 Speedup because of hardware prefetching on Intel Pentium 4 with hardware prefetching turned on for 2 of 12 SPECint2000 benchmarks and 9 of 14 SPECfp2000 benchmarks. Only the programs that benefit the most from prefetching are shown; prefetching speeds up the missing 15 SPECCPU benchmarks by less than 15% (Boggs et al., 2004).

>> 图 2.15 加速由于在英特尔五角 4 上进行了硬件预取，硬件预取的开启了 12 个 Specint2000 基准中的 2 个和 14 个 SPECFP2000 基准中的 9 个。仅显示从预摘要中受益最大的程序；预摘要使缺少的 15 个 SpecCPU 基准速度不到 15％(Boggs 等，2004)。
>>

Prefetching relies on utilizing memory bandwidth that otherwise would be unused, but if it interferes with demand misses, it can actually lower performance. Help from compilers can reduce useless prefetching. When prefetching works well, its impact on power is negligible. When prefetched data are not used or useful data are displaced, prefetching will have a very negative impact on power.

> 预摘要依赖于利用内存带宽，否则将是未使用的，但是如果它会干扰需求错过，则实际上可以降低性能。编译器的帮助可以减少无用的预摘要。当预取良好效果时，其对功率的影响可以忽略不计。当未使用预取数据或有用的数据流离失所时，预取的预取会对功率产生非常负面的影响。

### Ninth Optimization: Compiler-Controlled Prefetching to Reduce Miss Penalty or Miss Rate

> ###第九优化：编译器控制的预取以减少罚款或错过率

An alternative to hardware prefetching is for the compiler to insert prefetch instruc- tions to request data before the processor needs it. There are two flavors of prefetch:

> 硬件预取的替代方法是使编译器插入预取指令，以便在处理器需要之前请求数据。预摘要有两种口味：

- _Register prefetch_ loads the value into a register.

> - *register prefetch* 将值加载到寄存器中。

- _Cache prefetch_ loads data only into the cache and not the register.

> - *cache Prefetch* 仅将数据加载到缓存而不将数据加载到寄存器中。

Either of these can be _faulting_ or _nonfaulting_; that is, the address does or does not cause an exception for virtual address faults and protection violations. Using this terminology, a normal load instruction could be considered a “faulting register prefetch instruction.” Nonfaulting prefetches simply turn into no-ops if they would normally result in an exception, which is what we want.

> 其中的任何一个都可以是 *faulting* 或 *nonfaulting*;也就是说，该地址确实或不引起虚拟地址错误和违反保护的例外。使用此术语，可以将正常的负载指令视为“故障寄存器预取指令”。如果通常会导致异常，那就是我们想要的，而不是折叠的预取。

The most effective prefetch is “semantically invisible” to a program: it doesn’t change the contents of registers and memory, _and_ it cannot cause virtual memory faults. Most processors today offer nonfaulting cache prefetches. This section assumes nonfaulting cache prefetch, also called _nonbinding_ prefetch.

> 最有效的预摘要是程序上的“语义上不可见”：它不会更改寄存器和内存的内容，*和*不会导致虚拟内存故障。如今，大多数处理器都提供非故障缓存预取。本节假设非故障缓存预取，也称为 *nonbinding* prefetch。

Prefetching makes sense only if the processor can proceed while prefetching the data; that is, the caches do not stall but continue to supply instructions and data while waiting for the prefetched data to return. As you would expect, the data cache for such computers is normally nonblocking.

> 仅当处理器可以在预取数据时进行进行预摘要才有意义。也就是说，在等待预取的数据返回的同时，缓存不会失速，而是继续提供说明和数据。正如您所期望的那样，此类计算机的数据缓存通常是不封锁的。

Like hardware-controlled prefetching, the goal is to overlap execution with the prefetching of data. Loops are the important targets because they lend themselves to prefetch optimizations. If the miss penalty is small, the compiler just unrolls the loop once or twice, and it schedules the prefetches with the execution. If the miss penalty is large, it uses software pipelining (see Appendix H) or unrolls many times to prefetch data for a future iteration.

> 像硬件控制的预摘要一样，目标是与数据的预摘要重叠。循环是重要的目标，因为它们借给预摘要的优化。如果罚款较小，则编译器只会展开一次或两次循环，并将执行时间安排为预取。如果罚款较大，它使用软件管道(请参阅附录 H)或展开很多次来预取数据以进行未来的迭代。

Issuing prefetch instructions incurs an instruction overhead, however, so com- pilers must take care to ensure that such overheads do not exceed the benefits. By concentrating on references that are likely to be cache misses, programs can avoid unnecessary prefetches while improving average memory access time significantly.

> 但是，发行预取指令会导致指示开销，因此，分校必须注意确保此类间接费用不会超过收益。通过专注于可能是缓存失误的参考文献，程序可以避免不必要的预取，同时大大改善平均内存访问时间。

Example For the following code, determine which accesses are likely to cause data cache misses. Next, insert prefetch instructions to reduce misses. Finally, calculate the number of prefetch instructions executed and the misses avoided by prefetching. Let’s assume we have an 8 KiB direct-mapped data cache with 16-byte blocks, and it is a write-back cache that does write allocate. The elements of a and b are 8 bytes long because they are double-precision floating-point arrays. There are 3 rows and 100 columns for a and 101 rows and 3 columns for b. Let’s also assume they are not in the cache at the start of the program.

> 以下代码的示例，确定哪些访问可能会导致数据缓存错过。接下来，插入预取指令以减少错过。最后，计算执行的预取指令的数量，并通过预摘要避免了错过。假设我们有一个具有 16 个字节块的 8 KIB 直接映射数据缓存，这是一个写入的缓存，确实写了分配。A 和 B 的元素长 8 个字节，因为它们是双精度的浮点阵列。A 和 101 行有 3 行和 100 列，b 有 3 列。我们还假设它们在程序开始时不在缓存中。

> ===

>> ===
>>

_Answer_ The compiler will first determine which accesses are likely to cause cache misses; otherwise, we will waste time on issuing prefetch instructions for data that would be hits. Elements of a are written in the order that they are stored in memory, so a will benefit from spatial locality: The even values of j will miss and the odd values will hit. Because a has 3 rows and 100 columns, its accesses will lead to 3 (100/2), or 150 misses.

> *answer *编译器将首先确定哪些访问可能会导致高速缓存。否则，我们将浪费时间发布有关命中的数据的预取指令。A 的元素是按照存储在内存中的顺序编写的，因此 A 将从空间位置受益：J 的均匀值会错过，并且奇数值将击中。由于 A 有 3 行和 100 列，因此其访问将导致 3(100/2)或 150 个失误。

The array b does not benefit from spatial locality because the accesses are not in the order it is stored. The array b does benefit twice from temporal locality: the same elements are accessed for each iteration of i, and each iteration of j uses the same value of b as the last iteration. Ignoring potential conflict misses, the misses because of b will be for b\[j + 1\]\[0\] accesses when i 0, and also the first access to b\[j\]\[0\] when j 0. Because j goes from 0 to 99 when i 0, accesses to b lead to 100 + 1, or 101 misses.

> 阵列 B 不会从空间区域中受益，因为访问不按其存储的顺序为单位。阵列 B 确实从时间位置受益两次：对于 I 的每个迭代，都可以访问相同的元素，并且 J 的每次迭代都使用 B 的值与上次迭代相同的值。忽略潜在的冲突错过，因为 b 而失踪将用于 b \ [j + 1 \] \ [0 \] i 0 时访问，也是 j 时第一次访问 b \ [j \] \ [0 \]0.因为 j 从 i 0 时从 0 到 99，因此可以访问 b 铅至 100 + 1 或 101 次失误。

Thus this loop will miss the data cache approximately 150 times for a plus 101 times for b, or 251 misses.

> 因此，该循环将遗漏大约 150 次的数据缓存，而 B 则为 101 次或 251 次失误。

To simplify our optimization, we will not worry about prefetching the first accesses of the loop. These may already be in the cache, or we will pay the miss penalty of the first few elements of a or b. Nor will we worry about suppressing the prefetches at the end of the loop that try to prefetch beyond the end of a (a\[i\] \[100\] … a\[i\]\[106\]) and the end of b (b\[101\]\[0\] … b\[107\]\[0\]). If these were faulting prefetches, we could not take this luxury. Let’s assume that the miss penalty is so large we need to start prefetching at least, say, seven itera- tions in advance. (Stated alternatively, we assume prefetching has no benefit until the eighth iteration.) We underline the changes to the preceding code needed to add prefetching.

> 为了简化我们的优化，我们不必担心预取循环的第一个访问。这些可能已经在缓存中，或者我们将支付 A 或 B 的前几个要素的罚款。我们也不会担心在循环末端抑制预取的预取，这些试图试图超越 A(a \ [i \] \ [100 \]…A \ [i \] \ [106 \])和 B(B \ [101 \] \ [0 \]…B \ [107 \] \ [0 \])的结尾。如果这些是错误的预取，我们将无法享受这种奢侈品。假设小姐的罚款是如此之大，我们需要至少提前提前摘要。(另外，我们假设预摘要在第八次迭代之前没有好处。)我们强调了添加预取的上述代码的更改。

> ===

>> ===
>>

Example Calculate the time saved in the preceding example. Ignore instruction cache misses and assume there are no conflict or capacity misses in the data cache. Assume that prefetches can overlap with each other and with cache misses, thereby transferring at the maximum memory bandwidth. Here are the key loop times ignoring cache misses: the original loop takes 7 clock cycles per iteration, the first prefetch loop takes 9 clock cycles per iteration, and the second prefetch loop takes 8 clock cycles per iteration (including the overhead of the outer for loop). A miss takes 100 clock cycles.

> 示例计算上一个示例中保存的时间。忽略指令缓存错过，并假定数据缓存中没有冲突或容量失误。假设预摘要可以彼此重叠，并与缓存失误重叠，从而在最大内存带宽处转移。以下是忽略缓存错过的关键循环时间：原始循环每次迭代需要 7 个时钟周期，第一个预取循环在每次迭代中进行 9 个时钟周期，第二个预摘要循环在每个迭代中采用 8 个时钟周期(包括外部的开销环形)。小姐需要 100 个时钟周期。

_Answer_ The original doubly nested loop executes the multiply 3 100 or 300 times. Because the loop takes 7 clock cycles per iteration, the total is 300 7 or 2100 clock cycles plus cache misses. Cache misses add 251 100 or 25,100 clock cycles, giving a total of 27,200 clock cycles. The first prefetch loop iterates 100 times; at 9 clock cycles per iteration the total is 900 clock cycles plus cache misses. Now add 11 100 or 1100 clock cycles for cache misses, giving a total of 2000. The second loop executes

> *answer* 原始双嵌套环执行乘以 3 100 或 300 次。由于循环在每个迭代中进行了 7 个时钟周期，因此总计为 300 7 或 2100 时钟周期以及高速缓存。缓存错过增加 251 或 25,100 时钟周期，总共 27,200 个时钟周期。第一个预取循环迭代 100 次；在 9 时钟循环时，总计是 900 个时钟周期和缓存遗漏。现在添加 11 个 100 或 1100 的时钟周期，以使缓存失误，总计 2000。第二个循环执行

2 100 or 200 times, and at 8 clock cycles per iteration, it takes 1600 clock cycles plus 8 100 or 800 clock cycles for cache misses. This gives a total of 2400 clock cycles. From the prior example, we know that this code executes 400 prefetch instructions during the 2000 + 2400 or 4400 clock cycles to execute these two loops. If we assume that the prefetches are completely overlapped with the rest of the exe- cution, then the prefetch code is 27,200/4400, or 6.2 times faster.

> 2 100 或 200 次，每次迭代时以 8 个时钟周期为单位，需要 1600 个时钟周期，以及 800 或 800 时钟循环，用于缓存错过。这总共提供了 2400 个时钟周期。从先前的示例中，我们知道该代码在 2000 + 2400 或 4400 时钟周期内执行 400 个预取指令以执行这两个循环。如果我们假设预摘要完全与其余的回顾完全重叠，则预取代码为 27,200/4400，或 6.2 倍的速度更快。

Although array optimizations are easy to understand, modern programs are more likely to use pointers. [Luk and Mowry (1999)](#_bookmark979) have demonstrated that compiler-based prefetching can sometimes be extended to pointers as well. Of 10 programs with recursive data structures, prefetching all pointers when a node is visited improved performance by 4%–31% in half of the programs. On the other hand, the remaining programs were still within 2% of their original performance. The issue is both whether prefetches are to data already in the cache and whether they occur early enough for the data to arrive by the time it is needed.

> 尽管阵列优化易于理解，但现代程序更有可能使用指针。[Luk and Mowry(1999)](#\_ bookmark979)证明，基于编译器的预摘要有时也可以扩展到指针。在具有递归数据结构的 10 个程序中，当访问节点的访问时，将所有指针提高到一半程序中的性能提高了 4％–31％。另一方面，其余程序仍在其原始性能的 2％之内。问题既是预摘要是否已经在缓存中已经存在数据，又要它们是否足够早，以使数据到达需要时到达。

Many processors support instructions for cache prefetch, and high-end proces- sors (such as the Intel Core i7) often also do some type of automated prefetch in hardware.

> 许多处理器都支持用于缓存预取的说明，以及高端验证器(例如 Intel Core i7)，通常也经常在硬件中执行某种类型的自动取回。

### Tenth Optimization: Using HBM to Extend the Memory Hierarchy

> ###第十优化：使用 HBM 扩展内存层次结构

Because most general-purpose processors in servers will likely want more memory than can be packaged with HBM packaging, it has been proposed that the in- package DRAMs be used to build massive L4 caches, with upcoming technologies ranging from 128 MiB to 1 GiB and more, considerably more than current on-chip L3 caches. Using such large DRAM-based caches raises an issue: where do the tags reside? That depends on the number of tags. Suppose we were to use a 64B block size; then a 1 GiB L4 cache requires 96 MiB of tags—far more static memory than exists in the caches on the CPU. Increasing the block size to 4 KiB, yields a dramatically reduced tag store of 256 K entries or less than 1 MiB total storage, which is probably acceptable, given L3 caches of 4–16 MiB or more in next-generation, multicore processors. Such large block sizes, however, have two major problems.

> 由于服务器中的大多数通用处理器可能需要比使用 HBM 包装包装更多的内存，因此已经提出，使用包装 DRAM 用于构建大型 L4 缓存，即将使用的技术从 128 MIB 到 1 GIB 和 1 GIB 和 1 GIB 和 1 GIB 和比目前的芯片 L3 缓存更大。使用如此大的基于 DRAM 的缓存提出了一个问题：标签在哪里？这取决于标签的数量。假设我们要使用 64B 块大小；然后，一个 1 GIB L4 缓存需要 96 个标签，比 CPU 上的缓存中存在的静态内存要多。将块大小增加到 4 KIB，产生 256 K 条目或小于 1 MIB 的总存储的标签存储库，在下一代多核心处理器中，这可能是可以接受的，这可能是可以接受的。但是，如此大的块尺寸有两个主要问题。

First, the cache may be used inefficiently when content of many blocks are not needed; this is called the _fragmentation problem_, and it also occurs in virtual mem- ory systems. Furthermore, transferring such large blocks is inefficient if much of the data is unused. Second, because of the large block size, the number of distinct blocks held in the DRAM cache is much lower, which can result in more misses, especially for conflict and consistency misses.

> 首先，当不需要许多块的内容时，缓存可能会效率低下。这称为* fragmentation roody*，它也发生在虚拟内存系统中。此外，如果大部分数据未使用，那么传输如此大的块将效率低下。其次，由于块尺寸较大，在 DRAM 缓存中保存的不同块的数量要低得多，这可能导致更多的错过，尤其是对于冲突和一致性而言。

One partial solution to the first problem is to add _sublocking_. _Subblocking_ allow parts of the block to be invalid, requiring that they be fetched on a miss. Sub- blocking, however, does nothing to address the second problem.

> 第一个问题的一个部分解决方案是添加 *sublocking*。*subblocking* 允许块的一部分无效，要求将它们在失误上获取。但是，封锁无助于解决第二个问题。

The tag storage is the major drawback for using a smaller block size. One pos- sible solution for that difficulty is to store the tags for L4 in the HBM. At first glance this seems unworkable, because it requires two accesses to DRAM for each L4 access: one for the tags and one for the data itself. Because of the long access time for random DRAM accesses, typically 100 or more processor clock cycles, such an approach had been discarded. Loh and Hill (2011) proposed a clever solution to this problem: place the tags and the data in the same row in the HBM SDRAM. Although opening the row (and eventually closing it) takes a large amount of time, the CAS latency to access a different part of the row is about one-third the new row access time. Thus we can access the tag portion of the block first, and if it is a hit,

> 标签存储是使用较小块大小的主要缺点。解决该困难的一种可能解决方案是将 L4 的标签存储在 HBM 中。乍一看，这似乎是不可行的，因为它需要每个 L4 访问访问 DRAM 的两个访问：一个用于标签，一个用于数据本身。由于随机 DRAM 访问的时间很长，通常是 100 个或更多的处理器时钟周期，因此已经丢弃了这种方法。Loh and Hill(2011)提出了一个巧妙的解决方案：将标签和数据放在 HBM SDRAM 的同一行中。尽管打开行(并最终关闭)需要大量时间，但 CAS 延迟访问该行的不同部分约为新的行访问时间的三分之一。因此，我们可以先访问块的标签部分，如果是命中，

then use a column access to choose the correct word. Loh and Hill (L-H) have pro- posed organizing the L4 HBM cache so that each SDRAM row consists of a set of tags (at the head of the block) and 29 data segments, making a 29-way set associa- tive cache. When L4 is accessed, the appropriate row is opened and the tags are read; a hit requires one more column access to get the matching data.

> 然后使用列访问选择正确的单词。LOH 和 HILL(L-H)已提出组织 L4 HBM 缓存，因此每个 SDRAM 行都由一组标签组成(在块的头部)和 29 个数据段，从而使 29 个路套件相关加速。访问 L4 后，打开适当的行并读取标签；命中需要再访问一个列才能获取匹配数据。

Qureshi and Loh (2012) proposed an improvement called an _alloy cache_ that reduces the hit time. An _alloy cache_ molds the tag and data together and uses a direct mapped cache structure. This allows the L4 access time to be reduced to a single HBM cycle by directly indexing the HBM cache and doing a burst transfer of both the tag and data. [Figure 2.16](#_bookmark66) shows the hit latency for the alloy cache, the L-H scheme, and SRAM based tags. The alloy cache reduces hit time by more than a factor of 2 versus the L-H scheme, in return for an increase in the miss rate by a factor of 1.1–1.2. The choice of benchmarks is explained in the caption.

> Qureshi 和 Loh(2012)提出了一个称为 *alloy Cache* 的改进，以减少命中时间。*alloy cache* 将标签和数据塑造在一起，并使用直接映射的高速缓存结构。这使 L4 访问时间可以通过直接索引 HBM 缓存并同时进行标签和数据的突发传输来简化为单个 HBM 循环。[图 2.16](#\_ bookmark66)显示了合金缓存，L-H 方案和基于 SRAM 的标签的热门延迟。与 L-H 方案相比，合金缓存的打数减少了 2 倍以上，以使失误率提高 1.1-1.2。标题中解释了基准的选择。

Unfortunately, in both schemes, misses require two full DRAM accesses: one to get the initial tag and a follow-on access to the main memory (which is even slower). If we could speed up the miss detection, we could reduce the miss time. Two different solutions have been proposed to solve this problem: one uses a map that keeps track of the blocks in the cache (not the location of the block, just whether it is present); the other uses a memory access predictor that predicts likely misses using history prediction techniques, similar to those used for global branch prediction (see the next chapter). It appears that a small predictor can predict likely misses with high accuracy, leading to an overall lower miss penalty.

> 不幸的是，在这两个方案中，错过都需要两个完整的 DRAM 访问：一个可以获取初始标签和对主内存的后续访问(甚至较慢)。如果我们可以加快错过检测的速度，我们可以减少错过的时间。已经提出了两种不同的解决方案来解决此问题：一张使用一张映射来跟踪缓存中的块(不是块的位置，而只是存在)；另一个使用内存访问预测器，该预测器可能使用历史预测技术预测可能会错过，类似于全球分支预测的预测技术(请参见下一章)。看来，一个小的预测指标可以预测可能以高度准确性的失误，从而导致较低的罚款。

![](./media/image113.png)

> ！[](./ Media/image113.png)

> Figure 2.16 Average hit time latency in clock cycles for the L-H scheme, a currently-impractical scheme using SRAM for the tags, and the alloy cache organization. In the SRAM case, we assume the SRAM is accessible in the same time as L3 and that it is checked before L4 is accessed. The average hit latencies are 43 (alloy cache), 67 (SRAM tags), and 107 (L-H). The 10 SPECCPU2006 benchmarks used here are the most memory-intensive ones; each of them would run twice as fast if L3 were perfect.

>> 图 2.16 L-H 方案的时钟周期中的平均命中时间延迟，这是一种使用 SRAM 用于标签的当前易于实践的方案，以及合金 CACHE 组织。在 SRAM 案例中，我们假设 SRAM 与 L3 同时访问，并且在访问 L4 之前对其进行检查。平均命中率是 43(合金缓存)，67(SRAM 标签)和 107(L-H)。这里使用的 10 个 SPECCPU2006 基准是最含有内存的基准。如果 L3 完美，他们每个人的运行速度都会迅速两倍。
>>

[Figure 2.17](#_bookmark67) shows the speedup obtained on SPECrate for the memory- intensive benchmarks used in [Figure 2.16](#_bookmark66). The alloy cache approach outperforms the LH scheme and even the impractical SRAM tags, because the combination of a fast access time for the miss predictor and good prediction results lead to a shorter time to predict a miss, and thus a lower miss penalty. The alloy cache performs close to the Ideal case, an L4 with perfect miss prediction and minimal hit time.

> [图 2.17](#_ bookmark67)显示了[图 2.16](#_ bookmark66)中使用的内存密集基准的规格上获得的加速。合金缓存方法的表现优于 LH 方案，甚至不切实际的 SRAM 标签，因为 MISS 预测指标的快速访问时间和良好的预测结果的组合导致预测失误的时间较短，从而导致较低的罚款。合金缓存的性能接近理想的表壳，这是一个完美的预测和最小命中时间的 L4。

![](./media/image126.png)

> ！[](./ Media/image126.png)

> Figure 2.17 Performance speedup running the SPECrate benchmark for the LH scheme, an SRAM tag scheme, and an ideal L4 (Ideal); a speedup of 1 indicates no improvement with the L4 cache, and a speedup of 2 would be achievable if L4 were perfect and took no access time. The 10 memory-intensive benchmarks are used with each benchmark run eight times. The accompanying miss prediction scheme is used. The Ideal case assumes that only the 64-byte block requested in L4 needs to be accessed and transferred and that prediction accuracy for L4 is perfect (i.e., all misses are known at zero cost).

>> 图 2.17 性能加速运行 LH 方案，SRAM 标签方案和理想 L4(理想)的规格基准；1 加速 1 表示 L4 缓存没有改进，如果 L4 完美且没有访问时间，则可以实现 2 的速度。每个基准测试运行八次，使用 10 个内存密集型基准测试。使用随附的小姐预测方案。理想的情况假设只有 L4 中要求的 64 个字节块需要访问和传输，并且 L4 的预测准确性是完美的(即，所有失误都是以零成本知道的)。
>>

HBM is likely to have widespread use in a variety of different configurations, from containing the entire memory system for some high-performance, special- purpose systems to use as an L4 cache for larger server configurations.

> HBM 可能在各种不同的配置中广泛使用，从包含整个存储系统，用于用于一些高性能，特殊目的系统，可用作用于较大服务器配置的 L4 缓存。

### Cache Optimization Summary

> ###缓存优化摘要

The techniques to improve hit time, bandwidth, miss penalty, and miss rate gen- erally affect the other components of the average memory access equation as well as the complexity of the memory hierarchy. [Figure 2.18](#_bookmark68) summarizes these tech- niques and estimates the impact on complexity, with + meaning that the technique improves the factor, meaning it hurts that factor, and blank meaning it has no impact. Generally, no technique helps more than one category.

> 改善命中时间，带宽，罚款和错过的技术，通常会影响平均内存访问方程式的其他组成部分以及内存层次结构的复杂性。[图 2.18](#\_ bookmark68)总结了这些技术，并估算了对复杂性的影响， + 意味着该技术会改善因素，这意味着它会造成该因素的伤害，而空白则没有影响。通常，没有任何技术可以帮助多个类别。

> Figure 2.18 Summary of 10 advanced cache optimizations showing impact on cache performance, power con- sumption, and complexity. Although generally a technique helps only one factor, prefetching can reduce misses if done sufficiently early; if not, it can reduce miss penalty. + means that the technique improves the factor, means it hurts that factor, and blank means it has no impact. The complexity measure is subjective, with 0 being the easiest and 3 being a challenge.

>> 图 2.18 10 个高级缓存优化的摘要，显示了对缓存性能，功率的影响和复杂性的影响。尽管通常一种技术只能帮助一个因素，但预摘要可以减少足够早的时间。如果没有，它可以减少罚款。+ 意味着该技术改善了因素，这意味着它会伤害该因素，而空白意味着它没有影响。复杂性度量是主观的，其中 0 是最简单的，3 是一个挑战。
>>
