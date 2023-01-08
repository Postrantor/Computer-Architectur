## Classes of Computers

These changes have set the stage for a dramatic change in how we view computing, computing applications, and the computer markets in this new century. Not since the creation of the personal computer have we seen such striking changes in the way computers appear and in how they are used. These changes in computer use have led to five diverse computing markets, each characterized by different applications, requirements, and computing technologies. [Figure 1.2](#_bookmark7) summarizes these mainstream classes of computing environments and their important characteristics.

> 这些变化为我们看待新世纪计算、计算应用程序和计算机市场的方式的巨大变化奠定了基础。 自从个人计算机问世以来，我们还没有看到计算机的外观和使用方式发生如此显着的变化。 计算机使用的这些变化导致了五个不同的计算市场，每个市场都有不同的应用程序、要求和计算技术。 [图 1.2](#_bookmark7) 总结了这些主流的计算环境类别及其重要特征。

### Internet of Things/Embedded Computers

| Feature                       | Personal mobile device (PMD)                    | Desktop                                        | Server                                        | Clusters/warehousescale computer                      | Internet of things/ embedded                   |
| ----------------------------- | ----------------------------------------------- | ---------------------------------------------- | --------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------- |
| Price of system               | $100–$1000                                     | $300–$2500                                    | $5000–$10,000,000                            | $100,000–$200,000,000                                | $10–$100,000                                  |
| Price of microprocessor       | $10–$100                                       | $50–$500                                      | $200–$2000                                   | $50–$250                                             | $0.01–$100                                    |
| Critical system design issues | Cost, energy, media performance, responsiveness | Priceperformance, energy, graphics performance | Throughput, availability, scalability, energy | Price-performance, throughput, energy proportionality | Price, energy, applicationspecific performance |

Figure 1.2 A summary of the five mainstream computing classes and their system characteristics. Sales in 2015 included about 1.6 billion PMDs (90% cell phones), 275 million desktop PCs, and 15 million servers. The total number of embedded processors sold was nearly 19 billion. In total, 14.8 billion ARM-technology-based chips were shipped in 2015. Note the wide range in system price for servers and embedded systems, which go from USB keys to network routers. For servers, this range arises from the need for very large-scale multiprocessor systems for high-end transaction processing.

> 图 1.2 五个主流计算类及其系统特性的总结。 2015 年的销售额包括约 16 亿台 PMD（90% 是手机）、2.75 亿台台式电脑和 1500 万台服务器。 嵌入式处理器总销量接近 190 亿颗。 2015 年总共出货了 148 亿个基于 ARM 技术的芯片。请注意服务器和嵌入式系统的系统价格差异很大，从 USB 密钥到网络路由器。 对于服务器，这个范围源于对用于高端事务处理的超大规模多处理器系统的需求。

Embedded computers are found in everyday machines: microwaves, washing machines, most printers, networking switches, and all automobiles. The phrase _Internet of Things_ (IoT) refers to embedded computers that are connected to the Internet, typically wirelessly. When augmented with sensors and actuators, IoT devices collect useful data and interact with the physical world, leading to a wide variety of “smart” applications, such as smart watches, smart thermostats, smart speakers, smart cars, smart homes, smart grids, and smart cities.

> 嵌入式计算机存在于日常机器中：微波炉、洗衣机、大多数打印机、网络交换机和所有汽车。 短语*物联网* (IoT) 是指连接到互联网的嵌入式计算机，通常是无线连接。 当增加传感器和执行器时，物联网设备会收集有用的数据并与物理世界交互，从而产生各种各样的“智能”应用，例如智能手表、智能恒温器、智能扬声器、智能汽车、智能家居、智能电网、 和智慧城市。

Embedded computers have the widest spread of processing power and cost. They include 8-bit to 32-bit processors that may cost one penny, and high-end 64-bit processors for cars and network switches that cost `$100`. Although the range of computing power in the embedded computing market is very large, price is a key factor in the design of computers for this space. Performance requirements do exist, of course, but the primary goal often meets the performance need at a minimum price, rather than achieving more performance at a higher price. The projections for the number of IoT devices in 2020 range from 20 to 50 billion.

> 嵌入式计算机具有最广泛的处理能力和成本。 它们包括价格可能为一美分的 8 位至 32 位处理器，以及用于汽车和网络交换机的高端 64 位处理器，价格为“100 美元”。 尽管嵌入式计算市场的计算能力范围非常大，但价格是为该领域设计计算机的关键因素。 当然，性能需求确实存在，但主要目标往往是以最低的价格满足性能需求，而不是以更高的价格获得更多的性能。 2020 年物联网设备数量预计在 20 到 500 亿之间。

Most of this book applies to the design, use, and performance of embedded processors, whether they are off-the-shelf microprocessors or microprocessor cores that will be assembled with other special-purpose hardware.

> 本书的大部分内容适用于嵌入式处理器的设计、使用和性能，无论它们是现成的微处理器还是将与其他专用硬件组装在一起的微处理器内核。

Unfortunately, the data that drive the quantitative design and evaluation of other classes of computers have not yet been extended successfully to embedded computing (see the challenges with EEMBC, for example, in [Section 1.8](#measuring-reporting-and-summarizing-performance)). Hence we are left for now with qualitative descriptions, which do not fit well with the rest of the book. As a result, the embedded material is concentrated in Appendix E. We believe a separate appendix improves the flow of ideas in the text while allowing readers to see how the differing requirements affect embedded computing.

> 不幸的是，驱动其他类别计算机的量化设计和评估的数据尚未成功扩展到嵌入式计算（参见 EEMBC 的挑战，例如，在[第 1.8 节]（#measuring-reporting-and-summarizing 表现））。 因此，我们现在只剩下定性描述，这与本书的其余部分不太相符。 因此，嵌入式材料集中在附录 E 中。我们认为单独的附录可以改善文本中的思想流，同时让读者了解不同的要求如何影响嵌入式计算。

### Personal Mobile Device

_Personal mobile device_ (PMD) is the term we apply to a collection of wireless devices with multimedia user interfaces such as cell phones, tablet computers, and so on. Cost is a prime concern given the consumer price for the whole product is a few hundred dollars. Although the emphasis on energy efficiency is frequently driven by the use of batteries, the need to use less expensive packaging—plastic versus ceramic—and the absence of a fan for cooling also limit total power consumption. We examine the issue of energy and power in more detail in [Section 1.5](#trends-in-power-and-energy-in-integrated-circuits). Applications on PMDs are often web-based and media-oriented, like the previously mentioned Google Translate example. Energy and size requirements lead to use of Flash memory for storage ([Chapter 2](#_bookmark46)) instead of magnetic disks.

> _个人移动设备_ (PMD) 是我们应用于具有多媒体用户界面的无线设备集合的术语，例如手机、平板电脑等。 考虑到整个产品的消费者价格为几百美元，成本是一个主要问题。 尽管对能源效率的重视通常是由电池的使用推动的，但使用更便宜的包装（塑料与陶瓷相比）的需要以及没有冷却风扇也限制了总功耗。 我们在 [第 1.5 节]（#trends-in-power-and-energy-in-integrated-circuits）中更详细地研究了能量和功率问题。 PMD 上的应用程序通常是基于 Web 和面向媒体的，就像前面提到的 Google Translate 示例一样。 能量和尺寸要求导致使用闪存进行存储（[第 2 章](#_bookmark46)）而不是磁盘。

The processors in a PMD are often considered embedded computers, but we are keeping them as a separate category because PMDs are platforms that can run externally developed software, and they share many of the characteristics of desktop computers. Other embedded devices are more limited in hardware and software sophistication. We use the ability to run third-party software as the dividing line between nonembedded and embedded computers.

> PMD 中的处理器通常被认为是嵌入式计算机，但我们将它们作为一个单独的类别保留，因为 PMD 是可以运行外部开发软件的平台，并且它们具有台式计算机的许多特征。 其他嵌入式设备在硬件和软件复杂性方面更为有限。 我们使用运行第三方软件的能力作为非嵌入式和嵌入式计算机之间的分界线。

Responsiveness and predictability are key characteristics for media applications. A _real-time performance_ requirement means a segment of the application has an absolute maximum execution time. For example, in playing a video on a PMD, the time to process each video frame is limited, since the processor must accept and process the next frame shortly. In some applications, a more nuanced requirement exists: the average time for a particular task is constrained as well as the number of instances when some maximum time is exceeded. Such approaches—sometimes called _soft real-time_—arise when it is possible to miss the time constraint on an event occasionally, as long as not too many are missed. Real-time performance tends to be highly application-dependent.

> 响应能力和可预测性是媒体应用程序的关键特征。 _real-time performance_ 要求意味着应用程序的一部分具有绝对最大执行时间。 例如，在 PMD 上播放视频时，处理每个视频帧的时间是有限的，因为处理器必须很快接受并处理下一帧。 在某些应用程序中，存在更细微的要求：限制特定任务的平均时间以及超过某个最大时间的实例数。 这种方法——有时称为 _soft real-time_ ——出现在有可能偶尔错过事件的时间限制时，只要没有错过太多。 实时性能往往高度依赖于应用程序。

Other key characteristics in many PMD applications are the need to minimize memory and the need to use energy efficiently. Energy efficiency is driven by both battery power and heat dissipation. The memory can be a substantial portion of the system cost, and it is important to optimize memory size in such cases. The importance of memory size translates to an emphasis on code size, since data size is dictated by the application.

> 许多 PMD 应用中的其他关键特性是需要最小化内存和需要高效使用能源。 能源效率由电池功率和散热共同驱动。 内存可能占系统成本的很大一部分，在这种情况下优化内存大小很重要。 内存大小的重要性转化为对代码大小的强调，因为数据大小由应用程序决定。

### Desktop Computing

The first, and possibly still the largest market in dollar terms, is desktop computing. Desktop computing spans from low-end netbooks that sell for under `$300` to high-end, heavily configured workstations that may sell for `$2500`. Since 2008, more than half of the desktop computers made each year have been battery operated lap-top computers. Desktop computing sales are declining.

> 以美元计算，第一个而且可能仍然是最大的市场是桌面计算。 桌面计算的范围从售价低于“300 美元”的低端上网本到售价可能为“2500 美元”的高端、高配置工作站。 自 2008 年以来，每年生产的台式电脑中有一半以上是电池供电的笔记本电脑。 台式电脑的销量正在下降。

Throughout this range in price and capability, the desktop market tends to be driven to optimize _price-performance._ This combination of performance (measured primarily in terms of compute performance and graphics performance) and price of a system is what matters most to customers in this market, and hence to computer designers. As a result, the newest, highest-performance microprocessors and cost-reduced microprocessors often appear first in desktop systems (see [Section 1.6](#trends-in-cost) for a discussion of the issues affecting the cost of computers).

> 在价格和功能的整个范围内，台式机市场倾向于优化 _价格-性能_。 性能（主要根据计算性能和图形性能衡量）和系统价格的组合是最重要的 这个市场的客户，因此也是计算机设计师。 因此，最新、性能最高的微处理器和成本更低的微处理器通常最先出现在桌面系统中（有关影响计算机成本的问题的讨论，请参见[第 1.6 节](#trends-in-cost)）。

Desktop computing also tends to be reasonably well characterized in terms of applications and benchmarking, though the increasing use of web-centric, interactive applications poses new challenges in performance evaluation.

> 桌面计算在应用程序和基准测试方面也往往具有相当好的特征，尽管越来越多地使用以网络为中心的交互式应用程序对性能评估提出了新的挑战。

### Servers

As the shift to desktop computing occurred in the 1980s, the role of servers grew to provide larger-scale and more reliable file and computing services. Such servers have become the backbone of large-scale enterprise computing, replacing the traditional mainframe.

> 随着 20 世纪 80 年代向桌面计算的转变，服务器的作用越来越大，以提供更大规模和更可靠的文件和计算服务。 此类服务器已成为大型企业计算的支柱，取代了传统的大型机。

For servers, different characteristics are important. First, availability is critical. (We discuss availability in [Section 1.7](#dependability).) Consider the servers running ATM machines for banks or airline reservation systems. Failure of such server systems is far more catastrophic than failure of a single desktop, since these servers must operate seven days a week, 24 hours a day. [Figure 1.3](#_bookmark8) estimates revenue costs of downtime for server applications.

> 对于服务器来说，不同的特性很重要。 首先，可用性至关重要。 （我们在 [第 1.7 节](#dependability) 中讨论了可用性。）考虑为银行或航空公司预订系统运行 ATM 机器的服务器。 此类服务器系统的故障比单个台式机的故障更具灾难性，因为这些服务器必须每周 7 天、每天 24 小时运行。 [图 1.3](#_bookmark8) 估算服务器应用程序停机的收入成本。

<span id="_bookmark8" class="anchor"></span>Annual losses with downtime of

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 29%" />
<col style="width: 15%" />
<col style="width: 20%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th>Application</th>
<th><blockquote>
<p>Cost of downtime per hour</p>
</blockquote></th>
<th><p>1%</p>
<p>(87.6 h/year)</p></th>
<th><blockquote>
<p>0.5%</p>
<p>(43.8 h/year)</p>
</blockquote></th>
<th><blockquote>
<p>0.1%</p>
<p>(8.8 h/year)</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Brokerage service</td>
<td>$4,000,000</td>
<td>$350,400,000</td>
<td>$175,200,000</td>
<td>$35,000,000</td>
</tr>
<tr class="even">
<td>Energy</td>
<td>$1,750,000</td>
<td>$153,300,000</td>
<td>$76,700,000</td>
<td>$15,300,000</td>
</tr>
<tr class="odd">
<td>Telecom</td>
<td>$1,250,000</td>
<td>$109,500,000</td>
<td>$54,800,000</td>
<td>$11,000,000</td>
</tr>
<tr class="even">
<td>Manufacturing</td>
<td>$1,000,000</td>
<td>$87,600,000</td>
<td>$43,800,000</td>
<td>$8,800,000</td>
</tr>
<tr class="odd">
<td>Retail</td>
<td>$650,000</td>
<td>$56,900,000</td>
<td>$28,500,000</td>
<td>$5,700,000</td>
</tr>
<tr class="even">
<td>Health care</td>
<td>$400,000</td>
<td>$35,000,000</td>
<td>$17,500,000</td>
<td>$3,500,000</td>
</tr>
<tr class="odd">
<td>Media</td>
<td>$50,000</td>
<td>$4,400,000</td>
<td>$2,200,000</td>
<td>$400,000</td>
</tr>
</tbody>
</table>

Figure 1.3 Costs rounded to nearest `$100,000` of an unavailable system are shown by analyzing the cost of downtime (in terms of immediately lost revenue), assuming three different levels of availability, and that downtime is distributed uniformly. These data are from Landstrom (2014) and were collected and analyzed by Contingency Planning Research.

> 图 1.3 不可用系统的成本四舍五入到最接近的“100,000 美元”，通过分析停机时间成本（根据立即损失的收入）显示，假设存在三个不同级别的可用性，并且停机时间均匀分布。 这些数据来自 Landstrom (2014)，由应急计划研究中心收集和分析。

A second key feature of server systems is scalability. Server systems often grow in response to an increasing demand for the services they support or an expansion in functional requirements. Thus the ability to scale up the computing capacity, the memory, the storage, and the I/O bandwidth of a server is crucial.

> 服务器系统的第二个关键特性是可扩展性。 服务器系统通常会随着对其支持的服务需求的增加或功能需求的扩展而增长。 因此，扩展服务器的计算能力、内存、存储和 I/O 带宽的能力至关重要。

Finally, servers are designed for efficient throughput. That is, the overall performance of the server—in terms of transactions per minute or web pages served per second—is what is crucial. Responsiveness to an individual request remains important, but overall efficiency and cost-effectiveness, as determined by how many requests can be handled in a unit time, are the key metrics for most servers. We return to the issue of assessing performance for different types of computing environments in [Section 1.8](#measuring-reporting-and-summarizing-performance).

> 最后，服务器是为高效吞吐量而设计的。 也就是说，服务器的整体性能——根据每分钟的交易量或每秒服务的网页——是至关重要的。 对单个请求的响应仍然很重要，但整体效率和成本效益（由单位时间内可以处理的请求数量决定）是大多数服务器的关键指标。 在[第 1.8 节]（#measuring-reporting-and-summarizing-performance）中，我们回到评估不同类型计算环境的性能的问题。

### Clusters/Warehouse-Scale Computers

The growth of Software as a Service (SaaS) for applications like search, social networking, video viewing and sharing, multiplayer games, online shopping, and so on has led to the growth of a class of computers called _clusters_. Clusters are collections of desktop computers or servers connected by local area networks to act as a single larger computer. Each node runs its own operating system, and nodes communicate using a networking protocol. WSCs are the largest of the clusters, in that they are designed so that tens of thousands of servers can act as one. [Chapter 6](#_bookmark268) describes this class of extremely large computers.

> 用于搜索、社交网络、视频查看和共享、多人游戏、在线购物等应用程序的软件即服务 (SaaS) 的增长导致了称为 _集群_ 的计算机类别的增长。 集群是通过局域网连接的台式计算机或服务器的集合，充当一台更大的计算机。 每个节点都运行自己的操作系统，并且节点使用网络协议进行通信。 WSC 是最大的集群，因为它们的设计使得数万台服务器可以作为一个集群。 [第 6 章](#_bookmark268) 描述了这类超大型计算机。

Price-performance and power are critical to WSCs since they are so large. As [Chapter 6](#_bookmark268) explains, the majority of the cost of a warehouse is associated with power and cooling of the computers inside the warehouse. The annual amortized computers themselves and the networking gear cost for a WSC is `$40 million`, because they are usually replaced every few years. When you are buying that much computing, you need to buy wisely, because a 10% improvement in priceperformance means an annual savings of `$4 million` (10% of `$40 million`) per WSC; a company like Amazon might have 100 WSCs!

> 性价比和功率对 WSC 至关重要，因为它们非常大。 正如 [第 6 章](#_bookmark268) 所解释的，仓库的大部分成本与仓库内计算机的电力和冷却有关。 WSC 的年度摊销计算机本身和网络设备成本为“4000 万美元”，因为它们通常每隔几年更换一次。 当您购买那么多计算设备时，您需要明智地购买，因为性价比提高 10% 意味着每个 WSC 每年可节省“400 万美元”（“4000 万美元”的 10%）； 像亚马逊这样的公司可能拥有 100 个 WSC！

WSCs are related to servers in that availability is critical. For example, Amazon.com had `$136 billion` in sales in 2016. As there are about 8800 hours in a year, the average revenue per hour was about `$15 million`. During a peak hour for Christmas shopping, the potential loss would be many times higher. As [Chapter 6](#_bookmark268) explains, the difference between WSCs and servers is that WSCs use redundant, inexpensive components as the building blocks, relying on a software layer to catch and isolate the many failures that will happen with computing at this scale to deliver the availability needed for such applications. Note that scalability for a WSC is handled by the local area network connecting the computers and not by integrated computer hardware, as in the case of servers.

> WSC 与服务器相关，因为可用性至关重要。 例如，Amazon.com 在 2016 年的销售额为“1360 亿美元”。由于一年大约有 8800 个小时，因此每小时的平均收入约为“1500 万美元”。 在圣诞节购物的高峰时段，潜在损失会高出许多倍。 正如[第 6 章](#_bookmark268) 所解释的那样，WSC 和服务器之间的区别在于 WSC 使用冗余、廉价的组件作为构建块，依靠软件层来捕获和隔离这种规模的计算中会发生的许多故障 提供此类应用程序所需的可用性。 请注意，WSC 的可扩展性由连接计算机的局域网处理，而不是像服务器那样由集成计算机硬件处理。

_Supercomputers_ are related to WSCs in that they are equally expensive, costing hundreds of millions of dollars, but supercomputers differ by emphasizing floating-point performance and by running large, communication-intensive batch programs that can run for weeks at a time. In contrast, WSCs emphasize interactive applications, large-scale storage, dependability, and high Internet bandwidth.

> _超级计算机_ 与 WSC 相关，因为它们同样昂贵，耗资数亿美元，但超级计算机的不同之处在于强调浮点性能和运行一次可以运行数周的大型、通信密集型批处理程序。 相比之下，WSC 强调交互式应用程序、大规模存储、可靠性和高互联网带宽。

### Classes of Parallelism and Parallel Architectures

Parallelism at multiple levels is now the driving force of computer design across all four classes of computers, with energy and cost being the primary constraints. There are basically two kinds of parallelism in applications:

> 多层次的并行性现在是所有四类计算机的计算机设计的驱动力，能源和成本是主要的限制因素。 应用程序中基本上有两种并行性：

1. _Data-level parallelism (DLP)_ arises because there are many data items that can be operated on at the same time.

   > 1. _Data-level parallelism (DLP)_ 的出现是因为有很多数据项可以同时操作。
   >
2. _Task-level parallelism (TLP)_ arises because tasks of work are created that can operate independently and largely in parallel. Computer hardware in turn can exploit these two kinds of application parallelism in four major ways:

   > 2. _Task-level parallelism (TLP)_ 的出现是因为创建的工作任务可以独立运行并且在很大程度上是并行的。 反过来，计算机硬件可以通过四种主要方式利用这两种应用程序并行性：
   >
3. _Instruction-level parallelism_ exploits data-level parallelism at modest levels with compiler help using ideas like pipelining and at medium levels using ideas like speculative execution.

   > 3. _指令级并行性_ 利用数据级并行性在中等水平上利用编译器帮助使用流水线等思想，在中等水平上利用推测执行等思想。
   >
4. _Vector architectures, graphic processor units (GPUs), and multimedia instruction sets_ exploit data-level parallelism by applying a single instruction to a collection of data in parallel.

   > 4. _矢量架构、图形处理器单元 (GPU) 和多媒体指令集_ 通过将单个指令并行应用于数据集合来利用数据级并行性。
   >
5. _Thread-level parallelism_ exploits either data-level parallelism or task-level parallelism in a tightly coupled hardware model that allows for interaction between parallel threads.

   > 5. _线程级并行_ 在紧密耦合的硬件模型中利用数据级并行或任务级并行，允许并行线程之间的交互。
   >
6. _Request-level parallelism_ exploits parallelism among largely decoupled tasks specified by the programmer or the operating system. When [Flynn (1966)](#_bookmark949) studied the parallel computing efforts in the 1960s, he found a simple classification whose abbreviations we still use today. They target data-level parallelism and task-level parallelism. He looked at the parallelism in the instruction and data streams called for by the instructions at the most constrained component of the multiprocessor and placed all computers in one of four categories:

   > 6. _请求级并行_ 利用程序员或操作系统指定的大部分解耦任务之间的并行性。 当 [Flynn (1966)](#_bookmark949) 研究 1960 年代的并行计算工作时，他发现了一个简单的分类，其缩写我们今天仍在使用。 它们针对数据级并行性和任务级并行性。 他研究了多处理器中最受限组件的指令所要求的指令和数据流的并行性，并将所有计算机归为以下四个类别之一：
   >
7. _Single instruction stream, single data stream_ (SISD)—This category is the uniprocessor. The programmer thinks of it as the standard sequential computer, but it can exploit ILP. [Chapter 3](#_bookmark93) covers SISD architectures that use ILP techniques such as superscalar and speculative execution.

   > 7. _单指令流、单数据流_（SISD）——这一类是单处理器。 程序员将其视为标准的顺序计算机，但它可以利用 ILP。 [第 3 章](#_bookmark93) 介绍了使用超标量和推测执行等 ILP 技术的 SISD 架构。
   >
8. _Single instruction stream, multiple data streams_ (SIMD)—The same instruction is executed by multiple processors using different data streams. SIMD computers exploit _data-level parallelism_ by applying the same operations to multiple items of data in parallel. Each processor has its own data memory (hence, the MD of SIMD), but there is a single instruction memory and control processor, which fetches and dispatches instructions. [Chapter 4](#_bookmark165) covers DLP and three different architectures that exploit it: vector architectures, multimedia extensions to standard instruction sets, and GPUs.

   > 8. _单指令流，多数据流_ (SIMD)——同一条指令由多个处理器使用不同的数据流执行。 SIMD 计算机通过对多个数据项并行应用相同的操作来利用 _数据级并行性_。 每个处理器都有自己的数据存储器（因此，SIMD 的 MD），但只有一个指令存储器和控制处理器，用于获取和调度指令。 [第 4 章](#_bookmark165) 涵盖 DLP 和利用它的三种不同架构：向量架构、标准指令集的多媒体扩展和 GPU。
   >
9. _Multiple instruction streams, single data stream_ (MISD)—No commercial multiprocessor of this type has been built to date, but it rounds out this simple classification.

   > 9. _多指令流，单数据流_ (MISD)——迄今为止还没有制造出这种类型的商用多处理器，但它完善了这个简单的分类。
   >
10. _Multiple instruction streams, multiple data streams_ (MIMD)—Each processor fetches its own instructions and operates on its own data, and it targets task-level parallelism. In general, MIMD is more flexible than SIMD and thus more generally applicable, but it is inherently more expensive than SIMD. For example, MIMD computers can also exploit data-level parallelism, although the overhead is likely to be higher than would be seen in an SIMD computer. This overhead means that grain size must be sufficiently large to exploit the parallelism efficiently. [Chapter 5](#_bookmark213) covers tightly coupled MIMD architectures, which exploit _thread-level parallelism_ because multiple cooperating threads operate in parallel. [Chapter 6](#_bookmark268) covers loosely coupled MIMD architectures—specifically, _clusters_ and _warehouse-scale computers_—that exploit _request-level parallelism_, where many independent tasks can proceed in parallel naturally with little need for communication or synchronization.

    > 10. _多指令流、多数据流_（MIMD）——每个处理器获取自己的指令并对自己的数据进行操作，它的目标是任务级并行性。 一般来说，MIMD 比 SIMD 更灵活，因此更普遍适用，但它本质上比 SIMD 更昂贵。 例如，MIMD 计算机也可以利用数据级并行性，尽管开销可能比在 SIMD 计算机中看到的要高。 这种开销意味着粒度必须足够大才能有效地利用并行性。 [第 5 章](#_bookmark213) 涵盖紧密耦合的 MIMD 架构，它利用 _线程级并行性_ ，因为多个协作线程并行运行。 [第 6 章](#_bookmark268) 涵盖了松散耦合的 MIMD 架构——特别是 _集群_ 和 _仓库规模的计算机_ ——利用 _请求级并行_，其中许多独立任务可以自然地并行进行，几乎不需要通信或同步。
    >

This taxonomy is a coarse model, as many parallel processors are hybrids of the SISD, SIMD, and MIMD classes. Nonetheless, it is useful to put a framework on the design space for the computers we will see in this book.
