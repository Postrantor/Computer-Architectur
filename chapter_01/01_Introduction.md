---
data: 2023-01-15
---

> [!note]
> 这一章中有介绍到摩尔定律的失效，以及接下来的发展趋势 - 特定领域的芯片，专注于某一领域的计算任务，相比于通用芯片效率更高，这个是接下来的发展趋势。

## Introduction

Computer technology has made incredible progress in the roughly 70 years since the first general-purpose electronic computer was created. Today, less than `$500` will purchase a cell phone that has as much performance as the world’s fastest computer bought in 1993 for `$50 million`. This rapid improvement has come both from advances in the technology used to build computers and from innovations in computer design.

> 自第一台通用电子计算机问世以来的大约 70 年里，计算机技术取得了令人难以置信的进步。今天，不到“500 美元”就可以买到一部性能与 1993 年以“5000 万美元”购买的世界上最快的电脑同等性能的手机。这种快速改进既来自用于构建计算机的技术的进步，也来自计算机设计的创新。

Although technological improvements historically have been fairly steady, progress arising from better computer architectures has been much less consistent. During the first 25 years of electronic computers, both forces made a major con- tribution, delivering performance improvement of about 25% per year. The late 1970s saw the emergence of the microprocessor. The ability of the microprocessor to ride the improvements in integrated circuit technology led to a higher rate of performance improvement—roughly 35% growth per year.

> 尽管从历史上看，技术进步相当稳定，但更好的计算机体系结构带来的进步却不太一致。在电子计算机出现的头 25 年里，这两股力量都做出了重大贡献，每年带来约 25% 的性能提升。20 世纪 70 年代后期见证了微处理器的出现。微处理器驾驭集成电路技术改进的能力导致了更高的性能改进率——每年大约增长 35%。

This growth rate, combined with the cost advantages of a mass-produced microprocessor, led to an increasing fraction of the computer business being based on microprocessors. In addition, two significant changes in the computer market- place made it easier than ever before to succeed commercially with a new archi- tecture. First, the virtual elimination of assembly language programming reduced the need for object-code compatibility. Second, the creation of standardized, vendor-independent operating systems, such as UNIX and its clone, Linux, low- ered the cost and risk of bringing out a new architecture.

> 这种增长率与大规模生产的微处理器的成本优势相结合，导致越来越多的计算机业务基于微处理器。此外，计算机市场的两个显着变化使得使用新架构在商业上取得成功比以往任何时候都容易。首先，汇编语言编程的虚拟消除减少了对目标代码兼容性的需求。其次，标准化、独立于供应商的操作系统(如 UNIX 及其克隆版 Linux)的创建降低了引入新体系结构的成本和风险。

These changes made it possible to develop successfully a new set of architectures with simpler instructions, called RISC (Reduced Instruction Set Computer) architectures, in the early 1980s. The RISC-based machines focused the attention of designers on two critical performance techniques, the exploitation of _instruc- tion-level parallelism_ (initially through pipelining and later through multiple instruction issue) and the use of caches (initially in simple forms and later using more sophisticated organizations and optimizations).

> 这些变化使得在 1980 年代早期成功开发一套指令更简单的新体系结构成为可能，称为 RISC(精简指令集计算机)体系结构。基于 RISC 的机器将设计者的注意力集中在两种关键的性能技术上，即“指令级并行”的开发(最初通过流水线，后来通过多指令发布)和高速缓存的使用(最初以简单形式，后来使用更多 复杂的组织和优化)。

The RISC-based computers raised the performance bar, forcing prior architec- tures to keep up or disappear. The Digital Equipment Vax could not, and so it was replaced by a RISC architecture. Intel rose to the challenge, primarily by translat- ing 80x86 instructions into RISC-like instructions internally, allowing it to adopt many of the innovations first pioneered in the RISC designs. As transistor counts soared in the late 1990s, the hardware overhead of translating the more complex x 86 architecture became negligible. In low-end applications, such as cell phones, the cost in power and silicon area of the x 86-translation overhead helped lead to a RISC architecture, ARM, becoming dominant.

