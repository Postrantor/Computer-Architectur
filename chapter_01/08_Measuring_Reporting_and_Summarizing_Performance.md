## Measuring, Reporting, and Summarizing Performance

> ##测量，报告和总结性能

When we say one computer is faster than another one is, what do we mean? The user of a cell phone may say a computer is faster when a program runs in less time, while an Amazon.com administrator may say a computer is faster when it com- pletes more transactions per hour. The cell phone user wants to reduce _response time_—the time between the start and the completion of an event—also referred to as _execution time_. The operator of a WSC wants to increase _throughput_—the total amount of work done in a given time.

> 当我们说一台计算机比另一台计算机更快时，我们的意思是什么？手机的用户可能会说，当程序在更少的时间内运行时，计算机的速度更快，而 Amazon.com 管理员可能会说计算机每小时进行更多交易时，计算机的速度更快。手机用户希望减少 *response Time*（事件的开始和完成之间的时间）也称为 *EXECTICE TIME*。WSC 的操作员希望增加 *throughput* - 在给定时间内完成的工作总量。

In comparing design alternatives, we often want to relate the performance of two different computers, say, X and Y. The phrase “X is faster than Y” is used here to mean that the response time or execution time is lower on X than on Y for the given task. In particular, “X is _n_ times as fast as Y” will mean

> 在比较设计替代方案时，我们经常想关联两台不同计算机的性能。y 对于给定的任务。特别是，“ x 是 *n* times tim and y tip y”将意味着

> ===

Since execution time is the reciprocal of performance, the following relationship holds:

> 由于执行时间是绩效的倒数，因此以下关系存在：

> ===

The phrase “the throughput of X is 1.3 times as fast as Y” signifies here that the number of tasks completed per unit time on computer X is 1.3 times the number completed on Y.

> “ X 的吞吐量是 y 的 1.3 倍”一词，表示在这里，计算机 X 上每单位时间完成的任务数量是 Y 上完成的数字的 1.3 倍。

Unfortunately, time is not always the metric quoted in comparing the perfor- mance of computers. Our position is that the only consistent and reliable measure of performance is the execution time of real programs, and that all proposed alter- natives to time as the metric or to real programs as the items measured have even- tually led to misleading claims or even mistakes in computer design.

> 不幸的是，在比较计算机的演奏时，时间并不总是引用的。我们的立场是，唯一一致且可靠的绩效衡量标准是真实程序的执行时间，所有拟议的替代方案都是按照指标或真实程序的替代时间，因为所衡量的项目甚至导致了误导性的索赔，甚至导致了误解计算机设计中的错误。

Even execution time can be defined in different ways depending on what we count. The most straightforward definition of time is called _wall-clock time_, _response time_, or _elapsed time_, which is the latency to complete a task, including storage accesses, memory accesses, input/output activities, operating system over- head—everything. With multiprogramming, the processor works on another pro- gram while waiting for I/O and may not necessarily minimize the elapsed time of one program. Thus we need a term to consider this activity. _CPU time_ recognizes this distinction and means the time the processor is computing, _not_ including the time waiting for I/O or running other programs. (Clearly, the response time seen by the user is the elapsed time of the program, not the CPU time.)

> 甚至可以根据我们计算的内容来以不同的方式定义执行时间。时间最直接的定义称为* WALL-CLOCK TIME*，*RESPONSE TIME* 或 *ELAPSED TIME*，这是完成任务的延迟，包括存储访问，内存访问，输入/输出活动，操作系统，操作系统，越过 heast-Heast-everything。通过多编程，处理器在等待 I/O 时在另一个程序上起作用，并且不一定会最大程度地减少一个程序的经过的时间。因此，我们需要一个术语来考虑这项活动。*cpu Time* 识别此区别，表示处理器正在计算的时间，_not_，包括等待 I/O 或运行其他程序的时间。（显然，用户看到的响应时间是程序的经过的时间，而不是 CPU 时间。）

