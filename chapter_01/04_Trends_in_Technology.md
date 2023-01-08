## Trends in Technology

> ##技术趋势

If an instruction set architecture is to prevail, it must be designed to survive rapid changes in computer technology. After all, a successful new instruction set architecture may last decades—for example, the core of the IBM mainframe has been in use for more than 50 years. An architect must plan for technology changes that can increase the lifetime of a successful computer.

> 如果要占上风，则必须将其设计为在计算机技术的快速变化中生存。毕竟，成功的新指令集架构可能持续了几十年，例如，IBM 大型机的核心已经使用了 50 多年。建筑师必须计划可以增加成功计算机寿命的技术变更。

To plan for the evolution of a computer, the designer must be aware of rapid changes in implementation technology. Five implementation technologies, which change at a dramatic pace, are critical to modern implementations:

> 为了计划计算机的演变，设计师必须意识到实施技术的快速变化。五种以显着速度改变的实施技术对于现代实施至关重要：

_Integrated circuit logic technology_—Historically, transistor density increased by about 35% per year, quadrupling somewhat over four years. Increases in die size are less predictable and slower, ranging from 10% to 20% per year. The combined effect was a traditional growth rate in transistor count on a chip of about 40%–55% per year, or doubling every 18–24 months. This trend is popularly known as Moore’s Law. Device speed scales more slowly, as we discuss below. Shockingly, Moore’s Law is no more. The number of devices per chip is still increasing, but at a decelerating rate. Unlike in the Moore’s Law era, we expect the doubling time to be stretched with each new technology generation.

> \_集成电路逻辑技术 - 从历史上讲，晶体管密度每年增加约 35％，在四年内稍微增加了三倍。死亡大小的增加较不可预测和较慢，每年从 10％到 20％不等。综合效果是晶体管的传统增长率，每年约 40％–55％，或每 18-24 个月增加一倍。这种趋势通常被称为摩尔定律。正如我们在下面讨论的那样，设备速度的缩放更慢。令人震惊的是，摩尔的定律已经不复存在了。每个芯片设备的数量仍在增加，但速率下降。与摩尔法律时代不同，我们预计每一代新技术都会延长两倍的时间。

