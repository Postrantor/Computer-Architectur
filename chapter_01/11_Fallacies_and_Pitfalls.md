## Fallacies and Pitfalls

> ##谬论和陷阱

The purpose of this section, which will be found in every chapter, is to explain some commonly held misbeliefs or misconceptions that you should avoid. We call such misbeliefs _fallacies._ When discussing a fallacy, we try to give a counterex- ample. We also discuss _pitfalls_—easily made mistakes. Often pitfalls are general- izations of principles that are true in a limited context. The purpose of these sections is to help you avoid making these errors in computers that you design.

> 本节的目的（将在每一章中找到）是要解释您应该避免的一些常见的错误或误解。我们称这种不信任 *Fallacies。我们还讨论了\_pitfalls*-犯了错误的错误。通常，陷阱是在有限背景下真实的原则的一般性。这些部分的目的是帮助您避免在设计的计算机中犯这些错误。

The first to go was Dennard scaling. Dennard’s 1974 observation was that power density was constant as transistors got smaller. If a transistor’s linear region shrank by a factor 2, then both the current and voltage were also reduced by a factor of 2, and so the power it used fell by 4. Thus chips could be designed to operate faster and still use less power. Dennard scaling ended 30 years after it was observed, not because transistors didn’t continue to get smaller but because integrated circuit dependability limited how far current and voltage could drop. The threshold voltage was driven so low that static power became a significant fraction of overall power. The next deceleration was hard disk drives. Although there was no law for disks, in the past 30 years the maximum areal density of hard drives—which deter- mines disk capacity—improved by 30%–100% per year. In more recent years, it has been less than 5% per year. Increasing density per drive has come primarily from adding more platters to a hard disk drive.

> 第一个去的是丹纳德缩放。丹纳德（Dennard）1974 年的观察是，随着晶体管变小，功率密度是恒定的。如果晶体管的线性区域缩小了一个因子 2，则电流和电压均降低了 2 倍，因此它使用的功率被降低了 4。因此，芯片可以设计得更快，并且仍然使用较少的功率。丹纳德的缩放在观察到 30 年后结束，不是因为晶体管没有继续变小，而是因为集成电路的可靠性限制了电流和电压的下降程度。阈值电压的驱动如此之低，以至于静态功率成为总体功率的很大一部分。下一个减速是硬盘驱动器。尽管没有关于磁盘的法律，但在过去的 30 年中，硬盘驱动器的最大面积密度（确定了磁盘容量）每年增长 30％–100％。近年来，每年不到 5％。每个驱动器的密度增加主要来自在硬盘驱动器中添加更多盘子。

Next up was the venerable Moore’s Law. It’s been a while since the number of transistors per chip doubled every one to two years. For example, the DRAM chip introduced in 2014 contained 8B transistors, and we won’t have a 16B transistor DRAM chip in mass production until 2019, but Moore’s Law predicts a 64B tran- sistor DRAM chip.

> 接下来是古老的摩尔定律。自每芯片的晶体管数量增加一到两年以来，已经有一段时间了。例如，2014 年推出的 DRAM 芯片包含 8B 晶体管，直到 2019 年，我们才有 16B 晶体管 DRAM 芯片，但 Moore 的法律预测，64B tran-Sistor DRAM 芯片。

