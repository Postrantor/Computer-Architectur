## Dependability

> ##可靠性

Historically, integrated circuits were one of the most reliable components of a com- puter. Although their pins may be vulnerable, and faults may occur over commu- nication channels, the failure rate inside the chip was very low. That conventional wisdom is changing as we head to feature sizes of 16 nm and smaller, because both transient faults and permanent faults are becoming more commonplace, so archi- tects must design systems to cope with these challenges. This section gives a quick overview of the issues in dependability, leaving the official definition of the terms and approaches to Section D.3 in Appendix D.

> 从历史上看，综合电路是计算机中最可靠的组件之一。尽管它们的引脚可能很脆弱，并且可能在通道上发生故障，但芯片内部的故障率非常低。随着我们的头大小为 16 nm 及较小的大小，这种传统的智慧正在发生变化，因为瞬态断层和永久性断层都变得越来越普遍，因此档案必须设计系统以应对这些挑战。本节详细概述了可靠性的问题，使附录 D 中第 D.3 节的术语和方法的官方定义。

Computers are designed and constructed at different layers of abstraction. We can descend recursively down through a computer seeing components enlarge themselves to full subsystems until we run into individual transistors. Although some faults are widespread, like the loss of power, many can be limited to a single component in a module. Thus utter failure of a module at one level may be con- sidered merely a component error in a higher-level module. This distinction is helpful in trying to find ways to build dependable computers.

> 计算机是在不同的抽象层设计和构造的。我们可以通过计算机递归向下下降，看到组件扩大到完整的子系统，直到我们遇到单个晶体管为止。尽管有些故障是广泛的，例如失电的损失，但许多故障可能仅限于模块中的单个组件。因此，模块在一个级别上的完全故障可能仅在更高级别的模块中考虑到一个组件误差。这种区别有助于尝试寻找构建可靠计算机的方法。

One difficult question is deciding when a system is operating properly. This theoretical point became concrete with the popularity of Internet services. Infra- structure providers started offering _service level agreements_ (SLAs) or _service level objectives_ (SLOs) to guarantee that their networking or power service would be dependable. For example, they would pay the customer a penalty if they did not meet an agreement of some hours per month. Thus an SLA could be used to decide whether the system was up or down.

> 一个困难的问题是确定系统何时正常运行。这个理论上的观点随着互联网服务的普及而变得具体。INFRA-结构提供商开始提供_服务级协议_（SLA）或_服务级别目标_（SLO），以确保其网络或电力服务可靠。例如，如果他们每月不符合数小时的协议，他们将向客户支付罚款。因此，可以使用 SLA 来确定系统是向上还是向下。

Systems alternate between two states of service with respect to an SLA:

> 相对于 SLA，系统在两个服务状态之间交替：

1. _Service accomplishment_, where the service is delivered as

> 1. _服务成就_，服务的交付为

specified.

> 指定的。

2. _Service interruption_, where the delivered service is different from the SLA. Transitions between these two states are caused by _failures_ (from state 1 to state 2) or _restorations_ (2 to 1). Quantifying these transitions leads to the two main mea- sures of dependability:

> 2. _服务中断_，其中交付服务与 SLA 不同。这两个状态之间的过渡是由_failures _（从状态 1 到状态 2）或_Restorations _（2 至 1）引起的。量化这些过渡会导致可靠性的两个主要表现：

- _Module reliability_ is a measure of the continuous service accomplishment (or, equivalently, of the time to failure) from a reference initial instant. Therefore the _mean time to failure_ (MTTF) is a reliability measure. The reciprocal of MTTF is a rate of failures, generally reported as failures per billion hours of operation, or _FIT_ (for _failures in time_). Thus an MTTF of 1,000,000 hours equals 10<sup>9</sup>/10<sup>6</sup> or 1000 FIT. Service interruption is measured as _mean time to repair_ (MTTR). _Mean time between failures_ (MTBF) is simply the sum of MTTF+ MTTR. Although MTBF is widely used, MTTF is often the more appropriate term. If a collection of modules has exponentially distributed lifetimes—meaning that the age of a module is not important in probability of failure—the overall failure rate of the collection is the sum of the failure rates of the modules.

> - _MODULE 可靠性_是从参考初始瞬间开始的连续服务成就（或等效的失败时间）的度量。因此，失败的时间_（MTTF）是一个可靠性度量。MTTF 的倒数是故障率，通常报告为每十亿小时的操作或_fit_（用于_FITURES _FITERINE）。因此，1,000,000 小时的 MTTF 等于 10 <sup> 9 </sup>/10 <sup> 6 </sup>或 1000 Fit。服务中断被测量为_ mean 进行修理时间（MTTR）。_失败_（MTBF）之间的时间只是 MTTF+ MTTR 的总和。尽管 MTBF 被广泛使用，但 MTTF 通常是更合适的术语。如果模块的集合具有指数分布的寿命（即模块的年龄在失败概率中并不重要），则该收集的总失败率是模块的故障率的总和。

