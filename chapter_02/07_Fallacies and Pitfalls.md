## Fallacies and Pitfalls

> ##谬论和陷阱

As the most naturally quantitative of the computer architecture disciplines, mem- ory hierarchy would seem to be less vulnerable to fallacies and pitfalls. Yet we were limited here not by lack of warnings, but by lack of space!

> 作为计算机架构学科最自然的定量，MEM-ORY 层次结构似乎不太容易受到谬论和陷阱的影响。然而，我们在这里不是缺乏警告，而是由于缺乏空间而受到限制！

Fallacy _Predicting cache performance of one program from another_.

> 谬误\_在另一个程序中提出一个程序的缓存性能。

> Figure 2.29 Instruction and data misses per 1000 instructions as cache size varies from 4 KiB to 4096 KiB. Instruction misses for gcc are 30,000–40,000 times larger than for lucas, and, conversely, data misses for lucas are 2–60 times larger than for gcc. The programs gap, gcc, and lucas are from the SPEC2000 benchmark suite.

>> 图 2.29 指令和数据遗失每 1000 个说明，因为高速缓存大小从 4 KIB 到 4096 KIB 不等。GCC 的指示误差比 Lucas 大 30,000–40,000 倍，相反，Lucas 的数据错过的数量是 GCC 的 2-60 倍。程序差距，GCC 和 Lucas 来自 Spec2000 基准套件。
>>

