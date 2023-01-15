---
data: 2023-01-15
---
> [!note] : 计算机架构的简单概述
> 这一章简单概述现有几种计算机架构

## Defining Computer Architecture(定义计算机架构)

The task the computer designer faces is a complex one: determine what attributes are important for a new computer, then design a computer to maximize performance and energy efficiency while staying within cost, power, and availability constraints. This task has many aspects, including instruction set design, functional organization, logic design, and implementation. The implementation may encompass integrated circuit design, packaging, power, and cooling. Optimizing the design requires familiarity with a very wide range of technologies, from compilers and operating systems to logic design and packaging.

> 计算机设计人员面对的任务是一个复杂的任务：确定哪些属性对新计算机很重要，然后设计计算机以最大程度地提高性能和能源效率，同时保持成本，功率和可用性约束。此任务具有许多方面，包括指令集设计，功能组织，逻辑设计和实施。实施可能包括集成电路设计，包装，电源和冷却。优化设计需要熟悉非常广泛的技术，从编译器和操作系统到逻辑设计和包装。

A few decades ago, the term _computer architecture_ generally referred to only instruction set design. Other aspects of computer design were called _implementation,_ often insinuating that implementation is uninteresting or less challenging.

> 几十年前，术语 _computer 架构_ 通常仅参考指令集设计。计算机设计的其他方面称为 _ IMPLENTATION，_ 通常暗示实施是无趣或挑战更少的。

We believe this view is incorrect. The architect’s or designer’s job is much more than instruction set design, and the technical hurdles in the other aspects of the project are likely more challenging than those encountered in instruction set design. We’ll quickly review instruction set architecture before describing the larger challenges for the computer architect.

> 我们认为这种观点是不正确的。工程师或设计师的工作远不止于指导集设计，而且该项目的其他方面的技术障碍可能比教学集设计中遇到的技术更具挑战性。在描述计算机架构师面临的更大挑战之前，我们将快速回顾指令集体系结构。

### Instruction Set Architecture: The Myopic View of Computer Architecture(指令集体系结构：计算机体系结构的近视视图)

We use the term _instruction set architecture_ (ISA) to refer to the actual programmer-visible instruction set in this book. The ISA serves as the boundary between the software and hardware. This quick review of ISA will use examples from 80x86, ARMv8, and RISC-V to illustrate the seven dimensions of an ISA. The most popular RISC processors come from ARM (Advanced RISC Machine), which were in 14.8 billion chips shipped in 2015, or roughly 50 times as many chips that shipped with 80x86 processors. Appendices A and K give more details on the three ISAs.

> 我们使用术语 _Instruction set architecture_ (ISA)来参考本书中的实际程序员可见指令。ISA 充当软件和硬件之间的边界。对 ISA 的快速回顾将使用 80x86，ARMV8 和 RISC-V 的示例来说明 ISA 的七个维度。最受欢迎的 RISC 处理器来自 ARM(Advanced Risc Machine)，该机器在 2015 年运送的 148 亿芯片，约为带有 80x86 处理器的芯片的 50 倍。附录 A 和 K 提供了有关三个 ISA 的更多详细信息。

RISC-V (“RISC Five”) is a modern RISC instruction set developed at the University of California, Berkeley, which was made free and openly adoptable in response to requests from industry. In addition to a full software stack (compilers, operating systems, and simulators), there are several RISC-V implementations freely available for use in custom chips or in field-programmable gate arrays. Developed 30 years after the first RISC instruction sets, RISC-V inherits its ancestors’ good ideas—a large set of registers, easy-to-pipeline instructions, and a lean set of operations—while avoiding their omissions or mistakes. It is a free and open, elegant example of the RISC architectures mentioned earlier, which is why more than 60 companies have joined the RISC-V foundation, including AMD, Google, HP Enterprise, IBM, Microsoft, Nvidia, Qualcomm, Samsung, and Western Digital. We use the integer core ISA of RISC-V as the example ISA in this book.

