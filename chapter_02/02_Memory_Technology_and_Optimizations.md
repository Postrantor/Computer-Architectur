## Memory Technology and Optimizations

> ##内存技术和优化

…the one single development that put computers on their feet was the invention of a reliable form of memory, namely, the core memory. …Its cost was reasonable, it was reliable and, because it was reliable, it could in due course be made large. (p. 209)

> …将计算机站起来的一个单一开发是一种可靠的内存形式的发明，即核心记忆。…它的成本是合理的，它是可靠的，并且由于它是可靠的，因此可以使其变得大。(第 209 页)

Maurice Wilkes.
_Memoirs of a Computer Pioneer_ (1985)

This section describes the technologies used in a memory hierarchy, specifically in building caches and main memory. These technologies are SRAM (static random- access memory), DRAM (dynamic random-access memory), and Flash. The last of these is used as an alternative to hard disks, but because its characteristics are based on semiconductor technology, it is appropriate to include in this section.

> 本节介绍了内存层次结构中使用的技术，特别是用于构建缓存和主内存的技术。这些技术是 SRAM(静态随机访问存储器)，DRAM(动态随机访问存储器)和 Flash。其中的最后一个被用作硬盘的替代方法，但是由于其特性基于半导体技术，因此在本节中包括。

Using SRAM addresses the need to minimize access time to caches. When a cache miss occurs, however, we need to move the data from the main memory as quickly as possible, which requires a high bandwidth memory. This high memory bandwidth can be achieved by organizing the many DRAM chips that make up the main memory into multiple memory banks and by making the memory bus wider, or by doing both.

> 使用 SRAM 解决需要最大程度地减少访问时间缓存的需求。但是，当缓存失误发生时，我们需要尽快将数据从主内存中移动，这需要高带宽内存。可以通过组织许多将主内存构成多个内存库，使内存总线更宽或两者同时进行的 DRAM 芯片来实现这种高内存带宽。

To allow memory systems to keep up with the bandwidth demands of modern processors, memory innovations started happening inside the DRAM chips themselves. This section describes the technology inside the memory chips and those innovative, internal organizations. Before describing the technologies and options, we need to introduce some terminology.

> 为了使内存系统能够跟上现代处理器的带宽需求，内存创新开始发生在 DRAM 芯片本身内。本节描述了内存芯片内部的技术和那些创新的内部组织。在描述技术和选项之前，我们需要引入一些术语。

With the introduction of burst transfer memories, now widely used in both Flash and DRAM, memory latency is quoted using two measures—access time and cycle time. _Access time_ is the time between when a read is requested and when the desired word arrives, and _cycle time_ is the minimum time between unrelated requests to memory.

> 随着爆发转移记忆的引入，现在已广泛用于闪光灯和 DRAM，记忆延迟是使用两种措施(访问时间和周期时间)引用的。*access 时间*是读取读取和所需单词到达时的时间，而 *cycle Time* 是无关请求到内存之间的最小时间。

Virtually all computers since 1975 have used DRAMs for main memory and SRAMs for cache, with one to three levels integrated onto the processor chip with the CPU. PMDs must balance power and performance, and because they have more modest storage needs, PMDs use Flash rather than disk drives, a decision increasingly being followed by desktop computers as well.

> 自 1975 年以来，几乎所有计算机都将 DRAM 用于主内存，而 SRAM 用于缓存，其中一到三个级别与 CPU 集成到处理器芯片上。PMD 必须平衡功率和性能，并且由于它们有更多适度的存储需求，因此 PMD 使用 Flash 而不是磁盘驱动器，台式计算机也越来越遵循这一决定。

### SRAM Technology

> ### SRAM 技术

The first letter of SRAM stands for _static_. The dynamic nature of the circuits in DRAM requires data to be written back after being read—thus the difference between the access time and the cycle time as well as the need to refresh. SRAMs don’t need to refresh, so the access time is very close to the cycle time. SRAMs typically use six transistors per bit to prevent the information from being disturbed when read. SRAM needs only minimal power to retain the charge in standby mode. In earlier times, most desktop and server systems used SRAM chips for their primary, secondary, or tertiary caches. Today, all three levels of caches are inte- grated onto the processor chip. In high-end server chips, there may be as many as 24 cores and up to 60 MiB of cache; such systems are often configured with 128–256 GiB of DRAM per processor chip. The access times for large, third-level, on-chip caches are typically two to eight times that of a second-level cache. Even so, the L3 access time is usually at least five times faster than a DRAM access.

> SRAM 的第一个字母代表 *static*。DRAM 中电路的动态性质需要在阅读后写入数据，因此访问时间和周期时间之间的差异以及需要刷新的需求。SRAM 不需要刷新，因此访问时间非常接近周期时间。SRAM 通常每位使用六个晶体管，以防止阅读时信息受到干扰。SRAM 只需要最小的功率即可将电荷保留在待机模式下。在较早的时候，大多数台式机和服务器系统都使用 SRAM 芯片作为其主要，次要或三级缓存。如今，所有三个级别的缓存都将整体置于处理器芯片上。在高端服务器芯片中，可能有多达 24 个核心和最多 60 个 MIB 的缓存；这样的系统通常配置为每个处理器芯片的 128-256 GIB DRAM。大型，第三级片上缓存的访问时间通常是第二级缓存的两到八倍。即便如此，L3 访问时间通常比 DRAM 访问快五倍。

