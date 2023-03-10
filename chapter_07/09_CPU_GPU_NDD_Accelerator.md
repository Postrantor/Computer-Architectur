## Putting It All Together: CPUs Versus GPUs Versus DNN Accelerators

We now use the DNN domain to compare the cost-performance of the accelerators in this chapter.[<sup>2</sup>](#_bookmark375) We start with a thorough comparison of the TPU to standard CPUs and GPUs and then add brief comparisons to Catapult and Pixel Visual Core.

> 现在，我们使用 DNN 域来比较本章中加速器的成本效果。[<sup> 2 </sup>](#_bookmark375)我们从将 TPU 与标准 CPU 和 GPU 进行彻底比较开始，然后添加添加与弹射器和像素视觉核心的简短比较。

[Figure 7.41](#_bookmark374) shows the six benchmarks we use in this comparison. They consist of two examples of each of the three types of DNNs in [Section 7.3](#example-domain-deep-neural-networks). These six benchmarks represent 95% of TPU inference workload in Google data centers in 2016. Typically written in TensorFlow, they are surprisingly short: just 100– 1500 lines of code. They are small pieces of larger applications that run on the host server, which can be thousands to millions of lines of C++ code. The applications are typically user-facing, which leads to rigid response-time limits, as we will see. [Figures 7.42](#_bookmark376) and [7.43](#_bookmark377) show the chips and servers being compared. They are server-class computers deployed in Google data centers at the same time that TPUs were deployed. To be deployed in Google data centers, they must at least check for internal memory errors, which excluded some choices, such as the Nvidia Maxwell GPU. For Google to purchase and deploy them, the machines had to be sensibly configured, and not awkward artifacts assembled solely to win benchmarks.

> [图 7.41](#_bookmark374) 显示了我们在此比较中使用的六个基准。它们由 [第 7.3 节](#example-domain-deep-neural-networks) 中三种类型的 DNN 中的每一种的两个示例组成。这六个基准代表了 2016 年谷歌数据中心 95% 的 TPU 推理工作量。通常用 TensorFlow 编写，它们非常短：只有 100-1500 行代码。它们是在主机服务器上运行的较大应用程序的一小部分，可能是数千到数百万行 C++ 代码。这些应用程序通常是面向用户的，这会导致严格的响应时间限制，正如我们将看到的那样。[图 7.42](#_bookmark376) 和 [7.43](#_bookmark377) 显示了被比较的芯片和服务器。它们是在部署 TPU 的同时部署在 Google 数据中心的服务器级计算机。要部署在谷歌数据中心，他们至少必须检查内存错误，这排除了一些选择，比如 Nvidia Maxwell GPU。为了让谷歌购买和部署它们，机器必须经过合理配置，而不是仅仅为了赢得基准测试而组装的笨拙工件。

The traditional CPU server is represented by an 18-core, dual-socket Haswell processor from Intel. This platform is also the host server for GPUs or TPUs.

> 传统的 CPU 服务器由 Intel 的 18 核双插座 Haswell 处理器表示。该平台也是 GPU 或 TPU 的主机服务器。

Figure 7.41 Six DNN applications (two per DNN type) that represent 95% of the TPU’s workload. The 10 columns are the DNN name; the number of lines of code; the types and number of layers in the DNN (FC is fully connected; Conv is convolution; Element is element-wise operation of LSTM, see [Section 7.3](#example-domain-deep-neural-networks); and Pool is pooling, which is a downsizing stage that replaces a group of elements with its average or maximum); the number of weights; TPU oper- ational intensity; and TPU application popularity in 2016. The operational intensity varies between TPU, CPU, and GPU because the batch sizes vary. The TPU can have larger batch sizes while still staying under the response time limit. One DNN is RankBrain (Clark, 2015), one LSTM is GNM Translate (Wu et al., 2016), and one CNN is DeepMind AlphaGo ([Silver et al., 2016; Jouppi, 2016](#_bookmark385)).

> 图 7.41 代表 95% 的 TPU 工作负载的六个 DNN 应用程序(每种 DNN 类型两个)。10 列是 DNN 名称；代码行数；DNN 中层的类型和层数(FC 是全连接；Conv 是卷积；Element 是 LSTM 的 element-wise 操作，见 [Section 7.3](#example-domain-deep-neural-networks)；Pool 是 pooling ，这是一个缩小阶段，用其平均值或最大值替换一组元素)；重量的数量；TPU 运行强度；以及 2016 年 TPU 应用程序流行度。TPU、CPU 和 GPU 之间的操作强度因批大小不同而不同。TPU 可以有更大的批量大小，同时仍保持在响应时间限制内。一种 DNN 是 RankBrain (Clark, 2015)，一种 LSTM 是 GNM Translate (Wu et al., 2016)，一种 CNN 是 DeepMind AlphaGo ([Silver et al., 2016; Jouppi, 2016](#_bookmark385))。

This section is also largely based upon the paper "In-Datacenter Performance Analysis of a Tensor Processing Unit" [Jouppi et al., 2017](#_bookmark966), of which one of your book authors was a coauthor.

> 本节还主要基于 "张量处理单元的纳入绩效分析" [Jouppi 等，2017](#_bookmark966)，其中一位书籍作者是合着者。

Figure 7.42 The chips used by the benchmarked servers are Haswell CPUs, K80 GPUs, and TPUs. Haswell has 18 cores, and the K80 has 13 SMX processors.

> 图 7.42 基准服务器使用的芯片是 Haswell CPU，K80 GPU 和 TPU。哈斯韦尔有 18 个核心，K80 有 13 个 SMX 处理器。

Figure 7.43 Benchmarked servers that use the chips in [Figure 7.42](#_bookmark376). The low-power TPU allows for better rack-level density than the high-power GPU. The 8 GiB DRAM per TPU is Weight Memory.

> 图 7.43 在[图 7.42]中使用芯片的基准服务器(#\_bookmark376)。低功率 TPU 可以比高功率 GPU 更高的机架级密度。每个 TPU 的 8 GIB DRAM 是重量记忆。

Haswell is fabricated in an Intel 22-nm process. Both the CPU and GPU are very large dies: about 600 mm<sup>2</sup>!

> 哈斯韦尔在英特尔 22 nm 的过程中制造。CPU 和 GPU 都非常大：大约 600 mm <sup> 2 </sup>！

The GPU accelerator is the Nvidia K80. Each K80 card contains two dies and offers SECDED on internal memory and DRAM. Nvidia states that ([Nvidia, 2016](#_bookmark982)) the K80 Accelerator dramatically lowers datacenter cost by delivering applica- tion performance with fewer, more powerful servers.

> GPU 加速器是 NVIDIA K80。每张 K80 卡都包含两个模具，并在内部内存和 DRAM 上提供了限制。NVIDIA 指出([NVIDIA，2016](#_bookmark982))K80 加速器通过以更少，更强大的服务器提供应用程序性能来大大降低数据中心成本。

DNN researchers frequently used K80s in 2015, which is when they were deployed at Google. Note that K80s were also chosen for new cloud-based GPUs by Amazon Web Services and by Microsoft Azure in late 2016.

> DNN 研究人员在 2015 年经常使用 K80，这是在 Google 部署的时候。请注意，Amazon Web Services 和 Microsoft Azure 在 2016 年底还为 K80 选择了基于云的 GPU。

Because the number of dies per benchmarked server varies between 2 and 8, the following figures show results normalized per die, except for [Figure 7.50](#_bookmark384), which compares the performance/watt of whole servers.

> 由于每个基准测试服务器的模具数量在 2 到 8 之间变化，因此以下图显示了每个模具的归一化结果，除了[图 7.50](#_bookmark384)，它比较了整个服务器的性能/瓦。

### Performance: Rooflines, Response Time, and Throughput

To illustrate the performance of the six benchmarks on the three processors, we adapt the Roofline performance model in [Chapter 4](#_bookmark165). To use the Roofline model for the TPU, when DNN applications are quantized, we first replace floating-point operations with integer multiply-accumulate operations. As weights do not normally fit in on-chip memory for DNN applications, the second change is to redefine operational intensity to be integer operations per byte of weights read ([Figure 7.41](#_bookmark374)).

> 为了说明三个处理器上六个基准测试的性能，我们采用了 [第 4 章](#_bookmark165) 中的 Roofline 性能模型。为了将 Roofline 模型用于 TPU，当 DNN 应用程序被量化时，我们首先用整数乘法累加运算替换浮点运算。由于权重通常不适合 DNN 应用程序的片上内存，第二个更改是将操作强度重新定义为每读取权重字节的整数操作([图 7.41](#_bookmark374))。

![](../media/image454.png)50
<img src="../media/image456.png" />
<img src="../media/image457.png" />

Figure 7.44 TPU Roofline. Its ridge point is far to the right at 1350 multiply- accumulate operations per byte of weight memory. CNN1 is much further below its Roofline than the other DNNs because it spends about a third of the time waiting for weights to be loaded into the matrix unit and because the shallow depth of some layers in the CNN results in only half of the elements within the matrix unit holding use- ful values ( [Jouppi et al., 2017](#_bookmark966)).

> 图 7.44 TPU 车顶线。它的山脊点距离右侧为 1350 多重累积操作，每个字节的重量记忆。CNN1 比其他 DNN 远远低于其车顶线，因为它花费了大约三分之一的时间等待重量加载到矩阵单元中，并且因为 CNN 中某些层的浅深度仅在一半的元素中产生矩阵单位保持使用值([[Jouppi 等，2017](#_bookmark966))。

[Figure 7.44](#_bookmark378) shows the Roofline model for a single TPU on log-log scales. The TPU has a long "slanted" part of its Roofline, where operational intensity means that performance is limited by memory bandwidth rather than by peak compute. Five of the six applications are happily bumping their heads against the ceiling: the MLPs and LSTMs are memory-bound, and the CNNs are computation-bound. The single DNN that is not bumping its head against the ceiling is CNN1. Despite CNNs having very high operational intensity, CNN1 is running at only 14.1 Tera Operations Per Second (TOPS), while CNN0 runs at a satisfying 86 TOPS.

> [图 7.44](#_bookmark378) 显示了日志尺度上单个 TPU 的车顶线模型。TPU 在其屋顶线中有一个长的 "倾斜" 部分，其中操作强度意味着性能受到内存带宽而不是峰值计算的限制。六个应用程序中有五个愉快地将他们的头撞到天花板上：MLP 和 LSTM 是记忆的，CNN 是计算的。没有撞到天花板的单一 DNN 是 CNN1。尽管 CNN 的操作强度很高，但 CNN1 的运行量仅为每秒 14.1 TERA 操作(顶部)，而 CNN0 的运行量为 86 个顶部。

For readers interested into a deep dive into what happened with CNN1, [Figure 7.45](#_bookmark379) uses performance counters to give partial visibility into the utilization of the TPU. The TPU spends less than half of its cycles performing matrix oper- ations for CNN1 (column 7, row 1). On each of those active cycles, only about half of the 65,536 MACs hold useful weights because some layers in CNN1 have shal- low feature depths. About 35% of cycles are spent waiting for weights to load from memory into the matrix unit, which occurs during the four fully connected layers that run at an operational intensity of just 32. This leaves roughly 19% of cycles not explained by the matrix-related counters. Because of overlapped execution on the TPU, we do not have exact accounting for those cycles, but we can see that 23% of cycles have stalls for RAW dependences in the pipeline and that 1% are spent stalled for input over the PCIe bus.

> 对于对 CNN1 发生的事情感兴趣的读者，[图 7.45](#_bookmark379) 使用性能计数器对 TPU 的利用进行部分可见性。TPU 花费不到一半的周期，用于 CNN1 的矩阵操作(第 7 列，第 1 行)。在这些活动循环中的每个循环中，只有 65,536 个 MAC 中的大约一半具有有用的权重，因为 CNN1 中的某些层具有较小的特征深度。大约 35％的循环花在等待重量从内存中加载到矩阵单元中，这是在四个完全连接的层中发生的，该层的运行强度仅为 32。相关计数器。由于 TPU 上的执行重叠，我们对这些周期没有确切的核算，但是我们可以看到 23％的周期有用于管道中原始依赖的失速，并且在 PCIE 总线上花费了 1％的时间。

Figure 7.45 Factors limiting TPU performance of the NN workload based on hardware performance counters. Rows 1, 4, 5, and 6 total 100% and are based on measurements of activity of the matrix unit. Rows 2 and 3 further break down the fraction of 64K weights in the matrix unit that hold useful weights on active cycles. Our counters cannot exactly explain the time when the matrix unit is idle in row 6; rows 7 and 8 show counters for two possible reasons, including RAW pipeline hazards and PCIe input stalls. Row 9 (TOPS) is based on measurements of production code while the other rows are based on performance-counter measurements, so they are not perfectly consistent. Host server overhead is excluded here. The MLPs and LSTMs are memory-bandwidth limited, but CNNs are not. CNN1 results are explained in the text.

> 图 7.45 基于硬件性能计数器限制 NN 工作负载的 TPU 性能的因素。第 1、4、5 和 6 行总共 100％，基于基质单元活性的测量。第 2 行和第 3 行进一步分解了基质单元中 64K 重量的分数，该重量在主动循环上保持有用的权重。我们的计数器无法准确解释矩阵单元在第 6 行中闲置的时间；第 7 行和 8 行显示柜台的原因有两个，包括原始管道危险和 PCIE 输入摊位。第 9 行(顶部)基于生产代码的测量值，而其他行是基于性能表测量的，因此它们并不完全一致。主机服务器开销在这里不包括。MLP 和 LSTM 是内存的带宽有限的，但 CNN 不是。CNN1 结果在文本中说明。

[Figures 7.46](#_bookmark380) and [7.47](#_bookmark381) show Rooflines for Haswell and the K80. The six NN applications are generally further below their ceilings than the TPU in [Figure 7.44](#_bookmark378). Response-time limits are the reason. Many of these DNN applications are parts of services that are part of end-user-facing services. Researchers have demonstrated that small increases in response time cause customers to use a service less (see [Chapter 6](#_bookmark268)). Thus, although training may not have hard response-time deadlines, inference usually does. That is, inference cares about throughput only while it is maintaining the latency bound.

> [图 7.46](#_bookmark380) 和 [7.47](#_bookmark381) 显示 Haswell 和 K80 的屋顶线。六个 NN 应用程序通常比[图 7.44]中的 TPU 远低于其天花板(#\_bookmark378)。响应时间限制是原因。这些 DNN 应用中的许多应用程序是面向最终用户服务的一部分服务的一部分。研究人员表明，响应时间的少量增加会导致客户使用服务较少(请参阅[第 6 章](#_bookmark268))。因此，尽管培训可能没有响应时间的截止日期，但推论通常会做到。也就是说，仅在维护延迟绑定时才关心吞吐量。

[Figure 7.48](#_bookmark382) illustrates the impact of the 99th percentile response-time limit of 7 ms for MLP0 on Haswell and the K80, which was required by the application developer. (The inferences per second and 7-ms latency include the server host time as well as the accelerator time.) They can operate at 42% and 37%, respec- tively, with the highest throughput achievable for MLP0, if the response-time limit is relaxed. Thus, although CPUs and GPUs have potentially much higher through- put, it’s wasted if they don’t meet the response-time limit. These bounds affect the TPU as well, but at 80% in [Figure 7.48](#_bookmark382), it is operating much closer to its highest MLP0 throughput. As compared with CPUs and GPUs, the single-threaded TPU has none of the sophisticated microarchitectural features discussed in [Section 7.1](#introduction-5) that consume transistors and energy to improve the average case but not the 99th- percentile case.

> [图 7.48](#_bookmark382) 说明了应用程序开发人员要求的第 99 个百分位响应时间限制对 MLP0 和 Haswell 和 K80 的影响。(每秒的推论和 7 毫秒的延迟包括服务器宿主时间以及加速器时间。)如果响应时间，则它们可以以 42％和 37％的速度运行，并且 MLP0 可实现最高的吞吐量限制放松。因此，尽管 CPU 和 GPU 可能在整个过程中都具有更高的范围，但如果他们不满足响应时间限制，则会浪费。这些界限也影响了 TPU，但是在[图 7.48](#_bookmark382) 中为 80％，它的运行更接近其最高的 MLP0 吞吐量。与 CPU 和 GPU 相比，单线读取的 TPU 没有[7.1](第 7.1 节](＃resoutding-5)中讨论的复杂的微体系特征，这些特征消耗了晶体管和能量来改善平均情况，但没有第 99％的情况。

Figure 7.46 Intel Haswell CPU Roofline with its ridge point at 13 multiply-accumulate operations/byte, which is much further to the left than in [Figure 7.44](#_bookmark378).

> 图 7.46 Intel Haswell CPU 屋顶线，其脊点处于 13 个多功能操作/字节，比[图 7.44]中的左侧距离远远远距离左侧(#\_bookmark378)。

![](../media/image461.png)
<img src="../media/image463.png" />
<img src="../media/image459.png" />

Figure 7.47 NVIDIA K80 GPU die Roofline. The much higher memory bandwidth moves the ridge point to 9 multiply-accumulate operations per weight byte, which is even further to the left than in [Figure 7.46](#_bookmark380).

> 图 7.47 NVIDIA K80 GPU 模具屋顶线。更高的存储器带宽将脊指数移至 9 个重量单字节的多重蓄电操作，比左边比[图 7.46]中的范围更远(#\_bookmark380)。

[Figure 7.49](#_bookmark383) gives the bottom line of relative inference performance per die, including the host server overhead for the two accelerators. Recall that architects use the geometric mean when they don’t know the actual mix of programs that will be run. For this comparison, however, we _do_ know the mix ([Figure 7.41](#_bookmark374)). The weighted mean in the last column of [Figure 7.49](#_bookmark383) using the actual mix makes the GPU up to 1.9 times, and the TPU is 29.2 times as fast as the CPU, so the TPU is 15.3 times as fast as the GPU.

> [图 7.49](#_bookmark383) 给出了每个模具相对推理性能的底线，包括两个加速器的主机服务器开销。回想一下，当工程师不知道将运行的程序的实际组合时，使用几何均值。但是，对于此比较，我们 *do* 知道混合物([图 7.41](#_bookmark374))。使用实际混合物的[图 7.49](图 7.49](#\_bookmark383)的最后一列中的加权平均值使 GPU 高达 1.9 倍，而 TPU 的速度是 CPU 的 29.2 倍，因此 TPU 的速度是 15.3 倍 GPU。

Figure 7.48 99th% response time and per die throughput (IPS) for MLP0 as batch size varies. The longest allowable latency is 7 ms. For the GPU and TPU, the maximum MLP0 throughput is limited by the host server overhead.

> 图 7.48 第 99％的响应时间和 MLP0 的每个死亡吞吐量(IP)随着批量大小而变化。最长的允许延迟是 7 ms。对于 GPU 和 TPU，最大 MLP0 吞吐量受主机服务器开销的限制。

Figure 7.49 K80 GPU and TPU performance relative to CPU for the DNN workload. The mean uses the actual mix of the six applications in [Figure 7.41](#_bookmark374). Relative performance for the GPU and TPU includes host server overhead. [Figure 7.48](#_bookmark382) corresponds to the second column of this table (MLP0), showing relative IPS that meet the 7-ms latency threshold.

> 图 7.49 K80 GPU 和 TPU 性能相对于 DNN 工作负载的 CPU。平均值使用[图 7.41]中的六个应用程序的实际组合(#\_bookmark374)。GPU 和 TPU 的相对性能包括主机服务器开销。[图 7.48](#_bookmark382) 对应于该表的第二列(MLP0)，显示了符合 7-MS 延迟阈值的相对 IP。

### Cost-Performance, TCO, and Performance/Watt

When buying computers by the thousands, cost-performance trumps general per- formance. The best cost metric in a data center is total cost of ownership (TCO). The actual price Google pays for thousands of chips depends on negotiations between the companies involved. For business reasons, Google can’t publish such price information or data that might let them be deduced. However, power is cor- related with TCO, and Google can publish watts per server, so we use performance/ watt as our proxy for performance/TCO. In this section, we compare servers ([Figure 7.43](#_bookmark377)) rather than single dies ([Figure 7.42](#_bookmark376)).

> 在数千台购买计算机时，成本绩效胜过一般性的表现。数据中心的最佳成本度量是总拥有成本(TCO)。Google 为成千上万的筹码支付的实际价格取决于相关公司之间的谈判。出于商业原因，Google 无法发布此类价格信息或可能允许它们推导的数据。但是，电源与 TCO 相关，Google 可以发布每台服务器的瓦特，因此我们将性能/瓦特用作性能/ TCO 的代理。在本节中，我们比较服务器([图 7.43](#_bookmark377))，而不是单个模具([图 7.42](#_bookmark376))。

[Figure 7.50](#_bookmark384) shows the weighted mean performance/watt for the K80 GPU and TPU relative to the Haswell CPU. We present two different calculations of perfor- mance/watt. The first ( "total" ) includes the power consumed by the host CPU server when calculating performance/watt for the GPU and TPU. The second ( "incremental" ) subtracts the host CPU server power from the total for the GPU and TPU beforehand.

> [图 7.50](#_bookmark384) 显示了 K80 GPU 和 TPU 相对于 Haswell CPU 的加权平均性能/瓦。我们提出了两种不同的表演/瓦特计算。在计算 GPU 和 TPU 的性能/瓦时，第一个( "总" )包括主机 CPU 服务器所消耗的功率。第二个( "增量" )事先从 GPU 和 TPU 的总数中减去主机 CPU 服务器功率。

Figure 7.50 Relative performance/watt of GPU and TPU servers to CPU or GPU servers. Total performance/watt includes host server power, but incremental doesn’t. It is a widely quoted metric, but we use it as a proxy for performance/TCO in the data center.

> 图 7.50 与 CPU 或 GPU 服务器的 GPU 和 TPU 服务器的相对性能/瓦。总体性能/瓦包括主机服务器功率，但增量不包括。它是一个广泛引用的指标，但我们将其用作数据中心中性能/TCO 的代理。

For total-performance/watt, the K80 server is 2.1 Haswell. For incremental- performance/watt, when Haswell power is omitted, the K80 server is 2.9 .

> 对于总体性能/瓦特，K80 服务器为 2.1 Haswell。对于增量性能/瓦特，当省略 Haswell Power 时，K80 服务器为 2.9。

The TPU server has 34 times better total-performance/watt than Haswell, which makes the TPU server 16 times the performance/watt of the K80 server. The relative incremental-performance/watt—which was Google’s justification for a custom ASIC—is 83 for the TPU, which lifts the TPU to 29 times the per- formance/watt of the GPU.

> TPU 服务器的总绩效/瓦比 Haswell 的 34 倍，这使 TPU 服务器的性能/瓦是 K80 服务器的 16 倍。TPU 的相对增量表现/瓦特(这是 Google 对定制 ASIC 的理由)为 83，它将 TPU 提升到 GPU 的效果/瓦的 29 倍。

### Evaluating Catapult and Pixel Visual Core

Catapult V1 runs CNNs 2.3 as fast as a 2.1 GHz, 16-core, dual-socket server ([Ovtcharov et al., 2015a](#_bookmark984)). Using the next generation of FPGAs (14-nm Arria 10), performance goes up 7 , and perhaps even 17 with more careful floorplan- ning and scaling up of the Processing Elements ([Ovtcharov et al., 2015b](#_bookmark985)). In both cases, Catapult increases power by less than 1.2 . Although it’s apples versus oranges, the TPU runs its CNNs 40 to 70 versus a somewhat faster server (see [Figures 7.42, 7.43](#_bookmark376), and [7.49](#_bookmark383)).

> Catapult V1 运行 CNNS 2.3 的速度与 2.1 GHz，16 核，双插槽服务器([Ovtcharov 等，2015a](#_bookmark984))。使用下一代 FPGA(14 nm Arria 10)，性能上升 7，甚至 17，甚至更仔细的平面图和扩展处理元素([Ovtcharov 等，2015b](#_kmark985))。在这两种情况下，弹射器将功率提高不到 1.2。尽管它是苹果与橙子，但 TPU 运行其 CNNS 40 至 70 对服务器的运行速度更快(请参见[图 7.42，7.43](#_bookmark376) 和[7.49]和 [7.49](#_bookmark383))。

Because Pixel Visual Core and the TPU are both made by Google, the good news is that we can directly compare performance for CNN1, which is a common DNN, although it had to be translated from TensorFlow. It runs with batch size of 1 instead of 32 as in the TPU. The TPU runs CNN1 about 50 times as fast as Pixel Visual Core, which makes Pixel Visual Core about half as fast as the GPU and a little faster than Haswell. Incremental performance/watt for CNN1 raises Pixel Visual Core to about half the TPU, 25 times the GPU, and 100 times the CPU.

> 由于 Pixel Visual Core 和 TPU 都是由 Google 制造的，所以好消息是我们可以直接比较 CNN1 的性能，这是常见的 DNN，尽管必须从 TensorFlow 翻译。它以 TPU 中的批量大小为 1 而不是 32。TPU 运行的 CNN1 的速度约为 Pixel Visual Core 的 50 倍，这使 Pixel Visual Core 的快速速度约为 GPU，并且比 Haswell 快一点。CNN1 的增量性能/瓦将像素视觉芯提高到 TPU 的一半，GPU 的 25 倍，CPU 的 100 倍。

Because the Intel Crest is designed for training rather than inference, it wouldn’t be fair to include it in this section, even if it were available to measure.

> 由于英特尔峰(Intel Crest)是为培训而不是推论而设计的，因此即使可以衡量它，也将其包括在本节中是不公平的。

In these early days of both DSAs and DNNs, fallacies abound.

> 在 DSA 和 DNN 的早期，谬论比比皆是。

Fallacy _It costs $100 million to design a custom chip_.

> 谬论\_IT 花费 1 亿美元来设计自定义芯片。

[Figure 7.51](#_bookmark386) shows a graph from an article that debunks the widely quoted $100- million myth that it was "only" $50 million, with most of the cost being salaries ([Olofsson, 2011](#_bookmark983)). Note that the author’s estimate is for sophisticated processors that include features that DSAs by definition omit, so even if there were no improvement to the development process, you would expect the cost of a DSA design to be less.

> [图 7.51](#_bookmark386) 显示了一篇文章中的一张图表，该图表揭露了被广泛报价的 10000 万美元的神话，即 "仅" 5000 万美元，大部分成本是薪水([Olofsson，2011](#_kmarksmark983)))))。请注意，作者的估计是针对复杂处理器的，其中包含 DSA 的定义省略的功能，因此，即使没有改进开发过程，您也希望 DSA 设计的成本会减少。

Why are we more optimistic six years later, when, if anything, mask costs are even higher for the smaller process technologies?

> 为什么六年后，我们更乐观的时候，如果有的话，较小的过程技术甚至更高的掩模成本更高？

First, software is the largest category, at almost a third of the cost. The avail- ability of applications written in domain-specific languages allows the compilers to do most of the work of porting the applications to your DSA, as we saw for the TPU and Pixel Visual Core. The open RISC-V instruction set will also help reduce the cost of getting system software as well as cut the large IP costs.

> 首先，软件是最大的类别，几乎是成本的三分之一。用特定于域的语言编写的应用程序的可用性使编译器可以完成将应用程序移植到您的 DSA 的大部分工作，就像我们为 TPU 和 Pixel Visual Core 所看到的那样。开放的 RISC-V 指令集还将有助于降低获得系统软件的成本，并降低大型 IP 成本。

Mask and fabrication costs can be saved by having multiple projects share a single reticle. As long as you have a small chip, amazingly enough, for $30,000 anyone can get 100 untested parts in 28-nm TSMC technology ([Patterson and Nikoli´](#_bookmark989)c, [2015](#_bookmark989)).

> 通过拥有多个项目共享一个标线，可以节省口罩和制造成本。只要您有一个很小的芯片，就足够了，对于 30,000 美元，任何人都可以在 28 nm TSMC 技术中获得 100 个未经测试的零件([[Patterson 和 Nikoli´](#_bookmark989)C，[2015](2015 年)(#\_bookmark989))。

![](../media/image464.png)HARDWARE,

Figure 7.51 The breakdown of the $50 million cost of a custom ASIC that came from surveying others ([Olofsson, 2011](#_bookmark983)). The author wrote that his company spent just $2 million for its ASIC.

> 图 7.51 从调查他人的定制 ASIC 的 5000 万美元成本([Olofsson，2011](#_bookmark983))的细分。作者写道，他的公司仅花了 200 万美元为其 ASIC 花费了 200 万美元。

Perhaps the biggest change is to hardware engineering, which is more than a quarter of the cost. Hardware engineers have begun to follow their software col- leagues to use agile development. The traditional hardware process not only has separate phases for design requirements, architecture, logical design, layout, ver- ification, and so on, but also it uses different job titles for the people who perform each of the phases. This process is heavy on planning, documentation, and sched- uling in part because of the change in personnel each phase.

> 也许最大的变化是硬件工程，这是成本的四分之一以上。硬件工程师已经开始遵循其软件集合以使用敏捷开发。传统的硬件过程不仅具有针对设计需求，架构，逻辑设计，布局，verification 等的单独阶段，而且还为执行每个阶段的人使用不同的工作标题。这个过程在计划，文档和计划方面很大，部分原因是每个阶段人员的变化。

Software used to follow this "waterfall" model as well, but projects were so com- monly late, over budget, and even canceled that it led to a radically different approach. The Agile Manifesto in 2001 basically said that it was much more likely that a small team that iterated on an incomplete but working prototype shown reg- ularly to customers would produce useful software on schedule and on budget more than the traditional plan-and-document approach of the waterfall process would.

> 软件也用于遵循这种 "瀑布" 模型，但是项目迟到了，预算超过预算，甚至取消了它导致了一种根本不同的方法。2001 年的敏捷宣言基本上说，在不完整但工作原型上迭代的小型团队更有可能与客户按计划制作有用的软件，而不是传统的计划和文档方法。瀑布过程将是。

Small hardware teams now do agile iterations ([Lee et al., 2016](#_bookmark977)). To ameliorate the long latency of a chip fabrication, engineers do some iterations using FPGAs because modern design systems can produce both the EDIF for FPGAs and chip layout from a single design. FPGA prototypes run 10–20 times slower than chips, but that is still much faster than simulators. They also do "tape-in" iterations, where you do all the work of a tape-out for your working but incomplete prototype, but you don’t pay the costs of fabricating a chip.

> 小型硬件团队现在进行敏捷迭代([Lee 等，2016](#_bookmark977))。为了改善芯片制造的长延迟，工程师使用 FPGA 进行了一些迭代，因为现代设计系统可以从单个设计中生产出用于 FPGA 的 EDIF 和芯片布局。FPGA 原型的运行速度比芯片慢 10-20 倍，但它仍然比模拟器快得多。他们还进行了 "磁带" 迭代，您可以在其中完成所有磁带的工作，但原型不完整，但是您不支付制造芯片的费用。

In addition to an improved development process, more modern hardware design languages to support them ([Bachrach et al., 2012](#_bookmark921)), and advances in automatic gen- eration of hardware from high-level domain-specific languages ([Canis et al., 2013;](#_bookmark932) [Huang et al., 2016; Prabhakar et al., 2016](#_bookmark932)). Open source cores that you can download for free and modify should also lower the cost of hardware design.

> 除了改进的开发过程之外，还有更多支持它们的现代硬件设计语言([Bachrach et al., 2012](#_bookmark921))，以及从高级领域特定语言自动生成硬件的进步([ Canis 等人，2013 年；](#_bookmark932) [Huang 等人，2016 年；Prabhakar 等人，2016 年](#_bookmark932))。您可以免费下载和修改的开源内核也应该会降低硬件设计成本。

Pitfall _Performance counters added as an afterthought for DSA hardware_.

> 陷阱* performance 计数器是 DSA 硬件*的事后思考。

The TPU has 106 performance counters, and the designers wanted even more (see [Figure 7.45](#_bookmark379)). The _raison d_’^_etre_ for _DSAs_ is performance, and it is way too early in their evolution to have a good idea about what is going on.

> TPU 有 106 个性能计数器，设计师想要更多(请参见[图 7.45](#_bookmark379))。对于 *dsas* 来说，_raison d _’^* etre*是性能，而他们的演变还为时过早，无法对正在发生的事情有一个好主意。

Fallacy _Architects are tackling the right DNN tasks_.

> 谬误 *architect 正在解决正确的 DNN 任务*。

The architecture community is paying attention to deep learning: 15% of the papers at ISCA 2016 were on hardware accelerators for DNNs! Alas, all nine papers looked at CNNs, and only two mentioned other DNNs. CNNs are more complex than MLPs and are prominent in DNN competitions ([Russakovsky et al., 2015](#_bookmark997)), which might explain their allure, but they are only about 5% of the Google data center NN workload. It seems wise try to accelerate MLPs and LSTMs with at least as much gusto.

> 架构社区正在关注深度学习：ISCA 2016 上 15% 的论文是关于 DNN 的硬件加速器！ 唉，九篇论文都研究了 CNN，只有两篇提到了其他 DNN。CNN 比 MLP 更复杂，并且在 DNN 竞赛中表现突出 ([Russakovsky et al., 2015](#_bookmark997))，这或许可以解释它们的吸引力，但它们仅占 Google 数据中心 NN 工作负载的 5% 左右。至少以同样的热情尝试加速 MLP 和 LSTM 似乎是明智的。

Fallacy _For DNN hardware, inferences per second (IPS) is a fair summary performance metric_.

> 谬误\_对于 DNN 硬件，每秒推断(IPS)是公平的摘要性能度量图。

IPS is not appropriate as a single, overall performance summary for DNN hardware because it’s simply the inverse of the complexity of the typical inference in the application (e.g., the number, size, and type of NN layers). For example, the

> IPS 不合适作为 DNN 硬件的单个总体性能摘要，因为它仅仅是应用程序中典型推断的复杂性的倒数(例如，NN 层的数字，大小和类型)。例如，

TPU runs the 4-layer MLP1 at 360,000 IPS but the 89-layer CNN1 at only 4700 IPS; thus TPU IPS varies by 75X! Therefore using IPS as the single-speed sum- mary is much more misleading for NN accelerators than MIPS or FLOPS is for traditional processors, so IPS should be even more disparaged. To compare DNN machines better, we need a benchmark suite written at a high level to port it to the wide variety of DNN architectures. Fathom is a promising new attempt at such a benchmark suite ([Adolf et al., 2016](#_bookmark916)).

> TPU 以 360,000 IPS 运行 4 层 MLP1，但仅为 4700 IP 的 89 层 CNN1；因此，TPU IP 的变化为 75 倍！因此，使用 IP 作为单速求发 - 对 NN 加速器的误导性比 MIPS 或 Flops 对传统处理器更具误导性，因此 IPS 应该更加贬低。为了更好地比较 DNN 机器，我们需要一个以高级写作的基准套件将其移植到各种 DNN 架构中。Fathom 是这种基准套件的有前途的新尝试([[Adolf 等，2016](#_bookmark916))。

Pitfall _Being ignorant of architecture history when designing a DSA_.

> 陷阱*设计 DSA* 时，对建筑历史记录一无所知。

Ideas that didn’t fly for general-purpose computing may be ideal for DSAs, thus history-aware architects could have a competitive edge. For the TPU, three impor- tant architectural features date back to the early 1980s: systolic arrays ([Kung and](#_bookmark973) [Leiserson, 1980](#_bookmark973)), decoupled-access/execute ([Smith, 1982b](#_bookmark1006)), and CISC instruc- tions ([Patterson and Ditzel, 1980](#_bookmark988)). The first reduced the area and power of the large Matrix Multiply Unit, the second fetched weights concurrently during operation of the Matrix Multiply Unit, and the third better utilized the limited bandwidth of the PCIe bus for delivering instructions. We advise mining the historical perspectives sections at the end of every chapter of this book to discover jewels that could embellish DSAs that you design.

> 不适用于通用计算的想法可能是 DSA 的理想选择，因此具有历史意识的架构师可能具有竞争优势。对于 TPU，三个重要的架构特征可以追溯到 80 年代初期：脉动数组([Kung 和](#_bookmark973)[Leiserson，1980](#_bookmark973))，解耦访问/执行([Smith，1982b](#_bookmark1006)) 和 CISC 指令 ([Patterson and Ditzel, 1980](#_bookmark988))。第一个减少了大型矩阵乘法单元的面积和功率，第二个在矩阵乘法单元的操作期间同时获取权重，第三个更好地利用 PCIe 总线的有限带宽来传递指令。我们建议挖掘本书每一章末尾的历史观点部分，以发现可以美化您设计的 DSA 的珠宝。