> RISC-V(“RISC Five”)是在加利福尼亚大学伯克利分校开发的现代 RISC 指导，该指令是免费的，可以根据行业的要求进行免费并公开采用。除了完整的软件堆栈(编译器，操作系统和模拟器)外，还有几种 RISC-V 实现可在自定义芯片或现场可编程的门阵列中免费使用。RISC-V 在第一批 RISC 教学集进行了 30 年后，继承了其祖先的好主意 - 一套大量的寄存器，易于高管的说明和一套精益的操作 - 虽然避免了他们的遗漏或错误。这是前面提到的 RISC 架构的免费开放，优雅的例子，这就是为什么有 60 多家公司加入 RISC-V 基金会的原因，包括 AMD，Google，HP Enterprise，IBM，Microsoft，Microsoft，Nvidia，Nvidia，Qualcomm，Qualcomm，Samsung 和 Samsung 和 Samsung 和 Samsung 和西部数据。我们将 RISC-V 的整数核心 ISA 作为本书中的 ISA 示例。

> ===

Figure 1.4 RISC-V registers, names, usage, and calling conventions. In addition to the 32 general-purpose registers (x0–x31), RISC-V has 32 floating-point registers (f0–f31) that can hold either a 32-bit single-precision number or a 64-bit double-precision number. The registers that are preserved across a procedure call are labeled “Callee” saved.

> 图 1.4 RISC-V 登记册，姓名，用法和呼叫惯例。除了 32 个通用寄存器(X0-X31)外，RISC-V 还具有 32 个浮点寄存器(F0-F31)，可以容纳 32 位单位精确编号或 64 位双重精确编号。保存在程序呼叫中的寄存器被标记为“ Callee”保存。

