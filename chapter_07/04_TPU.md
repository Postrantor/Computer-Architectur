## Google’s Tensor Processing Unit, an Inference Data Center Accelerator

The Tensor Processing Unit (_TPU_)[<sup>1</sup>](#_bookmark339) is Google’s first custom ASIC DSA for WSCs. Its domain is the inference phase of DNNs, and it is programmed using the Tensor- Flow framework, which was designed for DNNs. The first TPU was been deployed in Google data centers in 2015.

> 张量处理单元(_tpu _)[<sup> 1 </sup>](#_bookmark339)是 Google WSCS 的第一个自定义 ASIC DSA。它的域是 DNNS 的推理阶段，它是使用张量流框架进行编程的，该张量框架是为 DNN 设计的。第一个 TPU 于 2015 年在 Google 数据中心部署。

The heart of the TPU is a 65,536 (256 256) 8-bit ALU Matrix Multiply Unit and a large software-managed on-chip memory. The TPU’s single-threaded, deter- ministic execution model is a good match to the 99th-percentile response-time requirement of the typical DNN inference application.

> TPU 的心脏是 65,536(256 256)8 位 ALU 矩阵多重单元和大型软件管理的片上存储器。TPU 的单线程，确定的执行模型与典型的 DNN 推理应用程序的第 99 级响应时间要求非常匹配。

### TPU Origin

Starting as far back as 2006, Google engineers had discussions about deploying GPUs, FPGAs, or custom ASICs in their data centers. They concluded that the few applications that could run on special hardware could be done virtually for free using the excess capacity of the large data centers, and it’s hard to improve on free. The conversation changed in 2013 when it was projected that if people used voice search for three minutes a day using speech recognition DNNs, it would have required Google’s data centers to double in order to meet computation demands. That would be very expensive to satisfy with conventional CPUs. Google then started a high-priority project to quickly produce a custom ASIC for inference (and bought off-the-shelf GPUs for training). The goal was to improve cost- performance by 10 over GPUs. Given this mandate, the TPU was designed, ver- ified ([Steinberg, 2015](#_bookmark1009)), built, and deployed in data centers in just 15 months.

> 从 2006 年开始，Google 工程师就其数据中心部署 GPU，FPGA 或定制 ASIC 进行了讨论。他们得出的结论是，使用大数据中心的过剩容量实际上可以免费完成一些可以在特殊硬件上运行的应用程序，而且很难免费改进。对话在 2013 年发生了变化，当时人们预计，如果人们每天使用语音识别 DNN 使用语音搜索三分钟，它将要求 Google 的数据中心加倍以满足计算需求。满足常规 CPU 将非常昂贵。然后，Google 开始了一个高优先级项目，以快速生产定制的推理(并购买了现成的 GPU 进行培训)。目的是将成本绩效提高 10 比 GPU。鉴于此任务，TPU 的设计，即([Steinberg，2015](#_bookmark1009))，在短短 15 个月内构建并部署在数据中心中。

### TPU Architecture

To reduce the chances of delaying deployment, the TPU was designed to be a coprocessor on the PCIe I/O bus, which allows it to be plugged into existing servers. Moreover, to simplify hardware design and debugging, the host server sends instructions over the PCIe bus directly to the TPU for it to execute, rather than having the TPU fetch the instructions. Thus the TPU is closer in spirit to an FPU (floating-point unit) coprocessor than it is to a GPU, which fetches instruc- tions from its memory.

> 为了减少延迟部署的机会，TPU 被设计为 PCIE I/O 总线上的协处理器，可以将其插入现有服务器中。此外，为了简化硬件设计和调试，主机服务器将指令直接发送到 TPU，以执行其执行，而不是让 TPU 获取指令。因此，TPU 在精神上与 FPU(浮点单元)的协处理器更接近，而不是与 GPU，该 GPU 从其内存中获取了指导。

[Figure 7.12](#_bookmark340) shows the block diagram of the TPU. The host CPU sends TPU instructions over the PCIe bus into an instruction buffer. The internal blocks are typically connected together by 256-byte-wide (2048-bits) paths. Starting in the upper-right corner, the _Matrix Multiply Unit_ is the heart of the TPU. It contains of which one of your book authors was a coauthor.

> [图 7.12](#_bookmark340) 显示了 TPU 的框图。主机 CPU 将 PCIE 总线上的 TPU 指令发送到指令缓冲区。内部块通常通过 256 字节(2048 位)路径连接在一起。从右上角开始，*matrix 乘数单位*是 TPU 的核心。它包含您的一位书籍作者是合着者。

This section is based on the paper "In-Datacenter Performance Analysis of a Tensor Processing Unit" [Jouppi et al., 2017](#_bookmark966), 256 256 ALUs that can perform 8-bit multiply-and-adds on signed or unsigned integers. The 16-bit products are collected in the 4 MiB of 32-bit _Accumulators_ below the matrix unit. When using a mix of 8-bit weights and 16-bit activations (or vice versa), the Matrix Unit computes at half-speed, and it computes at a quarter-speed when both are 16 bits. It reads and writes 256 values per clock cycle and can perform either a matrix multiply or a convolution. The nonlinear functions are calculated by the _Activation_ hardware.

> 本节基于论文 "张量处理单元的成分性能分析" [Jouppi 等，2017](#_bookmark966)，256 256 Alus，可以在签名或无签名上执行 8 位乘以乘以 8 位的 Alus 整数。16 位产品收集在矩阵单元下方的 32 位 *accumulators* 的 4 个 MIB 中。当使用 8 位权重和 16 位激活(反之亦然)时，矩阵单元以半速计算，并且在两者都是 16 位时，它以四分之一速计算。它读取并写入每个时钟周期 256 个值，并且可以执行矩阵乘法或卷积。非线性函数由 *activation* 硬件计算。

![](../media/image395.png)

Figure 7.12 TPU Block Diagram. The PCIe bus is Gen3 16. The main computation part is the light-shaded Matrix Multiply Unit in the upper-right corner. Its inputs are the medium-shaded Weight FIFO and the medium-shaded Uni- fied Buffer and its output is the medium-shaded Accumulators. The light-shaded Activation Unit performs the non- linear functions on the Accumulators, which go to the Unified Buffer.

> 图 7.12 TPU 框图。PCIE 总线是 Gen3 16.主要计算部分是右上角的浅色矩阵乘单元。它的输入是中型的重量 FIFO 和中型的 Uniped Buffer，其输出是中型蓄能器。浅色激活单元在累加器上执行非线性函数，该功能转到统一缓冲液。

The weights for the matrix unit are staged through an on-chip _Weight FIFO_ that reads from an off-chip 8 GiB DRAM called _Weight Memory_ (for inference, weights are read-only; 8 GiB supports many simultaneously active models). The intermediate results are held in the 24 MiB on-chip _Unified Buffer_, which can serve as inputs to the Matrix Multiply Unit. A programmable DMA controller transfers data to or from CPU Host memory and the Unified Buffer.

> 矩阵单元的权重通过芯片*的 fifo* 进行了上演，该芯片从芯片 8 GIB DRAM 中读取，称为* Weakeight Memory*(为了推断，权重仅阅读；8 GIB 支持许多同时活跃的模型)。中间结果保存在 24 个 MIB 芯片上 *unified Buffer* 中，可以用作矩阵乘单元的输入。可编程 DMA 控制器将数据传输到或从 CPU 主机内存和统一缓冲区传输。

### TPU Instruction Set Architecture

As instructions are sent over the relatively slow PCIe bus, TPU instructions follow the CISC tradition, including a repeat field. The TPU does not have a program counter, and it has no branch instructions; instructions are sent from the host CPU. The clock cycles per instruction (CPI) of these CISC instructions are typi- cally 10–20. It has about a dozen instructions overall, but these five are the key ones:

> 随着指示在相对较慢的 PCIE 总线上发送，TPU 说明遵循 CISC 传统，包括重复字段。TPU 没有程序计数器，也没有分支说明；说明是从主机 CPU 发送的。这些 CISC 说明的每指令(CPI)的时钟周期典型为 10–20。它总体上有大约十二个说明，但这五个是关键：

1. Read_Host_Memory reads data from the CPU host memory into the Unified Buffer.
2. Read_Weights reads weights from Weight Memory into the Weight FIFO as input to the Matrix Unit.
3. MatrixMultiply/Convolve causes the Matrix Multiply Unit to perform a matrix-matrix multiply, a vector-matrix multiply, an element-wise matrix multi- ply, an element-wise vector multiply, or a convolution from the Unified Buffer into the Accumulators. A matrix operation takes a variable-sized B\*256 input, multiplies it by a 256 256 constant input, and produces a B\*256 output, taking B pipelined cycles to complete. For example, if the input were 4 vectors of 256 elements, B would be 4, so it would take 4 clock cycles to complete.
4. Activate performs the nonlinear function of the artificial neuron, with options for ReLU, Sigmoid, tanh, and so on. Its inputs are the Accumulators, and its output is the Unified Buffer.
5. Write_Host_Memory writes data from the Unified Buffer into the CPU host memory.

> 1. read_host_memory 将来自 CPU 主机内存的数据读取到统一的缓冲区中。
> 2. read_weaights 将重量从重量存储器读取为矩阵单元的输入。
> 3. 矩阵 multiply/卷曲会导致矩阵倍数单元执行矩阵矩阵倍数，矢量矩阵乘数，元素矩阵多层，元素 - 元素矢量乘数或从统一的缓冲液中的卷积，。矩阵操作采用可变大小的 B \*256 输入，将其乘以 256 256 常数输入，并产生 A B \*256 输出，将 B 管道循环完成。例如，如果输入是 256 个元素的 4 个向量，则 B 为 4 个，因此需要 4 个时钟周期才能完成。
> 4. 激活执行人造神经元的非线性功能，并具有 relu，sigmoid，tanh 等的选项。它的输入是累加器，其输出是统一的缓冲区。
> 5. write_host_memory 将统一缓冲区的数据写入 CPU 主机内存。

The other instructions are alternate host memory read/write, set configuration, two versions of synchronization, interrupt host, debug-tag, nop, and halt. The CISC MatrixMultiply instruction is 12 bytes, of which 3 are Unified Buffer address; 2 are accumulator address; 4 are length (sometimes 2 dimensions for convolutions); and the rest are opcode and flags.

> 其他说明是替代主机存储器读/写，设置配置，两个版本的同步，中断主机，调试标签，NOP 和 Halt。CISC MatrixMultiply 指令为 12 个字节，其中 3 个是统一的缓冲区地址；2 是累加器地址；4 是长度(有时是卷积的 2 个维度)；其余的是 OpCode 和标志。

The goal is to run whole inference models in the TPU to reduce interactions with the host CPU and to be flexible enough to match the DNN needs of 2015 and beyond, instead of just what was required for 2013 DNNs.

> 目的是运行 TPU 中的整个推理模型，以减少与主机 CPU 的交互作用，并具有足够的灵活性，以符合 2015 年及以后的 DNN 需求，而不仅仅是 2013 年 DNN 所需的内容。

### TPU Microarchitecture

The microarchitecture philosophy of the TPU is to keep the Matrix Multiply Unit busy. The plan is to hide the execution of the other instructions by overlapping their execution with the MatrixMultiply instruction. Thus each of the preceding four general categories of instructions have separate execution hardware (with read and write host memory combined into the same unit). To increase instruction par- allelism further, the Read_Weights instruction follows the decoupled access/ execute philosophy ([Smith, 1982b](#_bookmark1006)) in that they can complete after sending their addresses but before the weights are fetched from Weight Memory. The matrix unit has not-ready signals from the Unified Buffer and the Weight FIFO that will cause the matrix unit to stall if their data are not yet available.

> TPU 的微体系结构理念是使矩阵倍数繁忙。该计划是通过将其执行与 matrixmultiply 指令重叠来隐藏其他指令的执行。因此，前面的四个指令类别中的每个指令中的每个类别都具有单独的执行硬件(将主机存储器组合到同一单元中)。为了进一步提高教学范围，read_weaights 指令遵循脱钩的访问/执行理念([[Smith，1982b](#_bookmark1006))，因为它们在发送地址后可以完成，但在重量从体重内存中获取之前可以完成。矩阵单元尚未准备好来自统一缓冲区的信号，如果尚不可用的数据，则会导致矩阵单元停滞不前的 FIFO。

Note that a TPU instruction can execute for many clock cycles, unlike the traditional RISC pipeline with one clock cycle per stage.

> 请注意，与传统的 RISC 管道不同，TPU 指令可以执行许多时钟周期，每个阶段一个时钟周期。

Because reading a large SRAM is much more expensive than arithmetic, the Matrix Multiply Unit uses systolic execution to save energy by reducing reads and writes of the Unified Buffer ([Kung and Leiserson, 1980;](#_bookmark973) [Ramacher et al., 1991; Ovtcharov et al., 2015b](#_bookmark973)). A _systolic array_ is a two- dimensional collection of arithmetic units that each independently compute a partial result as a function of inputs from other arithmetic units that are con- sidered upstream to each unit. It relies on data from different directions arriv- ing at cells in an array at regular intervals where they are combined. Because the data flows through the array as an advancing wave front, it is similar to blood being pumped through the human circulatory system by the heart, which is the origin of the systolic name.

> 由于阅读大 SRAM 比算术要贵得多，因此矩阵多重单元使用收缩期执行来通过减少统一缓冲液的读数和写入来节省能量([[Kung and Leisoserson，1980;](#_bookmark973)[Ramacher 等，1991; Ovtcharov 等人，2015b](#_bookmark973))。*节奏阵列*是算术单元的两维集合，每个算术单位都独立计算部分结果，这是来自其他算术单元的输入的函数，这些单元在每个单元上游都定位。它依靠来自不同方向的数据以阵列的定期间隔在阵列中到达。由于数据作为前进的波阵线流过阵列，因此类似于心脏通过人类循环系统泵送的血液，这是收缩期名称的起源。

[Figure 7.13](#_bookmark341) demonstrates how a systolic array works. The six circles at the bot- tom are the multiply-accumulate units that are initialized with the weights _wi_. The staggered input data _xi_ are shown coming into the array from above. The 10 steps of the figure represent 10 clock cycles moving down from top to bottom of the page. The systolic array passes the inputs down and the products and sums to the right. The desired sum of products emerges as the data completes its path through the systolic array. Note that in a systolic array, the input data is read only once from memory, and the output data is written only once to memory.

> [图 7.13](#_bookmark341) 演示了收缩期数组的工作原理。bottom 的六个圆圈是用权重 *WI* 初始化的多元蓄电单元。交错的输入数据 *xi* 显示从上方进入数组。图的 10 个步骤表示 10 个时钟周期从页面的顶部向下移动。收缩期阵列将输入传递给右侧的输入和产品和总和。随着数据完成通过收缩期阵列的路径，所需的产品总和会出现。请注意，在收缩期数组中，输入数据仅在内存中读取一次，并且输出数据仅写入一次存储器。

In the TPU, the systolic array is rotated. [Figure 7.14](#_bookmark342) shows that the weights are loaded from the top and the input data flows into the array in from the left. A given 256-element multiply-accumulate operation moves through the matrix as a diag- onal wave front. The weights are preloaded and take effect with the advancing wave alongside the first data of a new block. Control and data are pipelined to give the illusion that the 256 inputs are read at once, and after a feed delay, they update one location of each of 256 accumulator memories. From a correctness perspec- tive, software is unaware of the systolic nature of the matrix unit, but for perfor- mance, it does worry about the latency of the unit.

> 在 TPU 中，收缩期阵列旋转。[图 7.14](#_bookmark342) 表明，权重是从顶部加载的，输入数据从左侧加入数组。给定的 256 个元素的多重蓄能操作以戴上乳腺波的前端移动。重量已预加载，并与新块的第一个数据一起生效。控制和数据被管道说明，以使 256 个输入立即读取，然后在提要延迟后，它们更新了 256 个累加器记忆中的每个位置。从正确的角度来看，软件不知道矩阵单元的收缩性质，但是对于表现来说，它确实担心该单元的延迟。

### TPU Implementation

The TPU chip was fabricated using the 28-nm process. The clock rate is 700 MHz. [Figure 7.15](#_bookmark343) shows the floor plan of the TPU. Although the exact die size is not revealed, it is less than half the size of an Intel Haswell server microprocessor, which is 662 mm<sup>2</sup>.

> 使用 28 nm 工艺制造了 TPU 芯片。时钟速率为 700 MHz。[图 7.15](#_bookmark343) 显示了 TPU 的平面图。尽管没有透露确切的模具大小，但它的尺寸不到 Intel Haswell Server 微处理器的一半，即 662 mm <sup> 2 </sup>。

The 24 MiB Unified Buffer is almost a third of the die, and the Matrix Multiply Unit is a quarter, so the datapath is nearly two-thirds of the die. The 24 MiB size was picked in part to match the pitch of the Matrix Unit on the die and, given the

> 24 个 MIB 统一的缓冲区几乎是模具的三分之一，并且矩阵乘单元为四分之一，因此数据结构几乎是模具的三分之二。挑选了 24 个 MIB 尺寸，以部分与模板单元的音调相匹配，并给予

![](../media/image398.png)
![](../media/image399.png)
![](../media/image400.png)**y = w**
![](../media/image402.png)**y2 = w21x1 + w22x2 + w23x3**

Figure 7.13 Example of systolic array in action, from top to bottom on the page. In this example, the six weights are already inside the multiply-accumulate units, as is the norm for the TPU. The three inputs are staggered in time to get the desired effect, and in this example are shown coming in from the top. (In the TPU, the data actually comes in from the left.) The array passes the data down to the next element and the result of the computation to the right to the next element. At the end of the process, the sum of products is found to the right. Drawings courtesy of Yaz Sato.

> 图 7.13 从页面上的顶部到底部的收缩期数组的示例。在此示例中，六个权重已经在多元蓄电单元内，就像 TPU 的规范一样。这三个输入及时交错以获得所需的效果，在此示例中，显示了从顶部出现。(在 TPU 中，数据实际上来自左侧。)数组将数据传递给下一个元素，以及计算结果向右传递到下一个元素。在过程结束时，可以找到产品的总和。图纸由 Yaz Sato 提供。

Figure 7.14 Systolic data flow of the Matrix Multiply Unit.

Figure 7.15 Floor plan of TPU die. The shading follows [Figure 7.14](#_bookmark342). The light data buffers are 37%, the light computation units are 30%, the medium I/O is 10%, and the dark control is just 2% of the die. Control is much larger (and much more difficult to design) in a CPU or GPU. The unused white space is a consequence of the emphasis on time to tape-out for the TPU.

> 图 7.15 TPU 死亡的平面图。阴影以下[图 7.14](#_bookmark342)。光数据缓冲区为 37％，光计算单元为 30％，介质 I/O 为 10％，黑暗控制仅占模具的 2％。在 CPU 或 GPU 中，控制更大(更难设计)。未使用的空白是强调时间弹出 TPU 的结果的结果。

<img src="../media/image403.jpeg" style="width:4.33534in;height:2.575in" />
Figure 7.16 TPU printed circuit board. It can be inserted into the slot for an SATA disk in a server, but the card uses the PCIe bus.

short development schedule, in part to simplify the compiler. Control is just 2%. [Figure 7.16](#_bookmark344) shows the TPU on its printed circuit card, which inserts into existing servers in a SATA disk slot.

> 简短的开发时间表，部分是为了简化编译器。控制只有 2％。[图 7.16](#_bookmark344) 在其印刷电路卡上显示了 TPU，该电路卡在 SATA 磁盘插槽中插入现有服务器。

### TPU Software

The TPU software stack had to be compatible with that developed for CPUs and GPUs so that applications could be ported quickly. The portion of the application run on the TPU is typically written using TensorFlow and is compiled into an API that can run on GPUs or TPUs ([Larabel, 2016](#_bookmark975)). [Figure 7.17](#_bookmark345) shows TensorFlow code for a portion of an MLP.

> TPU 软件堆栈必须与为 CPU 和 GPU 开发的堆栈兼容，以便可以快速移植应用程序。该应用程序运行在 TPU 上的部分通常使用 TensorFlow 编写，并将其编译为可以在 GPU 或 TPU 上运行的 API([[Larabel，2016](#_bookmark975))。[图 7.17](#_bookmark345) 显示了 MLP 一部分的 TensorFlow 代码。

Like GPUs, the TPU stack is split into a User Space Driver and a Kernel Driver. The Kernel Driver is lightweight and handles only memory management and interrupts. It is designed for long-term stability. The User Space driver changes frequently. It sets up and controls TPU execution, reformats data into TPU order, and translates API calls into TPU instructions and turns them into an application binary. The User Space driver compiles a model the first time it is evaluated, caching the program image and writing the weight image into the TPU Weight Memory; the second and following evaluations run at full speed. The TPU runs most models completely from inputs to outputs, maximizing the ratio of TPU compute time to I/O time. Computation is often done one layer at a time, with overlapped execution allowing the matrix unit to hide most noncritical path operations.

> 像 GPU 一样，TPU 堆栈分为用户空间驱动程序和内核驱动程序。内核驱动程序是轻量级的，仅处理内存管理和中断。它是为长期稳定而设计的。用户空间驱动程序经常更改。它设置并控制 TPU 执行，将数据重新格式化为 TPU 顺序，并将 API 调用转换为 TPU 指令，并将其转换为应用程序二进制。用户空间驱动程序首次评估模型，缓存程序图像并将重量图像写入 TPU 重量存储器；第二和以下评估以全速运行。TPU 完全从输入到输出完全运行大多数模型，从而最大化 TPU 计算时间与 I/O 时间的比率。计算通常一次进行一层，重叠的执行允许矩阵单元隐藏大多数非关键路径操作。

Figure 7.17 Portion of the TensorFlow program for the MNIST MLP. It has two hidden 256 256 layers, with each layer using a ReLU as its nonlinear function.

### Improving the TPU

The TPU architects looked at variations of the microarchitecture to see whether they could have improved the TPU.

> TPU 架构师着眼于微体系结构的变化，以查看它们是否可以改善 TPU。

Like an FPU, the TPU coprocessor has a relatively easy microarchitecture to evaluate, so the TPU architects created a performance model and estimated performance as the memory bandwidth, the matrix unit size, and the clock rate and number of accumulators varied. Measurements using TPU hardware coun- ters found that the modeled performance was on average within 8% of the hardware.

> 像 FPU 一样，TPU 协处理器具有相对简单的微体系结构，因此 TPU Architects 创建了性能模型和估计的性能，作为存储器带宽，矩阵单位大小以及时钟速率和累加器数量。使用 TPU 硬件顾问的测量结果发现，建模的性能平均在硬件的 8％之内。

![](../media/image404.png)

Figure 7.18 Performance as metrics scale from 0.25 × to 4 ×: memory bandwidth, clock rate+accumulators, clock rate, matrix unit dimension +accumulators, and one dimension of the square matrix unit This is the average performance calculated from six DNN applications in [Section 7.9](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators). The CNNs tend to be computation-bound, but the MLPs and LSTMs are memory-bound. Most applications benefit from a faster mem- ory, but a faster clock makes little difference, and a bigger matrix unit actually hurts per- formance. This performance model is only for code running inside the TPU and does not factor in the CPU host overhead.

> 图 7.18 作为指标量表的性能从 0.25× 至 4×：内存带宽，时钟速率 + 累加器，时钟速率，矩阵单位尺寸 + 累加器和方形矩阵单元的一个维第 7.9 节](＃putting-it-it-all-together-cpus-vers-vpus-vers-versus-dnn-accelerators)。CNN 倾向于限制计算，但 MLP 和 LSTM 是内存的。大多数应用程序都受益于更快的 mem-fory，但是更快的时钟几乎没有什么不同，较大的矩阵单元实际上会受到影响。该性能模型仅用于在 TPU 内部运行的代码，并且不考虑 CPU 主机开销。

[Figure 7.18](#_bookmark346) shows the performance sensitivity of the TPU as these parameters scale over the range for 0.25 to 4 . ([Section 7.9](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators) lists the benchmarks used.) In addition to evaluating the impact of only raising clock rates (_clock_ in [Figure 7.18](#_bookmark346)), [Figure 7.18](#_bookmark346) also plots a design (_clock_+) that increases the clock rate and scales the number of accumulators correspondingly so that the compiler can keep more mem- ory references in flight. Likewise, [Figure 7.18](#_bookmark346) plots matrix unit expansion if the number of accumulators increase with the square of the rise in one dimension (_matrix_+), because the matrix grows in both dimensions, as well as only increasing the matrix unit (_matrix_).

> [图 7.18](#_bookmark346) 显示了 TPU 的性能敏感性，因为这些参数在 0.25 到 4 的范围内缩放。([第 7.9 节](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators) 列出了使用的基准。)除了评估仅提高时钟速率的影响([图 7.18 中的*clock* ](#_bookmark346))，[图 7.18](#_bookmark346) 还绘制了一个设计 (_clock_+)，它增加了时钟频率并相应地缩放了累加器的数量，以便编译器可以在运行中保留更多的内存引用。同样，[图 7.18](#_bookmark346) 绘制了如果累加器的数量随着一维增长的平方 (_matrix_+) 增加而增加的矩阵单位扩展，因为矩阵在两个维度上都增长，并且仅增加矩阵单位 (_矩阵_)。

First, increasing memory bandwidth (_memory_) has the biggest impact: perfor- mance improves 3 on average when memory bandwidth increases 4 , because it reduces the time waiting for weight memory. Second, clock rate has little benefit on average with or without more accumulators. Third, the average performance in [Figure 7.18](#_bookmark346) slightly _degrades_ when the matrix unit expands from 256 256 to 512 512 for all applications, whether or not they get more accumulators. The issue is analogous to internal fragmentation of large pages, only worse because it’s in two dimensions.

> 首先，增加内存带宽 (_memory_) 的影响最大：当内存带宽增加 4 时，性能平均提高 3 ，因为它减少了等待权重内存的时间。其次，不管有没有更多的累加器，时钟速率平均都没有什么好处。第三，当所有应用程序的矩阵单元从 256 256 扩展到 512 512 时，[图 7.18](#_bookmark346) 中的平均性能略有 *degrades*，无论它们是否获得更多累加器。这个问题类似于大页面的内部碎片，只是更糟，因为它是二维的。

Consider the 600 600 matrix used in LSTM1. With a 256 256 matrix unit, it takes nine steps to tile 600 600, for a total of 18 μs of time. The larger 512 512 unit requires only four steps, but each step takes four times longer, or 32 μs of time. The TPU’s CISC instructions are long, so decode is insignificant and does not hide the overhead of loading from the DRAM.

> 考虑 LSTM1 中使用的 600 600 矩阵。使用 256 256 矩阵单元，总共需要九个步骤才能达到 600 600 瓷砖 600。较大的 512 512 单元仅需要四个步骤，但每个步骤需要更长的时间或 32μs 的时间。TPU 的 CISC 说明很长，因此解码很小，并且不会隐藏 DRAM 的负载开销。

Given these insights from the performance model, the TPU architects next evaluated an alternative and hypothetical TPU that they might have designed in the same process technology if they’d had more than 15 months to do so. More aggressive logic synthesis and block design might have increased the clock rate by 50%. The architects found that designing an interface circuit for GDDR5 memory, as used by the K80, would improve Weight Memory bandwidth by more than a factor of five. As [Figure 7.18](#_bookmark346) shows, increasing clock rate to 1050 MHz, but not helping memory, made almost no change in performance. If the clock is left at 700 MHz, but it uses GDDR5 instead for Weight Memory, performance is increased by 3.2 , even accounting for the host CPU overhead of invoking the DNN on the revised TPU. Doing both does not improve average performance further.

> 鉴于性能模型的这些见解，TPU 架构师接下来评估了一种替代和假设的 TPU，如果他们有超过 15 个月的时间，他们可能会使用相同的工艺技术设计这种 TPU。更积极的逻辑综合和模块设计可能会将时钟速率提高 50%。架构师发现，为 K80 使用的 GDDR5 内存设计接口电路可以将权重内存带宽提高五倍以上。正如 [图 7.18](#_bookmark346) 所示，将时钟速率提高到 1050 MHz，但对内存没有帮助，性能几乎没有变化。如果时钟保持在 700 MHz，但它使用 GDDR5 代替权重内存，则性能提高 3.2，甚至考虑了在修改后的 TPU 上调用 DNN 的主机 CPU 开销。两者都做并不能进一步提高平均性能。

### Summary: How TPU Follows the Guidelines

Despite living on an I/O bus and having relatively little memory bandwidth that limits full utilization of the TPU, a small fraction of a big number can, nonetheless, be relatively large. As we will see in [Section 7.9](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators), the TPU delivered on its goal of a tenfold improvement in cost-performance over the GPU when running DNN infer- ence applications. Moreover, a redesigned TPU with the only change being a switch to the same memory technology as in the GPU would be three times faster.

> 尽管存在于 I/O 总线上并且内存带宽相对较小，这限制了 TPU 的充分利用，但大数字中的一小部分仍然可以相对较大。正如我们将在 [第 7.9 节](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators)中看到的那样，TPU 实现了其成本性能比 GPU 提高十倍的目标 运行 DNN 推理应用程序。此外，经过重新设计的 TPU 唯一的变化是切换到与 GPU 中相同的内存技术，速度将快三倍。

One way to explain the TPU’s success is to see how it followed the guidelines in [Section 7.2](#guidelines-for-dsas).

> 解释 TPU 成功的一种方法是查看它如何遵循[7.2 节](＃guideLines for-dsas)中的准则。

1. _Use dedicated memories to minimize the distance over which data is moved_. The TPU has the 24 MiB Unified Buffer that holds the intermediate matrices and vectors of MLPs and LSTMs and the feature maps of CNNs. It is optimized for accesses of 256 bytes at a time. It also has the 4 MiB Accumulators, each 32-bits wide, that collect the output of the Matrix Unit and act as input to the hardware that calculates the nonlinear functions. The 8-bit weights are stored in a separate off-chip weight memory DRAM and are accessed via an on-chip weight FIFO. In contrast, all these types and sizes of data would exist in redundant copies at sev- eral levels of the inclusive memory hierarchy of a general-purpose CPU.

> 1. 1._使用专用存储器来最小化数据移动的距离_。TPU 具有 24 MiB 统一缓冲区，用于保存 MLP 和 LSTM 的中间矩阵和向量以及 CNN 的特征图。它针对一次访问 256 个字节进行了优化。它还具有 4 个 MiB 累加器，每个 32 位宽，用于收集矩阵单元的输出并作为计算非线性函数的硬件的输入。8 位权重存储在单独的片外权重内存 DRAM 中，并通过片上权重 FIFO 访问。相比之下，所有这些类型和大小的数据都将存在于通用 CPU 的包容性内存层次结构的多个级别的冗余副本中。

2. _Invest the resources saved from dropping advanced microarchitectural optimi- zations into more arithmetic units or bigger memories_. The TPU offers 28 MiB of dedicated memory and 65,536 8-bit ALUs, which means it has about 60% of the memory and 250 times as many ALUs as a server-class CPU, despite being half its size and power (see [Section 7.9](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators)). Com- pared to a server-class GPU, the TPU has 3.5 times the on-chip memory and 25 times as many ALUs.

> 2._将高级微架构优化节省下来的资源投资到更多的算术单元或更大的内存中_。TPU 提供 28 MiB 专用内存和 65,536 个 8 位 ALU，这意味着它拥有大约 60% 的内存和 250 倍于服务器级 CPU 的 ALU，尽管它的大小和功率只有它的一半(参见 [第 7.9 节) ](#putting-it-all-together-cpus-versus-gpus-versus-dnn-accelerators)。与服务器级 GPU 相比，TPU 具有 3.5 倍的片上内存和 25 倍的 ALU。

3. _Use the easiest form of parallelism that matches the domain_. The TPU delivers its performance via a two-dimensional SIMD parallelism with its 256 256 Matrix Multiply Unit, which is internally pipelined with a systolic organization, plus a simple overlapped execution pipeline of its instructions. GPUs rely instead on multiprocessing, multithreading, and one- dimensional SIMD, and CPUs rely on multiprocessing, out-of-order execution, and one-dimensional SIMD.

> 3. 3._使用与领域匹配的最简单的并行形式_。TPU 通过其 256 256 矩阵乘法单元的二维 SIMD 并行性提供其性能，该单元在内部流水线化收缩组织，外加其指令的简单重叠执行流水线。GPU 依赖多处理、多线程和一维 SIMD，而 CPU 依赖多处理、乱序执行和一维 SIMD。

4. _Reduce data size and type to the simplest needed for the domain_. The TPU computes primarily on 8-bit integers, although it supports 16-bit integers and accumulates in 32-bit integers. CPUs and GPUs also support 64-bit integers and 32-bit and 64-bit floating point.

> 4. 4._将数据大小和类型减少到域所需的最简单_。TPU 主要计算 8 位整数，但它支持 16 位整数并以 32 位整数累加。CPU 和 GPU 还支持 64 位整数以及 32 位和 64 位浮点数。

5. _Use a domain-specific programming language to port code to the DSA_. The TPU is programmed using the TensorFlow programming framework, whereas GPUs rely on CUDA and OpenCL and CPUs must run virtually everything.

> 5. 5._使用特定领域的编程语言将代码移植到 DSA_。TPU 使用 TensorFlow 编程框架进行编程，而 GPU 依赖于 CUDA 和 OpenCL，而 CPU 必须运行几乎所有的东西。
