## Concluding Remarks: Looking Ahead

> ##结论：展望未来

Over the past thirty years there have been several predictions of the eminent \[sic\] cessation of the rate of improvement in computer performance. Every such pre- diction was wrong. They were wrong because they hinged on unstated assump- tions that were overturned by subsequent events. So, for example, the failure to foresee the move from discrete components to integrated circuits led to a predic- tion that the speed of light would limit computer speeds to several orders of mag- nitude slower than they are now. Our prediction of the memory wall is probably wrong too but it suggests that we have to start thinking “out of the box.”

> 在过去的三十年中，已经有几个预测，即对计算机性能的提高率的重要\ [SIC \]。每个这样的词典都是错误的。他们之所以错过，是因为他们铰接了随后事件所推翻的未陈述。因此，例如，未能预见从离散组件到集成电路的移动导致一个预测，光速将计算机速度将计算机速度限制在比现在慢的几个级别范围内。我们对记忆墙的预测可能也是错误的，但这表明我们必须开始思考“开箱即用”。

Wm. A. Wulf and Sally A. McKee,

> Wm。A. Wulf 和 Sally A. McKee，

_Hitting the Memory Wall: Implications of the Obvious,_

> _ hitting 记忆墙：明显的含义，_

Department of Computer Science, University of Virginia (December 1994).

> 弗吉尼亚大学计算机科学系（1994 年 12 月）。

This paper introduced the term _memory wall._

> 本文介绍了术语* Memory Wall.*

The possibility of using a memory hierarchy dates back to the earliest days of general-purpose digital computers in the late 1940s and early 1950s. Virtual mem- ory was introduced in research computers in the early 1960s and into IBM main- frames in the 1970s. Caches appeared around the same time. The basic concepts have been expanded and enhanced over time to help close the access time gap between main memory and processors, but the basic concepts remain.

> 使用内存层次结构的可能性可以追溯到 1940 年代末和 1950 年代初的通用数字计算机的最早时代。虚拟 mem-在 1960 年代初在研究计算机中引入了 1970 年代的 IBM 主框架。缓存大约同时出现。随着时间的流逝，基本概念已经扩展和增强，以帮助缩小主内存和处理器之间的访问时间差距，但基本概念仍然存在。

One trend that is causing a significant change in the design of memory hierar- chies is a continued slowdown in both density and access time of DRAMs. In the past 15 years, both these trends have been observed and have been even more obvi- ous over the past 5 years. While some increases in DRAM bandwidth have been achieved, decreases in access time have come much more slowly and almost van- ished between DDR4 and DDR3. The end of Dennard scaling as well as a slow- down in Moore’s Law both contributed to this situation. The trenched capacitor design used in DRAMs is also limiting its ability to scale. It may well be the case that packaging technologies such as stacked memory will be the dominant source of improvements in DRAM access bandwidth and latency.

> 导致记忆层设计的显着变化的一种趋势是，DRAM 的密度和访问时间都在继续放缓。在过去的 15 年中，这两种趋势都已被观察到，并且在过去的 5 年中更加明显。尽管已经达到了 DRAM 带宽的一些增加，但在 DDR4 和 DDR3 之间，访问时间的减少速度却慢得多，并且几乎可以进行。丹纳德（Dennard）规模的结束以及摩尔法律的缓慢降低都促成了这种情况。DRAM 中使用的挖沟电容器设计也限制了其扩展能力。很可能是包装技术（例如堆叠内存）将成为 DRAM 访问带宽和延迟的主要改进来源。

Independently of improvements in DRAM, Flash memory has been playing a much larger role. In PMDs, Flash has dominated for 15 years and became the stan- dard for laptops almost 10 years ago. In the past few years, many desktops have shipped with Flash as the primary secondary storage. Flash’s potential advantage over DRAMs, specifically the absence of a per-bit transistor to control writing, is also its Achilles heel. Flash must use bulk erase-rewrite cycles that are consider- ably slower. As a result, although Flash has become the fastest growing form of secondary storage, SDRAMs still dominate for main memory.

> 闪存独立于 DRAM 的改进，扮演着更大的角色。在 PMD 中，Flash 已经占据了 15 年的统治地位，并在 10 年前成为笔记本电脑的 Standard。在过去的几年中，许多台式机以 Flash 作为主要次要存储。Flash 比 DRAM 的潜在优势，特别是没有每位晶体管控制写作的潜在优势，也是其致命弱点。Flash 必须使用散装擦除 - 剥离循环，这些循环要慢得多。结果，尽管 Flash 已成为辅助存储的增长最快的形式，但 SDRAM 仍然占主导地位。

Although phase-change materials as a basis for memory have been around for a while, they have never beenserious competitors either formagnetic disksorfor Flash. The recent announcement by Intel and Micron of the cross-point technology may change this. The technology appears tohaveseveral advantagesover Flash, including the elimination of the slow erase-to-write cycle and greater longevity in terms. It could be that this technology will finally be the technology that replaces the electro- mechanical disks that have dominated bulk storage for more than 50 years!

> 尽管相变材料作为记忆的基础已经存在了一段时间，但它们从来都不是响应磁盘闪光灯的竞争者。Intel 和 Micron 最近公告的交叉点技术可能会改变这一点。该技术似乎是 Tohaveseveral 的优势闪光灯，包括消除缓慢的擦除到写周期和术语上的寿命更长。这项技术可能最终将是取代已经占主导存储超过 50 年的电力磁盘的技术！