Computer users who routinely run the same programs would be the perfect can- didates to evaluate a new computer. To evaluate a new system, these users would simply compare the execution time of their _workloads_—the mixture of programs and operating system commands that users run on a computer. Few are in this happy situation, however. Most must rely on other methods to evaluate computers, and often other evaluators, hoping that these methods will predict performance for their usage of the new computer. One approach is benchmark programs, which are programs that many companies use to establish the relative performance of their computers.

> 经常运行相同程序的计算机用户将是评估新计算机的理想选择。为了评估新系统，这些用户将简单地比较其 *workloads* 的执行时间，即用户在计算机上运行的程序和操作系统命令的混合物。但是，很少有人在这种快乐的情况下。大多数人必须依靠其他方法来评估计算机以及其他评估人员，希望这些方法能够预测其使用新计算机的性能。一种方法是基准程序，该程序是许多公司用来建立计算机相对性能的程序。

### Benchmarks

> ###基准

The best choice of benchmarks to measure performance is real applications, such as Google Translate mentioned in [Section 1.1](#introduction). Attempts at running programs that are much simpler than a real application have led to performance pitfalls. Examples include

> 衡量性能的最佳选择是真实的应用程序，例如[1.1 节]（＃简介）中提到的 Google 翻译。尝试运行比实际应用程序要简单得多的程序导致了性能陷阱。示例包括

- _Kernels_, which are small, key pieces of real applications.

> - _KERNELS_，是实际应用的小关键部分。

- _Toy programs_, which are 100-line programs from beginning programming assignments, such as Quicksort.

> - _toy 程序_，是从启动编程作业（例如 QuickSort）的 100 行程序。

- _Synthetic benchmarks_, which are fake programs invented to try to match the profile and behavior of real applications, such as Dhrystone.

> - _ synthetic Benchmarks_，它是发明的假程序，以尝试匹配真实应用程序的轮廓和行为，例如 Dhrystone。

All three are discredited today, usually because the compiler writer and architect can conspire to make the computer appear faster on these stand-in programs than on real applications. Regrettably for your authors—who dropped the fallacy about using synthetic benchmarks to characterize performance in the fourth edition of this book since we thought all computer architects agreed it was disreputable— the synthetic program Dhrystone is still the most widely quoted benchmark for embedded processors in 2017!

> 今天，这三个都被抹黑了，通常是因为编译器作家和建筑师可以合谋使计算机在这些备用程序上的显示速度比实际应用程序更快。遗憾的是，对于您的作者来说，他放弃了使用合成基准来表征本书第四版的性能的谬论，因为我们认为所有计算机建筑师都同意它是不可偿还的 - 合成程序 Dhrystone 仍然是 2017 年嵌入式处理器的最广泛报道呢

Another issue is the conditions under which the benchmarks are run. One way to improve the performance of a benchmark has been with benchmark-specific compiler flags; these flags often caused transformations that would be illegal on many programs or would slow down performance on others. To restrict this pro- cess and increase the significance of the results, benchmark developers typically require the vendor to use one compiler and one set of flags for all the programs in the same language (such as C++ or C). In addition to the question of compiler flags, another question is whether source code modifications are allowed. There are three different approaches to addressing this question:

> 另一个问题是运行基准的条件。提高基准性能的一种方法是使用基准特定的编译器标志。这些标志通常会导致在许多程序上是非法的转换，或者会减慢其他程序的性能。为了限制此过程并提高结果的重要性，基准开发人员通常要求供应商使用一个编译器和一组标志，以使用相同语言的所有程序（例如 C ++ 或 C）。除了编译器标志的问题外，另一个问题是是否允许源代码修改。解决这个问题有三种不同的方法：

1. No source code modifications are allowed.

> 1.不允许修改源代码。

2. Source code modifications are allowed but are essentially impossible. For example, database benchmarks rely on standard database programs that are tens of millions of lines of code. The database companies are highly unlikely to make changes to enhance the performance for one particular computer.

> 2.允许源代码修改，但基本上是不可能的。例如，数据库基准依赖于标准数据库程序，这些数据库程序是数千万行代码。数据库公司极不可能进行更改以提高一台特定计算机的性能。

3. Source modifications are allowed, as long as the altered version produces the same output.

> 3.只要更改版本产生相同的输出，允许源修改。

The key issue that benchmark designers face in deciding to allow modification of the source is whether such modifications will reflect real practice and provide useful insight to users, or whether these changes simply reduce the accuracy of the bench- marks as predictors of real performance. As we will see in [Chapter 7](#_bookmark322), domain- specific architects often follow the third option when creating processors for well-defined tasks.

> 基准设计师决定允许修改源的关键问题是这种修改是否会反映出真实实践并为用户提供有用的见解，或者这些更改是否仅仅降低了基准标记的准确性作为真实性能的预测指标。正如我们将在[第 7 章]（#\_ bookmark322）中看到的那样，域特定的架构师通常在创建定义明确任务的处理器时遵循第三个选项。

To overcome the danger of placing too many eggs in one basket, collections of benchmark applications, called _benchmark suites_, are a popular measure of perfor- mance of processors with a variety of applications. Of course, such collections are only as good as the constituent individual benchmarks. Nonetheless, a key advan- tage of such suites is that the weakness of any one benchmark is lessened by the presence of the other benchmarks. The goal of a benchmark suite is that it will char- acterize the real relative performance of two computers, particularly for programs not in the suite that customers are likely to run.

> 为了克服将太多鸡蛋放在一个篮子中的危险，称为 *benchmark Suites* 的基准应用集合是对带有多种应用的处理器的流行量度。当然，这样的收藏仅与组成单独的基准一样好。但是，这种套房的关键优势是，其他基准的存在减少了任何一个基准的弱点。基准套件的目的是，它将符合两台计算机的实际相对性能，尤其是对于不可能运行的套件中的程序。

A cautionary example is the Electronic Design News Embedded Microproces- sor Benchmark Consortium (or EEMBC, pronounced “embassy”) benchmarks.

> 一个警告的例子是电子设计新闻嵌入的微处理学基准联盟（或 EEMBC，发音为“使馆”）基准。

It is a set of 41 kernels used to predict performance of different embedded applications: automotive/industrial, consumer, networking, office automation, and telecommunications. EEMBC reports unmodified performance and “full fury” performance, where almost anything goes. Because these benchmarks use small kernels, and because of the reporting options, EEMBC does not have the reputation of being a good predictor of relative performance of different embedded computers in the field. This lack of success is why Dhrystone, which EEMBC was trying to replace, is sadly still used.

> 它是一组 41 个内核，用于预测不同嵌入式应用程序的性能：汽车/工业，消费者，网络，办公室自动化和电信。EEMBC 报告了未修改的表现和“完全愤怒”的表现，几乎所有事情都会发生。由于这些基准测试使用小核，并且由于报告选项，因此 EEMBC 并没有声誉，即对现场不同嵌入式计算机的相对性能的良好预测指标。缺乏成功的原因是 EEMBC 试图取代的 Dhrystone 仍然被遗憾地使用了。

One of the most successful attempts to create standardized benchmark appli- cation suites has been the SPEC (Standard Performance Evaluation Corporation), which had its roots in efforts in the late 1980s to deliver better benchmarks for workstations. Just as the computer industry has evolved over time, so has the need for different benchmark suites, and there are now SPEC benchmarks to cover many application classes. All the SPEC benchmark suites and their reported results are found at [http://www.spec.org](http://www.spec.org/).

> 建立标准化基准应用套件的最成功的尝试之一是 Spec（标准绩效评估公司），该规范扎根于 1980 年代后期的努力，以为工作站提供更好的基准。正如计算机行业随着时间的推移而发展的一样，需要不同的基准套件，现在有规格的基准来涵盖许多申请课程。所有规格基准套件及其报告的结果均在[http://www.spec.org]（[http://www.spec.org/](http://www.spec.org/)）上找到。

Although we focus our discussion on the SPEC benchmarks in many of the following sections, many benchmarks have also been developed for PCs running the Windows operating system.

> 尽管我们将讨论集中在以下许多部分中的规格基准上，但也为运行 Windows 操作系统的 PC 开发了许多基准。

##### _Desktop Benchmarks_

> ##### _desktop 基准_

Desktop benchmarks divide into two broad classes: processor-intensive bench- marks and graphics-intensive benchmarks, although many graphics benchmarks include intensive processor activity. SPEC originally created a benchmark set focusing on processor performance (initially called SPEC89), which has evolved into its sixth generation: SPEC CPU2017, which follows SPEC2006, SPEC2000, SPEC95 SPEC92, and SPEC89. SPEC CPU2017 consists of a set of 10 integer benchmarks (CINT2017) and 17 floating-point benchmarks (CFP2017). [Figure 1.17](#_bookmark26) describes the current SPEC CPU benchmarks and their ancestry.

> 桌面基准分为两个广泛的类别：处理器密集型标记和图形密集型基准测试，尽管许多图形基准都包括密集的处理器活动。Spec 最初创建了一个专注于处理器性能（最初称为 SPEC89）的基准集，该基准已演变为第六代：SPEC CPU2017，遵循 SPEC2006，SPEC2000，SPEC95 SPEC92 和 SPEC89。规格 CPU2017 由一组 10 个整数基准（CINT2017）和 17 个浮点基准（CFP2017）组成。[图 1.17]（#\_ bookmark26）描述了当前的规格 CPU 基准及其祖先。

> ===

> Figure 1.17 SPEC2017 programs and the evolution of the SPEC benchmarks over time, with integer programs above the line and floating- point programs below the line. Of the 10 SPEC2017 integer programs, 5 are written in C, 4 in C++., and 1 in Fortran. For the floating-point programs, the split is 3 in Fortran, 2 in C++, 2 in C, and 6 in mixed C, C++, and Fortran. The figure shows all 82 of the programs in the 1989, 1992, 1995, 2000, 2006, and 2017 releases. Gcc is the senior citizen of the group. Only 3 integer programs and 3 floating-point programs survived three or more generations. Although a few are carried over from generation to generation, the version of the program changes and either the input or the size of the benchmark is often expanded to increase its running time and to avoid perturbation in measurement or domination of the execution time by some factor other than CPU time. The benchmark descriptions on the left are for SPEC2017 only and do not apply to earlier versions. Programs in the same row from different generations of SPEC are generally not related; for example, fpppp is not a CFD code like bwaves.

>> 图 1.17 Spec2017 程序和规格基准的演变随着时间的推移，线上上方的整数程序和浮点程序在行下方。在 10 个 Spec2017 整数程序中，有 5 个以 C ++ 为 4 英寸，其中 1 个编写了 Fortran。对于浮点程序，拆分为 3，在 fortran 中为 3，C ++ 为 2，C ++ 为 2，而混合 C，C ++ 和 FORTRAN 中的分裂为 6。该图显示了 1989、1992、1995、2000、2006 和 2017 年版本中的所有 82 个程序。GCC 是该小组的老年人。只有 3 个整数程序和 3 个浮点程序在三代或更多世代中幸存。尽管几个人曾经载有一些，但程序的版本更改以及基准的输入或大小通常会扩大以增加其运行时间，并避免通过某些因素进行测量或执行时间的侵入扰动除了 CPU 时间。左侧的基准描述仅适用于 Spec2017，不适用于早期版本。来自不同一代规格的同一行中的程序通常无关；例如，FPPPP 不是 Bwaves 这样的 CFD 代码。
>>

SPEC benchmarks are real programs modified to be portable and to minimize the effect of I/O on performance. The integer benchmarks vary from part of a C compiler to a go program to a video compression. The floating-point benchmarks include molecular dynamics, ray tracing, and weather forecasting. The SPEC CPU suite is useful for processor benchmarking for both desktop systems and single-processor servers. We will see data on many of these programs throughout this book. However, these programs share little with modern programming lan- guages and environments and the Google Translate application that [Section 1.1](#introduction) describes. Nearly half of them are written at least partially in Fortran! They are even statically linked instead of being dynamically linked like most real pro- grams. Alas, the SPEC2017 applications themselves may be real, but they are not inspiring. It’s not clear that SPECINT2017 and SPECFP2017 capture what is exciting about computing in the 21st century.

> 规格基准是实际修改的实际程序，以便于便携式，并最大程度地减少 I/O 对性能的影响。整数基准分析从 C 编译器的一部分到 GO 程序，再到视频压缩。浮点基准包括分子动力学，射线跟踪和天气预报。SPEC CPU 套件可用于台式系统和单处理器服务器的处理器基准测试。在本书中，我们将看到许多此类程序的数据。但是，这些程序与现代编程语言和环境以及 Google 翻译应用程序[1.1]（＃简介）所描述的应用程序很少。其中近一半至少部分写在 Fortran 中！它们甚至是静态链接的，而不是像大多数真实的研究一样动态链接。las，Spec2017 应用程序本身可能是真实的，但它们并不鼓舞。目前尚不清楚 Specint2017 和 SpecFP2017 捕获了 21 世纪的计算令人兴奋的内容。

In [Section 1.11](#_bookmark33), we describe pitfalls that have occurred in developing the SPEC CPUbenchmark suite, as well as the challenges in maintaining a useful and pre- dictive benchmark suite.

> 在[第 1.11 节]（#\_ bookmark33）中，我们描述了开发规格 CPubench 套件中发生的陷阱，以及维护有用且具有预示性基准的基准套件的挑战。

SPEC CPU2017 is aimed at processor performance, but SPEC offers many other benchmarks. [Figure 1.18](#_bookmark27) lists the 17 SPEC benchmarks that are active in 2017.

> SPEC CPU2017 针对处理器性能，但 SPEC 提供了许多其他基准测试。[图 1.18]（#\_ bookmark27）列出了 2017 年活跃的 17 个规格基准。

##### _Server Benchmarks_

> ##### _Server Benchmarks_

Just as servers have multiple functions, so are there multiple types of benchmarks. The simplest benchmark is perhaps a processor throughput-oriented benchmark. SPEC CPU2017 uses the SPEC CPU benchmarks to construct a simple throughput benchmark where the processing rate of a multiprocessor can be measured by run- ning multiple copies (usually as many as there are processors) of each SPEC CPU benchmark and converting the CPU time into a rate. This leads to a measurement called the SPECrate, and it is a measure of request-level parallelism from Section

> 就像服务器具有多个功能一样，也有多种类型的基准测试。最简单的基准可能是处理器吞吐量的基准测试。SPEC CPU2017 使用 SPEC CPU 基准来构建一个简单的吞吐量基准，在其中可以通过运行多个副本（通常与处理器一样多的处理器）来测量多处理器的处理速率，并将 CPU 时间转换为 CPU 时间速度。这导致了一个称为 Specrate 的测量，它是从部分中的请求级并行性的度量

1.2. To measure thread-level parallelism, SPEC offers what they call high- performance computing benchmarks around OpenMP and MPI as well as for accelerators such as GPUs (see [Figure 1.18](#_bookmark27)).

> 1.2。为了测量线程级并行性，SPEC 提供了他们所谓的 OpenMP 和 MPI 周围的高性能计算基准以及 GPU 等加速器（请参见[图 1.18]（#\_ bookmark27））。

Other than SPECrate, most server applications and benchmarks have signifi- cant I/O activity arising from either storage or network traffic, including bench- marks for file server systems, for web servers, and for database and transaction- processing systems. SPEC offers both a file server benchmark (SPECSFS) and a Java server benchmark. (Appendix D discusses some file and I/O system bench- marks in detail.) SPECvirt_Sc2013 evaluates end-to-end performance of virtua- lized data center servers. Another SPEC benchmark measures power, which we examine in [Section 1.10](#_bookmark30).

> 除了规格外，大多数服务器应用程序和基准测试都具有由存储或网络流量引起的重要的 I/O 活动，包括文件服务器系统的基准标记，用于 Web 服务器以及数据库和交易 - 处理系统。SPEC 同时提供文件服务器基准（SPECSFS）和 Java 服务器基准。（附录 D 详细讨论了一些文件和 I/O 系统基准标记。）SpecVirt*Sc2013 评估 Virtua-Lized 数据中心服务器的端到端性能。另一个规格基准测量功率，我们在[1.10]（#* Bookmark30）中检查了功率。

Transaction-processing (TP) benchmarks measure the ability of a system to handle transactions that consist of database accesses and updates. Airline reserva- tion systems and bank ATM systems are typical simple examples of TP; more sophisticated TP systems involve complex databases and decision-making.

> 事务处理（TP）基准测量系统处理由数据库访问和更新组成的交易的能力。航空公司保留系统和银行 ATM 系统是 TP 的典型简单示例；更复杂的 TP 系统涉及复杂的数据库和决策。

> Figure 1.18 Active benchmarks from SPEC as of 2017.

>> 图 1.18 截至 2017 年的 Spec 的主动基准测试。
>>

In the mid-1980s, a group of concerned engineers formed the vendor-independent Transaction Processing Council (TPC) to try to create realistic and fair benchmarks for TP. The TPC benchmarks are described at [http://www.tpc.org](http://www.tpc.org/).

> 在 1980 年代中期，一群有关工程师组成了独立于供应商的交易处理委员会（TPC），试图为 TP 创建现实且公平的基准。TPC 基准测试在[http://www.tpc.org]（[http://www.tpc.org/](http://www.tpc.org/)）上进行了描述。

The first TPC benchmark, TPC-A, was published in 1985 and has since been replaced and enhanced by several different benchmarks. TPC-C, initially created in 1992, simulates a complex query environment. TPC-H models ad hoc decision support—the queries are unrelated and knowledge of past queries cannot be used to optimize future queries. The TPC-DI benchmark, a new data integration (DI) task also known as ETL, is an important part of data warehousing. TPC-E is an online transaction processing (OLTP) workload that simulates a brokerage firm’s customer accounts.

> 第一个 TPC 基准 TPC-A 于 1985 年出版，此后已被几种不同的基准取代和增强。TPC-C 最初于 1992 年创建，模拟了一个复杂的查询环境。TPC-H 模型临时决策支持 - 查询是无关的，过去查询的知识不能用于优化未来的查询。TPC-DI 基准测试是一种新的数据集成（DI）任务，也称为 ETL，是数据仓库的重要组成部分。TPC-E 是一种在线交易处理（OLTP）工作负载，可模拟经纪公司的客户帐户。

Recognizing the controversy between traditional relational databases and “No SQL” storage solutions, TPCx-HS measures systems using the Hadoop file system running MapReduce programs, and TPC-DS measures a decision support system that uses either a relational database or a Hadoop-based system. TPC-VMS and TPCx-V measure database performance for virtualized systems, and TPC-Energy adds energy metrics to all the existing TPC benchmarks.

> 识别传统关系数据库与“无 SQL”存储解决方案之间的争议，TPCX-HS 使用 Hadoop 文件系统运行 MAPREDUCE 程序来测量系统，而 TPC-DS 测量了使用关系数据库或基于 Hadoop 的系统的决策支持系统。TPC-VM 和 TPCX-V 测量虚拟化系统的数据库性能，TPC-Energy 为所有现有的 TPC 基准增添了能源指标。

All the TPC benchmarks measure performance in transactions per second. In addition, they include a response time requirement so that throughput performance is measured only when the response time limit is met. To model real-world sys- tems, higher transaction rates are also associated with larger systems, in terms of both users and the database to which the transactions are applied. Finally, the system cost for a benchmark system must be included as well to allow accurate comparisons of cost-performance. TPC modified its pricing policy so that there is a single specification for all the TPC benchmarks and to allow verification of the prices that TPC publishes.

> 所有 TPC 基准测量每秒交易的性能。此外，它们还包括响应时间要求，以便仅在满足响应时间限制时测量吞吐量性能。为了建模现实世界的系统，就用户和应用交易的数据库而言，较高的交易率也与较大的系统相关联。最后，还必须包括基准系统的系统成本，以便准确比较成本绩效。TPC 修改了其定价策略，因此所有 TPC 基准都有单个规范，并允许验证 TPC 发布的价格。

### Reporting Performance Results

> ###报告绩效结果

The guiding principle of reporting performance measurements should be _repro- ducibility_—list everything another experimenter would need to duplicate the results. A SPEC benchmark report requires an extensive description of the com- puter and the compiler flags, as well as the publication of both the baseline and the optimized results. In addition to hardware, software, and baseline tuning parameter descriptions, a SPEC report contains the actual performance times, shown both in tabular form and as a graph. A TPC benchmark report is even more complete, because it must include results of a benchmarking audit and cost information. These reports are excellent sources for finding the real costs of com- puting systems, since manufacturers compete on high performance and cost- performance.

> 报告绩效测量的指导原理应为 *repro- ducibility*-列出另一个实验者需要复制结果的所有内容。SPEC 基准报告需要对计算机和编译器标志的广泛描述，以及基线和优化结果的发布。除了硬件，软件和基线调整参数描述外，SPEC 报告还包含表格形式和图形的实际性能时间。TPC 基准报告更加完整，因为它必须包括基准审核和成本信息的结果。这些报告是找到合并系统的实际成本的绝佳来源，因为制造商在高性能和成本绩效上竞争。

### Summarizing Performance Results

> ###总结性能结果

In practical computer design, one must evaluate myriad design choices for their relative quantitative benefits across a suite of benchmarks believed to be relevant. Likewise, consumers trying to choose a computer will rely on performance mea- surements from benchmarks, which ideally are similar to the users’ applications. In both cases, it is useful to have measurements for a suite of benchmarks so that the performance of important applications is similar to that of one or more benchmarks in the suite and so that variability in performance can be understood. In the best case, the suite resembles a statistically valid sample of the application space, but such a sample requires more benchmarks than are typically found in most suites and requires a randomized sampling, which essentially no benchmark suite uses.

> 在实用的计算机设计中，必须评估无数的设计选择，以确保其相对定量的好处，这是一套相关的基准。同样，试图选择计算机的消费者将依靠基准的性能调查，这与用户的应用程序相似。在这两种情况下，对一套基准套件进行测量都非常有用，以便重要应用的性能与套件中一个或多个基准测试的性能相似，以便可以理解性能的可变性。在最好的情况下，套件类似于应用空间的统计有效样本，但是该样本需要比大多数套件中通常发现的基准测试，并且需要随机采样，这基本上没有基准测试套件的使用。

Once we have chosen to measure performance with a benchmark suite, we want to be able to summarize the performance results of the suite in a unique num- ber. A simple approach to computing a summary result would be to compare the arithmetic means of the execution times of the programs in the suite. An alternative would be to add a weighting factor to each benchmark and use the weighted arith- metic mean as the single number to summarize performance. One approach is to use weights that make all programs execute an equal time on some reference com- puter, but this biases the results toward the performance characteristics of the ref- erence computer.

> 一旦我们选择使用基准套件来衡量性能，我们希望能够以独特的数字来概括该套件的性能结果。计算摘要结果的一种简单方法是比较套件中程序执行时间的算术手段。另一种选择是为每个基准添加一个加权因子，并使用加权算术平均值作为单个数字来汇总性能。一种方法是使用使所有程序在某些参考器上执行平等时间的权重，但是这将结果偏向参考计算机的性能特征。

Rather than pick weights, we could normalize execution times to a reference computer by dividing the time on the reference computer by the time on the computer being rated, yielding a ratio proportional to performance. SPEC uses this approach, calling the ratio the SPECRatio. It has a particularly useful property that matches the way we benchmark computer performance throughout this text—namely, comparing performance ratios. For example, suppose that the SPECRatio of computer A on a benchmark is 1.25 times as fast as computer B; then we know

> 我们可以通过将参考计算机上的时间除以所评估计算机的时间来将执行时间归一化为参考计算机，从而将其标准化为参考计算机，从而产生与性能成正比的比率。规格使用此方法，将比率称为规格。它具有一个特别有用的属性，该属性与我们在本文中进行基准计算机性能的方式相匹配，即比较性能比率。例如，假设计算机 A 在基准测试上的规格是计算机 B 的 1.25 倍；然后我们知道

> ===

Notice that the execution times on the reference computer drop out and the choice of the reference computer is irrelevant when the comparisons are made as a ratio, which is the approach we consistently use. [Figure 1.19](#_bookmark28) gives an example.

> 请注意，当对比较进行比较时，参考计算机上的执行时间掉落，参考计算机的选择是无关紧要的，这是我们一致使用的方法。[图 1.19]（#\_ bookmark28）给出了一个示例。

Because a SPECRatio is a ratio rather than an absolute execution time, the mean must be computed using the _geometric_ mean. (Because SPECRatios have no units, comparing SPECRatios arithmetically is meaningless.) The formula is

> 由于规格是比率而不是绝对执行时间，因此必须使用* geometric*均值来计算平均值。（因为 Specratios 没有单位，因此算术比较算术是毫无意义的。）公式为

> ===

In the case of SPEC, _sample<sub>i</sub>_ is the SPECRatio for program _i_. Using the geometric mean ensures two important properties:

> 在规格的情况下，*sample <sub> i </sub> *是程序* i*的规格。使用几何均值可确保两个重要属性：

1. The geometric mean of the ratios is the same as the ratio of the geometric means.

> 1.比率的几何平均值与几何均值之比相同。

1. The ratio of the geometric means is equal to the geometric mean of the perfor- mance ratios, which implies that the choice of the reference computer is irrelevant.

> 1.几何均值的比率等于穿孔比率的几何平均值，这意味着参考计算机的选择是无关紧要的。

Therefore the motivations to use the geometric mean are substantial, especially when we use performance ratios to make comparisons.

> 因此，使用几何平均值的动机是实质性的，尤其是当我们使用性能比进行比较时。

Example Show that the ratio of the geometric means is equal to the geometric mean of the performance ratios and that the reference computer of SPECRatio does not matter.

> 示例表明，几何均值的比率等于性能比的几何平均值，并且 Specratio 的参考计算机无关紧要。

> ===

That is, the ratio of the geometric means of the SPECRatios of A and B is the geo- metric mean of the performance ratios of A to B of all the benchmarks in the suite. [Figure 1.19](#_bookmark28) demonstrates this validity using examples from SPEC.

> 也就是说，A 和 B 的 Specratios 的几何平均值比是套件中所有基准的 A 与 B 的性能比的地理平均值。[图 1.19]（#\_ bookmark28）使用规格中的示例演示了这种有效性。

> Figure 1.19 SPEC2006Cint execution times (in seconds) for the Sun Ultra 5—the reference computer of SPEC2006—and execution times and SPECRatios for the AMD A10 and Intel Xeon E5-2690. The final two columns show the ratios of execution times and SPEC ratios. This figure demonstrates the irrelevance of the reference computer in relative performance. The ratio of the execution times is identical to the ratio of the SPEC ratios, and the ratio of the geometric means (63.7231.91/20.86 2.00) is identical to the geometric mean of the ratios (2.00). [Section 1.11](#_bookmark33) discusses libquantum, whose performance is orders of magnitude higher than the other SPEC benchmarks.

>> 图 1.19 Spec2006Cint 执行时间（以秒为单位）的 Sun Ultra 5（Spec2006 的参考计算机）以及 AMD A10 和 Intel Xeon E5-2690 的执行时间和 Specratios。最后两列显示了执行时间和规格比率的比率。该图证明了参考计算机在相对性能方面的无关。执行时间的比率与规格比的比率相同，几何均值（63.7231.91/20.86 2.00）的比率与比率的几何平均值相同（2.00）。[1.11]（#\_ bookmark33）讨论了 libquantum，其性能比其他规格基准高的数量级。
>>
