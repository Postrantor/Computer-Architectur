---
data: 2023-01-15
---

> [!note] 芯片与功耗之间的平衡
> 随着晶体管的数量增长，对能量的消耗也快速增长，但是芯片的散热面积并没有增加，这就需要更好的平衡功耗设计，如：
>
> - 降低电压
> - 对于面向特定对象计算的芯片，减少浮点数的计算
> - 减少内存的访问
> - 等等
>
> 这一章中分析了影响芯片功耗的最主要因素，并相应的给出建议

## Trends in Power and Energy in Integrated Circuits(集成电路中的电力和能量趋势)

Today, energy is the biggest challenge facing the computer designer for nearly every class of computer. First, power must be brought in and distributed around the chip, and modern microprocessors use hundreds of pins and multiple intercon- nect layers just for power and ground. Second, power is dissipated as heat and must be removed.

> 如今，对于几乎每类计算机，能源是计算机设计人员面临的最大挑战。首先，必须在芯片周围带入并分发功率，现代微处理器使用数百个销钉和多个互连层仅用于电源和地面。其次，将功率散发为热量，必须去除。

### Power and Energy: A Systems Perspective(电力和能源：系统视角)

How should a system architect or a user think about performance, power, and energy? From the viewpoint of a system designer, there are three primary concerns. First, what is the maximum power a processor ever requires? Meeting this demand can be important to ensuring correct operation. For example, if a processor attempts to draw more power than a power-supply system can provide (by drawing more current than the system can supply), the result is typically a voltage drop, which can cause devices to malfunction. Modern processors can vary widely in power consumption with high peak currents; hence they provide voltage indexing methods that allow the processor to slow down and regulate voltage within a wider margin. Obviously, doing so decreases performance.

> 系统架构师或用户应该如何考虑性能，功率和能量？从系统设计师的角度来看，有三个主要问题。首先，处理器有史以来最大功率是什么？满足此需求对于确保正确的操作可能很重要。例如，如果处理器试图比电力系统可以提供的功率更多的功率(通过绘制电流比系统所能提供的更多)，则结果通常是电压下降，这可能会导致设备故障。使用高峰值电流，现代处理器的功耗可以差异很大。因此，它们提供了电压索引方法，使处理器可以放慢速度并在更宽的边距内调节电压。显然，这样做会降低性能。

Second, what is the sustained power consumption? This metric is widely called the _thermal design power_ (TDP) because it determines the cooling requirement. TDP is neither peak power, which is often 1.5 times higher, nor is it the actual aver- age power that will be consumed during a given computation, which is likely to be lower still. A typical power supply for a system is typically sized to exceed the TDP, and a cooling system is usually designed to match or exceed TDP. Failure to provide adequate cooling will allow the junction temperature in the processor to exceed its maximum value, resulting in device failure and possibly permanent damage. Modern processors provide two features to assist in managing heat, since the highest power (and hence heat and temperature rise) can exceed the long-term average specified by the TDP. First, as the thermal temperature approaches the junction temperature limit, circuitry lowers the clock rate, thereby reducing power. Should this technique not be successful, a second thermal overload trap is activated to power down the chip.

> 第二，持续的功耗是什么？该指标被广泛称为 _thermal Design Power_(TDP)，因为它决定了冷却要求。TDP 既不是峰值功率，通常是高 1.5 倍，也不是在给定计算过程中消耗的实际平均功率，这可能会降低。系统的典型电源通常大小超过 TDP，并且冷却系统通常设计为匹配或超过 TDP。不提供足够的冷却将使处理器中的连接温度超过其最大值，从而导致设备故障和可能的永久损坏。现代处理器提供了两个功能来帮助管理热量，因为最高功率(因此热量和温度升高)可以超过 TDP 指定的长期平均值。首先，随着热温接近连接温度极限，电路会降低时钟速率，从而降低功率。如果该技术不成功，则会激活第二个热超载陷阱以降低芯片运行频率。

