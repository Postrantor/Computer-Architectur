# H Hardware and Software for VLIW and EPIC

> ＃H 的 vliw 和 Epic 的硬件和软件

1. Introduction: Exploiting Instruction-Level Parallelism Statically

> 1.简介：在静态上利用指令级并行性

[H-2](#introduction-exploiting-instruction-level-parallelism-statically)

> [h-2]（＃resoutral-exploiting-Instruction-level-parallelisis statatataterism）

2. Detecting and Enhancing Loop-Level Parallelism

> 2.检测和增强循环级别的并行性

[H-2](#detecting-and-enhancing-loop-level-parallelism-1)

> [H-2]（＃检测和增强的环级 - 平行 - 1）

3. Scheduling and Structuring Code for Parallelism

> 3.并行的调度和结构代码

[H-12](#scheduling-and-structuring-code-for-parallelism)

> [H-12]（＃调度和结构代码 - 对并行性）

4. Hardware Support for Exposing Parallelism: Predicated Instructions

> 4.用于暴露并行性的硬件支持：预测指令

[H-23](#hardware-support-for-exposing-parallelism-predicated-instructions)

> [H-23]（＃硬件 - 支持 - 公平 - 平行式预测 - 实体）

5. Hardware Support for Compiler Speculation

> 5.编译器投机的硬件支持

[H-27](#hardware-support-for-compiler-speculation)

> [H-27]（＃硬件支持 - 补偿器规范）

6. The Intel IA-64 Architecture and Itanium Processor

> 6. Intel IA-64 体系结构和 Itanium 处理器

[H-32](#the-intel-ia-64-architecture-and-itanium-processor)

> [H-32]（＃The-Intel-ia-64-Architecture and-Itanium Processor）

7. Concluding Remarks [H-43](#concluding-remarks-13)

> 7.总结备注[H-43]（＃结论 remarks-13）

The EPIC approach is based on the application of massive resources. These resources include more load-store, computational, and branch units, as well as larger, lower-latency caches than would be required for a superscalar processor. Thus, IA-64 gambles that, in the future, power will not be the critical limitation, and that massive resources, along with the machinery to exploit them, will not penalize performance with their adverse effect on clock speed, path length, or CPI factors.

> 史诗般的方法基于大量资源的应用。这些资源包括更多的负载店，计算和分支单元，以及比超级量表处理器所需的更大的延迟缓存。因此，IA-64 赌博，将来，权力将不会成为关键限制，并且大量资源以及利用它们的机械，不会以其对时钟速度，路径长度或 CPI 的不利影响而惩罚性能因素。

M. Hopkins

> 霍普金斯

_in a commentary on the EPIC approach and the IA-64 architecture (2000)_

> *在有关史诗方法和 IA-64 体系结构（2000）*的评论

## Introduction: Exploiting Instruction-Level Parallelism Statically

> ##简介：在静态上利用指令级并行性

In this chapter, we discuss compiler technology for increasing the amount of par- allelism that we can exploit in a program as well as hardware support for these compiler techniques. The next section defines when a loop is parallel, how a depen- dence can prevent a loop from being parallel, and techniques for eliminating some types of dependences. The following section discusses the topic of scheduling code to improve parallelism. These two sections serve as an introduction to these techniques.

> 在本章中，我们将讨论编译器技术，以增加我们可以在程序中利用的阶段性的数量以及对这些编译器技术的硬件支持。下一节定义循环何时平行，依赖性如何防止循环平行，以及消除某些类型的依赖性的技术。以下部分讨论了调度代码的主题以改善并行性。这两个部分是对这些技术的介绍。

We do not attempt to explain the details of ILP-oriented compiler techniques, since that would take hundreds of pages, rather than the 20 we have allotted. Instead, we view this material as providing general background that will enable the reader to have a basic understanding of the compiler techniques used to exploit ILP in modern computers.

> 我们不会尝试解释面向 ILP 的编译器技术的细节，因为这将需要数百页，而不是我们分配的 20 页。取而代之的是，我们将此材料视为提供一般背景，使读者能够对用于利用现代计算机中 ILP 的编译器技术有基本的了解。

Hardware support for these compiler techniques can greatly increase their effectiveness, and [Sections H.4](#hardware-support-for-exposing-parallelism-predicated-instructions) and [H.5](#hardware-support-for-compiler-speculation) explore such support. The IA-64 repre- sents the culmination of the compiler and hardware ideas for exploiting parallelism statically and includes support for many of the concepts proposed by researchers during more than a decade of research into the area of compiler- based instruction-level parallelism. [Section H.6](#the-intel-ia-64-architecture-and-itanium-processor) provides a description and perfor- mance analyses of the Intel IA-64 architecture and its second-generation imple- mentation, Itanium 2.

> 这些编译器技术的硬件支持可以极大地提高其效率，[＃H.4]（＃硬件 - 支持 - 公平 - 平行式 - 预测 - 实施结构）和[h.5] - 调查）探索这种支持。IA-64 代表了编译器和硬件想法的高潮，以统计地利用并行性利用并行性，并包括对研究人员在编译器基于编译器的指导级并行性领域的研究中提出的许多概念的支持。[H.6 节]（＃The-Intel-ia-64-Architecture and-Itanium Processor）提供了对 Intel IA-64 体系结构及其第二代 Itanium 2 的描述和演出分析。

The core concepts that we exploit in statically based techniques—finding par- allelism, reducing control and data dependences, and using speculation—are the same techniques we saw exploited in [Chapter 3](#_bookmark93) using dynamic techniques. The key difference is that the techniques in this appendix are applied at compile time by the compiler, rather than at runtime by the hardware. The advantages of compile time techniques are primarily two: They do not burden runtime execution with any inefficiency, and they can take into account a wider range of the program than a runtime approach might be able to incorporate. As an example of the latter, the next section shows how a compiler might determine that an entire loop can be executed in parallel; hardware techniques might or might not be able to find such parallel- ism. The major disadvantage of static approaches is that they can use only compile time information. Without runtime information, compile time techniques must often be conservative and assume the worst case.

> 我们在基于静态的技术中利用的核心概念（探索阶段性，减少控制和数据依赖以及使用推测）是我们使用动态技术中[第 3 章]（#\_ bookmark93）中看到的相同技术的相同技术。关键区别在于，本附录中的技术是由编译器在编译时应用的，而不是在运行时使用硬件。编译时间技术的优点主要是两个：它们不会以任何效率低下的运行时执行，并且可以考虑到程序的更广泛范围，而不是运行时方法可能会合并。作为后者的一个示例，下一部分显示了编译器如何并行执行整个循环。硬件技术可能或可能无法找到这种并行的 ISM。静态方法的主要缺点是他们只能使用编译时间信息。没有运行时信息，编译时间技术通常必须是保守的，并且假定最坏的情况。
