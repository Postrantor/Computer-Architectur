## Putting It All Together: Performance, Price, and Power

> ##将所有内容放在一起：性能，价格和力量

In the “Putting It All Together” sections that appear near the end of every chapter, we provide real examples that use the principles in that chapter. In this section, we look at measures of performance and power-performance in small servers using the SPECpower benchmark.

> 在每章结尾处出现的“将所有内容放在一起”部分中，我们提供了使用该章中原理的真实示例。在本节中，我们使用 SpecPower 基准来研究小型服务器中性能和功率绩效的度量。

[Figure 1.20](#_bookmark31) shows the three multiprocessor servers we are evaluating along with their price. To keep the price comparison fair, all are Dell PowerEdge servers. The first is the PowerEdge R710, which is based on the Intel Xeon 85670 micro- processor with a clock rate of 2.93 GHz. Unlike the Intel Core i7-6700 in [Chapters](#_bookmark46) [2–5](#_bookmark46), which has 20 cores and a 40 MB L3 cache, this Intel chip has 22 cores and a 55 MB L3 cache, although the cores themselves are identical. We selected a two- socket system—so 44 cores total—with 128 GB of ECC-protected 2400 MHz DDR4 DRAM. The next server is the PowerEdge C630, with the same processor, number of sockets, and DRAM. The main difference is a smaller rack-mountable package: “2U” high (3.5 inches) for the 730 versus “1U” (1.75 inches) for the 630.

> [图 1.20]（#_ bookmark31）显示了我们正在评估其价格的三个多处理器服务器。为了保持价格比较公平，所有都是戴尔·鲍尔德（Dell PowerEdge）服务器。第一个是 PowerEdge R710，该 R710 基于 Intel Xeon 85670 Micro-Prokestor，时钟速率为 2.93 GHz。与[章节]（#_ bookmark46）中的 Intel Core i7-6700 不同，[2-5]（#\_ bookmark46）具有 20 个内核和 40 MB L3 缓存，该英特尔芯片具有 22 个内核和 55 MB L3 CACHE，尽管核心本身是相同的。我们选择了一个两个插座系统（总计 44 个核心），其中 128 GB 的 ECC 保护 2400 MHz DDR4 DRAM。下一个服务器是 PowerEdge C630，具有相同的处理器，插座数和 DRAM。主要区别是一个较小的机架安装套件：730 对 630 的“ 1U”（1.75 英寸）的“ 2U”高（3.5 英寸）。

> Figure 1.20 Three Dell PowerEdge servers being measured and their prices as of July 2016. We calculated the cost of the processors by subtracting the cost of a second processor. Similarly, we calculated the overall cost of memory by seeing what the cost of extra memory was. Hence the base cost of the server is adjusted by removing the estimated cost of the default processor and memory. [Chapter 5](#_bookmark0) describes how these multisocket systems are connected together, and [Chapter 6](#_bookmark0) describes how clusters are connected together.

>> 图 1.20 截至 2016 年 7 月，测量了三个 Dell PowerEdge 服务器。我们通过减去第二处理器的成本来计算处理器的成本。同样，我们通过查看额外记忆的成本来计算内存的整体成本。因此，通过删除默认处理器和内存的估计成本来调整服务器的基本成本。[第 5 章]（#_ bookmark0）描述了这些多源系统如何连接在一起，[第 6 章]（#_ bookmark0）描述了簇如何连接在一起。
>>

The third server is a cluster of 16 of the PowerEdge 630 s that is connected together with a 1 Gbit/s Ethernet switch. All are running the Oracle Java HotSpot version 1.7 Java Virtual Machine (JVM) and the Microsoft Windows Server 2012 R2 Datacenter version 6.3 operating system.

> 第三个服务器是 PowerEdge 630 s 的 16 个群集，与 1 GBIT/S 以太网开关连接在一起。所有人都在运行 Oracle Java 热点版 1.7 Java Virtual Machine（JVM）和 Microsoft Windows Server 2012 R2 Datacenter 版本 6.3 操作系统。

Note that because of the forces of benchmarking (see [Section 1.11](#_bookmark33)), these are unusually configured servers. The systems in [Figure 1.20](#_bookmark31) have little memory rel- ative to the amount of computation, and just a tiny 120 GB solid-state disk. It is inexpensive to add cores if you don’t need to add commensurate increases in mem- ory and storage!

> 请注意，由于基准测试的力（请参阅[1.11]（#_ bookmark33）），因此这些是异常配置的服务器。[图 1.20]（#_ bookmark31）中的系统几乎没有与计算量的相关性，只有一个小型 120 GB 固态磁盘。如果您不需要在 mem-ory 和存储中添加相称的增加，则添加内核是很便宜的！

Rather than run statically linked C programs of SPEC CPU, SPECpower uses a more modern software stack written in Java. It is based on SPECjbb, and it repre- sents the server side of business applications, with performance measured as the number of transactions per second, called _ssj_ops_ for _server side Java operations per second_. It exercises not only the processor of the server, as does SPEC CPU, but also the caches, memory system, and even the multiprocessor interconnection system. In addition, it exercises the JVM, including the JIT runtime compiler and garbage collector, as well as portions of the underlying operating system.

> SpecPower 并没有运行 Spec CPU 的静态链接 C 程序，而是使用 Java 编写的更现代的软件堆栈。它基于 SPECJBB，并代表业务应用程序的服务器端，绩效衡量为每秒的交易数，称为 *ssj_ops* for \_server side Java 每秒操作。它不仅可以像规格 CPU 一样练习服务器的处理器，还可以练习缓存，内存系统甚至多处理器互连系统。此外，它可以行使 JVM，包括 JIT 运行时编译器和垃圾收集器以及基础操作系统的一部分。

As the last two rows of [Figure 1.20](#_bookmark31) show, the performance winner is the cluster of 16 R630s, which is hardly a surprise since it is by far the most expensive. The price-performance winner is the PowerEdge R630, but it barely beats the cluster at 213 versus 211 ssj-ops. Amazingly, the 16 node cluster is within 1% of the same price-performances of a single node despite being 16 times as large.

> 正如[图 1.20]（#\_ bookmark31）节目的最后两行，表演冠军是 16 R630S 的集群，这并不令人惊讶，因为它是迄今为止最昂贵的。价格绩效的获胜者是 PowerEdge R630，但它几乎没有以 213 对与 211 SSJ-OPS 的群体击败集群。令人惊讶的是，尽管 16 个节点群集在单个节点的相同价格绩效的 1％以内，尽管大于 16 倍。

While most benchmarks (and most computer architects) care only about per- formance of systems at peak load, computers rarely run at peak load. Indeed, Figure 6.2 in [Chapter 6](#_bookmark268) shows the results of measuring the utilization of tens of thousands of servers over 6 months at Google, and less than 1% operate at an aver- age utilization of 100%. The majority have an average utilization of between 10% and 50%. Thus the SPECpower benchmark captures power as the target workload varies from its peak in 10% intervals all the way to 0%, which is called Active Idle. [Figure 1.21](#_bookmark32) plots the ssj_ops (SSJ operations/second) per watt and the average power as the target load varies from 100% to 0%. The Intel R730 always has the lowest power and the single node R630 has the best ssj_ops per watt across each target workload level. Since watts joules/second, this metric is proportional to

> 虽然大多数基准测试（和大多数计算机架构师）仅在峰值负载下关心系统的实现，但计算机很少在峰值负载下运行。实际上，图 6.2 在[第 6 章]（#_ bookmark268）中显示了在 Google 6 个月内测量数千台服务器利用的结果，而在平均利用率为 100％的情况下不到 1％。大多数人的平均利用率在 10％至 50％之间。因此，SpecPower 基准测试将捕获功率，因为目标工作负载从其峰值以 10％的间隔而变化为 0％，这称为活动空闲。[图 1.21]（#_ bookmark32）绘制每瓦的 SSJ_OPS（SSJ 操作/秒），并且由于目标负载从 100％到 0％不等，平均功率。英特尔 R730 始终具有最低的功率，并且在每个目标工作负载级别上，单节点 R630 的每瓦最佳 SSJ_OPS。由于瓦茨焦耳/秒，该指标与

> Figure 1.21 Power-performance of the three servers in [Figure 1.20](#_bookmark31). Ssj_ops/watt values are on the left axis, with the three columns associated with it, and watts are on the right axis, with the three lines associated with it. The hor- izontal axis shows the target workload, as it varies from 100% to Active Idle. The single node R630 has the best ssj_ops/watt at each workload level, but R730 consumes the lowest power at each level.

>> 图 1.21 [图 1.20]（#\_ bookmark31）中的三个服务器的功率表现。ssj_ops/watt 值在左轴上，与之关联的三列，瓦特在右轴上，与之相关的三行。HOR-IZONTAL 轴显示目标工作负载，因为它从 100％到主动空闲不等。单个节点 R630 在每个工作负载级别具有最佳的 SSJ_OPS/瓦，但是 R730 在每个级别上都消耗最低功率。
>>

To calculate a single number to use to compare the power efficiency of sys- tems, SPECpower uses

> 为了计算一个用于比较系统的功率效率的单个数字，SpecPower 使用

Overall ssj_ops/watt = Xssj_ops

> 总体 ssj_ops/watt = xssj_ops

The overall ssj_ops/watt of the three servers is 10,802 for the R730, 11,157 for the R630, and 10,062 for the cluster of 16 R630s. Therefore the single node R630 has the best power-performance. Dividing by the price of the servers, the ssj_ops/watt/ `$1,000` is 879 for the R730, 899 for the R630, and 789 (per node) for the 16-node cluster of R630s. Thus, after adding power, the single-node R630 is still in first place in performance/price, but now the single-node R730 is significantly more efficient than the 16-node cluster.

> R730 的三个服务器的总 SSJ_OPS/瓦为 10,802，R630 为 11,157，16 R630 的群集为 10,062。因此，单节点 R630 具有最佳的功率性能。除以服务器的价格，SSJ_OPS/ WATT/ $ 1,000` 为 R730 为 879、899，R630 为 R630 的 16 节点群集为 789（每个节点）。因此，在添加功率之后，单节点 R630 在性能/价格中仍然排名第一，但是现在单节点 R730 比 16 节点群集高得多。