- _Module availability_ is a measure of the service accomplishment with respect to the alternation between the two states of accomplishment and interruption. For nonredundant systems with repair, module availability is Module availability MTTF

> - _ module availability_是对两种成就状态和中断状态之间的交替的衡量结果。对于具有维修的非冗余系统，模块可用性是模块可用性 mttf

(MTTF + MTTR)

> （MTTF + MTTR）

Note that reliability and availability are now quantifiable metrics, rather than syn- onyms for dependability. From these definitions, we can estimate reliability of a system quantitatively if we make some assumptions about the reliability of com- ponents and that failures are independent.

> 请注意，可靠性和可用性现在是可量化的指标，而不是可靠性的合成。从这些定义来看，如果我们对合并的可靠性做出一些假设，并且失败是独立的，则可以定量地估算系统的可靠性。

Example Assume a disk subsystem with the following components and MTTF:

> 示例假设具有以下组件和 MTTF 的磁盘子系统：

- 10 disks, each rated at 1,000,000-hour MTTF

> - 10 个磁盘，每个磁盘为 1,000,000 小时 MTTF

- 1 ATA controller, 500,000-hour MTTF

> -1 个 ATA 控制器，500,000 小时 MTTF

- 1 power supply, 200,000-hour MTTF

> -1 个电源，200,000 小时 MTTF

- 1 fan, 200,000-hour MTTF

> -1 个粉丝，200,000 小时 MTTF

- 1 ATA cable, 1,000,000-hour MTTF

> -1 个 ATA 电缆，1,000,000 小时 MTTF

Using the simplifying assumptions that the lifetimes are exponentially distributed and that failures are independent, compute the MTTF of the system as a whole.

> 使用简化的假设，即生命值是指数分的并且失败是独立的，请计算整个系统的 MTTF。

> ===

>> ===
>>

The primary way to cope with failure is redundancy, either in time (repeat the operation to see if it still is erroneous) or in resources (have other components to take over from the one that failed). Once the component is replaced and the system is fully repaired, the dependability of the system is assumed to be as good as new. Let’s quantify the benefits of redundancy with an example.

> 应对故障的主要方法是冗余，无论是及时的（重复该操作以查看仍然是错误的）还是资源中（有其他组件可以从失败的组件中接管）。一旦更换了组件并完全维修了系统，则假定系统的可靠性与新型一样好。让我们以示例来量化冗余的好处。

Example Disk subsystems often have redundant power supplies to improve dependability. Using the preceding components and MTTFs, calculate the reliability of redundant power supplies. Assume that one power supply is sufficient to run the disk subsys- tem and that we are adding one redundant power supply.

> 示例磁盘子系统通常具有冗余电源以提高可靠性。使用前面的组件和 MTTF，计算冗余电源的可靠性。假设一个电源足以运行磁盘子量，并且我们正在添加一个冗余电源。

_Answer_ We need a formula to show what to expect when we can tolerate a failure and still provide service. To simplify the calculations, we assume that the lifetimes of the components are exponentially distributed and that there is no dependency between the component failures. MTTF for our redundant power supplies is the mean time until one power supply fails divided by the chance that the other will fail before the first one is replaced. Thus, if the chance of a second failure before repair is small, then the MTTF of the pair is large.

> _answer_我们需要一个公式来显示我们可以忍受失败并仍然提供服务的期望。为了简化计算，我们假设组件的寿命是指数分布的，并且组件故障之间没有依赖性。MTTF 对于我们的冗余电源是平均时间，直到一个电源失败除以另一个电源在更换第一个电源之前失败的机会。因此，如果修复前第二次故障的机会很小，则两对的 MTTF 很大。

Since we have two power supplies and independent failures, the mean time until one supply fails is MTTF<sub>power</sub> <sub>supply</sub>/2*.* A good approximation of the probability of a second failure is MTTR over the mean time until the other power supply fails. Therefore a reasonable approximation for a redundant pair of power supplies is

> 由于我们有两个电源和独立故障，因此一个供应失败的平均时间是 mttf <ub> Power </sub> <sub>供应</sub>/2*。*第二次失败概率的良好近似直到其他电源失败之前，是 MTTR。因此，冗余电源的合理近似值是

> ===

>> ===
>>

Using the preceding MTTF numbers, if we assume it takes on average 24 hours for a human operator to notice that a power supply has failed and to replace it, the reli- ability of the fault tolerant pair of power supplies is making the pair about 4150 times more reliable than a single power supply.

> 使用前面的 MTTF 号码，如果我们假设人类操作员平均需要 24 小时，那么人们注意到电源失败并替换它，那么容耐容忍的电源的可靠性使这对使这对大约 4150 比单个电源更可靠。

Having quantified the cost, power, and dependability of computer technology, we are ready to quantify performance.

> 量化计算机技术的成本，功率和可靠性后，我们准备量化性能。

> ===

>> ===
>>