1. _Class of ISA_—Nearly all ISAs today are classified as general-purpose register architectures, where the operands are either registers or memory locations. The 80x86 has 16 general-purpose registers and 16 that can hold floating-point data, while RISC-V has 32 general-purpose and 32 floating-point registers (see [Figure 1.4](#_bookmark10)). The two popular versions of this class are _register-memory_ ISAs, such as the 80x86, which can access memory as part of many instructions, and _load-store_ ISAs, such as ARMv8 and RISC-V, which can access memory only with load or store instructions. All ISAs announced since 1985 are load-store.

> 1. _Class of ISA_- 今天，所有 ISA 都被归类为通用寄存器体系结构，其中操作数为寄存器或内存位置。80x86 具有 16 个通用寄存器和 16 个可以容纳浮点数据的通用寄存器，而 RISC-V 具有 32 个通用物和 32 个浮点寄存器(请参见[图 1.4](#_bookmark10))。该类的两个流行版本是 \_register-memory\_ isas，例如 80x86，可以作为许多指令的一部分访问内存，以及\*load-load-store\* isas，例如 armv8 和 risc-v，它们只能使用 load 或 load 或存储说明。自 1985 年以来宣布的所有 ISA 都是负载商店。

1. _Memory addressing_—Virtually all desktop and server computers, including the 80x86, ARMv8, and RISC-V, use byte addressing to access memory operands. Some architectures, like ARMv8, require that objects must be _aligned_. An access to an object of size _s_ bytes at byte address _A_ is aligned if _A_ mod _s_ 0. (See Figure A.5 on page A-8.) The 80x86 and RISC-V do not require alignment, but accesses are generally faster if operands are aligned.

> 2. _Memory addressing_-将所有台式机和服务器计算机(包括 80x86，ARMV8 和 RISC-V)列出，使用字节地址来访问内存操作数。某些架构(例如 ARMV8)要求对象必须为\_ALIGN*。如果 *a* mod \_s* 0，请访问大小 *s* 字节的对象 *s* bytes _a_。如果操作数是对齐的。

3. _Addressing modes_—In addition to specifying registers and constant operands, addressing modes specify the address of a memory object. RISC-V addressing modes are Register, Immediate (for constants), and Displacement, where a constant offset is added to a register to form the memory address. The 80x86 supports those three modes, plus three variations of displacement: no register (absolute), two registers (based indexed with displacement), and two registers where one register is multiplied by the size of the operand in bytes (based with scaled index and displacement). It has more like the last three modes, minus the displacement field, plus register indirect, indexed, and based with scaled index. ARMv8 has the three RISC-V addressing modes plus PC-relative addressing, the sum of two registers, and the sum of two registers where one register is multiplied by the size of the operand in bytes. It also has autoincrement and autodecrement addressing, where the calculated address replaces the contents of one of the registers used in forming the address.

> 3. _ addressing 模式_-除指定寄存器和常数操作数外，要解决模式指定内存对象的地址。RISC-V 地址模式是寄存器，即时(用于常数)和位移，其中将常数偏移添加到寄存器中以形成内存地址。80x86 支持这三种模式，加上三个位移的变化：无寄存器(绝对)，两个寄存器(基于位移索引)和两个寄存器，其中一个寄存器乘以字节中的操作数的大小(基于 scaled Index 和 scaled Index 和 scale index 和移位)。它更像是最后三个模式，减去位移字段，以及寄存器间接，索引和基于缩放索引。ARMV8 具有三个 RISC-V 寻址模式加上 PC 相关的地址，两个寄存器的总和以及两个寄存器的总和，其中一个寄存器乘以字节中的操作数大小。它还具有自动插入和自动编制地址，其中计算出的地址代替了用于形成地址的寄存器之一的内容。

4. _Types and sizes of operands_—Like most ISAs, 80x86, ARMv8, and RISC-V support operand sizes of 8-bit (ASCII character), 16-bit (Unicode character or half word), 32-bit (integer or word), 64-bit (double word or long integer), and IEEE 754 floating point in 32-bit (single precision) and 64-bit (double precision). The 80x86 also supports 80-bit floating point (extended double precision).

> 4. \_型和大小的操作数 - 例如大多数 ISA，80x86，ARMV8 和 RISC-V 支持操作数 8 位(ASCII 字符)，16 位(Unicode 字符或半字)，32 位(Integer 或 Word)(integer 或 Word))，64 位(双词或长整数)，以及 32 位(单个精度)和 64 位(双精度)的 IEEE 754 浮点。80x86 还支持 80 位浮点(扩展双精度)。

5. _Operations_—The general categories of operations are data transfer, arithmetic logical, control (discussed next), and floating point. RISC-V is a simple and easy-to-pipeline instruction set architecture, and it is representative of the RISC architectures being used in 2017. [Figure 1.5](#_bookmark11) summarizes the integer RISC-V ISA, and [Figure 1.6](#_bookmark12) lists the floating-point ISA. The 80x86 has a much richer and larger set of operations (see Appendix K).

> 5. _ operations_ - 操作的一般类别是数据传输，算术逻辑，控制(下一个讨论)和浮点。RISC-V 是一种简单易于访问的指令集架构，它代表了 2017 年使用的 RISC 架构。[图 1.5](#\_ bookmark11)总结了整数 RISC-V ISA，[图 1.6](%EF%BC%83_Bookmark12) 列出了浮点 ISA。80x86 具有更丰富和更大的操作集(请参阅附录 K)。

6. _Control flow instructions_—Virtually all ISAs, including these three, support conditional branches, unconditional jumps, procedure calls, and returns. All three use PC-relative addressing, where the branch address is specified by an address field that is added to the PC. There are some small differences. RISC-V conditional branches (BE, BNE, etc.) test the contents of registers, and the 80x86 and ARMv8 branches test condition code bits set as side effects of arithmetic/logic operations. The ARMv8 and RISC-V procedure call places the return address in a register, whereas the 80x86 call (CALLF) places the return address on a stack in memory.

> 6. _ Control Flow 指令_-即时所有 ISA，包括这三个，支持条件分支，无条件跳跃，过程调用和返回。这三个都使用 PC 轴承地址，其中分支地址由添加到 PC 的地址字段指定。有一些很小的差异。RISC-V 条件分支(BE，BNE 等)测试寄存器的内容，以及 80x86 和 ARMV8 分支测试条件代码位设置为算术/逻辑操作的副作用。ARMV8 和 RISC-V 程序呼叫将返回地址放在寄存器中，而 80x86 调用(CALLF)将返回地址放在存储器中的堆栈中。

7. _Encoding an ISA_—There are two basic choices on encoding: _fixed length_ and _variable length_. All ARMv8 and RISC-V instructions are 32 bits long, which simplifies instruction decoding. [Figure 1.7](#_bookmark13) shows the RISC-V instruction formats. The 80x86 encoding is variable length, ranging from 1 to 18 bytes. Variable-length instructions can take less space than fixed-length instructions, so a program compiled for the 80x86 is usually smaller than the same program compiled for RISC-V. Note that choices mentioned previously will affect how the instructions are encoded into a binary representation. For example, the number of registers and the number of addressing modes both have a significant impact on the size of instructions, because the register field and addressing mode field can appear many times in a single instruction. (Note that ARMv8 and RISC-V later offered extensions, called Thumb-2 and RV64IC, that provide a mix of 16-bit and 32-bit length instructions, respectively, to reduce program size. Code size for these compact versions of RISC architectures are smaller than that of the 80x86. See Appendix K.)

> 7. *对 isa *进行编码 - 编码有两个基本选择：_ fixed length_ and _variable Length_。所有 ARMV8 和 RISC-V 指令长 32 位，简化了指导解码。[图 1.7](#\_ bookmark13)显示了 RISC-V 指令格式。80x86 编码是可变长度，范围从 1 到 18 个字节。可变长度指令的空间比固定长度说明更少，因此为 80x86 编译的程序通常小于 RISC-V 编译的同一程序。请注意，前面提到的选择将影响指令如何编码为二进制表示。例如，寄存器的数量和地址模式的数量都对指令的大小都有重大影响，因为寄存器字段和地址模式字段可以在单个指令中出现多次。(请注意，ARMV8 和 RISC-V 后来提供的扩展名为 Thumb-2 和 Rv64IC，分别提供了 16 位和 32 位长度说明的混合，以减少程序尺寸。比 80x86 小。请参见附录 K.)

Figure 1.5 Subset of the instructions in RISC-V. RISC-V has a base set of instructions (R64I) and offers optional extensions: multiply-divide (RVM), single-precision floating point (RVF), double-precision floating point (RVD). This figure includes RVM and the next one shows RVF and RVD. [Appendix A](#_bookmark0) gives much more detail on RISC-V.

> 图 1.5 RISC-V 中的指令子集。RISC-V 具有一组指令集(R64i)，并提供可选扩展：多偏见(RVM)，单精度浮点(RVF)，双精度浮点(RVD)。该图包括 RVM，下一个图显示了 RVF 和 RVD。[附录 A](#\_ bookmark0)在 RISC-V 上提供了更多详细信息。

Figure 1.6 Floating point instructions for RISC-V. RISC-V has a base set of instructions (R64I) and offers optional extensions for single-precision floating point (RVF) and double-precision floating point (RVD). SP=single precision; DP=double precision.

> 图 1.6 RISC-V 的浮点指令。RISC-V 具有一组指令集(R64I)，并为单精度浮点(RVF)和双精度浮点(RVD)提供可选扩展。sp =单精度;DP =双精度。

Figure 1.7 The base RISC-V instruction set architecture formats. All instructions are 32 bits long. The R format is for integer register-to-register operations, such as ADD, SUB, and so on. The I format is for loads and immediate operations, such as LD and ADDI. The B format is for branches and the J format is for jumps and link. The S format is for stores. Having a separate format for stores allows the three register specifiers (rd, rs1, rs2) to always be in the same location in all formats. The U format is for the wide immediate instructions (LUI, AUIPC).

> 图 1.7 基本 RISC-V 指令集架构格式。所有说明长 32 位。R 格式适用于整数寄存器到注册操作，例如 Add，sub 等。I 格式用于负载和立即操作，例如 LD 和 ADDI。B 格式用于分支，J 格式用于跳跃和链接。S 格式用于商店。拥有单独的商店格式允许三个寄存器说明器(RD，RS1，RS2)以所有格式始终处于同一位置。U 格式用于广泛的立即说明(LUI，AUIPC)。

The other challenges facing the computer architect beyond ISA design are particularly acute at the present, when the differences among instruction sets are small and when there are distinct application areas. Therefore, starting with the fourth edition of this book, beyond this quick review, the bulk of the instruction set material is found in the appendices (see Appendices A and K).

> 目前，当指令集之间的差异很小并且在有不同的应用领域时，计算机架构师范围之外的其他挑战尤其特别严重。因此，从本书的第四版开始，除了这篇快速的评论之外，大部分指令集材料都可以在附录中找到(请参阅附录 A 和 K)。

### Genuine Computer Architecture: Designing the Organization and Hardware to Meet Goals and Functional Requirements(正版计算机体系结构：设计组织和硬件以满足目标和功能要求)

The implementation of a computer has two components: organization and hardware. The term _organization_ includes the high-level aspects of a computer’s design, such as the memory system, the memory interconnect, and the design of the internal processor or CPU (central processing unit—where arithmetic, logic, branching, and data transfer are implemented). The term _microarchitecture_ is also used instead of organization. For example, two processors with the same instruction set architectures but different organizations are the AMD Opteron and the Intel Core i7. Both processors implement the 80 x 86 instruction set, but they have very different pipeline and cache organizations.

> 计算机的实现具有两个组件：组织和硬件。术语 _organization_ 包括计算机设计的高级方面)。术语 _microarchitecture_ 也使用而不是组织。例如，两个具有相同指令集架构但不同组织的处理器是 AMD Opteron 和 Intel Core i7。两个处理器都实现了 80 x 86 指令集，但是它们的管道和缓存组织却非常不同。

The switch to multiple processors per microprocessor led to the term _core_ also being used for processors. Instead of saying multiprocessor microprocessor, the term _multicore_ caught on. Given that virtually all chips have multiple processors, the term central processing unit, or CPU, is fading in popularity.

> 每个微处理器的切换到多个处理器导致术语 _core_ 也用于处理器。术语 _Multicore_ 不用说多处理器微处理器。鉴于几乎所有芯片都有多个处理器，因此“中央处理单元”(CPU)一词在受欢迎程度上逐渐消失。

_Hardware_ refers to the specifics of a computer, including the detailed logic design and the packaging technology of the computer. Often a line of computers contains computers with identical instruction set architectures and very similar organizations, but they differ in the detailed hardware implementation. For example, the Intel Core i7 (see [Chapter 3](#_bookmark93)) and the Intel Xeon E7 (see [Chapter 5](#_bookmark213)) are nearly identical but offer different clock rates and different memory systems, making the Xeon E7 more effective for server computers.

> _hardware_ 是指计算机的细节，包括详细的逻辑设计和计算机的包装技术。通常，一系列计算机包含具有相同指令集体系结构和非常相似组织的计算机，但是它们在详细的硬件实现方面有所不同。例如，Intel Core i7(请参阅[第 3 章](#_bookmark93))和 Intel Xeon E7(请参阅[第 5 章](#_bookmark213))几乎相同，但提供了不同的时钟速率和不同的内存系统，使 Xeon 成为 XeonE7 对服务器计算机更有效。

In this book, the word _architecture_ covers all three aspects of computer design—instruction set architecture, organization or microarchitecture, and hardware.

> 在本书中，_architecture_ 词涵盖了计算机设计的所有三个方面：Instruction Set 架构，组织或微观结构以及硬件。

Computer architects must design a computer to meet functional requirements as well as price, power, performance, and availability goals. [Figure 1.8](#_bookmark14) summarizes requirements to consider in designing a new computer. Often, architects also must determine what the functional requirements are, which can be a major task. The requirements may be specific features inspired by the market. Application software typically drives the choice of certain functional requirements by determining how the computer will be used. If a large body of software exists for a particular instruction set architecture, the architect may decide that a new computer should implement an existing instruction set. The presence of a large market for a particular class of applications might encourage the designers to incorporate requirements that would make the computer competitive in that market. Later chapters examine many of these requirements and features in depth.

> 计算机架构师必须设计计算机以满足功能需求以及价格，电源，性能和可用性目标。[图 1.8](#_bookmark14) 总结了设计新计算机时要考虑的要求。通常，工程师还必须确定功能要求是什么，这可能是一项主要任务。要求可能是受市场启发的特定功能。应用软件通常通过确定如何使用计算机来推动某些功能要求的选择。如果存在针对特定指令集体系结构的大量软件，则工程师可以决定新计算机应实现现有指令集。对于特定类别的应用程序，大型市场的存在可能会鼓励设计师合并使计算机在该市场中竞争的要求。后来的章节研究了许多此类要求和特征。

Figure 1.8 Summary of some of the most important functional requirements an architect faces. The left-hand column describes the class of requirement, while the right-hand column gives specific examples. The right-hand column also contains references to chapters and appendices that deal with the specific issues.

> 图 1.8 工程师面部的一些最重要的功能要求摘要。左侧列描述了需求类，而右列给出了特定的示例。右栏还包含对处理特定问题的章节和附录的引用。

Architects must also be aware of important trends in both the technology and the use of computers because such trends affect not only the future cost but also the longevity of an architecture.

> 工程师还必须意识到计算机技术和使用的重要趋势，因为这种趋势不仅会影响未来的成本，而且会影响建筑的寿命。