On-chip, cache SRAMs are normally organized with a width that matches the block size of the cache, with the tags stored in parallel to each block. This allows an entire block to be read out or written into a single cycle. This capability is partic- ularly useful when writing data fetched after a miss into the cache or when writing back a block that must be evicted from the cache. The access time to the cache (ignoring the hit detection and selection in a set associative cache) is proportional to the number of blocks in the cache, whereas the energy consumption depends both on the number of bits in the cache (static power) and on the number of blocks (dynamic power). Set associative caches reduce the initial access time to the mem- ory because the size of the memory is smaller, but increase the time for hit detection and block selection, a topic we will cover in [Section 2.3](#ten-advanced-optimizations-of-cache-performance).

> 片上，缓存 SRAM 通常以与缓存的块大小相匹配的宽度，标签并行存储在每个块中。这允许整个块被读取或写入一个周期。当在遗漏中写入缓存或编写必须从缓存中驱逐的块时编写数据时，该功能非常有用。缓存的访问时间(忽略集合关联缓存中的 HIT 检测和选择)与缓存中的块数量成正比，而能量消耗既取决于缓存(静态功率)中的位数和 ON 块数(动态功率)。设置关联缓存将最初的访问时间缩短到对内存的最初访问时间，因为内存的大小较小，但是增加了 HIT 检测和块选择的时间，我们将在[2.3]中介绍一个主题(＃TEN-ADVADCAND-OPTIMIZATIONS - 绩效效果)。

### DRAM Technology

> ### DRAM 技术

Figure 2.3 Internal organization of a DRAM. Modern DRAMs are organized in banks, up to 16 for DDR4. Each bank consists of a series of rows. Sending an ACT (Activate) command opens a bank and a row and loads the row into a row buffer. When the row is in the buffer, it can be transferred by successive column addresses at whatever the width of the DRAM is (typically 4, 8, or 16 bits in DDR4) or by specifying a block trans- fer and the starting address. The Precharge commend (PRE) closes the bank and row and readies it for a new access. Each command, as well as block transfers, are synchro- nized with a clock. See the next section discussing SDRAM. The row and column signals are sometimes called RAS and CAS, based on the original names of the signals.

> 图 2.3 DRAM 的内部组织。现代 DRAM 是在银行组织的，DDR4 最多可容纳 16 个。每个银行都包含一系列行。发送 ACT(激活)命令将打开一个银行和一行，并将行加载到行缓冲区中。当行在缓冲区中时，可以通过连续的列地址以 DRAM 的宽度(通常为 DDR4 中的 4、8 或 16 位)传输，也可以通过指定块 Transfer 和起始地址指定。Precharge 赞扬(Pre)关闭银行和行，并准备好新访问权限。每个命令以及块转移都与时钟同步。请参阅下一部分讨论 SDRAM。根据信号的原始名称，行和列信号有时称为 RAS 和 CAS。

As early DRAMs grew in capacity, the cost of a package with all the necessary address lines was an issue. The solution was to multiplex the address lines, thereby Row cutting the number of address pins in half. [Figure 2.3](#_bookmark52) shows the basic DRAM orga- nization. One-half of the address is sent first during the _row access strobe_ (RAS). The other half of the address, sent during the _column access strobe_ (CAS), follows it. These names come from the internal chip organization, because the memory is organized as a rectangular matrix addressed by rows and columns.

> 随着早期 DRAM 的能力增长，带有所有必要地址线的包装成本是一个问题。解决方案是多重地址线，从而将地址引脚的数量切成两半。[图 2.3](#_ bookmark52)显示了基本的 dram orga-nization。地址的一半是在\_row Access strobe_(RAS)期间首先发送的。在 *column Access strobe*(CAS)期间发送的另一半，遵循它。这些名称来自内部芯片组织，因为内存是由行和列解决的矩形矩阵。

An additional requirement of DRAM derives from the property signified by its first letter, _D_, for _dynamic_. To pack more bits per chip, DRAMs use only a single transistor, which effectively acts as a capacitor, to store a bit. This has two implica- tions: first, the sensing wires that detect the charge must be precharged, which sets them “halfway” between a logical 0 and 1, allowing the small charge stored in the cell to cause a 0 or 1 to be detected by the sense amplifiers. On reading, a row is placed into a row buffer, where CAS signals can select a portion of the row to read out from the DRAM. Because reading a row destroys the information, it must be written back when the row is no longer needed. This write back happens in overlapped fashion, but in early DRAMs, it meant that the cycle time before a new row could be read was larger than the time to read a row and access a portion of that row.

> DRAM 的附加要求来自其第一个字母 *d* 表示 *dynamic* 的属性。为了打包每个芯片的更多位，DRAM 仅使用一个有效充当电容器的单个晶体管来存储一点。这有两个含义：首先，必须对检测电荷的传感电线进行预测，这将它们设置在逻辑 0 和 1 之间，允许存储在单元格中的小电荷导致检测到 0 或 1 从意义上讲，放大器。在阅读时，将一行放入行缓冲区中，CAS 信号可以选择该行的一部分以从 DRAM 中读取。由于阅读一行会破坏信息，因此必须在不再需要行时写回。这本书以重叠的方式发生，但是在早期的急剧中，这意味着可以读取新行的循环时间大于阅读一行并访问该行的一部分的时间。

In addition, to prevent loss of information as the charge in a cell leaks away (assuming it is not read or written), each bit must be “refreshed” periodically. For- tunately, all the bits in a row can be refreshed simultaneously just by reading that row and writing it back. Therefore every DRAM in the memory system must access every row within a certain time window, such as 64 ms. DRAM controllers include hardware to refresh the DRAMs periodically.

> 此外，为防止信息丢失，因为单元格中的电荷泄漏(假设未读取或书面)，则必须定期“刷新”每个位。从而，仅通过阅读该行并将其写回来，可以同时刷新一排中的所有位。因此，内存系统中的每个 DRAM 都必须在特定时间窗口中访问每个行，例如 64 ms。DRAM 控制器包括硬件，以定期刷新 DRAM。

This requirement means that the memory system is occasionally unavailable because it is sending a signal telling every chip to refresh. The time for a refresh is a row activation and a precharge that also writes the row back (which takes roughly 2/3 of the time to get a datum because no column select is needed), and this is required for each row of the DRAM. Because the memory matrix in a DRAM is conceptually square, the number of steps in a refresh is usually the square root of the DRAM capacity. DRAM designers try to keep time spent refreshing to less than 5% of the total time. So far we have presented main memory as if it operated like a Swiss train, consistently delivering the goods exactly according to schedule. In fact, with SDRAMs, a DRAM controller (usually on the processor chip) tries to optimize accesses by avoiding opening new rows and using block transfer when possible. Refresh adds another unpredictable factor.

> 此要求意味着内存系统偶尔不可用，因为它会发送信号告诉每个芯片进行刷新。刷新的时间是一行激活和一个预处理，也将行回编写(大约需要 2/3 的时间来获取基准，因为不需要列选择)，这是 DRAM 的每一行都需要的。由于 DRAM 中的内存矩阵在概念上是正方形的，因此刷新中的步骤数通常是 DRAM 容量的平方根。DRAM 设计师试图将花费的时间刷新到总时间的不到 5％。到目前为止，我们已经展示了主要记忆，就好像它像瑞士火车一样运行，始终按照时间表准确地交付商品。实际上，使用 SDRAMS，DRAM 控制器(通常在处理器芯片上)试图通过避免打开新行并在可能的情况下使用块转移来优化访问。刷新增加了另一个不可预测的因素。

Amdahl suggested as a rule of thumb that memory capacity should grow linearly with processor speed to keep a balanced system. Thus a 1000 MIPS processor should have 1000 MiB of memory. Processor designers rely on DRAMs to supply that demand. In the past, they expected a fourfold improvement in capacity every three years, or 55% per year. Unfortunately, the performance of DRAMs is growing at a much slower rate. The slower performance improvements arise primarily because of smaller decreases in the row access time, which is determined by issues such as power limitations and the charge capacity (and thus the size) of an individual mem- ory cell. Before we discuss these performance trends in more detail, we need to describe the major changes that occurred in DRAMs starting in the mid-1990s.

> Amdahl 建议根据经验，记忆能力应随处理器速度线性增长，以保持平衡系统。因此，1000 MIPS 处理器应具有 1000 MIB 的内存。处理器设计人员依靠 DRAM 来提供需求。过去，他们预计每三年的容量增加四倍，即每年 55％。不幸的是，DRAM 的性能以较慢的速度增长。较慢的性能改善主要是因为行访问时间的减小较小，这取决于诸如电源限制和单个内存单元的电荷容量(以及大小)等问题。在我们更详细地讨论这些绩效趋势之前，我们需要描述从 1990 年代中期开始的 DRAM 中发生的重大变化。

### Improving Memory Performance Inside a DRAM Chip: SDRAMs

> ###提高 DRAM 芯片中的内存性能：SDRAMS

Although very early DRAMs included a buffer allowing multiple column accesses to a single row, without requiring a new row access, they used an asynchronous interface, which meant that every column access and transfer involved overhead to synchronize with the controller. In the mid-1990s, designers added a clock sig- nal to the DRAM interface so that the repeated transfers would not bear that over- head, thereby creating _synchronous DRAM_ (SDRAM). In addition to reducing overhead, SDRAMs allowed the addition of a burst transfer mode where multiple transfers can occur without specifying a new column address. Typically, eight or more 16-bit transfers can occur without sending any new addresses by placing the DRAM in burst mode. The inclusion of such burst mode transfers has meant that there is a significant gap between the bandwidth for a stream of random accesses versus access to a block of data.

> 尽管非常早期的 DRAM 包括一个缓冲区，允许多列访问单行，而无需新的行访问，但他们使用了异步界面，这意味着每个列访问和传输涉及的台面都可以与控制器同步。在 1990 年代中期，设计师在 DRAM 接口上添加了一个时钟词，以使重复的转移不会忍受这一偏高，从而创建了 *synchronous dram*(SDRAM)。除了减少开销外，SDRAM 还允许添加突发传输模式，在该模式下可以进行多次转移，而无需指定新的列地址。通常，可以通过将 DRAM 放置在爆发模式下发送任何新地址的情况下进行八次或多个 16 位转移。包含这种爆发模式传输意味着，随机访问流的带宽与访问数据块之间存在显着差距。

To overcome the problem of getting more bandwidth from the memory as DRAM density increased, DRAMS were made wider. Initially, they offered a four-bit transfer mode; in 2017, DDR2, DDR3, and DDR DRAMS had up to 4, 8, or 16 bit buses.

> 为了克服随着 DRAM 密度的增加，从记忆中获得更多带宽的问题，使 DRAM 变得更宽。最初，他们提供了四位转移模式。在 2017 年，DDR2，DDR3 和 DDR DRAMS 最多可达 4、8 或 16 辆巴士。

In the early 2000s, a further innovation was introduced: _double data rate_ (DDR), which allows a DRAM to transfer data both on the rising and the falling edge of the memory clock, thereby doubling the peak data rate.

> 在 2000 年代初期，引入了进一步的创新：_Double Data Rate_(DDR)，它允许 DRAM 在内存时钟的上升和下降边缘上传输数据，从而使峰值数据速率增加一倍。

Finally, SDRAMs introduced _banks_ to help with power management, improve access time, and allow interleaved and overlapped accesses to different banks.

> 最后，SDRAMS 引入了 *BANKS*，以帮助电源管理，改善访问时间，并允许交错和重叠的访问权限到不同银行。

Access to different banks can be overlapped with each other, and each bank has its own row buffer. Creating multiple banks inside a DRAM effectively adds another segment to the address, which now consists of bank number, row address, and col- umn address. When an address is sent that designates a new bank, that bank must be opened, incurring an additional delay. The management of banks and row buffers is completely handled by modern memory control interfaces, so that when a subsequent access specifies the same row for an open bank, the access can happen quickly, sending only the column address.

> 访问不同银行的访问可以彼此重叠，每个银行都有自己的行缓冲区。在 DRAM 内部创建多个银行会有效地将另一个细分市场添加到该地址，现在由银行号，行地址和 COL-UMN 地址组成。当发送一个指定新银行的地址时，必须开设该银行，从而产生额外的延迟。现代内存控制接口完全处理了银行和行缓冲区的管理，因此，当随后的访问指定开放式银行的同一行时，访问可以快速进行，仅发送列地址。

To initiate a new access, the DRAM controller sends a bank and row number (called _Activate_ in SDRAMs and formerly called RAS—row select). That com- mand opens the row and reads the entire row into a buffer. A column address can then be sent, and the SDRAM can transfer one or more data items, depending on whether it is a single item request or a burst request. Before accessing a new row, the bank must be precharged. If the row is in the same bank, then the pre- charge delay is seen; however, if the row is in another bank, closing the row and precharging can overlap with accessing the new row. In synchronous DRAMs, each of these command cycles requires an integral number of clock cycles.

> 为了启动新的访问，DRAM 控制器发送了一个银行和行号(在 SDRAMS 中称为 *activate*，以前称为 RAS -ROW SELECT)。该组合将打开行，并将整个行读为缓冲区。然后可以发送列地址，SDRAM 可以转移一个或多个数据项，具体取决于它是单个项目请求还是爆发请求。在访问新行之前，必须对银行进行预测。如果行在同一银行中，则可以看到预电荷延迟；但是，如果该行在另一个银行中，关闭行和预处理可以与访问新行重叠。在同步 DRAM 中，这些命令周期中的每一个都需要数量不可或缺的时钟周期。

From 1980 to 1995, DRAMs scaled with Moore’s Law, doubling capacity every 18 months (or a factor of 4 in 3 years). From the mid-1990s to 2010, capacity increased more slowly with roughly 26 months between a doubling. From 2010 to 2016, capacity only doubled! [Figure 2.4](#_bookmark53) shows the capacity and access time for various generations of DDR SDRAMs. From DDR1 to DDR3, access times improved by a factor of about 3, or about 7% per year. DDR4 improves power and bandwidth over DDR3, but has similar access latency.

> 从 1980 年到 1995 年，DRAMS 按照摩尔定律的规模扩大，每 18 个月的能力加倍(或 3 年内 4 倍)。从 1990 年代中期到 2010 年，由于两倍的两倍，容量增加了大约 26 个月。从 2010 年到 2016 年，容量只翻了一番！[图 2.4](#\_ bookmark53)显示了各个世代 DDR SDRAM 的容量和访问时间。从 DDR1 到 DDR3，访问时间提高了约 3 倍，每年约 7％。DDR4 改善了 DDR3 的功率和带宽，但具有相似的访问延迟。

As [Figure 2.4](#_bookmark53) shows, DDR is a sequence of standards. DDR2 lowers power from DDR1 by dropping the voltage from 2.5 to 1.8 V and offers higher clock rates: 266, 333, and 400 MHz. DDR3 drops voltage to 1.5 V and has a maximum clock speed of 800 MHz. (As we discuss in the next section, GDDR5 is a graphics

> 如[图 2.4](#\_ bookmark53)所示，DDR 是一系列标准。DDR2 通过将电压从 2.5 降低到 1.8 V，从而降低了 DDR1 的功率，并提供了更高的时钟速率：266、333 和 400 MHz。DDR3 将电压降为 1.5 V，最大时钟速度为 800 MHz。(正如我们在下一节中讨论的那样，GDDR5 是图形

> Figure 2.4 Capacity and access times for DDR SDRAMs by year of production. Access time is for a random memory word and assumes a new row must be opened. If the row is in a different bank, we assume the bank is precharged; if the row is not open, then a precharge is required, and the access time is longer. As the number of banks has increased, the ability to hide the precharge time has also increased. DDR4 SDRAMs were initially expected in 2014, but did not begin production until early 2016.

>> 图 2.4 DDR SDRAM 的容量和访问时间到生产。访问时间是一个随机存储单词，并假定必须打开新行。如果该行在另一个银行中，我们假设银行已经预先掌握；如果行不打开，则需要进行预学，并且访问时间更长。随着银行数量的增加，隐藏预处理时间的能力也有所增加。DDR4 SDRAM 最初预计在 2014 年，但直到 2016 年初才开始生产。
>>

> Figure 2.5 Clock rates, bandwidth, and names of DDR DRAMS and DIMMs in 2016. Note the numerical relationship between the columns. The third column is twice the second, and the fourth uses the number from the third column in the name of the DRAM chip. The fifth column is eight times the third column, and a rounded version of this number is used in the name of the DIMM. DDR4 saw significant first use in 2016.

>> 图 2.5 2016 年的时钟速率，带宽以及 DDR DRAM 和 DIMM 的名称。请注意列之间的数值关系。第三列是第二列的两倍，第四列以 DRAM 芯片的名称使用第三列中的数字。第五列是第三列的八倍，并且该数字的圆形版本以 DIMM 的名称使用。DDR4 在 2016 年看到了大量首次使用。
>>

RAM and is based on DDR3 DRAMs.) DDR4, which shipped in volume in early 2016, but was expected in 2014, drops the voltage to 1–1.2 V and has a maximum expected clock rate of 1600 MHz. DDR5 is unlikely to reach production quantities until 2020 or later.

> RAM 且基于 DDR3 DRAM。)DDR4，该 DDR4 在 2016 年初发货，但预计在 2014 年，电压降至 1-1.2 V，最大预期时钟速率为 1600 MHz。直到 2020 年或更晚，DDR5 不太可能达到生产量。

With the introduction of DDR, memory designers increasing focused on band- width, because improvements in access time were difficult. Wider DRAMs, burst transfers, and double data rate all contributed to rapid increases in memory band- width. DRAMs are commonly sold on small boards called _dual inline memory modules_ (DIMMs) that contain 4–16 DRAM chips and that are normally organized to be 8 bytes wide (+ ECC) for desktop and server systems. When DDR SDRAMs are packaged as DIMMs, they are confusingly labeled by the peak _DIMM_ band- width. Therefore the DIMM name PC3200 comes from 200 MHz 2 8 bytes, or 3200 MiB/s; it is populated with DDR SDRAM chips. Sustaining the confusion, the chips themselves are labeled with _the number of bits per second_ rather than their clock rate, so a 200 MHz DDR chip is called a DDR400. [Figure 2.5](#_bookmark54) shows the relationships’ I/O clock rate, transfers per second per chip, chip bandwidth, chip name, DIMM bandwidth, and DIMM name.

> 随着 DDR 的引入，内存设计师越来越集中于频带，因为很难改善访问时间。更广泛的 DRAM，爆发转移和双重数据速率都导致记忆带宽的快速增加。DRAM 通常在称为* Duul Inline Memory 模块*(DIMM)的小型板上出售，其中包含 4–16 DRAM 芯片，通常组织为 8 个字节宽(+ ECC)，用于台式机和服务器系统。当 DDR SDRAM 被包装为 DIMM 时，它们会被峰 *dimm* 带宽度贴上。因此，DIMM 名称 PC3200 来自 200 MHz 2 8 字节或 3200 MIB/S；它填充了 DDR SDRAM 芯片。使芯片本身保持困惑，并用*每秒的位数而不是其时钟速率标记，因此 200 MHz DDR 芯片称为 DDR400。[图 2.5](#* bookmark54)显示了关系的 I/O 时钟速率，每秒每秒传输，芯片带宽，芯片名称，DIMM 带宽和 DIMM 名称。

##### _Reducing Power Consumption in SDRAMs_

> ##### \_ sdrams 中的功耗

Power consumption in dynamic memory chips consists of both dynamic power used in a read or write and static or standby power; both depend on the operating voltage. In the most advanced DDR4 SDRAMs, the operating voltage has dropped to 1.2 V, significantly reducing power versus DDR2 and DDR3 SDRAMs. The addition of banks also reduced power because only the row in a single bank is read.

> 动态内存芯片中的功耗包括读写或静态或待机功率中使用的动态功率；两者都取决于工作电压。在最先进的 DDR4 SDRAM 中，工作电压已降至 1.2 V，大大降低了功率与 DDR2 和 DDR3 SDRAM。银行的增加也降低了功率，因为仅阅读单个银行中的行。

> Figure 2.6 Power consumption for a DDR3 SDRAM operating under three condi- tions: low-power (shutdown) mode, typical system mode (DRAM is active 30% of the time for reads and 15% for writes), and fully active mode, where the DRAM is continuously reading or writing. Reads and writes assume bursts of eight transfers. These data are based on a Micron 1.5V 2GB DDR3-1066, although similar savings occur in DDR4 SDRAMs.

>> 图 2.6 DDR3 SDRAM 在三个条件下运行的 DDR3 SDRAM 的功耗：低功率(关闭)模式，典型的系统模式(DRAM 为读取时间的 30％，写入 15％)，并且完全有效的模式，DRAM 在不断阅读或写作的地方。读写和写作假设八次转移。这些数据基于 Micron 1.5V 2GB DDR3-1066，尽管在 DDR4 SDRAM 中进行了类似的节省。
>>

In addition to these changes, all recent SDRAMs support a power-down mode, which is entered by telling the DRAM to ignore the clock. Power-down mode dis- ables the SDRAM, except for internal automatic refresh (without which entering power-down mode for longer than the refresh time will cause the contents of mem- ory to be lost). [Figure 2.6](#_bookmark55) shows the power consumption for three situations in a 2 GB DDR3 SDRAM. The exact delay required to return from low power mode depends on the SDRAM, but a typical delay is 200 SDRAM clock cycles.

> 除了这些更改外，所有最近的 SDRAM 都支持了降低电力模式，该模式是通过告诉 DRAM 忽略时钟来输入的。除了内部自动刷新以外，功率降低模式删除了 SDRAM(如果没有该模式，则在此刷新模式的情况下，比刷新时间更长的时间将导致丢失 mem-Ory 的内容)。[图 2.6](#\_ bookmark55)显示了 2 GB DDR3 SDRAM 中三种情况的功耗。从低功率模式返回所需的确切延迟取决于 SDRAM，但典型的延迟是 200 SDRAM 时钟周期。

### Graphics Data RAMs

> ###图形数据公羊

GDRAMs or GSDRAMs (Graphics or Graphics Synchronous DRAMs) are a spe- cial class of DRAMs based on SDRAM designs but tailored for handling the higher bandwidth demands of graphics processing units. GDDR5 is based on DDR3 with earlier GDDRs based on DDR2. Because graphics processor units (GPUs; see [Chapter 4](#_bookmark165)) require more bandwidth per DRAM chip than CPUs, GDDRs have several important differences:

> GDRAMS 或 GSDRAM(图形或图形同步 DRAM)是基于 SDRAM 设计的一类 DRAM 类，但量身定制用于处理图形处理单元的较高带宽需求。GDDR5 基于 DDR3，其较早的 GDDR 基于 DDR2。因为图形处理器单元(GPU；请参见[第 4 章](#\_ bookmark165))比 CPU 所需的每次 DRAM 芯片带宽更多，所以 GDDRS 具有几个重要区别：

1. GDDRs have wider interfaces: 32-bits versus 4, 8, or 16 in current designs.

> 1. GDDR 具有较宽的接口：32 位与当前设计中的 4、8 或 16。

2. GDDRs have a higher maximum clock rate on the data pins. To allow a higher transfer rate without incurring signaling problems, GDRAMS normally connect directly to the GPU and are attached by soldering them to the board, unlike DRAMs, which are normally arranged in an expandable array of DIMMs.

> 2. GDDR 在数据引脚上具有更高的最大时钟速率。为了允许更高的传输速率而不会引起信号问题，GDRAM 通常直接连接到 GPU，并通过将它们焊接到板上，与 DRAM 不同，这些 DRAM 通常在可扩展的 DIMMS 阵列中排列。

Altogether, these characteristics let GDDRs run at two to five times the bandwidth per DRAM versus DDR3 DRAMs.

> 这些特性总共使 GDDR 的每次 DRAM 与 DDR3 DRAM 的带宽的速度是两到五倍。

### Packaging Innovation: Stacked or Embedded DRAMs

> ###包装创新：堆叠或嵌入 DRAM

The newest innovation in 2017 in DRAMs is a packaging innovation, rather than a circuit innovation. It places multiple DRAMs in a stacked or adjacent fashion embedded within the same package as the processor. (Embedded DRAM also is used to refer to designs that place DRAM on the processor chip.) Placing the DRAM and processor in the same package lowers access latency (by shortening the delay between the DRAMs and the processor) and potentially increases band- width by allowing more and faster connections between the processor and DRAM; thus several producers have called it _high bandwidth memory (HBM)_.

> 2017 年在 DRAMS 上的最新创新是包装创新，而不是电路创新。它以与处理器相同的包裹中嵌入的堆叠或相邻时尚的方式放置多个 DRAM。(嵌入式 DRAM 还用于指代将 DRAM 放在处理器芯片上的设计。)将 DRAM 和处理器放在同一包装中，降低访问延迟(通过缩短 DRAM 和处理器之间的延迟)，并可能将带宽的带宽增加在处理器和 DRAM 之间允许更多，更快的连接；因此，一些生产商称其为*高带宽内存(HBM)*。

One version of this technology places the DRAM die directly on the CPU die using solder bump technology to connect them. Assuming adequate heat manage- ment, multiple DRAM dies can be stacked in this fashion. Another approach stacks only DRAMs and abuts them with the CPU in a single package using a substrate (interposer) containing the connections. [Figure 2.7](#_bookmark56) shows these two different inter- connection schemes. Prototypes of HBM that allow stacking of up to eight chips have been demonstrated. With special versions of SDRAMs, such a package could contain 8 GiB of memory and have data transfer rates of 1 TB/s. The 2.5D tech- nique is currently available. Because the chips must be specifically manufactured to stack, it is quite likely that most early uses will be in high-end server chipsets.

> 该技术的一种版本将 DRAM 直接使用焊接凹凸技术将其连接到 CPU 模具上。假设有足够的热量管理，可以以这种方式堆叠多个 DRAM 模具。另一种方法仅堆叠 DRAM 并将 CPU 与 CPU 插入单个软件包中，并使用包含连接的基板(Interposer)。[图 2.7](#\_ bookmark56)显示了这两种不同的间连接方案。已经证明了最多可堆叠八个芯片的 HBM 的原型。使用特殊版本的 SDRAM，这样的包装可以包含 8 个 GIB 内存，并且数据传输速率为 1 TB/s。2.5D 技术目前可用。由于芯片必须专门制造以堆叠，因此大多数早期用途很可能会在高端服务器芯片组中。

In some applications, it may be possible to internally package enough DRAM to satisfy the needs of the application. For example, a version of an Nvidia GPU used as a node in a special-purpose cluster design is being developed using HBM, and it is likely that HBM will become a successor to GDDR5 for higher-end appli- cations. In some cases, it may be possible to use HBM as main memory, although the cost limitations and heat removal issues currently rule out this technology for some embedded applications. In the next section, we consider the possibility of using HBM as an additional level of cache.

> 在某些应用程序中，可能可以内部包装足够的 DRAM 来满足应用程序的需求。例如，正在使用 HBM 开发特殊用作群集设计中的 NVIDIA GPU 版本，而 HBM 可能会成为高端应用程序的 GDDR5 的后继产品。在某些情况下，尽管目前的成本限制和去除量问题排除了某些嵌入式应用程序的技术，但可能会将 HBM 用作主要内存。在下一节中，我们将使用 HBM 作为额外的缓存级别的可能性。

![](./media/image111.jpeg)
![](./media/image112.jpeg)<span id="_bookmark56" class="anchor"></span>xPU

**Vertical stacking (3D) Interposer stacking (2.5D)**

> **垂直堆叠(3D)插座堆叠(2.5D)**

Figure 2.7 Two forms of die stacking. The 2.5D form is available now. 3D stacking is under development and faces heat management challenges due to the CPU.

> 图 2.7 两种形式的堆叠形式。2.5D 表格现在可用。3D 堆叠正在开发中，并且由于 CPU 而面临热管理挑战。

### Flash Memory

> ###闪存

Flash memory is a type of EEPROM (electronically erasable programmable read- only memory), which is normally read-only but can be erased. The other key prop- erty of Flash memory is that it holds its contents without any power. We focus on NAND Flash, which has higher density than NOR Flash and is more suitable for large-scale nonvolatile memories; the drawback is that access is sequential and writing is slower, as we explain below.

> 闪存是一种 EEPROM(电子上可擦除的可编程读取 - 仅记忆)，通常是只读但可以删除。闪存的另一个关键支持是，它拥有其内容，而无需任何功率。我们专注于 NAND Flash，其密度比 NOR Flash 更高，并且更适合大规模的非易失性记忆。缺点是访问是顺序的，写作较慢，正如我们在下面解释的那样。

Flash is used as the secondary storage in PMDs in the same manner that a disk functions in a laptop or server. In addition, because most PMDs have a limited amount of DRAM, Flash may also act as a level of the memory hierarchy, to a much greater extent than it might have to do in a desktop or server with a main memory that might be 10–100 times larger.

> Flash 以与笔记本电脑或服务器中功能相同的方式用作 PMD 中的辅助存储。此外，由于大多数 PMD 的 DRAM 数量有限，因此 Flash 也可能充当内存层次结构的水平，其程度要比在台式机或服务器中使用的主内存可能为 10–的台式机或服务器要大得多。大 100 倍。

Flash uses a very different architecture and has different properties than stan- dard DRAM. The most important differences are

> Flash 使用非常不同的架构，并且具有与 Standard DRAM 不同的属性。最重要的区别是

1. Reads to Flash are sequential and read an entire page, which can be 512 bytes, 2 KiB, or 4 KiB. Thus NAND Flash has a long delay to access the first byte from a random address (about 25 μS), but can supply the remainder of a page block at about 40 MiB/s. By comparison, a DDR4 SDRAM takes about 40 ns to the first byte and can transfer the rest of the row at 4.8 GiB/s. Comparing the time to transfer 2 KiB, NAND Flash takes about 75 μS, while DDR SDRAM takes less than 500 ns, making Flash about 150 times slower. Compared to mag- netic disk, however, a 2 KiB read from Flash is 300 to 500 times faster. From these numbers, we can see why Flash is not a candidate to replace DRAM for main memory, but is a candidate to replace magnetic disk.

> 1.读取闪光灯是顺序的，并读取整个页面，可以是 512 个字节，2 KIB 或 4 KIB。因此，NAND Flash 有很长的延迟，可以从随机地址(约 25μs)访问第一个字节，但可以在约 40 MIB/s 的页面块中提供剩余的页面块。相比之下，DDR4 SDRAM 大约需要 40 ns 到第一个字节，并且可以以 4.8 Gib/s 的速度转移该行的其余部分。比较传输 2 KIB 的时间，NAND Flash 需要大约 75μs，而 DDR SDRAM 的时间少于 500 ns，闪光灯的速度慢了约 150 倍。但是，与磁盘相比，Flash 的 2 KIB 读数的速度更快为 300 至 500 倍。从这些数字中，我们可以理解为什么 Flash 不是替代 DRAM 以替换主内存的候选者，而是替换磁盘的候选者。

2. Flash memory must be erased (thus the name flash for the “flash” erase process) before it is overwritten, and it is erased in blocks rather than individual bytes or words. This requirement means that when data must be written to Flash, an entire block must be assembled, either as new data or by merging the data to be written and the rest of the block’s contents. For writing, Flash is about 1500 times slower then SDRAM, and about 8–15 times as fast as magnetic disk.

> 2.必须在覆盖闪存之前删除闪存(因此“闪存”擦除过程的名称 flash)，并以块而不是单个字节或单词的形式删除。此要求意味着，当必须将数据写入 Flash 时，必须通过新数据或合并要编写的数据以及块内容的其余内容来组装整个块。对于写作，Flash 的速度比 SDRAM 慢 1500 倍，而磁盘的速度约为 8-15 倍。

3. Flash memory is nonvolatile (i.e., it keeps its contents even when power is not applied) and draws significantly less power when not reading or writing (from less than half in standby mode to zero when completely inactive).

> 3.闪存是非挥发性的(即，即使不使用电源，它也可以保留其内容)，并且在不读取或写作时(从备用模式下的一半小于一半到零时，完全不活动时)的功率明显较小。

4. Flash memory limits the number of times that any given block can be written, typically at least 100,000. By ensuring uniform distribution of written blocks throughout the memory, a system can maximize the lifetime of a Flash memory system. This technique, called _write leveling_, is handled by Flash memory controllers.

> 4.闪存限制了可以编写任何给定块的次数，通常至少为 100,000。通过确保整个内存中书面块的均匀分布，系统可以最大化闪存系统的寿命。该技术称为 *write Leveling*，由闪存控制器处理。

5. High-density NAND Flash is cheaper than SDRAM but more expensive than disks: roughly $2/GiB for Flash, $20 to $40/GiB for SDRAM, and $0.09/GiB for magnetic disks. In the past five years, Flash has decreased in cost at a rate that is almost twice as fast as that of magnetic disks.

> 5.高密度 NAND 闪光灯比 SDRAM 便宜，但比磁盘更昂贵：闪光灯大约 2 美元/gib，SDRAM $ 20 至 40 美元，磁盘为 0.09 美元/gib。在过去的五年中，Flash 的成本下降，其速度几乎是磁盘的速度两倍。

Like DRAM, Flash chips include redundant blocks to allow chips with small numbers of defects to be used; the remapping of blocks is handled in the Flash chip. Flash controllers handle page transfers, provide caching of pages, and handle write leveling.

> 像 DRAM 一样，闪存芯片包括冗余块，以允许使用少量缺陷的芯片；块的重新块在闪光芯片中处理。闪存控制器处理页面传输，提供页面的缓存以及处理写入级别。

The rapid improvements in high-density Flash have been critical to the devel- opment of low-power PMDs and laptops, but they have also significantly changed both desktops, which increasingly use solid state disks, and large servers, which often combine disk and Flash-based storage.

> 高密度闪光的快速改善对于低功率 PMD 和笔记本电脑的开发至关重要，但是它们也显着改变了两个台式机，这些台式机越来越多地使用固态磁盘和大型服务器，这些服务器通常结合了磁盘和闪光灯。- 基于存储。

### Phase-Change Memory Technology

> ###阶段变换记忆技术

Phase-change memory (PCM) has been an active research area for decades. The technology typically uses a small heating element to change the state of a bulk sub- strate between its crystalline form and an amorphous form, which have different resistive properties. Each bit corresponds to a crosspoint in a two-dimensional net- work that overlays the substrate. Reading is done by sensing the resistance between an x and y point (thus the alternative name _memristor_), and writing is accomplished by applying a current to change the phase of the material. The absence of an active device (such as a transistor) should lead to lower costs and greater density than that of NAND Flash.

> 几十年来，相位变化记忆(PCM)一直是一个活跃的研究领域。该技术通常使用一个小的加热元件来改变其结晶形式和具有不同电阻性能的无定形形式之间的体积状态。每个位都对应于覆盖基板的二维净工作中的跨点。读数是通过感知 X 和 Y 点之间的电阻(因此替代名称 *Memristor*)完成的，并且通过应用电流来更改材料的相位来完成写作。与 NAND Flash 相比，没有活性设备(例如晶体管)的成本和更高的密度应导致更低的成本和更高的密度。

In 2017 Micron and Intel began delivering Xpoint memory chips that are believed to be based on PCM. The technology is expected to have much better write durability than NAND Flash and, by eliminating the need to erase a page before writing, achieve an increase in write performance versus NAND of up to a factor of ten. Read latency is also better than Flash by perhaps a factor of 2–3. Initially, it is expected to be priced slightly higher than Flash, but the advan- tages in write performance and write durability may make it attractive, especially for SSDs. Should this technology scale well and be able to achieve additional cost reductions, it may be the solid state technology that will depose magnetic disks, which have reigned as the primary bulk nonvolatile store for more than 50 years.

> 在 2017 年，Micron 和 Intel 开始提供 XPoint Memory Chip，据信基于 PCM。预计该技术的写入耐用性要比 NAND Flash 更好得多，并且通过消除在写作之前删除页面的需求，可以提高写作性能与 NAND 的需求高达十倍。阅读延迟也可能比 Flash 更好。最初，预计它的价格将比 Flash 略高，但是写入性能和写入耐用性的优势可能会使它具有吸引力，尤其是对于 SSD。如果这项技术规模良好并能够实现额外的成本降低，则可能是固态技术，它将破坏磁盘，该磁盘已成为 50 多年来一直是主要批量非易失性商店。

### Enhancing Dependability in Memory Systems

> ###增强内存系统中的可靠性

Large caches and main memories significantly increase the possibility of errors occurring both during the fabrication process and dynamically during operation. Errors that arise from a change in circuitry and are repeatable are called _hard errors_ or _permanent faults._ Hard errors can occur during fabrication, as well as from a circuit change during operation (e.g., failure of a Flash memory cell after many writes). All DRAMs, Flash memory, and most SRAMs are manufactured with spare rows so that a small number of manufacturing defects can be accommodated by programming the replacement of a defective row by a spare row. Dynamic errors, which are changes to a cell’s contents, not a change in the circuitry, are called _soft errors_ or _transient faults_.

> 大型缓存和主要记忆显着增加了在制造过程中和操作过程中动态发生错误的可能性。由电路更改和可重复的错误引起的错误称为 *hard 错误*或* Permanent 故障。所有的 DRAM，闪存和大多数 SRAM 都是用备用行制造的，因此可以通过编程备用行替换有缺陷的行来容纳少量的制造缺陷。动态错误是对单元内容的更改，而不是电路中的更改，称为\_soft errors *或* transient FARDS*。

Dynamic errors can be detected by parity bits and detected and fixed by the use of error correcting codes (ECCs). Because instruction caches are read-only, parity

> 动态错误可以通过奇偶校验比特检测，并通过使用错误校正代码(ECC)检测和固定。因为教学缓存是只读的，所以平等

suffices. In larger data caches and in main memory, ECC is used to allow errors to be both detected and corrected. Parity requires only one bit of overhead to detect a single error in a sequence of bits. Because a multibit error would be undetected with parity, the number of bits protected by a parity bit must be limited. One parity bit per 8 data bits is a typical ratio. ECC can detect two errors and correct a single error with a cost of 8 bits of overhead per 64 data bits.

> 足够。在较大的数据缓存和主内存中，ECC 用于允许检测和纠正错误。奇偶校验只需要一点点开销即可检测一系列位的单个误差。由于多级误差将无法以平等的态度检测，因此必须限制受平等位保护的位数。每 8 个数据位一个奇偶元位是一个典型的比例。ECC 可以检测两个错误并纠正单个错误，每 64 个数据位的成本为 8 位开销。

In very large systems, the possibility of multiple errors as well as complete fail- ure of a single memory chip becomes significant. Chipkill was introduced by IBM to solve this problem, and many very large systems, such as IBM and SUN servers and the Google Clusters, use this technology. (Intel calls their version SDDC.) Similar in nature to the RAID approach used for disks, Chipkill distributes the data and ECC information so that the complete failure of a single memory chip can be handled by supporting the reconstruction of the missing data from the remaining memory chips. Using an analysis by IBM and assuming a 10,000 processor server with 4 GiB per processor yields the following rates of unrecoverable errors in three years of operation:

> 在非常大的系统中，单个内存芯片的多个错误以及完全失败的可能性变得很重要。Chipkill 是由 IBM 引入的，以解决此问题，许多非常大的系统(例如 IBM 和 Sun Servers 以及 Google 簇)使用此技术。(英特尔称其版本 SDDC。)本质上类似于用于磁盘使用的 RAID 方法，Chipkill 分发了数据和 ECC 信息，因此可以通过支持剩余的剩余数据的重建单个内存芯片的完全失败来处理单个内存芯片的完整失败内存芯片。使用 IBM 的分析并假设拥有每个处理器 4 GIB 的 10,000 个处理器服务器在三年操作中产生以下无法恢复的错误率：

- Parity only: About 90,000, or one unrecoverable (or undetected) failure every 17 minutes.
- ECC only: About 3500, or about one undetected or unrecoverable failure every 7.5 hours.
- Chipkill: About one undetected or unrecoverable failure every 2 months.

> - 仅均衡：大约 90,000，或每 17 分钟一次未恢复(或未检测到的)故障。
> - 仅 ECC：大约 3500，或每 7.5 小时大约一个未发现或无法恢复的故障。
>   -Chipkill：每 2 个月大约一次未发现或无法恢复的故障。

Another way to look at this is to find the maximum number of servers (each with 4 GiB) that can be protected while achieving the same error rate as demon- strated for Chipkill. For parity, even a server with only one processor will have an unrecoverable error rate higher than a 10,000-server Chipkill protected system. For ECC, a 17-server system would have about the same failure rate as a 10,000-server Chipkill system. Therefore Chipkill is a requirement for the 50,000–100,00 servers in warehouse-scale computers (see Section 6.8 of [Chapter 6](#_bookmark268)).

> 考虑到这一点的另一种方法是找到可以保护的最大服务器数(每个都有 4 个 GIB)，同时达到与 Chipkill 相同的错误率。对于奇偶校验，即使只有一个处理器的服务器也将其错误率高于 10,000 服务器 Chipkill 保护系统。对于 ECC 而言，17 个服务器系统的故障率与 10,000 服务器的 Chipkill 系统大致相同。因此，Chipkill 是仓库规模计算机中 50,000–100,00 服务器的要求(请参阅[第 6 章]第 6.8 节(#\_ bookmark268)。