For some years, a variety of predictions have been made about the coming memory wall (see previously cited quote and paper), which would lead to serious limits on processor performance. Fortunately, the extension of caches to multiple levels (from 2 to 4), more sophisticated refill and prefetch schemes, greater com- piler and programmer awareness of the importance of locality, and tremendous improvements in DRAM bandwidth (a factor of over 150 times since the mid- 1990s) have helped keep the memory wall at bay. In recent years, the combination of access time constraints on the size of L1 (which is limited by the clock cycle) and energy-related limitations on the size of L2 and L3 have raised new challenges. The evolution of the i7 processor class over 6–7 years illustrates this: the caches are the same size in the i7 6700 as they were in the first generation i7 processors! The more aggressive use of prefetching is an attempt to overcome the inability to increase L2 and L3. Off-chip L4 caches are likely to become more important because they are less energy-constrained than on-chip caches.

> 几年来，已经对即将到来的记忆墙做出了各种预测（请参阅先前引用的报价和纸张），这将导致严重限制处理器性能。幸运的是，将缓存扩展到多个级别（从 2 到 4），更复杂的补充和预取方案，更大的堆积者和程序员对当地重要性的认识以及 DRAM 带宽的巨大改进（自从自从以来的一个因素超过 150 倍）1990 年代中期）有助于使记忆墙陷入困境。近年来，访问时间限制对 L1 的大小（受时钟周期的限制）以及与 L2 和 L3 大小相关的限制的组合提出了新的挑战。i7 处理器类的演变超过 6 - 7 年，说明了这一点：在 i7 6700 中，缓存的大小与第一代 i7 处理器相同！预摘要的更具积极用途是试图克服无法增加 L2 和 L3 的尝试。芯片外 L4 缓存可能会变得更加重要，因为它们比片上缓存的能量受限少。

In addition to schemes relying on multilevel caches, the introduction of out-of- order pipelines with multiple outstanding misses has allowed available instruction- level parallelism to hide the memory latency remaining in a cache-based system. The introduction of multithreading and more thread-level parallelism takes this a step further by providing more parallelism and thus more latency-hiding opportunities. It is likely that the use of instruction- and thread-level parallelism will be a more important tool in hiding whatever memory delays are encountered in modern multilevel cache systems.

> 除了依靠多级缓存的方案外，引入了具有多个未出色错过的订单外管道，还允许可用的指导级并行性隐藏基于缓存系统中的存储潜伏期。多线程和更多线程级并行性的引入通过提供更多的并行性，从而更进一步，从而更进一步。使用指令和线程级并行性的使用可能是隐藏现代多级缓存系统中遇到的任何内存延迟的更重要工具。

One idea that periodically arises is the use of programmer-controlled scratch- pad or other high-speed visible memories, which we will see are used in GPUs. Such ideas have never made the mainstream in general-purpose processors for sev- eral reasons: First, they break the memory model by introducing address spaces with different behavior. Second, unlike compiler-based or programmer-based cache optimizations (such as prefetching), memory transformations with scratch- pads must completely handle the remapping from main memory address space to the scratchpad address space. This makes such transformations more difficult and limited in applicability. In GPUs (see [Chapter 4](#_bookmark165)), where local scratchpad memories are heavily used, the burden for managing them currently falls on the programmer. For domain-specific software systems that can use such memories, the perfor- mance gains are very significant. It is likely that HBM technologies will thus be used for caching in large, general-purpose computers and quite possibility as the main working memories in graphics and similar systems. As domain-specific architectures become more important in overcoming the limitations arising from the end of Dennard’s Law and the slowdown in Moore’s Law (see [Chapter 7](#_bookmark322)), scratchpad memories and vector-like register sets are likely to see more use.

> 定期出现的一个想法是使用程序员控制的刮擦垫或其他高速可见记忆，我们将在 GPU 中使用。这种想法从未成为通用处理器中的主流，原因有几个原因：首先，它们通过引入不同行为的地址空间来打破内存模型。其次，与基于编译器或基于程序员的缓存优化（例如预取）不同，带有 SCRATCH-PADS 的内存转换必须完全处理从主存储地址空间到 ScratchPad 地址空间的重新映射。这使得这种转换更加困难和有限。在 GPU 中（请参阅[第 4 章]（#_ bookmark165）），在当地的 ScratchPad 记忆中大量使用，当前管理它们的负担属于程序员。对于可以使用此类记忆的特定域特异性软件系统，绩效增长非常重要。因此，HBM 技术可能会用于在大型通用计算机中缓存，并且可能是图形和类似系统中的主要工作记忆。随着特定于领域的架构在克服丹纳德定律终结以及摩尔定律的放缓（见[第 7 章]（#_ bookmark322））的局限性方面变得越来越重要，因此，ScratchPad 记忆和类似矢量的寄存器集可能会看到更多利用。

The implications of the end of Dennard’s Law affect both DRAM and proces- sor technology. Thus, rather than a widening gulf between processors and main memory, we are likely to see a slowdown in both technologies, leading to slower overall growth rates in performance. New innovations in computer architecture and in related software that together increase performance and efficiency will be key to continuing the performance improvements seen over the past 50 years.

> 丹纳德定律终结的含义都会影响 DRAM 和 Proces-SOR 技术。因此，我们可能会看到两种技术的放缓，而不是在处理器和主要内存之间扩大鸿沟，从而导致性能的总体增长率较低。计算机体系结构和相关软件的新创新，共同提高性能和效率将是继续在过去 50 年中继续进行的性能改进的关键。