The third factor that designers and users need to consider is energy and energy efficiency. Recall that power is simply energy per unit time: 1 watt 1 joule per second. Which metric is the right one for comparing processors: energy or power? In general, energy is always a better metric because it is tied to a specific task and the time required for that task. In particular, the energy to complete a workload is equal to the average power times the execution time for the workload.

> 设计师和用户需要考虑的第三个因素是能源和能源效率。回想一下功率仅为每单位时间的能量：每秒 1 瓦 1 焦点。哪个指标是比较处理器的合适度量：能源或功率？通常，能源始终是一个更好的指标，因为它与特定任务和该任务所需的时间相关。特别是，完成工作负载的能量等于工作量执行时间的平均功率时间。

Thus, if we want to know which of two processors is more efficient for a given task, we need to compare energy consumption (not power) for executing the task. For example, processor A may have a 20% higher average power consumption than processor B, but if A executes the task in only 70% of the time needed by B, its energy consumption will be 1.2 0.7 0.84, which is clearly better.

> 因此，如果我们想知道两个处理器中的哪个对给定任务更有效，则需要比较执行任务的能源消耗(不是功率)。例如，处理器 A 的平均功耗可能比处理器 B 高 20％，但是如果 A 仅在 B 所需的 70％的时间内执行任务，则其能耗为 1.2 0.7 0.84，显然更好。

One might argue that in a large server or cloud, it is sufficient to consider the average power, since the workload is often assumed to be infinite, but this is mis- leading. If our cloud were populated with processor Bs rather than As, then the cloud would do less work for the same amount of energy expended. Using energy to compare the alternatives avoids this pitfall. Whenever we have a fixed workload, whether for a warehouse-size cloud or a smartphone, comparing energy will be the right way to compare computer alternatives, because the electricity bill for the cloud and the battery lifetime for the smartphone are both determined by the energy consumed.

> 有人可能会争辩说，在大型服务器或云中，由于通常认为工作负载是无限的，因此足以考虑平均功率，但这是错误的。如果我们的云配有处理器 BS 而不是 AS，则云将减少所消耗的能量的工作更少。使用能量比较替代方案可以避免此陷阱。每当我们有固定的工作量时，无论是仓库大小的云还是智能手机，比较能量都会是比较计算机替代方案的正确方法，因为云的电费和智能手机的电池寿命都取决于能量消耗。

When is power consumption a useful measure? The primary legitimate use is as a constraint: for example, an air-cooled chip might be limited to 100 W. It can be used as a metric if the workload is fixed, but then it’s just a variation of the true metric of energy per task.

> 功耗何时是有用的措施？合法用途的主要用途是一种约束：例如，气冷芯片可能仅限于 100W。如果工作负载固定，则可以用作度量任务。

### Energy and Power Within a Microprocessor(微处理器中的能量和力量)

For CMOS chips, the traditional primary energy consumption has been in switch- ing transistors, also called _dynamic energy._ The energy required per transistor is proportional to the product of the capacitive load driven by the transistor and the square of the voltage:

> 对于 CMOS 芯片，传统的主要能量消耗一直在转换晶体管，也称为 _Dynamic Energy_。

> ===

For a fixed task, slowing clock rate reduces power, but not energy.

> 对于固定的任务，减慢时钟速率会减少功率，但不能减少能量。

Clearly, dynamic power and energy are greatly reduced by lowering the volt- age, so voltages have dropped from 5 V to just under 1 V in 20 years. The capac- itive load is a function of the number of transistors connected to an output and the technology, which determines the capacitance of the wires and the transistors.

> 显然，通过降低电压大大降低了动态功率和能量，因此电压从 20 年来从 5 V 下降到不到 1V。电容负载是连接到输出和技术的晶体管数的函数，该晶体管决定了电线和晶体管的电容。

Example Some microprocessors today are designed to have adjustable voltage, so a 15% reduction in voltage may result in a 15% reduction in frequency. What would be the impact on dynamic energy and on dynamic power?