> 基于 RISC 的计算机提高了性能标准，迫使先前的体系结构跟上或消失。Digital Equipment Vax 做不到，所以它被 RISC 架构所取代。英特尔迎难而上，主要是在内部将 80x86 指令翻译成类似 RISC 的指令，使其能够采用许多在 RISC 设计中率先开创的创新。随着晶体管数量在 20 世纪 90 年代后期飙升，转换更复杂的 x 86 架构的硬件开销变得可以忽略不计。在手机等低端应用中，x 86 转换开销的功耗和硅面积成本导致 RISC 架构 ARM 成为主导。

[Figure 1.1](#_bookmark5) shows that the combination of architectural and organizational enhancements led to 17 years of sustained growth in performance at an annual rate of over 50%—a rate that is unprecedented in the computer industry.

> [图 1.1](#_bookmark5) 表明，架构和组织改进的结合导致性能持续增长 17 年，年增长率超过 50%——这在计算机行业中是前所未有的。

The effect of this dramatic growth rate during the 20th century was fourfold. First, it has significantly enhanced the capability available to computer users. For many applications, the highest-performance microprocessors outperformed the supercomputer of less than 20 years earlier.

> 20 世纪这种急剧的增长率产生了四方面的影响。首先，它显着增强了计算机用户可用的能力。对于许多应用，最高性能的微处理器优于不到 20 年前的超级计算机。

Figure 1.1 Growth in processor performance over 40 years. This chart plots program performance relative to the VAX 11/780 as measured by the SPEC integer benchmarks (see [Section 1.8](#measuring-reporting-and-summarizing-performance)). Prior to the mid-1980s, growth in processor performance was largely technology-driven and averaged about 22% per year, or doubling performance every 3.5 years. The increase in growth to about 52% starting in 1986, or doubling every 2 years, is attributable to more advanced architectural and organizational ideas typified in RISC architectures. By 2003 this growth led to a dif- ference in performance of an approximate factor of 25 versus the performance that would have occurred if it had continued at the 22% rate. In 2003 the limits of power due to the end of Dennard scaling and the available instruction-level parallelism slowed uniprocessor performance to 23% per year until 2011, or doubling every 3.5 years. (The fastest SPECintbase performance since 2007 has had automatic parallelization turned on, so uniprocessor speed is harder to gauge. These results are limited to single-chip systems with usually four cores per chip.) From 2011 to 2015, the annual improvement was less than 12%, or doubling every 8 years in part due to the limits of parallelism of Amdahl’s Law. Since 2015, with the end of Moore’s Law, improvement has been just 3.5% per year, or doubling every 20 years! Performance for floating-point-oriented calculations follows the same trends, but typically has 1% to 2% higher annual growth in each shaded region. [Figure 1.11](#_bookmark19) on page 27 shows the improvement in clock rates for these same eras. Because SPEC has changed over the years, performance of newer machines is estimated by a scaling factor that relates the performance for different versions of SPEC: SPEC89, SPEC92, SPEC95, SPEC2000, and SPEC2006. There are too few results for SPEC2017 to plot yet.

> 图 1.1 40 年来处理器性能的增长。此图表绘制了程序相对于 VAX 11/780 的性能，这是通过 SPEC 整数基准测量的(参见 [第 1.8 节](#measuring-reporting-and-summarizing-performance))。在 20 世纪 80 年代中期之前，处理器性能的增长主要由技术驱动，平均每年增长 22%，即每 3.5 年性能翻一番。从 1986 年开始，增长率增加到约 52%，或每 2 年翻一番，这归功于以 RISC 架构为代表的更先进的架构和组织理念。到 2003 年，这种增长导致业绩相差大约 25 倍，而如果它继续以 22% 的速度增长则会出现业绩。2003 年，由于 Dennard 缩放的终结和可用的指令级并行性导致的功率限制将单处理器性能降低到每年 23%，直到 2011 年，或每 3.5 年翻一番。(自 2007 年以来最快的 SPECintbase 性能已启用自动并行化，因此单处 ​​ 理器速度更难衡量。这些结果仅限于单芯片系统，通常每个芯片有四个内核。)从 2011 年到 2015 年，每年的改进不到 12%，或每 8 年翻一番，部分原因是阿姆达尔定律的平行性限制。自 2015 年以来，随着摩尔定律的终结，每年仅增长 3.5%，即每 20 年翻一番！ 面向浮点计算的性能遵循相同的趋势，但每个阴影区域的年增长率通常高出 1% 到 2%。[图 1.11](#_bookmark19) 第 27 页显示了这些相同时代时钟速率的改进。由于多年来 SPEC 发生了变化，新机器的性能是通过与不同版本 SPEC 的性能相关的比例因子来估算的：SPEC89、SPEC92、SPEC95、SPEC2000 和 SPEC2006。SPEC2017 的结果太少，无法绘制。

Second, this dramatic improvement in cost-performance led to new classes of computers. Personal computers and workstations emerged in the 1980s with the availability of the microprocessor. The past decade saw the rise of smart cell phones and tablet computers, which many people are using as their primary com- puting platforms instead of PCs. These mobile client devices are increasingly using the Internet to access warehouses containing 100,000 servers, which are being designed as if they were a single gigantic computer.

> 其次，这种性价比的显着提高导致了新型计算机的出现。随着微处理器的出现，个人计算机和工作站出现在 1980 年代。过去十年见证了智能手机和平板电脑的兴起，许多人将其用作主要计算平台而不是 PC。这些移动客户端设备越来越多地使用互联网访问包含 100,000 台服务器的仓库，这些服务器被设计成一台巨大的计算机。

Third, improvement of semiconductor manufacturing as predicted by Moore’s law has led to the dominance of microprocessor-based computers across the entire range of computer design. Minicomputers, which were traditionally made from off-the-shelf logic or from gate arrays, were replaced by servers made by using microprocessors. Even mainframe computers and high-performance supercom- puters are all collections of microprocessors.

> 第三，正如摩尔定律所预测的那样，半导体制造的改进导致基于微处理器的计算机在整个计算机设计领域占据主导地位。传统上由现成的逻辑或门阵列制成的小型计算机被使用微处理器制成的服务器所取代。甚至大型计算机和高性能超级计算机都是微处理器的集合。

The preceding hardware innovations led to a renaissance in computer design, which emphasized both architectural innovation and efficient use of technology improvements. This rate of growth compounded so that by 2003, high- performance microprocessors were 7.5 times as fast as what would have been obtained by relying solely on technology, including improved circuit design, that is, 52% per year versus 35% per year.

> 前面的硬件创新导致了计算机设计的复兴，它强调架构创新和技术改进的有效利用。这种复合式增长使得到 2003 年，高性能微处理器的速度是仅依靠技术(包括改进的电路设计)所能达到的速度的 7.5 倍，即每年 52% 对 35%。

This hardware renaissance led to the fourth impact, which was on software development. This 50,000-fold performance improvement since 1978 (see [Figure 1.1](#_bookmark5)) allowed modern programmers to trade performance for productivity. In place of performance-oriented languages like C and C++, much more program- ming today is done in managed programming languages like Java and Scala. More- over, scripting languages like JavaScript and Python, which are even more productive, are gaining in popularity along with programming frameworks like AngularJS and Django. To maintain productivity and try to close the performance gap, interpreters with just-in-time compilers and trace-based compiling are repla- cing the traditional compiler and linker of the past. Software deployment is chang- ing as well, with Software as a Service (SaaS) used over the Internet replacing shrink-wrapped software that must be installed and run on a local computer.

> 硬件复兴带来了第四个影响，即对软件开发的影响。自 1978 年以来性能提高了 50,000 倍(参见 [图 1.1](#_bookmark5))，现代程序员可以用性能换取生产力。与 C 和 C++ 等面向性能的语言不同，如今更多的编程是使用 Java 和 Scala 等托管编程语言完成的。此外，JavaScript 和 Python 等效率更高的脚本语言与 AngularJS 和 Django 等编程框架一起越来越受欢迎。为了保持生产力并试图缩小性能差距，带有即时编译器和基于跟踪的编译器的解释器正在取代过去的传统编译器和链接器。软件部署也在发生变化，通过 Internet 使用的软件即服务 (SaaS) 取代了必须在本地计算机上安装和运行的压缩包装软件。

The nature of applications is also changing. Speech, sound, images, and video are becoming increasingly important, along with predictable response time that is so critical to the user experience. An inspiring example is Google Translate. This application lets you hold up your cell phone to point its camera at an object, and the image is sent wirelessly over the Internet to a warehouse-scale computer (WSC) that recognizes the text in the photo and translates it into your native language. You can also speak into it, and it will translate what you said into audio output in another language. It translates text in 90 languages and voice in 15 languages. Alas, [Figure 1.1](#_bookmark5) also shows that this 17-year hardware renaissance is over. The fundamental reason is that two characteristics of semiconductor processes that were true for decades no longer hold.

> 应用程序的性质也在发生变化。语音、声音、图像和视频以及对用户体验至关重要的可预测响应时间变得越来越重要。一个鼓舞人心的例子是谷歌翻译。这个应用程序可以让你举起你的手机，将它的相机对准一个物体，图像通过互联网无线发送到仓库规模的计算机 (WSC)，它可以识别照片中的文字并将其翻译成你的母语。你也可以对着它说话，它会把你说的话翻译成另一种语言的音频输出。它可以翻译 90 种语言的文本和 15 种语言的语音。唉，[图 1.1](#_bookmark5) 也表明这 17 年的硬件复兴已经结束。根本原因是几十年来一直存在的半导体工艺的两个特征不再成立。

In 1974 Robert Dennard observed that power density was constant for a given area of silicon even as you increased the number of transistors because of smaller dimensions of each transistor. Remarkably, transistors could go faster but use less power. _Dennard scaling_ ended around 2004 because current and voltage couldn’t keep dropping and still maintain the dependability of integrated circuits.

> 1974 年，罗伯特·丹纳德 (Robert Dennard) 观察到，对于给定的硅面积，功率密度是恒定的，即使由于每个晶体管的尺寸较小而增加晶体管的数量也是如此。值得注意的是，晶体管可以运行得更快但功耗更低。*Dennard 缩放*在 2004 年左右结束，因为电流和电压无法继续下降并仍然保持集成电路的可靠性。

This change forced the microprocessor industry to use multiple efficient pro- cessors or cores instead of a single inefficient processor. Indeed, in 2004 Intel can- celed its high-performance uniprocessor projects and joined others in declaring that the road to higher performance would be via multiple processors per chip rather than via faster uniprocessors. This milestone signaled a historic switch from relying solely on instruction-level parallelism (ILP), the primary focus of the first three editions of this book, to _data-level parallelism_ (DLP) and _thread-level par- allelism_ (TLP), which were featured in the fourth edition and expanded in the fifth edition. The fifth edition also added WSCs and _request-level parallelism_ (RLP), which is expanded in this edition. Whereas the compiler and hardware conspire to exploit ILP implicitly without the programmer’s attention, DLP, TLP, and RLP are explicitly parallel, requiring the restructuring of the application so that it can exploit explicit parallelism. In some instances, this is easy; in many, it is a major new burden for programmers.

> 这种变化迫使微处理器行业使用多个高效处理器或内核，而不是单个低效处理器。事实上，2004 年英特尔取消了它的高性能单处理器项目，并与其他人一起宣布通向更高性能的道路是通过每个芯片的多个处理器，而不是通过更快的单处理器。这一里程碑标志着一个历史性的转变，即从仅依赖指令级并行性 (ILP)(本书前三个版本的主要重点)到*数据级并行性*(DLP)和*线程级并行性*(TLP)，后者 在第四版中进行了介绍，并在第五版中进行了扩充。第五版还添加了 WSC 和*请求级并行性*(RLP)，这在本版中得到了扩展。尽管编译器和硬件密谋在程序员不注意的情况下隐式利用 ILP，但 DLP、TLP 和 RLP 是显式并行的，需要重构应用程序以便它可以利用显式并行性。在某些情况下，这很容易； 在许多情况下，它是程序员的主要新负担。

_Amdahl_’_s Law_ ([Section 1.9](#quantitative-principles-of-computer-design)) prescribes practical limits to the number of useful cores per chip. If 10% of the task is serial, then the maximum performance benefit from parallelism is 10 no matter how many cores you put on the chip.

> _Amdahl_'_定律_([第 1.9 节](#quantitative-principles-of-computer-design))规定了每个芯片的有用内核数量的实际限制。如果 10% 的任务是串行的，那么无论您在芯片上放置多少个内核，并行性带来的最大性能收益都是 10。

The second observation that ended recently is _Moore_’_s Law._ In 1965 Gordon Moore famously predicted that the number of transistors per chip would double every year, which was amended in 1975 to every two years. That prediction lasted for about 50 years, but no longer holds. For example, in the 2010 edition of this book, the most recent Intel microprocessor had 1,170,000,000 transistors. If Moore’s Law had continued, we could have expected microprocessors in 2016 to have 18,720,000,000 transistors. Instead, the equivalent Intel microprocessor has just 1,750,000,000 transistors, or off by a factor of 10 from what Moore’s Law would have predicted.

> 最近结束的第二个观察是*摩尔定律。* 1965 年，戈登·摩尔 (Gordon Moore) 著名地预测，每个芯片的晶体管数量将每年翻一番，该预测在 1975 年被修正为每两年一次。该预测持续了大约 50 年，但不再成立。例如，在本书的 2010 年版中，最新的英特尔微处理器有 1,170,000,000 个晶体管。如果摩尔定律继续存在，我们可以预计 2016 年的微处理器将拥有 18,720,000,000 个晶体管。相反，等效的英特尔微处理器只有 1,750,000,000 个晶体管，或者与摩尔定律的预测相差 10 倍。

The combination of

- transistors no longer getting much better because of the slowing of Moore’s Law and the end of Dinnard scaling,
- the unchanging power budgets for microprocessors,
- the replacement of the single power-hungry processor with several energyefficient processors, and
- the limits to multiprocessing to achieve Amdahl’s Law caused improvements in processor performance to slow down, that is, to _double every 20 years,_ rather than every 1.5 years as it did between 1986 and 2003 (see [Figure 1.1](#_bookmark5)).

The only path left to improve energy-performance-cost is specialization. Future microprocessors will include several domain-specific cores that perform only one class of computations well, but they do so remarkably better than general-purpose cores. The new [Chapter 7](#_bookmark322) in this edition introduces _domain-specific architectures_.

> 提高能源性能成本的唯一途径是专业化。未来的微处理器将包括几个特定领域的核心，它们只能很好地执行一类计算，但它们比通用核心要好得多。此版本中新的 [第 7 章](#_bookmark322) 介绍了*领域特定的架构*。

This text is about the architectural ideas and accompanying compiler improve- ments that made the incredible growth rate possible over the past century, the rea- sons for the dramatic change, and the challenges and initial promising approaches to architectural ideas, compilers, and interpreters for the 21st century. At the core is a quantitative approach to computer design and analysis that uses empirical obser- vations of programs, experimentation, and simulation as its tools. It is this style and approach to computer design that is reflected in this text. The purpose of this chap- ter is to lay the quantitative foundation on which the following chapters and appen- dices are based.

> 本书是关于架构思想和伴随的编译器改进，这些改进使上个世纪令人难以置信的增长率成为可能，发生巨大变化的原因，以及架构思想、编译器和解释器的挑战和最初有希望的方法 21 世纪。其核心是一种计算机设计和分析的定量方法，它使用对程序、实验和模拟的经验观察作为其工具。本书反映的正是这种计算机设计风格和方法。本章的目的是为后续章节和附录奠定定量基础。

This book was written not only to explain this design style but also to stimulate you to contribute to this progress. We believe this approach will serve the computers of the future just as it worked for the implicitly parallel computers of the past.

> 本书的编写不仅是为了解释这种设计风格，也是为了激励你为这一进步做出贡献。我们相信这种方法将服务于未来的计算机，就像它适用于过去的隐式并行计算机一样。
