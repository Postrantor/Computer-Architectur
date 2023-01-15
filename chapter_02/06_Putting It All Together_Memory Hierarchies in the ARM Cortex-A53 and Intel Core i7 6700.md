---
data: 2023-01-15
---
> [!note] 缓存命中
> 这一章以真实的机器为例，针对缓存命中给出了一些指导性的分析。

## Putting It All Together: Memory Hierarchies in the ARM Cortex-A53 and Intel Core i7 6700(ARM Cortex-A53 中的内存层次结构和 Intel Core i7 6700)

This section reveals the ARM Cortex-A53 (hereafter called the A53) and Intel Core i76700 (hereafter called i7) memory hierarchies and shows the performance of their components on a set of single-threaded benchmarks. We examine the Cortex-A53 first because it has a simpler memory system; we go into more detail for the i7, tracing out a memory reference in detail. This section presumes that readers are familiar with the organization of a two-level cache hierarchy using vir- tually indexed caches. The basics of such a memory system are explained in detail in [Appendix B](#_bookmark436), and readers who are uncertain of the organization of such a system are strongly advised to review the Opteron example in [Appendix B](#_bookmark436). Once they understand the organization of the Opteron, the brief explanation of the A53 sys- tem, which is similar, will be easy to follow.

> 本节揭示了 ARM Cortex-A53(以下称为 A53)和 Intel Core i76700(以下称为 i7)内存层次结构，并在一组单线程基准中显示了它们的组件的性能。我们首先检查 Cortex-A53，因为它具有更简单的内存系统。我们更详细地详细介绍了 i7，并详细介绍了一个内存参考。本节假设读者熟悉使用索引索引的缓存的两级缓存层次结构的组织。强烈建议在[附录 B](#_bookmark436) 中详细说明此类内存系统的基础知识，并强烈建议不确定该系统组织的读者来审查[附录 B]中的 Opteron 示例(#\_bookmark436)。一旦他们了解了 Opteron 的组织，A53 系统的简要说明将很容易遵循。

### The ARM Cortex-A53

The Cortex-A53 is a configurable core that supports the ARMv8A instruction set architecture, which includes both 32-bit and 64-bit modes. The Cortex-A53 is delivered as an IP (intellectual property) core. IP cores are the dominant form of technology delivery in the embedded, PMD, and related markets; billions of ARM and MIPS processors have been created from these IP cores. Note that IP cores are different from the cores in the Intel i7 or AMD Athlon multicores. An IP core (which may itself be a multicore) is designed to be incorporated with other logic (thus it is the core of a chip), including application-specific processors (such as an encoder or decoder for video), I/O interfaces, and memory interfaces, and then fabricated to yield a processor optimized for a particular application. For example, the Cortex-A53 IP core is used in a variety of tablets and smartphones; it is designed to be highly energy-efficient, a key criteria in battery-based PMDs. The A53 core is capable of being configured with multiple cores per chip for use in high-end PMDs; our discussion here focuses on a single core.

> Cortex-A53 是支持 ARMV8A 指令集体系结构的可配置核心，其中包括 32 位和 64 位模式。Cortex-A53 作为 IP(知识产权)核心传递。IP 核心是嵌入式，PMD 和相关市场中技术交付的主要形式。这些 IP 内核创建了数十亿个 ARM 和 MIP 处理器。请注意，IP 内核与英特尔 i7 或 AMD Athlon Multicors 中的核心不同。IP 核心(本身可能是多核心)被设计为与其他逻辑合并(因此它是芯片的核心)，包括特定于应用程序的处理器(例如视频的编码器或解码器)，I/O 接口，以及内存界面，然后制造以产生针对特定应用程序优化的处理器。例如，Cortex-A53 IP 核心用于多种平板电脑和智能手机。它旨在高能效率，这是基于电池的 PMD 的关键标准。A53 核心能够使用每个芯片配置多个内核，以用于高端 PMD。我们在这里的讨论集中在一个核心上。

Generally, IP cores come in two flavors. _Hard cores_ are optimized for a par- ticular semiconductor vendor and are black boxes with external (but still on-chip) interfaces. Hard cores typically allow parametrization only of logic outside the core, such as L2 cache sizes, and the IP core cannot be modified. _Soft cores_ are usually delivered in a form that uses a standard library of logic elements. A soft core can be compiled for different semiconductor vendors and can also be modi- fied, although extensive modifications are very difficult because of the complexity of modern-day IP cores. In general, hard cores provide higher performance and smaller die area, while soft cores allow retargeting to other vendors and can be more easily modified.

> 通常，IP 内核有两种口味。_hard 内核_ 是针对特性半导体供应商进行了优化的，是具有外部(但仍在片上)接口的黑匣子。硬核通常仅允许核心外逻辑的参数化，例如 L2 缓存大小，而 IP 核心无法修改。\_soft cores\*通常以使用标准逻辑元素库的形式传递。可以为不同的半导体供应商编译软芯，也可以进行修改，尽管由于现代 IP 核心的复杂性，广泛的修改非常困难。通常，硬核提供更高的性能和较小的模具区域，而软芯则可以对其他供应商进行重新定位，并且可以更容易修改。

The Cortex-A53 can issue two instructions per clock at clock rates up to 1.3 GHz. It supports both a two-level TLB and a two-level cache; [Figure 2.19](#_bookmark72) sum- marizes the organization of the memory hierarchy. The critical term is returned first, and the processor can continue while the miss completes; a memory system with up to four banks can be supported. For a D-cache of 32 KiB and a page size of 4 KiB, each physical page could map to two different cache addresses; such aliases are avoided by hardware detection on a miss as in Section B.3 of [Appendix B](#_bookmark436). [Figure 2.20](#_bookmark73) shows how the 32-bit virtual address is used to index the TLB and the caches, assuming 32 KiB primary caches and a 1 MiB secondary cache with 16 KiB page size.

> Cortex-A53 可以以最高 1.3 GHz 的时钟速率发布两个说明。它支持两级 TLB 和两级缓存。[图 2.19](#_bookmark72) 总结内存层次结构的组织。关键术语首先返回，并且在错过完成时可以继续处理。最多可以支持具有多达四个银行的存储系统。对于 32 KIB 的 D-CACHE 和 4 KIB 的页面大小，每个物理页面都可以映射到两个不同的缓存地址。如[附录 B](#_bookmark436) 的第 3 节中的硬件检测，可以避免使用此类别名。[图 2.20](#_bookmark73) 显示了如何使用 32 位虚拟地址来索引 TLB 和缓存，假设 32 KIB 主缓存和 1 个具有 16 KIB 页面大小的 MIB 辅助缓存。

Figure 2.19 The memory hierarchy of the Cortex A53 includes multilevel TLBs and caches. A page map cache keeps track of the location of a physical page for a set of virtual pages; it reduces the L2 TLB miss penalty. The L1 caches are virtually indexed and physically tagged; both the L1 D cache and L2 use a write-back policy defaulting to allocate on write. Replacement policy is LRU approximation in all the caches. Miss penalties to L2 are higher if both a MicroTLB and L1 miss occur. The L2 to main memory bus is 64–128 bits wide, and the miss penalty is larger for the narrow bus.

> 图 2.19 Cortex A53 的内存层次结构包括多级 TLB 和缓存。页面地图缓存跟踪一组虚拟页面的物理页面的位置；它减少了 L2 TLB 小姐的点球。L1 缓存实际上是索引的，并在物理上标记了；L1 D 缓存和 L2 都使用写下策略默认值在写入时分配。替代政策是所有缓存中的 LRU 近似。如果 MicroTLB 和 L1 失误都发生，对 L2 的罚款较高。L2 到主存储总线的宽度为 64-128 位，而狭窄的巴士的罚款更大。

![](./media/image135.png)
![](./media/image136.png)

Figure 2.20 The virtual address, physical and data blocks for the ARM Cortex-A53 caches and TLBs, assuming 32- bit addresses. The top half (A) shows the instruction access; the bottom half (B) shows the data access, including L2. The TLB (instruction or data) is fully associative each with 10 entries, using a 64 KiB page in this example. The L1 I- cache is two-way set associative, with 64-byte blocks and 32 KiB capacity; the L1 D-cache is 32 KiB, four-way set asso- ciative, and 64-byte blocks. The L2 TLB is 512 entries and four-way set associative. The L2 cache is 16-way set asso- ciative with 64-byte blocks and 128 cKiB to 2 MiB capacity; a 1 MiB L2 is shown. This figure doesn’t show the valid bits and protection bits for the caches and TLB.

> 图 2.20 假设 32 位地址，ARM Cortex-A53 缓存和 TLB 的虚拟地址，物理和数据块。上半(a)显示了指令访问；下半部分(b)显示了包括 L2 在内的数据访问。TLB(指令或数据)在此示例中使用 64 KIB 页面完全关联了 10 个条目。L1 I-CACHE 是双向集合的关联，具有 64 字节块和 32 KIB 容量；L1 D-CACHE 为 32 KIB，四向套件协会和 64 字节块。L2 TLB 为 512 个条目和四向套件的关联。L2 高速缓存是 16 向套件，具有 64 字节块和 128 CKIB 至 2 MIB 的容量；显示了 1 个 MIB L2。该图没有显示缓存和 TLB 的有效位和保护位。

### Performance of the Cortex-A53 Memory Hierarchy(Cortex-A53 内存层次结构的性能)

The memory hierarchy of the Cortex-A8 was measured with 32 KiB primary caches and a 1 MiB L2 cache running the SPECInt2006 benchmarks. The instruc- tion cache miss rates for these SPECInt2006 are very small even for just the L1: close to zero for most and under 1% for all of them. This low rate probably results from the computationally intensive nature of the SPECCPU programs and the two- way set associative cache that eliminates most conflict misses.

> 用 32 个 KIB 主缓存和 1 个 MIB L2 缓存运行 Specint2006 基准测量了 Cortex-A8 的内存层次结构。这些 SpecInt2006 的指导缓存率即使仅在 L1：大多数接近零，所有它们的 1％以下也很小。这种较低的速率可能是由于 SpecCPU 程序的计算密集性性质以及消除大多数冲突错过的双向协会缓存而产生的。

[Figure 2.21](#_bookmark74) shows the data cache results, which have significant L1 and L2 miss rates. The L1 rate varies by a factor of 75, from 0.5% to 37.3% with a median miss rate of 2.4%. The global L2 miss rate varies by a factor of 180, from 0.05% to 9.0% with a median of 0.3%. MCF, which is known as a cache buster, sets the upper bound and significantly affects the mean. Remember that the L2 global miss rate is significantly lower than the L2 local miss rate; for example, the median L2 stand-alone miss rate is 15.1% versus the global miss rate of 0.3%. Using these miss penalties in [Figure 2.19, Figure 2.22](#_bookmark72) shows the average pen- alty per data access. Although the L1 miss rates are about seven times higher than the L2 miss rate, the L2 penalty is 9.5 times as high, leading to L2 misses slightly dominating for the benchmarks that stress the memory system. In the next chapter, we will examine the impact of the cache misses on overall CPI.

> [图 2.21](#_bookmark74) 显示了具有显着 L1 和 L2 失误率的数据缓存结果。L1 速率差 75 倍，从 0.5％到 37.3％，中位数率为 2.4％。全球 L2 失误率在 180 倍之间，中位数为 0.3％，从 0.05％到 9.0％。MCF 被称为缓存 Buster，设置了上限并显着影响平均值。请记住，L2 全球损失率明显低于 L2 本地失误率。例如，中位 L2 独立失误率为 15.1％，而全球损失率为 0.3％。在[图 2.19，图 2.22](#_bookmark72) 中使用这些未罚款，显示了每个数据访问的平均笔。尽管 L1 失误率是 L2 失误率的七倍，但 L2 罚款的高 9.5 倍，导致 L2 错过对压力记忆系统的基准略有支配。在下一章中，我们将研究缓存错过对整体 CPI 的影响。

Figure 2.21 The data miss rate for ARM with a 32 KiB L1 and the global data miss rate for a 1 MiB L2 using the SPECInt2006 benchmarks are significantly affected by the applications. Applications with larger memory footprints tend to have higher miss rates in both L1 and L2. Note that the L2 rate is the global miss rate that is counting all references, including those that hit in L1. MCF is known as a cache buster.

> 图 2.21 使用 Specint2006 基准，具有 32 KIB L1 的 ARM 的数据误差和 1 MIB L2 的全局数据遗漏速率受到应用程序的显着影响。在 L1 和 L2 中，具有较大记忆足迹的应用程序往往具有更高的失误率。请注意，L2 利率是计算所有参考的全球损失率，包括在 L1 中击中的参考。MCF 被称为缓存巴斯特。

Figure 2.22 The average memory access penalty per data memory reference coming from L1 and L2 is shown for the A53 processor when running SPECInt2006. Although the miss rates for L1 are significantly higher, the L2 miss penalty, which is more than five times higher, means that the L2 misses can contribute significantly.

> 图 2.22 在运行 SpecInt2006 时，显示了来自 L1 和 L2 的每个数据存储器参考的平均内存访问惩罚。尽管 L1 的失误率显着更高，但 L2 罚款高于五倍以上，这意味着 L2 失误可以显着贡献。

### The Intel Core i7 6700

The i7 supports the x 86-64 instruction set architecture, a 64-bit extension of the 80x86 architecture. The i7 is an out-of-order execution processor that includes four cores. In this chapter, we focus on the memory system design and performance from the viewpoint of a single core. The system performance of multiprocessor designs, including the i7 multicore, is examined in detail in [Chapter 5](#_bookmark213).

> i7 支持 X 86-64 指令集体系结构，这是 80x86 体系结构的 64 位扩展。i7 是一个排序的执行处理器，其中包括四个内核。在本章中，我们从单个核心的角度专注于内存系统设计和性能。在[第 5 章](#_bookmark213)中详细检查了包括 i7 多核在内的多处理器设计的系统性能。

Each core in an i7 can execute up to four 80x86 instructions per clock cycle, using a multiple issue, dynamically scheduled, 16-stage pipeline, which we describe in detail in [Chapter 3](#_bookmark93). The i7 can also support up to two simultaneous threads per processor, using a technique called simultaneous multithreading, described in [Chapter 4](#_bookmark165). In 2017 the fastest i7 had a clock rate of 4.0 GHz (in Turbo Boost mode), which yielded a peak instruction execution rate of 16 billion instruc- tions per second, or 64 billion instructions per second for the four-core design. Of course, there is a big gap between peak and sustained performance, as we will see over the next few chapters.

> i7 中的每个核心每个时钟周期最多可以执行四个 80x86 指令，使用多个问题，动态安排的 16 阶段管道，我们在[第 3 章](#_bookmark93)中详细描述。i7 还可以使用一种称为同时多线程的技术最多支持两个同时线程，该技术在[第 4 章](#_bookmark165)中描述。在 2017 年，最快的 i7 的时钟速率为 4.0 GHz(以涡轮增压模式)，每秒产生了 160 亿个指令执行率，四核设计的每秒 640 亿个指令。当然，正如我们将在接下来的几章中看到的那样，峰值和持续性能之间存在很大的差距。

The i7 can support up to three memory channels, each consisting of a separate set of DIMMs, and each of which can transfer in parallel. Using DDR3-1066 (DIMM PC8500), the i7 has a peak memory bandwidth of just over 25 GB/s.

> i7 最多可以支持三个内存通道，每个通道由一组单独的 DIMM 组成，每个 DIMM 都可以并行传输。使用 DDR3-1066(DIMM PC8500)，i7 的峰值存储器带宽仅为 25 GB/s。

i7 uses 48-bit virtual addresses and 36-bit physical addresses, yielding a maximum physical memory of 36 GiB. Memory management is handled with a two-level TLB (see [Appendix B](#_bookmark436), Section B.4), summarized in [Figure 2.23](#_bookmark75).

> i7 使用 48 位虚拟地址和 36 位物理地址，产生 36 GIB 的最大物理内存。内存管理用两级 TLB 处理(请参见[附录 B](#_Bookmark436)，B.4 节)，摘要在[图 2.23](#_bookmark75) 中。

[Figure 2.24](#_bookmark76) summarizes the i7’s three-level cache hierarchy. The first-level caches are virtually indexed and physically tagged (see [Appendix B](#_bookmark436), Section B.3), while the L2 and L3 caches are physically indexed. Some versions of the i7 6700 will support a fourth-level cache using HBM packaging.

> [图 2.24](#_bookmark76) 总结了 i7 的三级缓存层次结构。第一级缓存实际上是索引的，并在物理上进行了标记(请参见[附录 B](#_bookmark436)，第 3 节)，而 L2 和 L3 Caches 则是物理索引的。i7 6700 的某些版本将使用 HBM 包装支持第四级缓存。

[Figure 2.25](#_bookmark77) is labeled with the steps of an access to the memory hierarchy.

> [图 2.25](#_bookmark77) 标记了对内存层次结构的访问的步骤。

First, the PC is sent to the instruction cache. The instruction cache index is

> 首先，将 PC 发送到指令缓存。指令缓存索引是

> ===

Figure 2.23 Characteristics of the i7’s TLB structure, which has separate first-level instruction and data TLBs, both backed by a joint second-level TLB. The first-level TLBs support the standard 4 KiB page size, as well as having a limited number of entries of large 2–4 MiB pages; only 4 KiB pages are supported in the second-level TLB. The i7 has the ability to handle two L2 TLB misses in parallel. See Section L.3 of online Appendix L for more discussion of multilevel TLBs and support for multiple page sizes.

> 图 2.23 i7 的 TLB 结构的特征，该结构具有单独的第一级指令和数据 TLB，均以关节二级 TLB 为支持。第一级 TLB 支持标准 4 KIB 页面大小，并且具有有限数量的 2-4 个 MIB 页面的条目；在第二级 TLB 中仅支持 4 个 KIB 页面。i7 具有并行处理两个 L2 TLB 失误的能力。有关多级 TLB 的更多讨论以及对多个页面尺寸的支持，请参见在线附录 L 的 L.3 节。

Figure 2.24 Characteristics of the three-level cache hierarchy in the i7. All three caches use write back and a block size of 64 bytes. The L1 and L2 caches are separate for each core, whereas the L3 cache is shared among the cores on a chip and is a total of 2 MiB per core. All three caches are nonblocking and allow multiple outstanding writes. A merging write buffer is used for the L1 cache, which holds data in the event that the line is not present in L1 when it is written. (That is, an L1 write miss does not cause the line to be allocated.) L3 is inclusive of L1 and L2; we explore this property in further detail when we explain multiprocessor caches. Replacement is by a variant on pseudo-LRU; in the case of L3, the block replaced is always the lowest numbered way whose access bit is off. This is not quite random but is easy to compute.

> 图 2.24 i7 中三级缓存层次结构的特征。所有三个缓存都使用写回，块大小为 64 个字节。L1 和 L2 缓存对于每个核心是分开的，而 L3 缓存在芯片上共享芯之间，每个核心总计 2 MIB。这三个缓存都是不封锁的，允许多个出色的写作。合并写缓冲区用于 L1 缓存，如果在编写 L1 中不存在该行，则可以保存数据。(也就是说，L1 写的错过不会导致分配线。)L3 包括 L1 和 L2；当我们解释多处理器缓存时，我们将进一步详细探讨该属性。替换是由伪 lru 上的变体;就 L3 而言，更换的块始终是最低的访问位数。这不是很随机，但易于计算。

![](./media/image139.png)

Figure 2.25 The Intel i7 memory hierarchy and the steps in both instruction and data access. We show only reads. Writes are similar, except that misses are handled by simply placing the data in a write buffer, because the L1 cache is not write-allocated.

> 图 2.25 Intel I7 内存层次结构以及指令和数据访问中的步骤。我们只显示读物。写入相似，除了通过简单地将数据放在写缓冲区中来处理错过，因为 L1 缓存未分配。

or 6 bits. The page frame of the instruction’s address (36 48 12 bits) is sent to the instruction TLB (step 1). At the same time, the 12-bit page offset from the vir- tual address is sent to the instruction cache (step 2). Notice that for the eight-way associative instruction cache, 12 bits are needed for the cache address: 6 bits to index the cache plus 6 bits of block offset for the 64-byte block, so no aliases are possible. The previous versions of the i7 used a four-way set associative I-cache, meaning that a block corresponding to a virtual address could actually be in two different places in the cache, because the corresponding physical address could have either a 0 or 1 in this location. For instructions this did not pose a prob- lem because even if an instruction appeared in the cache in two different locations, the two versions must be the same. If such duplication, or aliasing, of data is allowed, the cache must be checked when the page map is changed, which is an infrequent event. Note that a very simple use of page coloring (see [Appendix B](#_bookmark436), Section B.3) can eliminate the possibility of these aliases. If even-address virtual pages are mapped to even-address physical pages (and the same for odd pages), then these aliases can never occur because the low-order bit in the virtual and phys- ical page number will be identical.

> 或 6 位。指令地址的页面框架(36 48 12 位)发送到指令 TLB(步骤 1)。同时，将 12 位页面的偏移量发送到指令缓存(步骤 2)。请注意，对于八向关联指令缓存，缓存地址需要 12 位：6 位为 64 个字节块索引加速器加 6 位的块偏移量，因此不可行。i7 的先前版本使用了四个方向的关联 i-cache，这意味着与虚拟地址相对应的块实际上可能位于缓存中的两个不同位置，因为相应的物理地址可以具有 0 或 1 这个位置。对于指令，这并不构成概率，因为即使在两个不同位置的缓存中出现了指令，这两个版本也必须相同。如果允许使用此类重复或混杂的数据，则必须在更改页面映射时检查缓存，这是一个很少的事件。请注意，非常简单地使用页面着色(请参阅[附录 B](#_bookmark436)，B.3 节)可以消除这些别名的可能性。如果将均匀的虚拟页面映射到均匀的 address 物理页面(对于奇数页面相同)，那么这些别名就永远不会发生，因为虚拟和物理页码中的低阶位将是相同的。

The instruction TLB is accessed to find a match between the address and a valid page table entry (PTE) (steps 3 and 4). In addition to translating the address, the TLB checks to see if the PTE demands that this access result in an exception because of an access violation.

> 访问指令 TLB 以查找地址和有效的页面条目(PTE)(步骤 3 和 4)之间的匹配。除了翻译地址外，TLB 还检查了 PTE 是否要求此访问导致由于访问违规而导致例外。

An instruction TLB miss first goes to the L2 TLB, which contains 1536 PTEs of 4 KiB page sizes and is 12-way set associative. It takes 8 clock cycles to load the L1 TLB from the L2 TLB, which leads to the 9-cycle miss penalty including the initial clock cycle to access the L1 TLB. If the L2 TLB misses, a hardware algorithm is used to walk the page table and update the TLB entry. Sections L.5 and L.6 of online Appendix L describe page table walkers and page structure caches. In the worst case, the page is not in memory, and the operating system gets the page from secondary storage. Because millions of instructions could execute during a page fault, the operating system will swap in another pro- cess if one is waiting to run. Otherwise, if there is no TLB exception, the instruc- tion cache access continues.

> 指令 TLB Miss 首先转到 L2 TLB，其中包含 1536 个 PTE，其中 4 KIB 页面尺寸，并且是 12 向套件的关联。从 L2 TLB 加载 L1 TLB 需要 8 个时钟循环，这导致 9 周期的罚款，包括最初的时钟周期，以访问 L1 TLB。如果 L2 TLB 错过了，则使用硬件算法来行走页面表并更新 TLB 条目。在线附录 l 的第 5 和 L.6 节描述页面式步行器和页面结构缓存。在最坏的情况下，该页面不在内存中，并且操作系统从辅助存储中获取页面。由于数百万个说明可以在页面故障期间执行，因此，如果一个人正在等待运行，则操作系统将在另一个程序中进行交换。否则，如果没有 TLB 例外，则可以继续使用指令缓存。

The index field of the address is sent to all eight banks of the instruction cache (step 5). The instruction cache tag is 36 bits 6 bits (index) 6 bits (block offset), or 24 bits. The four tags and valid bits are compared to the physical page frame from the instruction TLB (step 6). Because the i7 expects 16 bytes each instruction fetch, an additional 2 bits are used from the 6-bit block offset to select the appro- priate 16 bytes. Therefore 6 + 2 or 8 bits are used to send 16 bytes of instructions to the processor. The L1 cache is pipelined, and the latency of a hit is 4 clock cycles (step 7). A miss goes to the second-level cache.

> 地址的索引字段发送到指令缓存的所有八个银行(步骤 5)。指令缓存标签为 36 位 6 位(索引)6 位(块偏移)或 24 位。将四个标签和有效位与指令 TLB 的物理页面框架进行比较(步骤 6)。由于 i7 期望每个指令提取 16 个字节，因此从 6 位块偏移量中使用了另外 2 位，以选择适当的 16 个字节。因此，6 + 2 或 8 位用于向处理器发送 16 个字节。L1 缓存是管道的，命中的延迟为 4 个时钟周期(步骤 7)。错过了第二级缓存。

As mentioned earlier, the instruction cache is virtually addressed and physi- cally tagged. Because the second-level caches are physically addressed, the phys- ical page address from the TLB is composed with the page offset to make an address to access the L2 cache. The L2 index is

> 如前所述，指令缓存实际上是处理并在物理上标记的。由于第二级缓存是物理上解决的，因此来自 TLB 的物理页面地址与页面偏移组成，以制作一个访问 L2 缓存的地址。L2 索引是

> ===

so the 30-bit block address (36-bit physical address 6-bit block offset) is divided into a 20-bit tag and a 10-bit index (step 8). Once again, the index and tag are sent to the four banks of the unified L2 cache (step 9), which are compared in parallel. If one matches and is valid (step 10), it returns the block in sequential order after the initial 12-cycle latency at a rate of 8 bytes per clock cycle.

> 因此，将 30 位块地址(36 位物理地址 6 位块偏移)分为 20 位标签和 10 位索引(步骤 8)。再次将索引和标签发送到并行比较的统一 L2 缓存的四个银行(步骤 9)。如果一个匹配并且是有效的(步骤 10)，它将在初始 12 周期延迟后按顺序返回块，每个时钟周期 8 字节的速率。

If the L2 cache misses, the L3 cache is accessed. For a four-core i7, which has an 8 MiB L3, the index size is

> 如果 L2 高速缓存错过，则可以访问 L3 缓存。对于具有 8 个 MIB L3 的四核 i7，索引大小为

> ===

The 13-bit index (step 11) is sent to all 16 banks of the L3 (step 12). The L3 tag, which is 36 (13 + 6) 17 bits, is compared against the physical address from the TLB (step 13). If a hit occurs, the block is returned after an initial latency of 42 clock cycles, at a rate of 16 bytes per clock and placed into both L1 and L3. If L3 misses, a memory access is initiated.

> 13 位指数(步骤 11)发送到 L3 的所有 16 个银行(步骤 12)。L3 标签，即 36(13 + 6)17 位，与 TLB 的物理地址进行了比较(步骤 13)。如果发生击球，则在 42 个时钟周期的初始潜伏期后返回块，以每个时钟 16 字节的速率，并将其放入 L1 和 L3 中。如果 L3 错过了，则会启动内存访问。

If the instruction is not found in the L3 cache, the on-chip memory controller must get the block from main memory. The i7 has three 64-bit memory channels that can act as one 192-bit channel, because there is only one memory controller and the same address is sent on both channels (step 14). Wide transfers happen when both channels have identical DIMMs. Each channel supports up to four DDR DIMMs (step 15). When the data return they are placed into L3 and L1 (step 16) because L3 is inclusive.

> 如果在 L3 缓存中找不到指令，则芯片内存控制器必须从主内存中获取块。i7 具有三个 64 位内存通道，可以充当一个 192 位通道，因为只有一个内存控制器，并且在两个频道上发送了相同的地址(步骤 14)。当两个通道都具有相同的 DIMM 时，就会发生宽广的转移。每个通道最多支持四个 DDR DIMM(步骤 15)。当数据返回时，将它们放入 L3 和 L1(步骤 16)，因为 L3 是包含的。

The total latency of the instruction miss that is serviced by main memory is approximately 42 processor cycles to determine that an L3 miss has occurred, plus the DRAM latency for the critical instructions. For a single-bank DDR4-2400 SDRAM and 4.0 GHz CPU, the DRAM latency is about 40 ns or 160 clock cycles to the first 16 bytes, leading to a total miss penalty of about 200 clock cycles. The memory controller fills the remainder of the 64-byte cache block at a rate of 16 bytes per I/O bus clock cycle, which takes another 5 ns or 20 clock cycles.

> 由主内存服务的指令错过的总延迟约为 42 个处理器周期，以确定已经发生了 L3 失误，以及关键指令的 DRAM 延迟。对于单银行 DDR4-2400 SDRAM 和 4.0 GHz CPU，DRAM 延迟约为 40 ns 或 160 个时钟循环到前 16 个字节，导致大约 200 个时钟周期的罚款总数。内存控制器以每/O 总线时钟周期的 16 字节速率填充 64 字节缓存块的其余部分，该速率又采用了 5 ns 或 20 个时钟周期。

Because the second-level cache is a write-back cache, any miss can lead to an old block being written back to memory. The i7 has a 10-entry merging write buffer that writes back dirty cache lines when the next level in the cache is unused for a read. The write buffer is checked on a miss to see if the cache line exists in the buffer; if so, the miss is filled from the buffer. A similar buffer is used between the L1 and L2 caches. If this initial instruction is a load, the data address is sent to the data cache and data TLBs, acting very much like an instruction cache access. Suppose the instruction is a store instead of a load. When the store issues, it does a data cache lookup just like a load. A miss causes the block to be placed in a write buffer because the L1 cache does not allocate the block on a write miss. On a hit, the store does not update the L1 (or L2) cache until later, after it is known to be nonspeculative. During this time, the store resides in a load-store queue, part of the out-of-order control mechanism of the processor.

> 由于第二级缓存是一个写下缓存，因此任何错过都可以导致一个旧块被写回记忆。i7 有一个 10 输入的合并写缓冲区，当缓存中的下一个级别未使用以读取时，将写入肮脏的高速缓存线。检查写入缓冲区，以查看缓冲区是否存在缓冲区中；如果是这样，则失踪是从缓冲区中填充的。L1 和 L2 缓存之间使用了类似的缓冲液。如果此初始指令是负载，则将数据地址发送到数据缓存和数据 TLB，非常类似于指令缓存访问。假设指令是商店而不是负载。当商店发行时，它会像负载一样进行数据缓存查找。小姐会导致块放在写缓冲区中，因为 L1 缓存不会分配写入错过的块。在命中率上，该商店直到稍后才更新 L1(或 L2)缓存。在此期间，该商店居住在负载店队列中，这是处理器的倒立控制机制的一部分。

The I7 also supports prefetching for L1 and L2 from the next level in the hierarchy. In most cases, the prefetched line is simply the next block in the cache. By prefetching only for L1 and L2, high-cost unnecessary fetches to memory are avoided.

> i7 还支持从层次结构的下一个级别的 L1 和 L2 进行预取。在大多数情况下，预取的线只是缓存中的下一个块。通过仅预拿到 L1 和 L2，可以避免高成本不必要的记忆。

##### _Performance of the i7 memory system_

We evaluate the performance of the i7 cache structure using the SPECint2006 benchmarks. The data in this section were collected by Professor Lu Peng and PhD student Qun Liu, both of Louisiana State University. Their analysis is based on earlier work (see [Prakash and Peng, 2008](#_bookmark991)).

> 我们使用 Specint2006 基准评估了 i7 缓存结构的性能。本节中的数据是由路易斯安那州立大学的 Lu Peng 教授和博士生 Qun Liu 收集的。他们的分析基于较早的工作(请参阅 [Prakash 和 Peng，2008](#_bookmark991))。

The complexity of the i7 pipeline, with its use of an autonomous instruction fetch unit, speculation, and both instruction and data prefetch, makes it hard to compare cache performance against simpler processors. As mentioned on page 110, processors that use prefetch can generate cache accesses independent of the memory accesses performed by the program. A cache access that is generated because of an actual instruction access or data access is sometimes called a _demand access_ to distinguish it from a _prefetch access_. Demand accesses can come from both speculative instruction fetches and speculative data accesses, some of which are subsequently canceled (see [Chapter 3](#_bookmark93) for a detailed description of speculation and instruction graduation). A speculative processor generates at least as many misses as an in-order nonspeculative processor, and typically more. In addition to demand misses, there are prefetch misses for both instructions and data.

> i7 管道的复杂性及其使用自主指令提取单元，投机以及指令和数据预取的复杂性，因此很难将缓存性能与更简单的处理器进行比较。如第 110 页所述，使用预取的处理器可以生成缓存访问，而与程序执行的内存访问无关。由于实际的指令访问或数据访问而生成的缓存访问有时被称为 _demand access_，以将其与 _prefetch access_ 区分开。需求访问可以来自投机指令提取和投机数据访问，其中一些被取消(请参阅[第 3 章](#_bookmark93)，以获取投机和指令毕业的详细说明)。投机处理器至少产生与内级非负责处理器一样多的错过，通常更多。除了需求错过外，指令和数据都有预摘要。

The i7’s instruction fetch unit attempts to fetch 16 bytes every cycle, which com- plicates comparing instruction cache miss rates because multiple instructions are fetched every cycle (roughly 4.5 on average). In fact, the entire 64-byte cache line is read and subsequent 16-byte fetches do not require additional accesses. Thus misses are tracked only on the basis of 64-byte blocks. The 32 KiB, eight-way set associative instruction cache leads to a very low instruction miss rate for the SPECint2006 programs. If, for simplicity, we measure the miss rate of SPECint2006 as the number of misses for a 64-byte block divided by the number of instructions that complete, the miss rates are all under 1% except for one benchmark (XALANCBMK), which has a 2.9% miss rate. Because a 64-byte block typically contains 16–20 instructions, the effective miss rate per instruction is much lower, depending on the degree of spatial locality in the instruction stream.

> i7 的指示提取单元试图在每个周期中获取 16 个字节，这可以比较指令缓存率，因为每个周期都会获取多个指令(平均约为 4.5 个说明)。实际上，读取了整个 64 个字节缓存线，随后的 16 个字节提取不需要其他访问。因此，仅根据 64 个字节块跟踪错过。32 个 KIB 八方集的关联指令缓存导致 Specint2006 程序的指令率非常低。如果为简单起见，我们衡量 SpecInt2006 的错率是 64 字节块的错过数，除以完成的说明数量，但未完成的指令均低于 1％以下，除了一个基准(XalancBMK)(XalancBMK)2.9％的错率。由于一个 64 字节块通常包含 16-20 个说明，因此根据说明流中空间位置的程度，每个说明的有效误差要低得多。

The frequency at which the instruction fetch unit is stalled waiting for the I-cache misses is similarly small (as a percentage of total cycles) increasing to 2% for two benchmarks and 12% for XALANCBMK, which has the highest I-cache miss rate. In the next chapter, we will see how stalls in the IFU contribute to overall reductions in pipeline throughput in the i7.

> 指令提取单元停滞不前等待 I-CACHE 遗失的频率同样很小(占总周期的百分比)，对于两个基准，XalancBMK 的频率增加到 2％，而 I-CACHE MISS 率最高的 XalancBMK。在下一章中，我们将看到 IFU 中的失速如何促进 i7 中管道吞吐量的总体减少。

The L1 data cache is more interesting and even trickier to evaluate because in addition to the effects of prefetching and speculation, the L1 data cache is not write-allocated, and writes to cache blocks that are not present are not treated as misses. For this reason, we focus only on memory reads. The performance monitor measurements in the i7 separate out prefetch accesses from demand accesses, but only keep demand accesses for those instructions that graduate. The effect of spec- ulative instructions that do not graduate is not negligible, although pipeline effects probably dominate secondary cache effects caused by speculation; we will return to the issue in the next chapter.

> L1 数据缓存更加有趣，甚至更棘手，因为除了预摘要和投机的效果外，L1 数据缓存未被编写，并且写信给不存在的缓存块，也没有被视为错过。因此，我们仅专注于内存读取。i7 中的性能监视器测量值将预取访问与需求访问分开，但仅保留那些毕业的说明的需求访问。尽管管道效应可能占据了由猜测引起的次要缓存效应，但不毕业的特定指令的影响并不能忽略不计。我们将在下一章中返回问题。

Figure 2.26 The L1 data cache miss rate for the SPECint2006 benchmarks is shown in two ways relative to the demand L1 reads: one including both demand and prefetch accesses and one including only demand accesses. The i7 separates out L1 misses for a block not present in the cache and L1 misses for a block already outstanding that is being prefetched from L2; we treat the latter group as hits because they would hit in a blocking cache. These data, like the rest in this section, were collected by Professor Lu Peng and PhD student Qun Liu, both of Louisiana State University, based on earlier studies of the Intel Core Duo and other processors (see Peng et al., 2008).

> 图 2.26 SpecInt2006 基准的 L1 数据缓存率相对于需求 L1 读取的方式显示了两种方式：一种包括需求和预取访问访问，其中一个包括需求访问。i7 分隔了 L1 遗漏，因为在缓存中不存在一个障碍物，而 L1 却错过了一个已经从 L2 预取的块；我们将后一组视为命中，因为它们会击中阻塞缓存。这些数据与本节中的其他数据一样，是由路易斯安那州立大学的 Lu Peng 教授和博士生 Qun Liu 收集的，基于对英特尔核心二人组和其他处理器的早期研究(参见 Peng 等，2008)。

To address these issues, while keeping the amount of data reasonable, [Figure 2.26](#_bookmark78) shows the L1 data cache misses in two ways:

> 为了解决这些问题，同时保持数据合理的量，[图 2.26](#_bookmark78) 以两种方式显示了 L1 数据缓存遗漏：

1. The L1 miss rate relative to demand references given by the L1 miss rate includ- ing prefetches and speculative loads/L1 demand read references for those instructions that graduate.
2. demand miss rate given by L1 demand misses/L1 demand read references, both measurements only for instructions that graduate.

> 1. L1 率相对于 L1 失误率的需求参考，包括预取预约和投机载荷/L1 需求读取参考的参考。
> 2. L1 需求错过/L1 需求的需求遗漏费率阅读参考，这两项测量仅针对毕业的指示。

On average, the miss rate including prefetches is 2.8 times as high as the demand- only miss rate. Comparing this data to that from the earlier i7 920, which had the same size L1, we see that the miss rate including prefetches is higher on the newer i7, but the number of demand misses, which are more likely to cause a stall, are usually fewer.

> 平均而言，包括预购在内的错率是需求率的 2.8 倍。将这些数据与较早的 i7 920 相同的数据进行比较，该数据具有相同大小的 L1，我们看到包括预购在内的较新的 i7 率更高，但需求误差的数量(更可能引起摊位)是通常更少。

To understand the effectiveness of the aggressive prefetch mechanisms in the i7, let’s look at some measurements of prefetching. [Figure 2.27](#_bookmark79) shows both the fraction of L2 requests that are prefetches versus demand requests and the prefetch miss rate. The data are probably astonishing at first glance: there are roughly 1.5 times as many prefetches as there are L2 demand requests, which come directly from L1 misses. Furthermore, the prefetch miss rate is amazingly high, with an average miss rate of 58%. Although the prefetch ratio varies considerably, the pre- fetch miss rate is always significant. At first glance, you might conclude that the designers made a mistake: they are prefetching too much, and the miss rate is too high. Notice, however, that the benchmarks with the higher prefetch ratios (ASTAR, BZIP2, HMMER, LIBQUANTUM, and OMNETPP) also show the greatest gap between the prefetch miss rate and the demand miss rate, more than a factor of 2 in each case. The aggressive prefetching is trading prefetch misses, which occur earlier, for demand misses, which occur later; and as a result, a pipe- line stall is less likely to occur due to the prefetching.

> 要了解 i7 侵略性预取机制的有效性，让我们看一下预取的一些测量。[图 2.27](#_bookmark79) 既显示了预取相对于要求请求的 L2 请求的分数，又显示了预取率率。乍一看，数据可能令人惊讶：预取的预取约 1.5 倍，而 L2 要求请求直接来自 L1 遗漏。此外，预摘要的错率非常高，平均失误率为 58％。尽管预取比的差异很大，但预提取率始终是显着的。乍一看，您可能会得出结论，设计师犯了一个错误：他们的预摘要太多，而错过的费率太高了。但是，请注意，预取比率较高的基准(ASTAR，BZIP2，HMMER，LIBQUANTUM 和 OMNETPP)也显示出预取率错过率和需求损失率之间的最大差距，在每种情况下都超过 2 倍。积极进取的预摘要是较早发生的预求次，以较早发生的时间发生。结果，由于预取，导管线失速不太可能发生。

Similarly, consider the high prefetch miss rate. Suppose that the majority of the prefetches are actually useful (this is hard to measure because it involves tracking individual cache blocks), then a prefetch miss indicates a likely L2 cache miss in the future. Uncovering and handling the miss earlier via the prefetch is likely to reduce the stall cycles. Performance analysis of speculative superscalars, like the i7, has shown that cache misses tend to be the primary cause of pipeline stalls, because it is hard to keep the processor going, especially for longer running L2 and L3 misses. The Intel designers could not easily increase the size of the caches with- out incurring both energy and cycle time impacts; thus the use of aggressive pre- fetching to try to lower effective cache miss penalties is an interesting alternative approach.

> 同样，考虑高预取率率。假设大多数预摘要实际上都是有用的(这很难测量，因为它涉及跟踪单个缓存块)，然后预摘要错过表示将来可能的 L2 缓存失误。较早通过预摘要发现和处理失误可能会减少失速周期。像 i7 这样的投机性超量表的性能分析表明，缓存失误往往是管道摊位的主要原因，因为很难保持处理器的运行，尤其是对于更长的运行 L2 和 L3 失误。英特尔设计师无法轻易增加缓存的大小，从而产生能量和周期时间的影响；因此，使用积极的预料来试图降低有效的高速缓存罚款是一种有趣的替代方法。

With the combination of the L1 demand misses and prefetches going to L2, roughly 17% of the loads generate an L2 request. Analyzing L2 performance requires including the effects of writes (because L2 is write-allocated), as well as the prefetch hit rate and the demand hit rate. [Figure 2.28](#_bookmark80) shows the miss rates of the L2 caches for demand and prefetch accesses, both versus the number of L1 references (reads and writes). As with L1, prefetches are a significant contributor, generating 75% of the L2 misses. Comparing the L2 demand miss rate with that of earlier i7 implementations (again with the same L2 size) shows that the i7 6700 has a lower L2 demand miss rate by an approximate factor of 2, which may well justify the higher prefetch miss rate.

> 随着 L1 需求错过和预取到 L2 的组合，大约 17％的负载产生了 L2 请求。分析 L2 性能需要包括写入的效果(因为 L2 是写入分配的)以及预取命中率和需求命中率。[图 2.28](#_bookmark80) 显示了需求和预取访问的 L2 缓存的错率，均与 L1 参考的数量(读取和写入)。与 L1 一样，预摘要是重要的贡献者，产生了 75％的 L2 错过。将 L2 的需求损失与早期的 I7 实施(同样具有相同 L2 尺寸)的需求率进行比较，这表明 i7 6700 的 L2 需求损失率较低，大约为 2 倍，这很可能证明较高的预购率错过率是合理的。

Figure 2.27 The fraction of L2 requests that are prefetches is shown via the columns and the left axis. The right axis and the line shows the prefetch hit rate. These data, like the rest in this section, were collected by Professor Lu Peng and PhD student Qun Liu, both of Louisiana State University, based on earlier studies of the Intel Core Duo and other processors (see Peng et al., 2008).

> 图 2.27 通过列和左轴显示预取的 L2 请求的分数。右轴和线显示预取命中率。这些数据与本节中的其他数据一样，是由路易斯安那州立大学的 Lu Peng 教授和博士生 Qun Liu 收集的，基于对英特尔核心二人组和其他处理器的早期研究(参见 Peng 等，2008)。

Because the cost for a miss to memory is over 100 cycles and the average data miss rate in L2 combining both prefetch and demand misses is over 7%, L3 is obvi- ously critical. Without L3 and assuming that about one-third of the instructions are loads or stores, L2 cache misses could add over two cycles per instruction to the CPI! Obviously, prefetching past L2 would make no sense without an L3.

> 由于对记忆的错过的成本超过 100 个周期，而 L2 的平均数据遗漏率结合了预摘要和需求错过的 7％以上，因此 L3 显然至关重要。没有 L3，并假设大约三分之一的说明是负载或商店，则 L2 缓存误差可以在 CPI 中增加两个周期！显然，没有 L3，预拿到过去的 L2 就没有意义。

In comparison, the average L3 data miss rate of 0.5% is still significant but less than one-third of the L2 demand miss rate and 10 times less than the L1 demand miss rate. Only in two benchmarks (OMNETPP and MCF) is the L3 miss rate above 0.5%; in those two cases, the miss rate of about 2.3% likely dominates all other performance losses. In the next chapter, we will examine the relationship between the i7 CPI and cache misses, as well as other pipeline effects.

> 相比之下，平均 L3 数据失误率为 0.5％仍然很明显，但不到 L2 需求损失率的三分之一，比 L1 需求损失率低 10 倍。仅在两个基准(OmnEtpp 和 MCF)中，L3 损失率超过 0.5％；在这两种情况下，大约 2.3％的失误可能会占主导地位。在下一章中，我们将研究 I7 CPI 和 CACHE 错过的关系以及其他管道效应之间的关系。

Figure 2.28 The L2 demand miss rate and prefetch miss rate, both shown relative to all the references to L1, which also includes prefetches, speculative loads that do not complete, and program-generated loads and stores (demand references). These data, like the rest in this section, were collected by Professor Lu Peng and PhD student Qun Liu, both of Louisiana State University.

> 图 2.28 L2 需求损失率和预摘要率，均相对于 L1 的所有参考显示，其中还包括预取的，未完成的投机负载以及程序生成的负载和商店(需求参考)。这些数据与本节中的其他数据一样，由路易斯安那州立大学的卢彭教授和博士生 Qun Liu 收集。
