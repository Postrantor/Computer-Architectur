## Concluding Remarks(总结)

This chapter has introduced a number of concepts and provided a quantitative framework that we will expand on throughout the book. Starting with the last edition, energy efficiency is the constant companion to performance.

> 本章介绍了许多概念，并提供了一个定量框架，我们将在整本书中扩展。从上一版本开始，能源效率是性能的持续伴侣。

In [Chapter 2](#_bookmark46), we start with the all-important area of memory system design. We will examine a wide range of techniques that conspire to make memory look infinitely large while still being as fast as possible. ([Appendix B](#_bookmark436) provides introductory material on caches for readers without much experience and background with them.) As in later chapters, we will see that hardware-software cooperation has become a key to high-performance memory systems, just as it has to highperformance pipelines. This chapter also covers virtual machines, an increasingly important technique for protection.

> 在[第 2 章](#_bookmark46)中，我们从内存系统设计的最重要区域开始。我们将研究多种技术，使记忆看起来无限大，同时仍然尽可能快。([[附录 B](#_bookmark436) 为读者提供介绍性材料，没有太多的经验和背景。)与以后的章节一样，我们将看到硬件软件合作已成为高性能记忆系统的关键，只是只有因为它必须提高性能管道。本章还涵盖了虚拟机，这是一种越来越重要的保护技术。

In [Chapter 3](#_bookmark93), we look at ILP, of which pipelining is the simplest and most common form. Exploiting ILP is one of the most important techniques for building high-speed uniprocessors. [Chapter 3](#_bookmark93) begins with an extensive discussion of basic concepts that will prepare you for the wide range of ideas examined in both chapters. [Chapter 3](#_bookmark93) uses examples that span about 40 years, drawing from one of the first supercomputers (IBM 360/91) to the fastest processors on the market in 2017. It emphasizes what is called the _dynamic_ or _runtime approach_ to exploiting ILP. It also talks about the limits to ILP ideas and introduces multithreading, which is further developed in both Chapters [4](#_bookmark165) and [5](#_bookmark213). [Appendix C](#_bookmark481) provides introductory material on pipelining for readers without much experience and background in pipelining. (We expect it to be a review for many readers, including those of our introductory text, _Computer Organization and Design: The Hardware/Software Interface_.)

> 在[第 3 章](#_bookmark93)中，我们查看了 ILP，哪种管道是最简单，最常见的形式。利用 ILP 是建造最重要的技术之一，高速单层处理器。[第 3 章](#_bookmark93)从对基本概念的广泛讨论开始，这些概念将为您准备两章中研究的广泛想法。[第 3 章](#_bookmark93)使用大约 40 年的示例，从第一个超级计算机之一(IBM 360/91)绘制到 2017 年市场上最快的处理器。利用 ILP。它还谈到了 ILP 想法的限制并介绍了多线程，这在两个章节 [4](#_bookmark165) 和 [5](#_bookmark213) 中进一步开发。[附录 C](#_bookmark481) 为读者提供介绍性材料，供读者提供很多经验和背景。(我们希望这是许多读者的评论，包括我们的介绍性文本，\_Computer 组织和设计：硬件/软件接口\_)。

[Chapter 4](#_bookmark165) explains three ways to exploit data-level parallelism. The classic and oldest approach is vector architecture, and we start there to lay down the principles of SIMD design. (Appendix G goes into greater depth on vector architectures.) We next explain the SIMD instruction set extensions found in most desktop microprocessors today. The third piece is an in-depth explanation of how modern graphics processing units (GPUs) work. Most GPU descriptions are written from the programmer’s perspective, which usually hides how the computer really works. This section explains GPUs from an insider’s perspective, including a mapping between GPU jargon and more traditional architecture terms.

> [第 4 章](#_bookmark165)解释了利用数据级并行性的三种方法。经典和最古老的方法是矢量体系结构，我们从那里开始，以制定 SIMD 设计的原理。(附录 G 对向量体系结构进行了更大的深度。)接下来，我们将解释当今大多数桌面微处理器中发现的 SIMD 指令集扩展名。第三部分是对现代图形处理单元(GPU)如何工作的深入解释。大多数 GPU 描述都是从程序员的角度写的，这通常隐藏了计算机的真正工作方式。本节从内部人的角度说明了 GPU，包括 GPU 术语和更传统的架构术语之间的映射。

[Chapter 5](#_bookmark213) focuses on the issue of achieving higher performance using multiple processors, or multiprocessors. Instead of using parallelism to overlap individual instructions, multiprocessing uses parallelism to allow multiple instruction streams to be executed simultaneously on different processors. Our focus is on the dominant form of multiprocessors, shared-memory multiprocessors, though we introduce other types as well and discuss the broad issues that arise in any multiprocessor. Here again we explore a variety of techniques, focusing on the important ideas first introduced in the 1980s and 1990s.

> [第 5 章](#_bookmark213)重点介绍使用多个处理器或多处理器实现较高性能的问题。多处理不是使用并行性重叠单个指令，而是使用并行性来允许在不同的处理器上同时执行多个指令流。我们的重点是多处理器的主要形式，共享的内存多处理器，尽管我们也介绍了其他类型，并讨论了任何多处理器中出现的广泛问题。在这里，我们再次探索了各种技术，重点是 1980 年代和 1990 年代首次引入的重要思想。

[Chapter 6](#_bookmark268) introduces clusters and then goes into depth on WSCs, which computer architects help design. The designers of WSCs are the professional descendants of the pioneers of supercomputers, such as Seymour Cray, in that they are designing extreme computers. WSCs contain tens of thousands of servers, and the equipment and the building that holds them cost nearly `$200` million. The concerns of price-performance and energy efficiency of the earlier chapters apply to WSCs, as does the quantitative approach to making decisions.

> [第 6 章](#_bookmark268)介绍了簇，然后深入到 WSC 上，计算机架构师帮助设计。WSC 的设计师是超级计算机先驱(例如 Seymour Cray)的专业后代，因为他们正在设计极端的计算机。WSC 包含成千上万的服务器，设备和建筑物的售价近 2 亿美元。早期章节的价格绩效和能源效率的关注也适用于 WSC，以及做出决策的定量方法。

[Chapter 7](#_bookmark322) is new to this edition. It introduces domain-specific architectures as the only path forward for improved performance and energy efficiency given the end of Moore’s Law and Dennard scaling. It offers guidelines on how to build effective domain-specific architectures, introduces the exciting domain of deep neural networks, describes four recent examples that take very different approaches to accelerating neural networks, and then compares their cost-performance.

> [第 7 章](#_bookmark322)是此版本的新事物。鉴于摩尔定律和丹纳德缩放的结束，它将特定于领域的体系结构引入了唯一提高性能和能源效率的途径。它提供了有关如何构建有效域特异性架构，引入深度神经网络的激动人心的领域的指南，描述了四个最近采用非常不同的方法来加速神经网络的方法，然后比较其成本的性能。

This book comes with an abundance of material online (see [Preface](#_bookmark1) for more details), both to reduce cost and to introduce readers to a variety of advanced topics. [Figure 1.25](#_bookmark38) shows them all. Appendices A–C, which appear in the book, will be a review for many readers.

> 本书在线附带丰富的材料(有关更多详细信息，请参见[前言](#_bookmark1))，既可以降低成本，又要向读者介绍各种高级主题。[图 1.25](#_bookmark38) 向他们展示了全部。本书中出现的附录 A – C 将为许多读者提供评论。

Figure 1.25 List of appendices.

In Appendix D, we move away from a processor-centric view and discuss issues in storage systems. We apply a similar quantitative approach, but one based on observations of system behavior and using an end-to-end approach to performance analysis. This appendix addresses the important issue of how to store and retrieve data efficiently using primarily lower-cost magnetic storage technologies. Our focus is on examining the performance of disk storage systems for typical I/O-intensive workloads, such as the OLTP benchmarks mentioned in this chapter. We extensively explore advanced topics in RAID-based systems, which use redundant disks to achieve both high performance and high availability. Finally, Appendix D introduces queuing theory, which gives a basis for trading off utilization and latency.

> 在附录 D 中，我们远离以处理器为中心的视图，并讨论存储系统中的问题。我们采用类似的定量方法，但基于对系统行为的观察以及使用端到端的性能分析方法。本附录解决了如何使用主要较低成本的磁性存储技术有效地存储和检索数据的重要问题。我们的重点是检查用于典型 I/O 密集型工作负载的磁盘存储系统的性能，例如本章中提到的 OLTP 基准。我们广泛地探索了基于 RAID 的系统中的高级主题，这些系统使用冗余磁盘来实现高性能和高可用性。最后，附录 D 介绍了排队理论，该理论为使用利用率和延迟提供了依据。

Appendix E applies an embedded computing perspective to the ideas of each of the chapters and early appendices.

> 附录 E 将嵌入式计算观点应用于每个章节和早期附录的想法。

Appendix F explores the topic of system interconnect broadly, including wide area and system area networks that allow computers to communicate.

> 附录 F 会广泛探索系统互连的主题，包括允许计算机通信的广泛区域和系统区域网络。

Appendix H reviews VLIW hardware and software, which, in contrast, are less popular than when EPIC appeared on the scene just before the last edition.

> 附录 H 评论 VLIW 硬件和软件，相比之下，它比 Epic 出现在上一版之前的现场时不那么受欢迎。

Appendix I describes large-scale multiprocessors for use in high-performance computing.

> 附录 I 描述了用于高性能计算的大规模多处理器。

Appendix J is the only appendix that remains from the first edition, and it covers computer arithmetic.

> 附录 J 是第一版中唯一保留的附录，它涵盖了计算机算术。

Appendix K provides a survey of instruction architectures, including the 80x86, the IBM 360, the VAX, and many RISC architectures, including ARM, MIPS, Power, RISC-V, and SPARC.

> 附录 K 提供了指导体系结构的调查，包括 80x86，IBM 360，VAX 和许多 RISC 架构，包括 ARM，MIPS，Power，Power，Risc-V 和 Sparc。

Appendix L is new and discusses advanced techniques for memory management, focusing on support for virtual machines and design of address translation for very large address spaces. With the growth in cloud processors, these architectural enhancements are becoming more important.

> 附录 L 是新的，并讨论了用于内存管理的高级技术，重点介绍了对虚拟机的支持以及针对非常大的地址空间的地址翻译设计。随着云处理器的增长，这些建筑增强功能变得越来越重要。

We describe Appendix M next.

> 我们下一步描述附录 M。