> 例如，当今一些微处理器设计为可调电压，因此电压降 15％可能会导致频率降低 15％。对动态能量和动态力量有什么影响？

> ===

As we move from one process to the next, the increase in the number of tran- sistors switching and the frequency with which they change dominate the decrease in load capacitance and voltage, leading to an overall growth in power consump- tion and energy. The first microprocessors consumed less than a watt, and the first 32-bit microprocessors (such as the Intel 80386) used about 2 W, whereas a 4.0 GHz Intel Core i7-6700K consumes 95 W. Given that this heat must be dissi- pated from a chip that is about 1.5 cm on a side, we are near the limit of what can be cooled by air, and this is where we have been stuck for nearly a decade.

> 当我们从一个过程转移到另一个过程时，转移数的数量的增加以及它们改变的频率占主导地位的负载电容和电压的减小，从而导致功率支出和能量的总体增长。第一批微处理器的消耗少于瓦，而前 32 位微处理器(例如英特尔 80386)使用了大约 2 W，而 4.0 GHz Intel Core i7-6700K 则消耗 95W。鉴于该热量必须被散布。从一侧约 1.5 厘米的芯片中，我们接近空气可以冷却的极限，这是我们被困近十年的地方。

Given the preceding equation, you would expect clock frequency growth to slow down if we can’t reduce voltage or increase power per chip. [Figure 1.11](#_bookmark19) shows that this has indeed been the case since 2003, even for the microprocessors in [Figure 1.1](#_bookmark5) that were the highest performers each year. Note that this period of flatter clock rates corresponds to the period of slow performance improvement range in [Figure 1.1](#_bookmark5).

> 鉴于上述方程式，如果我们不能降低电压或增加每个芯片的功率，您可能会预计时钟频率的增长会减慢。[图 1.11](#_bookmark19)表明，自 2003 年以来，即使是[图 1.1](#_bookmark5)中表现最高的微处理器，这确实是这种情况。请注意，这个平整时钟速率的时期对应于[图 1.1](#_bookmark5)中的缓慢性能改进范围的时期。

Distributing the power, removing the heat, and preventing hot spots have become increasingly difficult challenges. Energy is now the major constraint to using transistors; in the past, it was the raw silicon area. Therefore modern microprocessors offer many techniques to try to improve energy efficiency despite flat clock rates and constant supply voltages:

> 分发电力，去除热量并防止热点成为越来越困难的挑战。现在的能量是使用晶体管的主要限制。过去，这是原始的硅区域。因此，现代微处理器提供了许多技术来试图提高能源效率，尽管时钟速率和恒定电压电压：

1. _Do nothing well._ Most microprocessors today turn off the clock of inactive modules to save energy and dynamic power. For example, if no floating-point instructions are executing, the clock of the floating-point unit is disabled. If some cores are idle, their clocks are stopped.

> 1. _Do nothing well._ 例如，如果没有执行浮点指令，则禁用浮点单元的时钟。如果某些内核是闲置的，他们的时钟就会停止。

2. _Dynamic voltage-frequency scaling (DVFS)._ The second technique comes directly from the preceding formulas. PMDs, laptops, and even servers have periods of low activity where there is no need to operate at the highest clock frequency and voltages. Modern microprocessors typically offer a few clock frequencies and voltages in which to operate that use lower power and energy. [Figure 1.12](#_bookmark20) plots the potential power savings via DVFS for a server as the work- load shrinks for three different clock rates: 2.4, 1.8, and 1 GHz. The overall server power savings is about 10%–15% for each of the two steps.

> 2. _Dynamic voltage-frequency scaling (DVFS)._ 第二技术直接来自前面的公式。PMD，笔记本电脑甚至服务器的活动期间不需要以最高的时钟频率和电压操作。现代微处理器通常提供一些时钟频率和电压，在其中操作使用较低的功率和能量。[图 1.12](#_bookmark20)通过服务器的 DVF 绘制潜在的功率节省，作为三个不同时钟速率的工作负载收缩：2.4、1.8 和 1 GHz。这两个步骤中的每个步骤的总体服务器功率节省约为 10％–15％。

3. _Design for the typical case._ Given that PMDs and laptops are often idle, memory and storage offer low power modes to save energy. For example, DRAMs have a series of increasingly lower power modes to extend battery life in PMDs and laptops, and there have been proposals for disks that have a mode that spins more slowly when unused to save power. However, you cannot access DRAMs or disks in these modes, so you must return to fully active mode to read or write, no matter how low the access rate. As mentioned, microprocessors for PCs have been designed instead for heavy use at high operating temperatures, relying on on-chip temperature sensors to detect when activity should be reduced automatically to avoid overheating. This “emergency slowdown” allows manufacturers to design for a more typical case and then rely on this safety mechanism if someone really does run programs that consume much more power than is typical.

> 3. _Design for the typical case._ 鉴于 PMD 和笔记本电脑通常是闲置的，Memory 和存储提供了低功率模式来节省能源。例如，DRAM 具有一系列越来越较低的功率模式，可以延长 PMD 和笔记本电脑中的电池寿命，并且已经提出了有关磁盘的提议，该磁盘的模式在未使用时旋转速度较慢以节省电源。但是，您无法在这些模式下访问 DRAM 或磁盘，因此无论访问率多么低，您都必须返回完全活跃的模式才能读取或写入。如前所述，PC 的微处理器已被设计为在高工作温度下进行大量使用，依靠片上温度传感器来检测应自动降低活动以避免过热。这种“紧急放缓”使制造商可以设计出更典型的情况，然后依靠这种安全机制，如果某个人确实运行了与典型的功能更大的程序。

Figure 1.11 Growth in clock rate of microprocessors in [Figure 1.1](#_bookmark5). Between 1978 and 1986, the clock rate improved less than 15% per year while performance improved by 22% per year. During the “renaissance period” of 52% performance improvement per year between 1986 and 2003, clock rates shot up almost 40% per year. Since then, the clock rate has been nearly flat, growing at less than 2% per year, while single processor performance improved recently at just 3.5% per year.

> 图 1.11 在[Figure 1.1](#_bookmark5)中的微处理器时钟速率增长。在 1978 年至 1986 年之间，时钟率每年不到 15％，而表现每年提高 22％。在 1986 年至 2003 年之间每年 52％的“文艺复兴时期”中，时钟率每年近 40％。从那时起，时钟速率几乎是平坦的，每年不到 2％，而单个处理器的性能最近以每年仅 3.5％的速度提高。

Figure 1.12 Energy savings for a server using an AMD Opteron microprocessor, 8 GB of DRAM, and one ATA disk. At 1.8 GHz, the server can handle at most up to two-thirds of the workload without causing service-level violations, and at 1 GHz, it can safely han- dle only one-third of the workload (Figure 5.11 in [Barroso and H](#_bookmark924)o€[lzle, 2009](#_bookmark924)).

> 图 1.12 使用 AMD Opteron 微处理器，8 GB DRAM 和一个 ATA 磁盘为服务器节省能源。在 1.8 GHz 时，服务器最多可以处理多达三分之二的工作负载，而不会引起服务级别的违规行为，而在 1 GHz 时，它只能安全地仅限于工作量的三分之一(图 5.11 在[Barroso 和[Barroso and and and and and]h](#_ bookmark924)o€[lzle，2009](#_ bookmark924))。

1. _Overclocking_. Intel started offering _Turbo mode_ in 2008, where the chip decides that it is safe to run at a higher clock rate for a short time, possibly on just a few cores, until temperature starts to rise. For example, the 3.3 GHz Core i7 can run in short bursts for 3.6 GHz. Indeed, the highest-performing microprocessors each year since 2008 shown in [Figure 1.1](#_bookmark5) have all offered temporary overclock- ing of about 10% over the nominal clock rate. For single-threaded code, these microprocessors can turn off all cores but one and run it faster. Note that, although the operating system can turn off Turbo mode, there is no notification once it is enabled, so the programmers may be surprised to see their programs vary in performance because of room temperature! Although dynamic power is traditionally thought of as the primary source of power dissipation in CMOS, static power is becoming an important issue because leakage current flows even when a transistor is off:

> 1. _Overclocking_. 英特尔于 2008 年开始提供 _Turbo 模式_，芯片决定在短时间内以更高的时钟速率运行，可能仅在几个内核上运行，直到温度开始升高为止。例如，3.3 GHz Core i7 可以在 3.6 GHz 的短爆发中运行。实际上，自 2008 年以来[图 1.1](#_bookmark5)所示的每年表现最高的微处理器都提供了比标称时钟速率约 10％的临时超频。对于单线读取代码，这些微处理器可以关闭所有内核，但要更快地运行它。请注意，尽管操作系统可以关闭 Turbo 模式，但是一旦启用了通知，因此没有通知，因此程序员可能会惊讶地看到他们的程序在室温下的性能有所不同！尽管传统上将动态功率视为 CMO 中功率耗散的主要来源，但静态功率正在成为一个重要问题，因为泄漏电流即使晶体管关闭，泄漏电流也会流动：

> ===

That is, static power is proportional to the number of devices.

> 也就是说，静电功率与设备数量成正比。

Thus increasing the number of transistors increases power even if they are idle, and current leakage increases in processors with smaller transistor sizes. As a result, very low-power systems are even turning off the power supply (_power gat- ing_) to inactive modules in order to control loss because of leakage. In 2011 the goal for leakage was 25% of the total power consumption, with leakage in high-performance designs sometimes far exceeding that goal. Leakage can be as high as 50% for such chips, in part because of the large SRAM caches that need power to maintain the storage values. (The S in SRAM is for static.) The only hope to stop leakage is to turn off power to the chips’ subsets.

> 因此，增加晶体管的数量也会增加功率，即使它们是闲置的，并且当前的泄漏增加了晶体管大小较小的处理器。结果，非常低的功率系统甚至将电源(_功率盖_)关闭到无效模块中，以控制泄漏，以控制损失。在 2011 年，泄漏的目标是总功耗的 25％，高性能设计中的泄漏有时远远超过该目标。对于此类芯片，泄漏可能高达 50％，部分原因是需要大量的 SRAM 缓存来维持存储值。(SRAM 中的 S 是静态的。)停止泄漏的唯一希望是关闭芯片子集的电源。

Finally, because the processor is just a portion of the whole energy cost of a sys- tem, it can make sense to use a faster, less energy-efficient processor to allow the rest of the system to go into a sleep mode. This strategy is known as _race-to-halt_.

> 最后，由于处理器只是系统的整个能源成本的一部分，因此使用更快，更低的节能处理器以允许系统的其余部分进入睡眠模式是有意义的。此策略称为 _race-to-halt_。

The importance of power and energy has increased the scrutiny on the effi- ciency of an innovation, so the primary evaluation now is tasks per joule or per- formance per watt, contrary to performance per $mm^{2}$ of silicon as in the past. This new metric affects approaches to parallelism, as we will see in Chapters [4](#_bookmark165) and [5](#_bookmark213).

> 权力和能源的重要性提高了对创新有效性的回顾，因此现在的主要评估是每瓦特的任务或每瓦的效果，与硅的每瓦尔德 $mm^{2}$ 相反，过去。正如我们将在章节[4](#_ bookmark165)和[5](#_ bookmark213)中看到的那样，这个新的指标影响了并行性的方法。

### The Shift in Computer Architecture Because of Limits of Energy(计算机体系结构的转移由于能源的限制)

As transistor improvement decelerates, computer architects must look elsewhere for improved energy efficiency. Indeed, given the energy budget, it is easy today to design a microprocessor with so many transistors that they cannot all be turned on at the same time. This phenomenon has been called _dark silicon_, in that much of a chip cannot be unused (“dark”) at any moment in time because of thermal con- straints. This observation has led architects to reexamine the fundamentals of pro- cessors’ design in the search for a greater energy-cost performance.

> 随着晶体管改进的减速，计算机工程师必须在其他地方寻找提高能源效率。的确，鉴于能源预算，今天很容易设计一个具有如此多晶体管的微处理器，以至于不能同时打开它们。这种现象被称为 _dark silicon_，因为由于热结构而无法在任何时候未使用(“ dark”)。这一观察结果使工程师重新回顾了教育员设计的基础知识，以寻求更大的能源成本性能。

[Figure 1.13](#_bookmark21), which lists the energy cost and area cost of the building blocks of a modern computer, reveals surprisingly large ratios. For example, a 32-bit

> [图 1.13](#_bookmark21)列出了现代计算机构建块的能源成本和面积成本，显示出惊人的比率。例如，32 位

> ===

Area numbers are from synthesized result using Design compiler under TSMC 45nm tech node. FP units used DesignWare Library.

> 区域数来自 TSMC 45NM 技术节点下的设计编译器的合成结果。FP 单元使用的设计软件库。

Figure 1.13 Comparison of the energy and die area of arithmetic operations and energy cost of accesses to SRAM and DRAM. [Azizi][dally]. Area is for TSMC 45 nm technology node.

> 图 1.13 算术操作的能量和模具面积的比较以及访问 SRAM 和 DRAM 的能源成本。[azizi ] [dally]。面积适用于 TSMC 45 nm 技术节点。

floating-point addition uses 30 times as much energy as an 8-bit integer add. The area difference is even larger, by 60 times. However, the biggest difference is in memory; a 32-bit DRAM access takes 20,000 times as much energy as an 8-bit addition. A small SRAM is 125 times more energy-efficient than DRAM, which demonstrates the importance of careful uses of caches and memory buffers.

> 浮点添加使用的能量是 8 位整数添加的 30 倍。区域差异更大，增加 60 次。但是，最大的区别在于记忆。32 位 DRAM 访问所需的 20,000 倍的能量是 8 位添加的能量。小型 SRAM 的能源效率是 DRAM 的 125 倍，这表明了仔细使用缓存和存储器缓冲区的重要性。

The new design principle of minimizing energy per task combined with the relative energy and area costs in [Figure 1.13](#_bookmark21) have inspired a new direction for com- puter architecture, which we describe in [Chapter 7](#_bookmark322). Domain-specific processors save energy by reducing wide floating-point operations and deploying special-pur- pose memories to reduce accesses to DRAM. They use those saving to provide 10–100 more (narrower) integer arithmetic units than a traditional processor. Although such processors perform only a limited set of tasks, they perform them remarkably faster and more energy efficiently than a general-purpose processor. Like a hospital with general practitioners and medical specialists, computers in this energy-aware world will likely be combinations of general-purpose cores that can perform any task and special-purpose cores that do a few things extremely well and even more cheaply.

> 最小化每个任务的能量的新设计原理，结合了[图 1.13](#_bookmark21)中的相对能量和面积成本，启发了一个新方向 - 我们在[第 7 章](#_bookmark322)中进行了描述。特定于域的处理器通过减少宽阔的浮点操作并部署特殊姿势记忆来节省能源，以减少对 DRAM 的访问。他们使用节省的人比传统处理器多提供 10-100 个整数算术单元。尽管此类处理器仅执行有限的任务，但它们的执行速度比通用处理器更快，更有效。像一家拥有全科医生和医学专家的医院一样，这个能源感知世界中的计算机可能是通用核心的组合，可以执行任何任务和专用核心，这些核心对一些事情做得非常好，甚至更便宜。