Moreover, the actual end of scaling of the planar logic transistor was even pre- dicted to end by 2021. [Figure 1.22](#_bookmark34) shows the predictions of the physical gate length of the logic transistor from two editions of the International Technology Roadmap for Semiconductors (ITRS). Unlike the 2013 report that projected gate lengths to reach 5 nm by 2028, the 2015 report projects the length stopping at 10 nm by 2021. Density improvements thereafter would have to come from ways other than shrinking the dimensions of transistors. It’s not as dire as the ITRS suggests, as companies like Intel and TSMC have plans to shrink to 3 nm gate lengths, but the rate of change is decreasing.

> 此外，平面逻辑晶体管的实际缩放结束甚至预先限制为 2021 年结束。[图 1.22]（#\_ bookmark34）显示了来自国际技术路线图的两个版本的逻辑晶体管的物理门长度的预测用于半导体（ITR）。与 2013 年的报告预计到 2028 年将达到 5 nm 的 2013 年报告，2015 年的报告预计，到 2021 年的长度停止了 10 nm。此后的密度改善不得不来自缩小晶体管尺寸的方式。它并不像 ITRS 所建议的那样可怕，因为英特尔和 TSMC 这样的公司计划缩小到 3 nm 的登机口长度，但变化速度正在降低。

![](./media/image80.png)<span id="_bookmark34" class="anchor"></span>25

> ！[]（./媒体/image80.png）<span ID =“ \_ bookmark34” class =“锚”> </span> 25

> Figure 1.22 Predictions of logic transistor dimensions from two editions of the ITRS report. These reports started in 2001, but 2015 will be the last edition, as the group has disbanded because of waning interest. The only companies that can produce state-of-the-art logic chips today are GlobalFoundaries, Intel, Samsung, and TSMC, whereas there were 19 when the first ITRS report was released. With only four companies left, sharing of plans was too hard to sustain. From IEEE Spectrum, July 2016, “Transistors will stop shrinking in 2021, Moore’s Law Roadmap Predicts,” by Rachel Courtland.

>> 图 1.22 ITRS 报告两个版本的逻辑晶体管维度的预测。这些报告始于 2001 年，但 2015 年将是最后版本，因为该小组由于兴趣而解散。当今唯一可以生产最先进的逻辑芯片的公司是 GlobalFoundaries，Intel，Samsung 和 TSMC，而第一份 ITRS 报告发布的 19 个。只剩下四家公司，分享计划就难以维持。雷切尔·考特兰（Rachel Courtland）从 2016 年 7 月的 IEEE Spectrum，“晶体管将在 2021 年停止缩小，摩尔的路线图预测”。
>>

[Figure 1.23](#_bookmark35) shows the changes in increases in bandwidth over time for micro- processors and DRAM—which are affected by the end of Dennard scaling and Moore’s Law—as well as for disks. The slowing of technology improvements is apparent in the dropping curves. The continued networking improvement is due to advances in fiber optics and a planned change in pulse amplitude modu- lation (PAM-4) allowing two-bit encoding so as to transmit information at 400 Gbit/s.

> [图 1.23]（#\_ bookmark35）显示了微处理器和 DRAM 随时间的带宽增加的变化（受到丹纳德缩放和摩尔定律的结束）以及磁盘的影响。在下降曲线中，技术改进的放缓是显而易见的。持续的网络改进是由于光纤的进步以及计划的脉冲振幅模拟变化（PAM-4）允许两位编码以便以 400 GBIT/s 传输信息。

> Figure 1.23 Relative bandwidth for microprocessors, networks, memory, and disks over time, based on data in [Figure 1.10](#_bookmark17).

>> 图 1.23 基于[图 1.10]中的数据，微处理器，网络，内存和磁盘的相对带宽（#\_ bookmark17）。
>>

The switch to multiple processors per chip around 2005 did not come from some breakthrough that dramatically simplified parallel programming or made it easy to build multicore computers. The change occurred because there was no other option due to the ILP walls and power walls. Multiple processors per chip do not guar- antee lower power; it’s certainly feasible to design a multicore chip that uses more power. The potential is just that it’s possible to continue to improve performance by replacing a high-clock-rate, inefficient core with several lower-clock-rate, effi- cient cores. As technology to shrink transistors improves, it can shrink both capac- itance and the supply voltage a bit so that we can get a modest increase in the number of cores per generation. For example, for the past few years, Intel has been adding two cores per generation in their higher-end chips.

> 2005 年左右的每个芯片的转换并不是来自一些突破性的突破，这些突破性显着简化了并行编程，或者使构建多核算计算机变得易于构建。发生这种变化是因为由于 ILP 壁和电源壁没有其他选择。每个芯片多个处理器不加权降低功率；设计使用更多功率的多层芯片当然是可行的。潜力是，通过用几个低稳定速率的高速率，有效的核心代替高频率，效率低下的核心，可以继续提高性能。随着收缩晶体管的技术的改善，它可以缩小电容和供应电压的收缩，以便我们可以使每一代核心数量适度增加。例如，在过去的几年中，英特尔在其高端筹码中每一代增加了两个核心。

As we will see in Chapters [4](#_bookmark165) and [5](#_bookmark213), performance is now a programmer’s bur- den. The programmers’ La-Z-Boy era of relying on a hardware designer to make their programs go faster without lifting a finger is officially over. If programmers want their programs to go faster with each generation, they must make their pro- grams more parallel.

> 正如我们将在章节[4]（#_ bookmark165）和[5]（#_ bookmark213）中看到的那样，性能现在已成为程序员的 burden。程序员的 La-Z-Boy 时代依靠硬件设计师来使他们的程序更快而无需举起手指的速度已正式结束。如果程序员希望他们的程序与每一代人更快，则必须使自己的程序更加平行。

The popular version of Moore’s law—increasing performance with each gen- eration of technology—is now up to programmers.

> 摩尔定律的流行版本（每种技术的每一个技术都表现出色）现在取决于程序员。

Virtually every practicing computer architect knows Amdahl’s Law. Despite this, we almost all occasionally expend tremendous effort optimizing some feature before we measure its usage. Only when the overall speedup is disappointing do we recall that we should have measured first before we spent so much effort enhancing it!

> 实际上，每个执业的计算机建筑师都知道 Amdahl 的定律。尽管如此，在测量其用法之前，我们几乎所有偶尔都会花费巨大的努力来优化某些功能。只有当总体加速令人失望时，我们还记得我们应该先测量，然后才花费太多努力来增强它！

The calculations of reliability improvement using Amdahl’s Law on page 53 show that dependability is no stronger than the weakest link in a chain. No matter how much more dependable we make the power supplies, as we did in our example, the single fan will limit the reliability of the disk subsystem. This Amdahl’s Law observation led to a rule of thumb for fault-tolerant systems to make sure that every component was redundant so that no single component failure could bring down the whole system. [Chapter 6](#_bookmark268) shows how a software layer avoids single points of failure inside WSCs.

> 使用 Amdahl 定律在第 53 页上提高可靠性的计算表明，可靠性并不比链中最弱的链接强。无论我们像在示例中所做的那样，我们制造电源多么可靠，单个风扇都会限制磁盘子系统的可靠性。该 AMDAHL 的法律观察导致了容忍故障系统的经验法则，以确保每个组件都是多余的，因此没有任何一个组件故障可以降低整个系统。[第 6 章]（#\_ bookmark268）显示了软件层如何避免 WSC 内部的单个故障点。

Fallacy _Hardware enhancements that increase performance also improve energy efficiency, or are at worst energy neutral_.

> 谬误*甲状化软件增强功能的增强功能也提高了能源效率，或者处于最坏的能量中性*。

[Esmaeilzadeh et al. (2011)](#_bookmark945) measured SPEC2006 on just one core of a 2.67 GHz Intel Core i7 using Turbo mode ([Section 1.5](#trends-in-power-and-energy-in-integrated-circuits)). Performance increased by a factor of 1.07 when the clock rate increased to 2.94 GHz (or a factor of 1.10), but the i7 used a factor of 1.37 more joules and a factor of 1.47 more watt hours!

> [Esmaeilzadeh 等。（2011）]（#\_ bookmark945）使用涡轮模式（[[1.5]第 1.5]（＃趋势中的趋势和能源融合循环），仅在 2.67 GHz Intel Core i7 的一个核心上测量了 Spec2006）。当时钟速率增加到 2.94 GHz（或 1.10 倍）时，性能增加了 1.07 倍，但是 i7 使用的焦点增加了 1.37 倍，倍数增加了 1.47 瓦小时！

Several factors influence the usefulness of a benchmark as a predictor of real per- formance, and some change over time. A big factor influencing the usefulness of a benchmark is its ability to resist “benchmark engineering” or “benchmarketing.” Once a benchmark becomes standardized and popular, there is tremendous pres- sure to improve performance by targeted optimizations or by aggressive interpre- tation of the rules for running the benchmark. Short kernels or programs that spend their time in a small amount of code are particularly vulnerable.

> 有几个因素影响基准作为实际功能的预测指标，并随着时间的推移有所改变。影响基准有用性的一个重要因素是其抵抗“基准工程”或“基准市场”的能力。一旦基准标准化和流行，就可以通过目标优化或通过积极地插入运行基准的规则来提高性能。在少量代码上花费时间的简短内核或程序特别容易受到伤害。

For example, despite the best intentions, the initial SPEC89 benchmark suite included a small kernel, called matrix300, which consisted of eight different

> 例如，尽管有最佳意图，但最初的 Spec89 基准套件包括一个称为 Matrix300 的小内核，该套件由八个不同

300 300 matrix multiplications. In this kernel, 99% of the execution time was in a single line (see [SPEC, 1989](#_bookmark1008)). When an IBM compiler optimized this inner loop

> 300 300 矩阵乘法。在此内核中，执行时间的 99％是在一行中（请参阅[Spec，1989]（#\_ bookmark1008））。当 IBM 编译器优化此内部循环时

(using a good idea called _blocking_, discussed in Chapters [2](#_bookmark46) and [4](#_bookmark165)), performance improved by a factor of 9 over a prior version of the compiler! This benchmark tested compiler tuning and was not, of course, a good indication of overall perfor- mance, nor of the typical value of this particular optimization.

> （使用一个称为 *blocking* 的好主意，在章节[2]（#_ bookmark46）和[4]（#_ bookmark165）中进行了讨论，性能在编译器的先前版本中提高了 9 倍！该基准测试了编译器调整，当然不是整体表现的良好指示，也不是这种特定优化的典型价值。

[Figure 1.19](#_bookmark28) shows that if we ignore history, we may be forced to repeat it. SPEC Cint2006 had not been updated for a decade, giving compiler writers sub- stantial time to hone their optimizers to this suite. Note that the SPEC ratios of all benchmarks but libquantum fall within the range of 16–52 for the AMD computer and from 22 to 78 for Intel. Libquantum runs about 250 times faster on AMD and 7300 times faster on Intel! This “miracle” is a result of optimizations by the Intel compiler that automatically parallelizes the code across 22 cores and optimizes memory by using bit packing, which packs together multiple narrow-range inte- gers to save memory space and thus memory bandwidth. If we drop this benchmark and recalculate the geometric means, AMD SPEC Cint2006 falls from 31.9 to 26.5 and Intel from 63.7 to 41.4. The Intel computer is now about 1.5 times as fast as the AMD computer instead of 2.0 if we include libquantum, which is surely closer to their real relative performances. SPECCPU2017 dropped libquantum.

> [图 1.19]（#\_ bookmark28）表明，如果我们忽略历史记录，我们可能会被迫重复它。Spec CINT2006 已经十年没有更新，为编译器作家提供了时间，以磨练他们的优化者为此套件。请注意，所有基准测试的规格比率却在 AMD 计算机的 16-52 范围内，英特尔的规格比率为 22 至 78。Libquantum 在 AMD 上的运行速度约为 250 倍，英特尔的运行速度快 7300 倍！这种“奇迹”是英特尔编译器优化的结果，该编译器会自动在 22 个内核中平行代码并通过使用 BIT 包装来优化内存，从而将多个窄范围的整数包装在一起以节省内存空间，从而保存内存带宽。如果我们删除此基准并重新计算几何均值，则 AMD 规格 CINT2006 从 31.9 下降至 26.5，而英特尔从 63.7 下降到 41.4。现在，英特尔计算机的速度约为 AMD 计算机的 1.5 倍，而不是 2.0 倍，如果我们包括 Libquantum，它肯定更接近其实际的相对性能。Speccpu2017 掉落了 libquantum。

To illustrate the short lives of benchmarks, [Figure 1.17](#_bookmark26) on page 43 lists the status of all 82 benchmarks from the various SPEC releases; Gcc is the lone sur- vivor from SPEC89. Amazingly, about 70% of all programs from SPEC2000 or earlier were dropped from the next release.

> 为了说明基准的短期寿命，[图 1.17]（#\_ bookmark26）第 43 页列出了各种规格发行版中所有 82 个基准的状态；GCC 是 Spec89 的唯一表面。令人惊讶的是，从下一个版本中删除了 Spec2000 或更早的所有程序中约 70％。

The current marketing practices of disk manufacturers can mislead users. How is such an MTTF calculated? Early in the process, manufacturers will put thousands of disks in a room, run them for a few months, and count the number that fail. They compute MTTF as the total number of hours that the disks worked cumulatively divided by the number that failed.

> 磁盘制造商的当前营销实践可能会误导用户。这样的 MTTF 如何计算？在此过程的早期，制造商将将数千个磁盘放入一个房间，将其运行几个月，并计算出失败的数字。他们将 MTTF 计算为磁盘工作的总小时数除以失败的数字。

One problem is that this number far exceeds the lifetime of a disk, which is commonly assumed to be five years or 43,800 hours. For this large MTTF to make some sense, disk manufacturers argue that the model corresponds to a user who buys a disk and then keeps replacing the disk every 5 years—the planned lifetime of the disk. The claim is that if many customers (and their great-grandchildren) did this for the next century, on average they would replace a disk 27 times before a failure, or about 140 years.

> 一个问题是，这个数字远远超过磁盘的寿命，通常认为这是五年或 43,800 小时。为了使这个大型 MTTF 具有一定的意义，磁盘制造商认为该模型对应于购买磁盘然后每 5 年替换磁盘的用户，即磁盘的计划寿命。声称是，如果许多客户（及其曾孙）在下个世纪这样做，那么他们平均而言，他们将在失败前 27 次更换磁盘，或者大约 140 年。

A more useful measure is the percentage of disks that fail, which is called the _annual failure rate_. Assume 1000 disks with a 1,000,000-hour MTTF and that the disks are used 24 hours a day. If you replaced failed disks with a new one having the same reliability characteristics, the number that would fail in a year (8760 hours) is

> 一个更有用的措施是失败的磁盘的百分比，称为 *ARTUAL 故障率*。假设使用 1000,000 小时 MTTF 的 1000 盘，并且每天 24 小时使用磁盘。如果您用具有相同可靠性特征的新磁盘代替了失败的磁盘，那么一年（8760 小时）会失败的数字是

Stated alternatively, 0.9% would fail per year, or 4.4% over a 5-year lifetime.

> 据说，每年的 0.9％将失败，在 5 年的寿命中为 4.4％。

Moreover, those high numbers are quoted assuming limited ranges of temper- ature and vibration; if they are exceeded, then all bets are off. A survey of disk drives in real environments ([Gray and van Ingen, 2005](#_bookmark954)) found that 3%–7% of drives failed per year, for an MTTF of about 125,000–300,000 hours. An even larger study found annual disk failure rates of 2%–10% ([Pinheiro et al., 2007](#_bookmark990)). Therefore the real-world MTTF is about 2–10 times worse than the manufacturer’s MTTF.

> 此外，假设有限的温度和振动范围有限。如果超过它们，则所有赌注都关闭。对真实环境中磁盘驱动器的一项调查（[Gray and van Ingen，2005]（#_ bookmark954））发现，每年 3％–7％的驱动器每年失败，MTTF 的 MTTF 约为 125,000-300,000 小时。一项更大的研究发现，年度磁盘故障率为 2％–10％（[Pinheiro 等，2007]（#_ bookmark990））。因此，现实世界中的 MTTF 比制造商的 MTTF 差 2-10 倍。

The only universally true definition of peak performance is “the performance level a computer is guaranteed not to exceed.” [Figure 1.24](#_bookmark36) shows the percentage of peak performance for four programs on four multiprocessors. It varies from 5% to 58%. Since the gap is so large and can vary significantly by benchmark, peak perfor- mance is not generally useful in predicting observed performance.

> 峰值性能的唯一普遍定义是“保证不超过计算机的性能级别”。[图 1.24]（#\_ bookmark36）显示了四个多处理器上四个程序的峰值性能百分比。它从 5％到 58％不等。由于差距如此之大，并且可以通过基准明显变化，因此峰值的峰值在预测观察到的性能方面没有用。

> Figure 1.24 Percentage of peak performance for four programs on four multiprocessors scaled to 64 processors. The Earth Simulator and X1 are vector processors (see [Chapter 4](#_bookmark165) and Appendix G). Not only did they deliver a higher fraction of peak performance, but they also had the highest peak performance and the lowest clock rates. Except for the Paratec program, the Power 4 and Itanium 2 systems delivered between 5% and 10% of their peak. From Oliker, L., Canning, A., Carter, J., Shalf, J., Ethier, S., 2004. Scientific computations on modern parallel vector systems. In: Proc. ACM/IEEE Conf. on Supercomputing, November 6–12, 2004, Pittsburgh, Penn., p. 10.

>> 图 1.24 在四个多处理器上缩放到 64 个处理器的四个程序的峰值性能百分比。地球模拟器和 X1 是向量处理器（请参阅[第 4 章]（#\_ bookmark165）和附录 G）。他们不仅提供了更高的峰值性能，而且还具有最高的峰值性能和最低的时钟率。除了帕拉特克计划以外，功率 4 和 ITANIUM 2 系统的峰值占其峰值的 5％至 10％。来自 Oliker，L.，Canning，A.，Carter，J.，Shalf，J.，Ethier，S.，2004 年。《现代平行矢量系统的科学计算》。在：Proc。ACM/IEEE conf。关于超级计算，2004 年 11 月 6 日至 12 日，宾夕法尼亚州匹兹堡，p。10。
>>

This apparently ironic pitfall is because computer hardware has a fair amount of state that may not always be critical to proper operation. For example, it is not fatal if an error occurs in a branch predictor, because only performance may suffer.

> 显然，这种讽刺的陷阱是因为计算机硬件具有相当多的状态，这对于正确的操作可能并不总是至关重要的。例如，如果分支预测因子中发生错误，则不会致命，因为只有性能就会受苦。

In processors that try to exploit ILP aggressively, not all the operations are needed for correct execution of the program. [Mukherjee et al. (2003)](#_bookmark980) found that less than 30% of the operations were potentially on the critical path for the SPEC2000 benchmarks.

> 在试图积极利用 ILP 的处理器中，并非需要所有操作来正确执行程序。[Mukherjee 等。（2003）]（#\_ bookmark980）发现，只有不到 30％的操作潜在地走上了 Spec2000 基准的关键路径。

The same observation is true about programs. If a register is “dead” in a pro- gram—that is, the program will write the register before it is read again—then errors do not matter. If you were to crash the program upon detection of a transient fault in a dead register, it would lower availability unnecessarily.

> 关于程序也是如此。如果寄存器在程序中“死了”，也就是说，该程序将在再次读取登记册之前编写登记册，那么错误无关紧要。如果您在发现死寄存器中检测到瞬态故障时将程序崩溃，则它将不必要地降低可用性。

The Sun Microsystems Division of Oracle lived this pitfall in 2000 with an L2 cache that included parity, but not error correction, in its Sun E3000 to Sun E10000 systems. The SRAMs they used to build the caches had intermittent faults, which parity detected. If the data in the cache were not modified, the processor would simply reread the data from the cache. Because the designers did not protect the cache with ECC (error-correcting code), the operating system had no choice but to report an error to dirty data and crash the program. Field engineers found no problems on inspection in more than 90% of the cases.

> Oracle 的 Sun Microsystems 部门在 2000 年以 L2 缓存为生，其中包括奇偶校验，但不校正误差，在其与 Sun E10000 系统的 Sun E3000 中。他们用来构建缓存的 SRAM 有间歇性的断层，这是均等的。如果缓存中的数据未经修改，则处理器将简单地从缓存中重新阅读数据。由于设计人员没有使用 ECC（错误校正代码）保护缓存，因此操作系统别无选择，只能向脏数据报告错误并崩溃程序。现场工程师发现超过 90％的案件没有检查问题。

To reduce the frequency of such errors, Sun modified the Solaris operating sys- tem to “scrub” the cache by having a process that proactively wrote dirty data to memory. Because the processor chips did not have enough pins to add ECC, the only hardware option for dirty data was to duplicate the external cache, using the copy without the parity error to correct the error.

> 为了减少此类错误的频率，Sun 通过具有主动将肮脏数据写入内存的过程来修改 Solaris 操作系统以“擦洗”缓存。由于处理器芯片没有足够的引脚来添加 ECC，因此使用无奇偶校验错误的副本来纠正错误，唯一的脏数据硬件选项是复制外部缓存。

The pitfall is in detecting faults without providing a mechanism to correct them. These engineers are unlikely to design another computer without ECC on external caches.

> 陷阱是在检测故障的情况下，而没有提供纠正它们的机制。这些工程师不太可能在没有 ECC 上设计另一台计算机。