_Semiconductor DRAM_ (dynamic random-access memory)—This technology is the foundation of main memory, and we discuss it in [Chapter 2](#_bookmark46). The growth of DRAM has slowed dramatically, from quadrupling every three years as in the past. The 8-gigabit DRAM was shipping in 2014, but the 16-gigabit DRAM won’t reach that state until 2019, and it looks like there will be no 32-gigabit DRAM ([Kim, 2005](#_bookmark969)). [Chapter 2](#_bookmark46) mentions several other technologies that may replace DRAM when it hits its capacity wall.

> _spoomendoductor DRAM_（动态随机访问记忆） - 这项技术是主内存的基础，我们在[第 2 章]（#_ bookmark46）中进行了讨论。DRAM 的增长急剧放缓，从过去的三年中进行四倍。8 gibit DRAM 在 2014 年发货，但直到 2019 年才能达到该州，看来没有 32 gigabit DRAM（[Kim，2005]（#_ bookmark969））。[第 2 章]（#_bookmark46）提到了其他几种在撞击其容量墙时可能会取代 DRAM 的技术。

_Semiconductor Flash_ (electrically erasable programmable read-only memory)—This nonvolatile semiconductor memory is the standard storage device in PMDs, and its rapidly increasing popularity has fueled its rapid growth rate in capacity. In recent years, the capacity per Flash chip increased by about 50%–60% per year, doubling roughly every 2 years. Currently, Flash memory is 8–10 times cheaper per bit than DRAM. [Chapter 2](#_bookmark46) describes Flash memory.

> _speadoductor flash_（可擦除的可编程可读取记忆） - 这种非挥发性半导体内存是 PMD 中的标准存储设备，其迅速增加的受欢迎程度促进了其容量的快速增长率。近年来，每年闪光芯片的容量每年增加约 50％–60％，大约每 2 年增加一倍。目前，闪存比 DRAM 便宜的每位便宜 8-10 倍。[第 2 章]（#_bookmark46）描述了闪存。

_Magnetic disk technology_—Prior to 1990, density increased by about 30% per year, doubling in three years. It rose to 60% per year thereafter, and increased to 100% per year in 1996. Between 2004 and 2011, it dropped back to about 40% per year, or doubled every two years. Recently, disk improvement has slowed to less than 5% per year. One way to increase disk capacity is to add more platters at the same areal density, but there are already seven platters within the one-inch depth of the 3.5-inch form factor disks. There is room for at most one or two more platters. The last hope for real density increase is to use a small laser on each disk read-write head to heat a 30 nm spot to 400°C so that it can be written magnetically before it cools. It is unclear whether Heat Assisted Magnetic Recording can be manufactured economically and reliably, although Seagate announced plans to ship HAMR in limited production in 2018. HAMR is the last chance for continued improvement in areal density of hard disk drives, which are now 8–10 times cheaper per bit than Flash and 200–300 times cheaper per bit than DRAM. This technology is central to serverand warehouse-scale storage, and we discuss the trends in detail in Appendix D.

> _磁盘技术_- 1990 年，密度每年增加约 30％，在三年内翻了一番。此后每年上升到每年 60％，在 1996 年每年增加到 100％。在 2004 年至 2011 年之间，每年下降到每年约 40％，或每两年翻一番。最近，磁盘的改进速度已减慢至每年不到 5％。增加磁盘容量的一种方法是在相同的面积密度下添加更多的盘子，但是在 3.5 英寸的外形磁盘的一英寸深度内已经有七个盘子。最多有一个或两个拼盘的空间。实际密度增加的最后希望是在每个磁盘上读取头上使用一个小的激光器将 30 nm 的斑点加热到 400°C，以便在冷却之前可以磁写入。目前尚不清楚是否可以在经济和可靠地制造热辅助磁性记录，尽管 Seagate 宣布计划在 2018 年将 Hamr 运送到有限的生产中。Hamr 是硬盘驱动器中心密度持续提高的最后机会，现在是 8-10 每位比闪光灯便宜的倍数，每位便宜 200-300 倍。该技术对于服务器和仓库规模存储至关重要，我们在附录 D 中详细讨论了趋势。

_Network technology_—Network performance depends both on the performance of switches and on the performance of the transmission system. We discuss the trends in networking in Appendix F. These rapidly changing technologies shape the design of a computer that, with speed and technology enhancements, may have a lifetime of 3–5 years. Key technologies such as Flash change sufficiently that the designer must plan for these changes. Indeed, designers often design for the next technology, knowing that, when a product begins shipping in volume, the following technology may be the most cost-effective or may have performance advantages. Traditionally, cost has decreased at about the rate at which density increases.

> _network Technology_-网络性能既取决于开关的性能和传输系统的性能。我们讨论了附录 F 中网络的趋势。这些迅速变化的技术塑造了计算机的设计，该设计具有速度和技术的增强功能，其寿命可能为 3 - 5 年。诸如 Flash 之类的关键技术发生了足够的变化，设计师必须为这些更改计划。的确，设计师经常为下一项技术设计，知道当产品开始发货时，以下技术可能是最具成本效益或可能具有性能优势的。传统上，成本的下降幅度约为密度增加的速度。

Although technology improves continuously, the impact of these increases can be in discrete leaps, as a threshold that allows a new capability is reached. For example, when MOS technology reached a point in the early 1980s where between 25,000 and 50,000 transistors could fit on a single chip, it became possible to build a single-chip, 32-bit microprocessor. By the late 1980s, first-level caches could go on a chip. By eliminating chip crossings within the processor and between the processor and the cache, a dramatic improvement in cost-performance and energyperformance was possible. This design was simply unfeasible until the technology reached a certain point. With multicore microprocessors and increasing numbers of cores each generation, even server computers are increasingly headed toward a single chip for all processors. Such technology thresholds are not rare and have a significant impact on a wide variety of design decisions.

> 尽管技术不断提高，但这些增加的影响可能会以离散的飞跃为基础，因为阈值可以达到新的能力。例如，当 MOS 技术在 1980 年代初达到 25,000 至 50,000 个晶体管可以放在单个芯片上时，就可以建造一个单芯片，32 位的微处理器。到 1980 年代后期，一级缓存可能会芯片。通过消除处理器内以及处理器和高速缓存之间的芯片交叉，可以显着提高成本效果和能量性能。直到技术达到一定程度，这种设计才是不可行的。随着多层微处理器和每一代芯的越来越多的核心，即使是服务器计算机也越来越多地朝所有处理器的单个芯片迈进。这样的技术阈值并不罕见，并且会对各种设计决策产生重大影响。

### Performance Trends: Bandwidth Over Latency

> ###性能趋势：潜伏期的带宽

As we shall see in [Section 1.8](#measuring-reporting-and-summarizing-performance), _bandwidth_ or _throughput_ is the total amount of work done in a given time, such as megabytes per second for a disk transfer. In contrast, _latency_ or _response time_ is the time between the start and the completion of an event, such as milliseconds for a disk access. [Figure 1.9](#_bookmark16) plots the relative improvement in bandwidth and latency for technology milestones for microprocessors, memory, networks, and disks. [Figure 1.10](#_bookmark17) describes the examples and milestones in more detail.

> 正如我们将在[第 1.8 节]中看到的（＃测量 - 报告和夏令化表现），*bandWidth* 或 *throughput* 是给定时间内完成的总数，例如磁盘传输的每秒兆字节。相反，*latency* 或 *response Time* 是事件的开始和完成之间的时间，例如用于磁盘访问的毫秒。[图 1.9]（#_ bookmark16）绘制微处理器，内存，网络和磁盘的技术里程碑的带宽和延迟的相对改进。[图 1.10]（#_ bookmark17）更详细地描述了示例和里程碑。

Performance is the primary differentiator for microprocessors and networks, so they have seen the greatest gains: 32,000–40,000 in bandwidth and 50–90 in latency. Capacity is generally more important than performance for memory and disks, so capacity has improved more, yet bandwidth advances of 400–2400 are still much greater than gains in latency of 8–9 .

> 性能是微处理器和网络的主要区别因素，因此他们看到了最大的收益：带宽 32,000–40,000，延迟 50-90。对于记忆和磁盘而言，容量通常比性能更重要，因此容量的提高了，但带宽的进步 400-2400 仍然比延迟的增长远大得多。

Clearly, bandwidth hasoutpaced latencyacrossthese technologies andwilllikely continue to do so. A simple rule of thumb is that bandwidth grows by at least the square of the improvement in latency. Computer designers should plan accordingly.

> 显然，带宽已经采用了潜伏期的技术，并且将继续这样做。一个简单的经验法则是，带宽至少在延迟改善的方面增长。计算机设计人员应该相应地计划。

> Figure 1.9 Log-log plot of bandwidth and latency milestones in [Figure 1.10](#_bookmark17) relative to the first milestone. Note that latency improved 8–91 , while bandwidth improved about 400–32,000 . Except for networking, we note that there were modest improvements in latency and bandwidth in the other three technologies in the six years since the last edition: 0%–23% in latency and 23%–70% in bandwidth. Updated from Patterson, D., 2004. Latency lags bandwidth. Commun. ACM 47 (10), 71–75.

>> 图 1.9 [图 1.10]（#_bookmark17）相对于第一个里程碑，带宽和延迟里程碑的日志图图。请注意，潜伏期改善了 8-91，而带宽则改善了约 400-32,000。除网络外，我们注意到，自上一版以来的六年中，其他三个技术的延迟和带宽有所改善：延迟 0％–23％和带宽的 23％–70％。从帕特森（D.社区。ACM 47（10），71-75。
>>

### Scaling of Transistor Performance and Wires

> ###晶体管性能和电线的缩放

Integrated circuit processes are characterized by the _feature size_, which is the minimum size of a transistor or a wire in either the _x_ or _y_ dimension. Feature sizes decreased from 10 μm in 1971 to 0.016 μm in 2017; in fact, we have switched units, so production in 2017 is referred to as “16 nm,” and 7 nm chips are underway. Since the transistor count per square millimeter of silicon is determined by the surface area of a transistor, the density of transistors increases quadratically with a linear decrease in feature size.

> 集成电路过程的特征是 *feature size*，它是 *x* 或 *y* dimension 中晶体管的最小尺寸或电线的最小尺寸。特征尺寸从 1971 年的 10μm 降低到 2017 年的 0.016μm；实际上，我们已经切换了单位，因此 2017 年的生产被称为“ 16 nm”，正在进行 7 nm 芯片。由于硅每平方毫米的晶体管计数取决于晶体管的表面积，因此晶体管的密度随着特征大小的线性降低而倍增。

> Figure 1.10 Performance milestones over 25–40 years for microprocessors, memory, networks, and disks. The microprocessor milestones are several generations of IA-32 processors, going from a 16-bit bus, microcoded 80286 to a 64-bit bus, multicore, out-of-order execution, superpipelined Core i7. Memory module milestones go from 16-bitwide, plain DRAM to 64-bit-wide double data rate version 3 synchronous DRAM. Ethernet advanced from 10 Mbits/s to 400 Gbits/s. Disk milestones are based on rotation speed, improving from 3600 to 15,000 RPM. Each case is bestcase bandwidth, and latency is the time for a simple operation assuming no contention. Updated from Patterson, D., 2004. Latency lags bandwidth. Commun. ACM 47 (10), 71–75.

>> 图 1.10 微处理器，内存，网络和磁盘超过 25 - 40 年的性能里程碑。微处理器里程碑是几代 IA-32 处理器，从 16 位总线，微编码的 80286 到 64 位总线，多核，额外执行，超级剥离 Core i7。内存模块里程碑从 16 位宽，平原 DRAM 到 64 位宽的双数据速率版本 3 同步 DRAM。以太网从 10 mbits/s 升至 400 gbits/s。磁盘里程碑基于旋转速度，从 3600 rpm 提高到 15,000 rpm。每种情况都是 BestCase 带宽，而延迟是假设没有争议的简单操作的时间。从帕特森（D.社区。ACM 47（10），71-75。
>>

The increase in transistor performance, however, is more complex. As feature sizes shrink, devices shrink quadratically in the horizontal dimension and also shrink in the vertical dimension. The shrink in the vertical dimension requires a reduction in operating voltage to maintain correct operation and reliability of the transistors. This combination of scaling factors leads to a complex interrelationship between transistor performance and process feature size. To a first approximation, in the past the transistor performance improved linearly with decreasing feature size. The fact that transistor count improves quadratically with a linear increase in transistor performance is both the challenge and the opportunity for which computer architects were created! In the early days of microprocessors, the higher rate of improvement in density was used to move quickly from 4-bit, to 8-bit, to 16-bit, to 32-bit, to 64-bit microprocessors. More recently, density improvements have supported the introduction of multiple processors per chip, wider SIMD units, and many of the innovations in speculative execution and caches found in [Chapters 2–5](#_bookmark46).

> 但是，晶体管性能的提高更加复杂。随着特征尺寸的收缩，设备在水平尺寸中四倍地缩小，并在垂直尺寸中收缩。垂直尺寸的收缩需要降低操作电压，以保持晶体管的正确操作和可靠性。缩放因子的这种组合导致晶体管性能和过程特征大小之间的复杂相互关系。到第一个近似值，过去晶体管性能随着特征大小的减小而线性改善。晶体管计数随着晶体管性能的线性增长而倍增的事实既是创建计算机架构师的挑战，也是创建计算机架构师的机会！在微处理器的早期，使用较高的密度提高速率从 4 位，8 位到 16 位移动到 32 位，再到 64 位的微处理器。最近，密度的改进支持了每个芯片，更宽的 SIMD 单元以及[第 2-5 章]中发现的投机执行和 caches 中的许多创新（#_bookmark46）中的许多创新。

Although transistors generally improve in performance with decreased feature size, wires in an integrated circuit do not. In particular, the signal delay for a wire increases in proportion to the product of its resistance and capacitance. Of course, as feature size shrinks, wires get shorter, but the resistance and capacitance per unit length get worse. This relationship is complex, since both resistance and capacitance depend on detailed aspects of the process, the geometry of a wire, the loading on a wire, and even the adjacency to other structures. There are occasional process enhancements, such as the introduction of copper, which provide one-time improvements in wire delay.

> 尽管晶体管的性能通常会随着特征大小的减小而提高，但集成电路中的电线却没有。特别是，导线的信号延迟与其电阻和电容的乘积成比例增加。当然，随着功能尺寸的收缩，电线变短，但单位长度的电阻和电容越来越差。这种关系很复杂，因为电阻和电容都取决于过程的详细方面，电线的几何形状，电线上的负载，甚至是其他结构的邻接。偶尔会增强过程的增强，例如引入铜，这些过程可在电线延迟方面进行一次性改进。

In general, however, wire delay scales poorly compared to transistor performance, creating additional challenges for the designer. In addition to the power dissipation limit, wire delay has become a major design obstacle for large integrated circuits and is often more critical than transistor switching delay. Larger and larger fractions of the clock cycle have been consumed by the propagation delay of signals on wires, but power now plays an even greater role than wire delay.

> 然而，通常，与晶体管性能相比，线延迟缩放量很差，为设计师带来了其他挑战。除了耗散限制外，电线延迟已成为大型集成电路的主要设计障碍物，并且通常比晶体管切换延迟更为关键。电线上的信号的传播延迟消耗了时钟周期的越来越大的部分，但是现在功率比电线延迟发挥了更大的作用。