[Figure 2.29](#_bookmark82) shows the instruction miss rates and data miss rates for three programs from the SPEC2000 benchmark suite as cache size varies. Depending on the program, the data misses per thousand instructions for a 4096 KiB cache are 9, 2, or 90, and the instruction misses per thousand instructions for a 4 KiB cache are 55, 19, or 0.0004. Commercial programs such as databases will have significant miss rates even in large second-level caches, which is generally not the case for the SPECCPU programs. Clearly, generalizing cache performance from one program to another is unwise. As [Figure 2.24](#_bookmark76) reminds us, there is a great deal of variation, and even predictions about the relative miss rates of integer and floating-point- intensive programs can be wrong, as mcf and sphnix3 remind us!

> [图 2.29]（#_ bookmark82）显示了 Spec2000 基准套件的三个程序的指令率和数据误费率，因为高速缓存大小各不相同。根据程序的不同，4096 KIB 缓存的每千个说明的数据丢失了 9、2 或 90，而 4 KIB CACHE 的指令却错过了 55、19 或 0.0004 的指令。即使在大型的二级缓存中，数据库等商业计划也将具有明显的遗漏费率，而对于 SpecCPU 计划来说，这通常不是这种情况。显然，将缓存性能从一个程序概括到另一个程序是不明智的。如[图 2.24]（#_ bookmark76）提醒我们，有很大的变化，甚至关于整数和浮点密集程序的相对失误率的预测可能是错误的，因为 MCF 和 SPHNIX3 提醒我们！

Pitfall _Simulating enough instructions to get accurate performance measures of the memory hierarchy_.

> 陷阱\_模拟足够的说明以获取内存层次结构的准确性能度量。

There are really three pitfalls here. One is trying to predict performance of a large cache using a small trace. Another is that a program’s locality behavior is not con- stant over the run of the entire program. The third is that a program’s locality behavior may vary depending on the input.

> 这里确实有三个陷阱。人们正在尝试使用小痕迹预测大型缓存的性能。另一个是，在整个程序的运行中，程序的局部性行为并不确定。第三个是程序的局部行为可能会根据输入而有所不同。

[Figure 2.30](#_bookmark83) shows the cumulative average instruction misses per thousand instructions for five inputs to a single SPEC2000 program. For these inputs, the average memory rate for the first 1.9 billion instructions is very different from the average miss rate for the rest of the execution.

> [图 2.30]（#\_ bookmark83）显示了单个 Spec2000 程序的五个输入的累积平均指令遗漏的累积平均指令。对于这些输入，前 19 亿指令的平均内存率与其余执行的平均遗漏率有很大不同。

Pitfall _Not delivering high memory bandwidth in a cache-based system_.

> 陷阱不会在基于缓存的系统中提供高内存带宽。

Caches help with average cache memory latency but may not deliver high memory bandwidth to an application that must go to main memory. The architect must design a high bandwidth memory behind the cache for such applications. We will revisit this pitfall in Chapters [4](#_bookmark165) and [5](#_bookmark213).

> 缓存有助于平均缓存内存延迟，但可能无法将高内存带宽输送到必须进入主内存的应用程序。建筑师必须在缓存后面设计高带宽内存，以进行此类应用程序。我们将在章节[4]（#_ bookmark165）和[5]（#_ bookmark213）中重新审视这一陷阱。

Instructions (billions)

> 指示（十亿）

Figure 2.30 Instruction misses per 1000 references for five inputs to the perl bench- mark in SPEC2000. There is little variation in misses and little difference between the five inputs for the first 1.9 billion instructions. Running to completion shows how misses vary over the life of the program and how they depend on the input. The top graph shows the running average misses for the first 1.9 billion instructions, which starts at about 2.5 and ends at about 4.7 misses per 1000 references for all five inputs. The bot- tom graph shows the running average misses to run to completion, which takes 16–41 billion instructions depending on the input. After the first 1.9 billion instructions, the misses per 1000 references vary from 2.4 to 7.9 depending on the input. The simulations were for the Alpha processor using separate L1 caches for instructions and data, each being two-way 64 KiB with LRU, and a unified 1 MiB direct-mapped L2 cache.

> 图 2.30 指令遗失了每 1000 个参考，五个输入到 Spec2000 中的 Perl 基准标记。对于前 19 亿个说明，五个输入之间的错过几乎没有差异，几乎没有差异。跑步完成表明，错过在程序的生活中如何变化以及它们如何依赖输入。顶部图显示了前 19 亿个说明的跑步平均值，该指令大约为 2.5，最终以大约 4.7 次失误，每 1000 次参考。botm tom 图显示了运行的平均值未能运行到完成，这取决于输入，需要进行 16-41 亿个说明。在第一个 19 亿个说明之后，根据输入的不同，每 1000 个参考文献的遗漏从 2.4 到 7.9 不等。这些仿真是使用单独的 L1 缓存用于指令和数据的 Alpha 处理器的，每个缓存都是带有 LRU 的双向 64 KIB，以及一个统一的 1 MIB 直接映射 L2 CACHE。

Pitfall _Implementing a virtual machine monitor on an instruction set architecture that wasn’t designed to be virtualizable_.

> 陷阱\_对指令集体系结构上的虚拟机监视器进行 IMPLENT，该体系结构并非被设计为虚拟机。

Many architects in the 1970s and 1980s weren’t careful to make sure that all instructions reading or writing information related to hardware resource informa- tion were privileged. This _laissez faire_ attitude causes problems for VMMs for all of these architectures, including the 80x86, which we use here as an example.

> 1970 年代和 1980 年代的许多建筑师都不谨慎，以确保所有与硬件资源信息有关的阅读或编写信息都具有特权。此 *laisseez faire* 态度为所有这些体系结构（包括 80x86）引起了 VMM 的问题，我们在这里以此为例。

[Figure 2.31](#_bookmark84) describes the 18 instructions that cause problems for paravirtuali- zation ([Robin and Irvine, 2000](#_bookmark996)). The two broad classes are instructions that

> [图 2.31]（#_ bookmark84）描述了 18 个指令，这些说明引起了寄生虫的问题（[Robin 和 Irvine，2000]（#_ bookmark996））。这两个范围是指示

- read control registers in user mode that reveal that the guest operating system is running in a virtual machine (such as POPF mentioned earlier) and

> - 在用户模式下读取控制寄存器，该寄存器揭示了来宾操作系统正在虚拟机中运行（例如前面提到的 POPF）和

- check protection as required by the segmented architecture but assume that the operating system is running at the highest privilege level.

> - 根据分段体系结构的要求检查保护，但假设操作系统正在以最高特权级别运行。

Virtual memory is also challenging. Because the 80x86 TLBs do not support process ID tags, as do most RISC architectures, it is more expensive for the VMM and guest OSes to share the TLB; each address space change typically requires a TLB flush.

> 虚拟内存也具有挑战性。由于 80x86 TLB 不支持流程 ID 标签，而且大多数 RISC 架构也是如此，因此 VMM 和来宾 OS 共享 TLB 更为昂贵。每个地址空间更改通常需要 TLB 冲洗。

> ===

>> ===
>>

Load access rights from segment descriptor (LAR) Load segment limit from segment descriptor (LSL) Verify if segment descriptor is readable (VERR) Verify if segment descriptor is writable (VERW) Pop to segment register (POP CS, POP SS, …) Push segment register (PUSH CS, PUSH SS, …) Far call to different privilege level (CALL)

> 从段描述符（LAR）负载段限制从段描述符（LSL）验证段描述符是否可读（VERR）验证段描述符（verr）是否段描述符（VERW）POP 到段寄存器（POP CS，POP SS，…）pusp 细分寄存器（推 CS，推送 SS，…）远至其他特权级别（呼叫）

> ===

>> ===
>>

Figure 2.31 Summary of 18 80x86 instructions that cause problems for virtualization (Robin and Irvine, 2000). The first five instructions of the top group allow a program in user mode to read a control register, such as a descriptor table register without causing a trap. The pop flags instruction modifies a control register with sensitive information but fails silently when in user mode. The protection checking of the segmented archi- tecture of the 80x86 is the downfall of the bottom group because each of these instruc- tions checks the privilege level implicitly as part of instruction execution when reading a control register. The checking assumes that the OS must be at the highest privilege level, which is not the case for guest VMs. Only the MOVE to segment register tries to modify control state, and protection checking foils it as well.

> 图 2.31 18 80x86 指令的摘要导致虚拟化问题（Robin 和 Irvine，2000）。顶级组的前五个说明允许以用户模式的程序读取控制寄存器，例如描述符表寄存器而不会引起陷阱。POP 标志指令修改了具有敏感信息的控制寄存器，但在用户模式下时会默默失败。80x86 的分段档案的保护检查是底部组的垮台，因为在读取控制寄存器时，这些仪器中的每一种都隐含地检查了指令执行的一部分。检查假设该操作系统必须处于最高特权级别，而对于来宾 VM 来说并非如此。只有移动到段寄存器的移动试图修改控制状态，并保护箔纸也将其检查。

Virtualizing I/O is also a challenge for the 80x86, in part because it supports memory-mapped I/O and has separate I/O instructions, but more importantly because there are a very large number and variety of types of devices and device drivers of PCs for the VMM to handle. Third-party vendors supply their own drivers, and they may not properly virtualize. One solution for conventional VM implementations is to load real device drivers directly into the VMM.

> 虚拟化 I/O 也是 80x86 的挑战，部分是因为它支持内存映射的 I/O 并具有单独的 I/O 指令，但更重要的是，因为有很多类型的设备和设备驱动程序的数量和多种类型 VMM 处理的 PC。第三方供应商提供自己的驾驶员，他们可能无法正确地虚拟化。常规 VM 实现的一种解决方案是将真实的设备驱动程序直接加载到 VMM 中。

To simplify implementations of VMMs on the 80x86, both AMD and Intel have proposed extensions to the architecture. Intel’s VT-x provides a new execu- tion mode for running VMs, a architected definition of the VM state, instructions to swap VMs rapidly, and a large set of parameters to select the circumstances where a VMM must be invoked. Altogether, VT-x adds 11 new instructions for the 80x86. AMD’s Secure Virtual Machine (SVM) provides similar functionality.

> 为了简化 80x86 上 VMM 的实现，AMD 和 Intel 都提出了对体系结构的扩展。英特尔的 VT-X 提供了用于运行 VM 的新执行模式，VM 状态的架构定义，快速交换 VM 的说明以及一组可以调用 VMM 的情况的参数。VT-X 总共为 80x86 添加了 11 个新说明。AMD 的安全虚拟机（SVM）提供了类似的功能。

After turning on the mode that enables VT-x support (via the new VMXON instruc- tion), VT-x offers four privilege levels for the guest OS that are lower in priority than the original four (and fix issues like the problem with the POPF instruction mentioned earlier). VT-xcapturesall thestatesofavirtualmachineinthe Virtual Machine Control State (VMCS) and then provides atomic instructions to save and restore a VMCS. In addition to critical state, the VMCS includes configuration information to deter- mine when to invoke the VMM and then specifically what caused the VMM to be invoked. To reduce the number of times the VMM must be invoked, this mode adds shadow versions of some sensitive registers and adds masks that check to see whether critical bits of a sensitive register will be changed before trapping. To reduce the cost of virtualizing virtual memory, AMD’s SVM adds an additional level of indirection, called _nested pagetables_, which makes shadow page tables unnecessary (see Section L.7 of Appendix L).

> 在打开了启用 VT-X 支持的模式（通过新的 VMXON 指导）之后，VT-X 为客座操作系统提供了四个特权级别，优先级比原始四个（并解决问题，例如解决问题的问题）POPF 指令前面提到）。VT-XCAPTURYSALL thestatesofirtualMachineinthe Virtual Machine 控制状态（VMC），然后提供原子指令以保存和恢复 VMC。除临界状态外，VMC 还包括配置信息，以确定何时调用 VMM，然后专门针对引起 VMM 的原因。为了减少必须调用 VMM 的次数，此模式添加了一些敏感寄存器的阴影版本，并添加了掩码，以检查敏感寄存器的关键位是否会在捕获之前更改。为了降低虚拟化虚拟内存的成本，AMD 的 SVM 添加了一个额外的间接级别，称为 *nested Pagetables*，这使得 Shadow Page 表不必要（请参阅附录 L 的 L.7 节）。
