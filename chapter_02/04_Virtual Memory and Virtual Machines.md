## Virtual Memory and Virtual Machines

> ##虚拟内存和虚拟机

A virtual machine is taken to be an efficient, isolated duplicate of the real machine. We explain these notions through the idea of a virtual machine monitor (VMM)… a VMM has three essential characteristics. First, the VMM provides an environment for programs which is essentially identical with the original machine; second, programs run in this environment show at worst only minor decreases in speed; and last, the VMM is in complete control of system resources.

> 虚拟机被视为真实机器的高效，孤立的重复。我们通过虚拟机监视器（VMM）的想法来解释这些概念……VMM 具有三个基本特征。首先，VMM 为程序提供了与原始机器基本相同的程序的环境。其次，在这种环境中运行的程序显示，最糟糕的速度只有很小的降低。最后，VMM 完全控制了系统资源。

Gerald Popek and Robert Goldberg,

> 杰拉尔德·波佩克和罗伯特·戈德堡，

_“Formal requirements for virtualizable third generation architectures,”_

> _“虚拟化第三代体系结构的正式要求”，_

Communications of the ACM (July 1974).

> ACM 的通信（1974 年 7 月）。

Section B.4 in [Appendix B](#_bookmark436) describes the key concepts in virtual memory. Recall that virtual memory allows the physical memory to be treated as a cache of secondary storage (which may be either disk or solid state). Virtual memory moves pages between the two levels of the memory hierarchy, just as caches move blocks between levels. Likewise, TLBs act as caches on the page table, eliminating the need to do a memory access every time an address is translated. Virtual memory also provides separation between processes that share one physical memory but have separate virtual address spaces. Readers should ensure that they understand both functions of virtual memory before continuing.

> B.4 节[附录 B]（#\_ Bookmark436）描述了虚拟内存中的关键概念。回想一下，虚拟内存允许物理内存被视为辅助存储的缓存（可能是磁盘或固态）。虚拟内存在内存层次结构的两个级别之间移动页面，就像缓存在级别之间移动块一样。同样，TLB 充当页面表上的缓存，无需每次翻译地址时进行内存访问。虚拟内存还提供了共享一个物理内存但具有单独的虚拟地址空间的进程之间的分离。读者应确保在继续之前了解虚拟内存的两个功能。

In this section, we focus on additional issues in protection and privacy between processes sharing the same processor. Security and privacy are two of the most vexing challenges for information technology in 2017. Electronic burglaries, often involving lists of credit card numbers, are announced regularly, and it’s widely believed that many more go unreported. Of course, such problems arise from programming errors that allow a cyberattack to access data it should be unable to access. Programming errors are a fact of life, and with modern complex software systems, they occur with significant regularity. Therefore both researchers and practitioners are looking for improved ways to make computing systems more secure. Although protecting information is not limited to hardware, in our view real security and privacy will likely involve innovation in computer architecture as well as in systems software.

> 在本节中，我们重点介绍共享同一处理器的流程之间的保护和隐私问题。安全性和隐私是 2017 年信息技术最烦人的两个挑战。经常宣布的电子入室盗窃案通常涉及信用卡号列表，并且人们普遍认为更多的是未报告的电子盗窃案。当然，这些问题是由允许网络攻击访问数据的编程错误引起的。编程错误是生活的事实，并且对于现代复杂的软件系统，它们的规律性很高。因此，研究人员和从业人员都在寻找改进的方法来使计算系统更加安全。尽管保护信息不仅限于硬件，但在我们看来，真正的安全性和隐私可能涉及计算机架构以及系统软件中的创新。

This section starts with a review of the architecture support for protecting processes from each other via virtual memory. It then describes the added protection provided by virtual machines, the architecture requirements of virtual machines, and the performance of a virtual machine. As we will see in [Chapter 6](#_bookmark268), virtual machines are a foundational technology for cloud computing.

> 本节从审查架构支持，以通过虚拟内存相互保护过程。然后，它描述了虚拟机提供的附加保护，虚拟机的体系结构要求以及虚拟机的性能。正如我们将在[第 6 章]（#\_ bookmark268）中看到的那样，虚拟机是云计算的基础技术。

### Protection via Virtual Memory

> ###通过虚拟内存保护

Page-based virtual memory, including a TLB that caches page table entries, is the primary mechanism that protects processes from each other. Sections B.4 and B.5 in [Appendix B](#_bookmark436) review virtual memory, including a detailed description of protection via segmentation and paging in the 80x86. This section acts as a quick review; if it’s too quick, please refer to the denoted [Appendix B](#_bookmark436) sections.

> 基于页面的虚拟内存，包括缓存页面表条目的 TLB，是彼此保护进程的主要机制。[附录 B]中的 B.4 和 B.5 节（#_ bookmark436）查看虚拟内存，包括通过分割和分页进行保护和分页的详细说明。本节是快速审查；如果太快了，请参考所表示的[附录 B]（#_ bookmark436）部分。

Multiprogramming, where several programs running concurrently share a computer, has led to demands for protection and sharing among programs and to the concept of a _process_. Metaphorically, a process is a program’s breathing air and living space—that is, a running program plus any state needed to continue running it. At any instant, it must be possible to switch from one process to another. This exchange is called a _process switch_ or _context switch_.

> 多编程，其中几个程序同时共享一台计算机，已导致对程序之间的保护和共享以及 *process* 的概念的需求。隐喻地，一个过程是程序的呼吸空气和生活空间，也就是说，一个跑步程序以及继续运行它所需的任何状态。在任何瞬间，都必须从一个过程切换到另一个过程。此交换称为 *Process Switch* 或 *Context Switch*。

The operating system and architecture join forces to allow processes to share the hardware yet not interfere with each other. To do this, the architecture must limit what a process can access when running a user process yet allow an operating system process to access more. At a minimum, the architecture must do the following:

> 操作系统和体系结构联合起来，允许进程共享硬件，但不会相互干扰。为此，体系结构必须限制运行用户进程时可以访问的过程，并允许操作系统进程访问更多。至少，体系结构必须执行以下操作：

1. Provide at least two modes, indicating whether the running process is a user process or an operating system process. This latter process is sometimes called a _kernel_ process or a _supervisor_ process.

> 1.至少提供两种模式，指示运行过程是用户过程还是操作系统过程。后一个过程有时称为 *kernel* 进程或 *supervisor* 进程。

2. Provide a portion of the processor state that a user process can use but not write. This state includes a user/supervisormode bit, an exceptionenable/disable bit, and memory protection information. Users are prevented from writing this state because the operating system cannot control user processes if users can give themselves supervisor privileges, disable exceptions, or change memory protection.

> 2.提供用户流程可以使用但不写的处理器状态的一部分。该状态包括用户/主管位，可异常/禁用位和内存保护信息。如果用户可以为自己提供主管特权，禁用异常或更改内存保护，则可以阻止用户编写此状态，因为操作系统无法控制用户流程。

3. Provide mechanisms whereby the processor can go from user mode to supervisor mode and vice versa. The first direction is typically accomplished by a _system call_, implemented as a special instruction that transfers control to a dedicated location in supervisor code space. The PC is saved from the point of the system call, and the processor is placed in supervisor mode. The return to user mode is like a subroutine return that restores the previous user/supervisor mode.

> 3.提供机制，使处理器可以从用户模式转变为主管模式，反之亦然。第一个方向通常是由 *system call* 实现的，该方向是作为特殊指令实现的，该指令将控制转移到主管代码空间中的专用位置。PC 是从系统调用点保存的，并且处理器处于主管模式。返回用户模式就像恢复以前的用户/主管模式的子例程返回。

4. Provide mechanisms to limit memory accesses to protect the memory state of a process without having to swap the process to disk on a context switch.

> 4.提供机制来限制内存访问以保护进程的内存状态，而无需将过程交换为上下文开关上的磁盘。

[Appendix A](#_bookmark391) describes several memory protection schemes, but by far the most popular is adding protection restrictions to each page of virtual memory. Fixedsized pages, typically 4 KiB, 16 KiB, or larger, are mapped from the virtual address space into physical address space via a page table. The protection restrictions are included in each page table entry. The protection restrictions might determine whether a user process can read this page, whether a user process can write to this page, and whether code can be executed from this page. In addition, a process can neither read nor write a page if it is not in the page table. Because only the OS can update the page table, the paging mechanism provides total access protection.

> [附录 A]（#\_ bookmark391）描述了几种内存保护方案，但到目前为止，最受欢迎的是在虚拟内存的每个页面上添加保护限制。固定的尺寸页面（通常为 4 KIB，16 KIB 或更大）通过页面表从虚拟地址空间映射到物理地址空间。保护限制包含在每个页表条目中。保护限制可能会确定用户进程是否可以读取此页面，用户进程是否可以写入此页面以及是否可以从此页面执行代码。此外，如果过程不在页面表中，则不能读取或编写页面。因为只有操作系统才能更新页面表，所以分页机制可提供全面的访问保护。

Paged virtual memory means that every memory access logically takes at least twice as long, with one memory access to obtain the physical address and a second access to get the data. This cost would be far too dear. The solution is to rely on the principle of locality; if the accesses have locality, then the _address translations_ for the accesses must also have locality. By keeping these address translations in a special cache, a memory access rarely requires a second access to translate the address. This special address translation cache is referred to as a TLB.

> 分类虚拟内存意味着每个内存访问在逻辑上至少需要两倍的时间，其中一个内存访问以获取物理地址和第二个访问以获取数据。这笔费用太珍贵了。解决方案是依靠当地的原则。如果访问权限具有局部性，则*address translations *的访问也必须具有局部性。通过将这些地址翻译放在特殊的缓存中，内存访问很少需要第二个访问来翻译地址。此特殊地址翻译缓存称为 TLB。

A TLB entry is like a cache entry where the tag holds portions of the virtual address and the data portion holds a physical page address, protection field, valid bit, and usually a use bit and a dirty bit. The operating system changes these bits by changing the value in the page table and then invalidating the corresponding TLB entry. When the entry is reloaded from the page table, the TLB gets an accurate copy of the bits.

> TLB 条目就像一个缓存条目，该标签保存虚拟地址的一部分，并且数据部分保留一个物理页面地址，保护字段，有效位，通常是使用位和肮脏位。操作系统通过更改页面表中的值，然后使相应的 TLB 条目无效。从页面表重新加载条目时，TLB 将获得位的准确副本。

Assuming the computer faithfully obeys the restrictions on pages and maps virtual addresses to physical addresses, it would seem that we are done. Newspaper headlines suggest otherwise.

> 假设计算机忠实地遵守页面上的限制，并将虚拟地址映射到物理地址，则似乎我们已经完成了。报纸头条建议。

The reason we’re not done is that we depend on the accuracy of the operating system as well as the hardware. Today’s operating systems consist of tens of millions of lines of code. Because bugs are measured in number per thousand lines of code, there are thousands of bugs in production operating systems. Flaws in the OS have led to vulnerabilities that are routinely exploited.

> 我们之所以没有完成的原因是我们依赖操作系统和硬件的准确性。当今的操作系统由数千万行代码组成。由于错误是每千行代码的数量测量的，因此生产操作系统中有数千个错误。操作系统中的缺陷导致了常规利用的漏洞。

This problem and the possibility that not enforcing protection could be much more costly than in the past have led some to look for a protection model with a much smaller code base than the full OS, such as virtual machines.

> 这个问题和不执行保护的可能性可能比过去的成本要高得多。

### Protection via Virtual Machines

> ###通过虚拟机保护

An idea related to virtual memory that is almost as old are virtual machines (VMs). They were first developed in the late 1960s, and they have remained an important part of mainframe computing over the years. Although largely ignored in the domain of single-user computers in the 1980s and 1990s, they have recently gained popularity because of

> 与虚拟内存有关的想法几乎与旧的是虚拟机（VM）。它们是在 1960 年代后期首次开发的，多年来，它们仍然是大型机计算的重要组成部分。尽管在 1980 年代和 1990 年代，在单用户计算机的域中基本上被忽略了，但由于

the increasing importance of isolation and security in modern systems;

> 现代系统中隔离和安全的重要性日益增加；

the failures in security and reliability of standard operating systems;

> 标准操作系统的安全性和可靠性失败；

the sharing of a single computer among many unrelated users, such as in a data center or cloud; and

> 在许多无关用户（例如数据中心或云）中共享一台计算机；和

the dramatic increases in the raw speed of processors, which make the overhead of VMs more acceptable.

> 原始处理器的原始速度急剧提高，这使得 VM 的开销更加可接受。

The broadest definition of VMs includes basically all emulation methods that provide a standard software interface, such as the Java VM. We are interested in

> VM 的最广泛定义包括所有提供标准软件接口的仿真方法，例如 Java VM。我们对

VMs that provide a complete system-level environment at the binary instruction set architecture (ISA) level. Most often, the VM supports the same ISA as the underlying hardware; however, it is also possible to support a different ISA, and such approaches are often employed when migrating between ISAs in order to allow software from the departing ISA to be used until it can be ported to the new ISA. Our focus here will be on VMs where the ISA presented by the VM and the underlying hardware match. Such VMs are called (operating) _system virtual machines_. IBM VM/370, VMware ESX Server, and Xen are examples. They present the illusion that the users of a VM have an entire computer to themselves, including a copy of the operating system. A single computer runs multiple VMs and can support a number of different operating systems (OSes). On a conventional platform, a single OS “owns” all the hardware resources, but with a VM, multiple OSes all share the hardware resources.

> 在二进制指令集体系结构（ISA）级别提供完整的系统级环境的 VM。大多数情况下，VM 支持与基础硬件相同的 ISA；但是，也可以支持不同的 ISA，并且在 ISA 之间迁移时通常采用此类方法，以便允许出发 ISA 的软件使用，直到可以将其移植到新的 ISA 为止。我们在这里的重点将放在 VM 上，其中 ISA 由 VM 和基础硬件匹配介绍。此类 VM 称为（操作）_System Virtual Machines_。IBM VM/370，VMware ESX 服务器和 XEN 是示例。他们幻想了 VM 的用户将整个计算机本身都具有整个计算机，包括操作系统的副本。一台计算机运行多个 VM，可以支持许多不同的操作系统（OS）。在传统平台上，单个 OS“拥有”所有硬件资源，但是使用 VM，多个 OS 都共享了硬件资源。

The software that supports VMs is called a _virtual machine monitor_ (VMM) or _hypervisor_; the VMM is the heart of virtual machine technology. The underlying hardware platform is called the _host_, and its resources are shared among the _guest_ VMs. The VMM determines how to map virtual resources to physical resources: A physical resource may be time-shared, partitioned, or even emulated in software. The VMM is much smaller than a traditional OS; the isolation portion of a VMM is perhaps only 10,000 lines of code.

> 支持 VM 的软件称为 *virtual Machine Monitor*（VMM）或 *HYPERVISOR*;VMM 是虚拟机技术的核心。基础硬件平台称为 *host*，其资源在 *GUEST* VM 之间共享。VMM 确定如何将虚拟资源映射到物理资源：物理资源可以在软件中进行定时，分区甚至模仿。VMM 比传统的操作系统小得多。VMM 的隔离部分可能只有 10,000 行代码。

In general, the cost of processor virtualization depends on the workload. Userlevel processor-bound programs, such as SPECCPU2006, have zero virtualization overhead because the OS is rarely invoked, so everything runs at native speeds. Conversely, I/O-intensive workloads generally are also OS-intensive and execute many system calls (which doing I/O requires) and privileged instructions that can result in high virtualization overhead. The overhead is determined by the number of instructions that must be emulated by the VMM and how slowly they are emulated. Therefore, when the guest VMs run the same ISA as the host, as we assume here, the goal of the architecture and the VMM is to run almost all instructions directly on the native hardware. On the other hand, if the I/O-intensive workload is also _I_/_O-bound_, the cost of processor virtualization can be completely hidden by low processor utilization because it is often waiting for I/O.

> 通常，处理器虚拟化的成本取决于工作负载。用户 Lelevel 处理器的程序（例如 SpecCPU2006）的虚拟化开销为零，因为很少调用操作系统，因此一切都以本机速度运行。相反，I/O 密集型工作负载通常也是 OS 密集型的，并且执行许多系统调用（使用 I/O 所需）和可能导致高虚拟化开销的特权说明。开销取决于必须由 VMM 模拟的指令数量以及模拟它们的慢速。因此，当访客 VM 与主机运行相同的 ISA 时，正如我们在此处假设的那样，架构的目标和 VMM 的目标是直接在本机硬件上直接运行所有指令。另一方面，如果 I/O 密集型工作负载也是*i */_ o-bound_，则处理器虚拟化的成本可以通过低处理器的利用来完全隐藏，因为它通常在等待 I/O。

Although our interest here is in VMs for improving protection, VMs provide two other benefits that are commercially significant:

> 尽管我们在这里的兴趣是改善保护的 VM，但 VM 提供了其他两个具有商业意义的好处：

1. _Managing software_—VMs provide an abstraction that can run the complete software stack, even including old operating systems such as DOS. A typical deployment might be some VMs running legacy OSes, many running the current stable OS release, and a few testing the next OS release.

> 1. _管理软件_- VM 提供一个可以运行完整软件堆栈的抽象，甚至包括旧操作系统（例如 DOS）。典型的部署可能是运行遗产 OS 的一些 VM，许多运行当前稳定的操作系统版本以及一些测试下一个 OS 版本。

2. _Managing hardware_—One reason for multiple servers is to have each application running with its own compatible version of the operating system on separate computers, as this separation can improve dependability. VMs allow these separate software stacks to run independently yet share hardware, thereby consolidating the number of servers. Another example is that most newer VMMs support migration of a running VM to a different computer, either to balance load or to evacuate from failing hardware. The rise of cloud computing has made the ability to swap out an entire VM to another physical processor increasingly useful.

> 2. _管理硬件_-多个服务器的一个原因是，每个应用程序都在单独的计算机上运行其自己的操作系统的兼容版本，因为此分离可以提高可靠性。VMS 允许这些单独的软件堆栈独立运行但共享硬件，从而巩固了服务器的数量。另一个示例是，大多数较新的 VMM 都支持运行 VM 迁移到另一台计算机，以平衡负载或撤离故障硬件。云计算的兴起使得将整个 VM 交换为另一个物理处理器的能力越来越有用。

These two reasons are why cloud-based servers, such as Amazon’s, rely on virtual machines.

> 这两个原因是为什么基于云的服务器（例如亚马逊）依赖虚拟机的原因。

### Requirements of a Virtual Machine Monitor

> ###虚拟机监视器的要求

What must a VM monitor do? It presents a software interface to guest software, it must isolate the state of guests from each other, and it must protect itself from guest software (including guest OSes). The qualitative requirements are

> VM 监视器必须做什么？它向访客软件提供了软件接口，必须将客人的状态隔离，并且必须保护自己免受访客软件（包括来宾 OS）的侵害。定性要求是

Guest software should behave on a VM exactly as if it were running on the native hardware, except for performance-related behavior or limitations of fixed resources shared by multiple VMs.

> 访客软件应在 VM 上的行为，就像在本机硬件上运行一样，除了与性能相关的行为或多个 VM 共享的固定资源的限制外。

Guest software should not be able to directly change allocation of real system resources.

> 来宾软件不应直接更改真实系统资源的分配。

To “virtualize” the processor, the VMM must control just about everything— access to privileged state, address translation, I/O, exceptions and interrupts—even though the guest VM and OS currently running are temporarily using them.

> 要“虚拟化”处理器，VMM 必须控制几乎所有内容 - 访问特权状态，地址翻译，I/O，异常和中断，即使当前运行的访客 VM 和 OS 暂时使用它们。

For example, in the case of a timer interrupt, the VMM would suspend the currently running guest VM, save its state, handle the interrupt, determine which guest VM to run next, and then load its state. Guest VMs that rely on a timer interrupt are provided with a virtual timer and an emulated timer interrupt by the VMM.

> 例如，在计时器中断的情况下，VMM 会暂停当前运行的访客 VM，保存其状态，处理中断，确定下一步运行的访客 VM，然后加载其状态。依靠计时器中断的访客 VM 带有虚拟计时器和 VMM 中断的模拟计时器。

To be in charge, the VMM must be at a higher privilege level than the guest VM, which generally runs in user mode; this also ensures that the execution of any privileged instruction will be handled by the VMM. The basic requirements of system virtual machines are almost identical to those for the previously mentioned paged virtual memory:

> 为了负责，VMM 必须比通常以用户模式运行的来宾 VM 的特权级别更高。这也确保了 VMM 的执行任何特权指令。系统虚拟机的基本要求几乎与前面提到的页面虚拟内存相同：

At least two processor modes, system and user.

> 至少两个处理器模式，系统和用户。

A privileged subset of instructions that is available only in system mode, resulting in a trap if executed in user mode. All system resources must be controllable only via these instructions.

> 仅在系统模式下可用的特权子集指令子集，如果在用户模式下执行，则会导致陷阱。所有系统资源必须仅通过这些说明才能控制。

### Instruction Set Architecture Support for Virtual Machines

> ###指令集架构支持虚拟机

If VMs are planned for during the design of the ISA, it’s relatively easy to reduce both the number of instructions that must be executed by a VMM and how long it takes to emulate them. An architecture that allows the VM to execute directly on the hardware earns the title _virtualizable_, and the IBM 370 architecture proudly bears that label.

> 如果在 ISA 设计期间计划进行 VM，则相对容易减少 VMM 必须执行的指令数量以及模拟它们所需的时间。允许 VM 直接在硬件上执行的架构可以赢得标题 *virtualizable*，而 IBM 370 架构自豪地承受了标签。

However, because VMs have been considered for desktop and PC-based server applications only fairly recently, most instruction sets were created without virtualization in mind. These culprits include 80x86 and most of the original RISC architectures, although the latter had fewer issues than the 80x86 architecture. Recent additions to the x 86 architecture have attempted to remedy the earlier shortcomings, and RISC V explicitly includes support for virtualization.

> 但是，由于仅最近公平地考虑了台式机和基于 PC 的服务器应用程序的 VM，因此大多数指令集是创建的，而没有虚拟化。这些罪魁祸首包括 80x86 和大多数原始 RISC 架构，尽管后者的问题少于 80x86 架构。X 86 体系结构的最新添加试图纠正较早的缺点，RISC V 明确包括对虚拟化的支持。

Because the VMM must ensure that the guest system interacts only with virtual resources, a conventional guest OS runs as a user mode program on top of the VMM. Then, if a guest OS attempts to access or modify information related to hardware resources via a privileged instruction—for example, reading or writing the page table pointer—it will trap to the VMM. The VMM can then effect the appropriate changes to corresponding real resources.

> 由于 VMM 必须确保访客系统仅与虚拟资源进行交互，因此传统的访客操作系统在 VMM 顶部作为用户模式程序运行。然后，如果来宾操作系统尝试通过特权指令（例如，读取或编写页面表指针）访问或修改与硬件资源相关的信息，则将陷入 VMM。然后，VMM 可以对相应的真实资源进行适当的更改。

Therefore, if any instruction that tries to read or write such sensitive information traps when executed in user mode, the VMM can intercept it and support a virtual version of the sensitive information as the guest OS expects.

> 因此，如果在用户模式下执行时尝试读取或写入此类敏感信息陷阱的任何指令，VMM 可以按照访客操作系统的期望拦截它并支持敏感信息的虚拟版本。

In the absence of such support, other measures must be taken. A VMM must take special precautions to locate all problematic instructions and ensure that they behave correctly when executed by a guest OS, thereby increasing the complexity of the VMM and reducing the performance of running the VM. [Sections 2.5](#cross-cutting-issues-the-design-of-memory-hierarchies) and

> 在没有这种支持的情况下，必须采取其他措施。VMM 必须采取特殊的预防措施来定位所有有问题的说明，并确保在由访客操作系统执行时正确行事，从而提高 VMM 的复杂性并降低运行 VM 的性能。[第 2.5 节]（＃跨剪辑发行的记忆层次结构）和

[2.7](#_bookmark81) give concrete examples of problematic instructions in the 80x86 architecture. One attractive extension allows the VM and the OS to operate at different privilege levels, each of which is distinct from the user level. By introducing an additional privilege level, some OS operations—e.g., those that exceed the permissions granted to a user program but do not require intervention by the VMM (because they cannot affect any other VM)—can execute directly without the overhead of trapping and invoking the VMM. The Xen design, which we examine shortly, makes use of three privilege levels.

> [2.7]（#\_ bookmark81）在 80x86 体系结构中给出具体说明的具体示例。一个有吸引力的扩展名使 VM 和 OS 可以在不同的特权级别上运行，每个特权级别都与用户级别不同。通过引入额外的特权级别，某些 OS 操作（例如，超过授予用户程序但不需要 VMM 干预的权限的操作系统（因为它们不能影响任何其他 VM））可以直接执行，而无需陷入困境和陷阱的开销调用 VMM。我们很快就检查了 XEN 设计，利用了三个特权级别。

### Impact of Virtual Machines on Virtual Memory and I/O

> ###虚拟机对虚拟内存的影响和 I/O

Another challenge is virtualization of virtual memory, as each guest OS in every VM manages its own set of page tables. To make this work, the VMM separates the notions of _real_ and _physical memory_ (which are often treated synonymously) and makes real memory a separate, intermediate level between virtual memory and physical memory. (Some use the terms _virtual memory, physical memory_, and _machine memory_ to name the same three levels.) The guest OS maps virtual memory to real memory via its page tables, and the VMM page tables map the guests’ real memory to physical memory. The virtual memory architecture is specified either via page tables, as in IBM VM/370 and the 80x86, or via the TLB structure, as in many RISC architectures.

> 另一个挑战是虚拟内存的虚拟化，因为每个 VM 中的每个来宾操作系统都管理着自己的一组页面表。为了进行这项工作，VMM 将 *real* 和 *physical Memory* 的概念分开（通常是代名词），并使真实内存成为虚拟内存和物理内存之间的单独的中间级别。（有些人使用术语 *virtual 内存，物理内存*和 *machine memory* 命名相同的三个级别。）访客操作系统映射虚拟内存通过其页面表格到真实内存，而 VMM 页面映射了来宾的真实内存到物理内存。如 IBM VM/370 和 80x86 或 TLB 结构，如许多 RISC 架构中，通过页面表指定了虚拟内存体系结构。

Rather than pay an extra level of indirection on every memory access, the VMM maintains a _shadow page table_ that maps directly from the guest virtual address space to the physical address space of the hardware. By detecting all modifications to the guest’s page table, the VMM can ensure that the shadow page table entries being used by the hardware for translations correspond to those of the guest OS environment, with the exception of the correct physical pages substituted for the real pages in the guest tables. Therefore the VMM must trap any attempt by the guest OS to change its page table or to access the page table pointer. This is commonly done by write protecting the guest page tables and trapping any access to the page table pointer by a guest OS. As previously noted, the latter happens naturally if accessing the page table pointer is a privileged operation.

> VMM 并没有在每个内存访问中支付额外的间接水平，而是维护一个 *Shadow Page table*，该页面\_直接从访客虚拟地址空间映射到硬件的物理地址空间。通过检测到客人页面表的所有修改，VMM 可以确保硬件用于翻译的阴影页表条目与访客 OS 环境的翻译相对应，除了正确的物理页面替换为“正确的物理页面”。来宾桌。因此，VMM 必须捕获来宾操作系统更改其页面表或访问页面表指针的任何尝试。这通常是通过编写保护“来宾”表并捕获访客操作系统对页面表指针的任何访问来完成的。如前所述，如果访问页面指针是特权操作，则后者自然发生。

The IBM 370 architecture solved the page table problem in the 1970s with an additional level of indirection that is managed by the VMM. The guest OS keeps its page tables as before, so the shadow pages are unnecessary. AMD has implemented a similar scheme for its 80x86.

> IBM 370 体系结构在 1970 年代解决了 Page 表问题，并以 VMM 管理的额外的间接水平。来宾操作系统像以前一样保留其页面表，因此不需要阴影页面。AMD 已针对其 80x86 实施了类似的方案。

To virtualize the TLB in many RISC computers, the VMM manages the real TLB and has a copy of the contents of the TLB of each guest VM. To pull this off, any instructions that access the TLB must trap. TLBs with Process ID tags can support a mix of entries from different VMs and the VMM, thereby avoiding flushing of the TLB on a VM switch. Meanwhile, in the background, the VMM supports a mapping between the VMs’ virtual Process IDs and the real Process IDs. Section L.7 of online Appendix L describes additional details.

> 为了在许多 RISC 计算机中虚拟化 TLB，VMM 管理了真实的 TLB，并具有每个来宾 VM 的 TLB 内容的副本。要实现这一目标，任何访问 TLB 的指令都必须陷入困境。带有流程 ID 标签的 TLB 可以支持来自不同 VM 和 VMM 的条目的混合，从而避免了 VM 开关上 TLB 的冲洗。同时，在后台，VMM 支持 VMS 的虚拟进程 ID 和实际过程 ID 之间的映射。在线附录 L 节 L.7 节介绍了其他详细信息。

The final portion of the architecture to virtualize is I/O. This is by far the most difficult part of system virtualization because of the increasing number of I/O devices attached to the computer _and_ the increasing diversity of I/O device types. Another difficulty is the sharing of a real device among multiple VMs, and yet another comes from supporting the myriad of device drivers that are required, especially if different guest OSes are supported on the same VM system. The VM illusion can be maintained by giving each VM generic versions of each type of I/O device driver, and then leaving it to the VMM to handle real I/O.

> 虚拟化架构的最后部分是 I/O。这是迄今为止系统虚拟化最困难的部分，因为在计算机上附加了 I/O 设备的数量越来越多，I/O 设备类型的多样性的增加。另一个困难是在多个 VM 中共享真实设备，而另一个困难来自支持所需的无数设备驱动程序，尤其是如果在同一 VM 系统上支持不同的访客 OSS 时。可以通过给出每种类型的 I/O 设备驱动程序的 VM 通用版本，然后将其保留到 VMM 以处理真实的 I/O 来维护 VM 幻觉。

The method for mapping a virtual-to-physical I/O device depends on the type of device. For example, physical disks are normally partitioned by the VMM to create virtual disks for guest VMs, and the VMM maintains the mapping of virtual tracks and sectors to the physical ones. Network interfaces are often shared between VMs in very short time slices, and the job of the VMM is to keep track of messages for the virtual network addresses to ensure that guest VMs receive only messages intended for them.

> 映射虚拟到物理 I/O 设备的方法取决于设备的类型。例如，通常通过 VMM 对物理磁盘进行分区，以为访客 VM 创建虚拟磁盘，并且 VMM 将虚拟轨道和扇区的映射保持到物理磁盘的映射。网络接口通常在很短的时间段内在 VM 之间共享，而 VMM 的作业是跟踪虚拟网络地址的消息，以确保访客 VM 仅接收给他们的消息。

### Extending the Instruction Set for Efficient Virtualization and Better Security

> ###扩展指令集以有效的虚拟化和更好的安全性

In the past 5–10 years, processor designers, including those at AMD and Intel (and to a lesser extent ARM), have introduced instruction set extensions to more efficiently support virtualization. Two primary areas of performance improvement have been in handling page tables and TLBs (the cornerstone of virtual memory) and in I/O, specifically handling interrupts and DMA. Virtual memory performance is enhanced by avoiding unnecessary TLB flushes and by using the nested page table mechanism, employed by IBM decades earlier, rather than a complete set of shadow page tables (see Section L.7 in Appendix L). To improve I/O performance, architectural extensions are added that allow a device to directly use DMA to move data (eliminating a potential copy by the VMM) and allow device interrupts and commands to be handled by the guest OS directly. These extensions show significant performance gains in applications that are intensive either in their memory-management aspects or in the use of I/O.

> 在过去的 5 - 10 年中，包括 AMD 和 Intel（以及较小程度的 ARM）在内的处理器设计师引入了指导集扩展，以更有效地支持虚拟化。在处理页面表和 TLB（虚拟内存的基石）以及 I/O 中的两个主要领域以及尤其是处理中断和 DMA 中的领域。通过避免不必要的 TLB 冲洗和使用 IBM 几十年前使用的嵌套页面表机制，而不是完整的阴影页面表（请参阅附录 L 中的 L.7 节），可以增强虚拟内存性能。为了提高 I/O 性能，添加了架构扩展，允许设备直接使用 DMA 移动数据（消除 VMM 的潜在副本），并允许客座操作系统直接处理设备中断和命令。这些扩展显示在其内存管理方面或使用 I/O 方面的应用程序中显示出显着的性能增长。

With the broad adoption of public cloud systems for running critical applications, concerns have risen about security of data in such applications. Any malicious code that is able to access a higher privilege level than data that must be kept secure compromises the system. For example, if you are running a credit card processing application, you must be absolutely certain that malicious users cannot get access to the credit card numbers, even when they are using the same hardware and intentionally attack the OS or even the VMM. Through the use of virtualization, we can prevent accesses by an outside user to the data in a different VM, and this provides significant protection compared to a multiprogrammed environment. That might not be enough, however, if the attacker compromises the VMM or can find out information by observations in another VMM. For example, suppose the attacker penetrates the VMM; the attacker can then remap memory so as to access any portion of the data.

> 随着公共云系统用于运行关键应用程序的广泛采用，对此类应用程序中数据安全性的担忧增加了。与必须保持安全的数据相比，任何能够访问更高的特权级别的恶意代码都会损害系统。例如，如果您正在运行信用卡处理应用程序，则必须绝对确定恶意用户即使使用相同的硬件并有意攻击 OS 甚至 VMM，也无法访问信用卡号。通过使用虚拟化，我们可以防止外部用户在不同 VM 中对数据的访问，并且与多编程环境相比，这提供了重要的保护。但是，如果攻击者损害 VMM 或可以通过其他 VMM 中的观察发现信息，则可能还不够。例如，假设攻击者穿透 VMM。然后，攻击者可以重建内存，以访问数据的任何部分。

Alternatively, an attack might rely on a Trojan horse (see [Appendix B](#_bookmark436)) introduced into the code that can access the credit cards. Because the Trojan horse is running in the same VM as the credit card processing application, the Trojan horse only needs to exploit an OS flaw to gain access to the critical data. Most cyberattacks have used some form of Trojan horse, typically exploiting an OS flaw, that either has the effect of returning access to the attacker while leaving the CPU still in privilege mode or allows the attacker to upload and execute code as if it were part of the OS. In either case, the attacker obtains control of the CPU and, using the higher privilege mode, can proceed to access anything within the VM. Note that encryption alone does not prevent this attacker. If the data in memory is unencrypted, which is typical, then the attacker has access to all such data. Furthermore, if the attacker knows where the encryption key is stored, the attacker can freely access the key and then access any encrypted data.

> 另外，攻击可能依赖于可以访问信用卡的代码中引入的特洛伊木马（请参阅[附录 B]（#\_ bookmark436））。由于特洛伊木马在与信用卡处理应用程序相同的 VM 中运行，因此特洛伊木马只需要利用操作系统缺陷即可访问关键数据。大多数网络攻击都使用了某种形式的特洛伊木马，通常利用 OS 缺陷，这要么具有返回攻击者访问权限的效果，而在使 CPU 保持特权模式下，要么允许攻击者上传和执行代码，就好像它是属于操作系统。无论哪种情况，攻击者都可以控制 CPU，并且使用较高的特权模式可以继续访问 VM 内的任何内容。请注意，仅加密并不能阻止此攻击者。如果内存中的数据未加密，这是典型的，则攻击者可以访问所有此类数据。此外，如果攻击者知道存储加密密钥的位置，则攻击者可以自由访问密钥，然后访问任何加密数据。

More recently, Intel introduced a set of instruction set extensions, called the software guard extensions (SGX), to allow user programs to create _enclaves_, portions of code and data that are always encrypted and decrypted only on use and only with the key provided by the user code. Because the enclave is always encrypted, standard OS operations for virtual memory or I/O can access the enclave (e.g., to move a page) but cannot extract any information. For an enclave to work, all the code and all the data required must be part of the enclave. Although the topic of finer-grained protection has been around for decades, it has gotten little traction before because of the high overhead and because other solutions that are more efficient and less intrusive have been acceptable. The rise of cyberattacks and the amount of confidential information online have led to a reexamination of techniques for improving such fine-grained security. Like Intel’s SGX, IBM and AMD’s recent processors support on-the-fly encryption of memory.

> 最近，英特尔引入了一组指令集扩展程序，称为软件保护扩展（SGX），以允许用户程序创建 *enclaves*，代码的一部分和数据，这些代码和数据始终仅在使用时加密和解密，并且仅在使用中提供。用户代码。由于飞地总是被加密的，因此虚拟内存的标准 OS 操作可以访问飞地（例如，移动页面），但无法提取任何信息。为了使飞地工作，所有所需的代码和所有数据都必须是飞地的一部分。尽管更细粒度的保护的话题已经存在数十年了，但由于头顶高的头顶和其他更有效且侵入性较低的解决方案是可以接受的，因此它以前几乎没有吸引力。网络攻击的兴起和在线机密信息的数量已导致重新审查改善这种细粒度安全的技术。像英特尔的 SGX 一样，IBM 和 AMD 最近的处理器支持内存的即时加密。

### An Example VMM: The Xen Virtual Machine

> ###示例 VMM：Xen 虚拟机

Early in the development of VMs, a number of inefficiencies became apparent. For example, a guest OS manages its virtual-to-real page mapping, but this mapping is ignored by the VMM, which performs the actual mapping to physical pages. In other words, a significant amount of wasted effort is expended just to keep the guest OS happy. To reduce such inefficiencies, VMM developers decided that it may be worthwhile to allow the guest OS to be aware that it is running on a VM. For example, a guest OS could assume a real memory as large as its virtual memory so that no memory management is required by the guest OS.

> 在 VM 的发展初期，许多低效率变得显而易见。例如，访客操作系统管理其虚拟通行的页面映射，但是该映射被 VMM 忽略，该映射执行了实际映射到物理页面。换句话说，花费了大量浪费的努力，只是为了让客人的操作系统快乐。为了减少这种低效率，VMM 开发人员认为，让访客操作系统可以意识到它在 VM 上运行可能是值得的。例如，来宾操作系统可以假设实际内存与其虚拟内存一样大，因此来宾操作系统不需要内存管理。

Allowing small modifications to the guest OS to simplify virtualization is referred to as _paravirtualization_, and the open source Xen VMM is a good example. The Xen VMM, which is used in Amazon’s web services data centers, provides a guest OS with a virtual machine abstraction that is similar to the physical hardware, but drops many of the troublesome pieces. For example, to avoid flushing the TLB, Xen maps itself into the upper 64 MiB of the address space of each VM. Xen allows the guest OS to allocate pages, checking only to be sure the guest OS does not violate protection restrictions. To protect the guest OS from the user programs in the VM, Xen takes advantage of the four protection levels available in the 80x86. The Xen VMM runs at the highest privilege level (0), the guest OS runs at the next level (1), and the applications run at the lowest privilege level (3). Most OSes for the 80x 86 keep everything at privilege levels 0 or 3. For subsetting to work properly, Xen modifies the guest OS to not use problematic portions of the architecture. For example, the port of Linux to Xen changes about 3000 lines, or about 1% of the 80x86-specific code. These changes, however, do not affect the application binary interfaces of the guest OS.

> 允许对来宾操作系统简化虚拟化的小修改称为 *paravirtualization*，开源 Xen VMM 是一个很好的例子。XEN VMM 在亚马逊的 Web 服务数据中心使用，为访客操作系统提供了一个类似于物理硬件的虚拟机器抽象，但丢弃了许多麻烦的作品。例如，为避免冲洗 TLB，Xen 将其映射到每个 VM 地址空间的上部 64 MIB 中。XEN 允许来宾操作系统分配页面，仅检查以确保访客操作系统不违反保护限制。为了保护客座操作系统免受 VM 中的用户程序的侵害，XEN 利用了 80x86 中可用的四个保护级别。XEN VMM 以最高特权级别（0）运行，来宾操作系统在下一级别（1）运行，并且应用程序以最低特权级别运行（3）。80x 86 的大多数 OS 将所有内容保持在特权级别为 0 或 3。要为子集正常工作，XEN 会修改来宾操作系统，以不使用架构的有问题的部分。例如，Linux 到 XEN 的端口更改约 3000 行，或约 1％的 80x86 特定代码。但是，这些更改不会影响来宾操作系统的应用二进制接口。

To simplify the I/O challenge of VMs, Xen assigned privileged virtual machines to each hardware I/O device. These special VMs are called _driver domains_. (Xen calls VMs “domains.”) Driver domains run the physical device drivers, although interrupts are still handled by the VMM before being sent to the appropriate driver domain. Regular VMs, called _guest domains_, run simple virtual device drivers that must communicate with the physical device drivers in the driver domains over a channel to access the physical I/O hardware. Data are sent between guest and driver domains by page remapping.

> 为了简化 VM 的 I/O 挑战，XEN 分配给每个硬件 I/O 设备的特权虚拟机。这些特殊的 VM 称为*驱动器域*。（Xen 称 VM 为“域”。）驱动程序域运行物理设备驱动程序，尽管在发送到适当的驱动程序域之前，VMM 仍在处理中断。常规 VM，称为* Guest 域*，运行简单的虚拟设备驱动程序，必须通过通道上的驱动程序域中的物理设备驱动程序通信，以访问物理 I/O 硬件。数据是通过页面重新映射在访客和驱动程序域之间发送的。
