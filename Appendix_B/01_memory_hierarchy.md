## Introduction

This appendix is a quick refresher of the memory hierarchy, including the basics of cache and virtual memory, performance equations, and simple optimizations. This first section reviews the following 36 terms:

> 该附录是内存层次结构的快速刷新，包括缓存和虚拟内存的基础，性能方程和简单的优化。第一部分回顾以下 36 个术语：

If this review goes too quickly, you might want to look at [Chapter 7](#_bookmark322) in _Computer Organization and Design_, which we wrote for readers with less experience.

> 如果这篇评论的速度太快，您可能需要在 _计算机组织与设计_ 中查看[第 7 章](#_bookmark322)，我们为经验较少的读者编写。

_Cache_ is the name given to the highest or first level of the memory hierarchy encountered once the address leaves the processor. Because the principle of local- ity applies at many levels, and taking advantage of locality to improve performance is popular, the term _cache_ is now applied whenever buffering is employed to reuse commonly occurring items. Examples include _file caches_, _name caches_, and so on. When the processor finds a requested data item in the cache, it is called a _cache hit_. When the processor does not find a data item it needs in the cache, a _cache miss_ occurs. A fixed-size collection of data containing the requested word, called a _block_ or line run, is retrieved from the main memory and placed into the cache. _Temporal locality_ tells us that we are likely to need this word again in the near future, so it is useful to place it in the cache where it can be accessed quickly.

> _Cache_ 是地址离开处理器后遇到的最高或第一级内存层次结构的名称。因为局部性原则适用于许多级别，并且利用局部性来提高性能很受欢迎，所以现在只要使用缓冲来重用经常出现的项目，就会应用术语 _cache_。示例包括 _文件缓存_、_名称缓存_ 等。当处理器在缓存中找到请求的数据项时，它被称为 _缓存命中_。当处理器在缓存中找不到它需要的数据项时，就会发生 _cache miss_。从主内存中检索包含请求字的固定大小的数据集合，称为 _块_ 或行运行，并将其放入缓存中。_Temporal locality_ 告诉我们，我们很可能在不久的将来再次需要这个词，因此将它放在可以快速访问的缓存中很有用。

Because of _spatial locality_--, there is a high probability that the other data in the block will be needed soon.

> 由于存在 _spatial locality_，很可能很快就需要块中的其他数据。

The time required for the cache miss depends on both the latency and band- width of the memory. Latency determines the time to retrieve the first word of the block, and bandwidth determines the time to retrieve the rest of this block. A cache miss is handled by hardware and causes processors using in-order execution to pause, or stall, until the data are available. With out-of-order execution, an instruction using the result must still wait, but other instructions may proceed dur- ing the miss.

> 缓存失误所需的时间取决于内存的延迟和带宽。延迟决定了检索块的第一个单词的时间，带宽决定了检索其余部分的时间。硬件处理缓存失误，并导致处理器使用订单执行暂停或失速，直到数据可用为止。通过序列执行，使用结果的指令仍必须等待，但是其他说明可能会在失误期间继续进行。

Similarly, not all objects referenced by a program need to reside in main memory. _Virtual memory_ means some objects may reside on disk. The address space is usually broken into fixed-size blocks, called _pages_. At any time, each page resides either in main memory or on disk. When the processor references an item within a page that is not present in the cache or main memory, a _palt_ occurs, and the entire page is moved from the disk to main memory. Because page faults take so long, they are handled in software and the processor is not stalled. The processor usually switches to some other task while the disk access occurs. From a high-level perspective, the reliance on locality of references and the relative relationships in size and relative cost per bit of cache versus main memory are similar to those of main memory versus disk.

> 同样，并非程序引用的所有对象都需要驻留在主要成员中。_virtual Memory_ 表示某些对象可能位于磁盘上。地址空间通常分为固定大小的块，称为 _pages_。在任何时候，每个页面都位于主内存或磁盘上。当处理器引用在缓存或主内存中不存在的页面中的项目时，会发生 _palt_，整个页面都从磁盘转移到主内存。由于页面故障需要很长时间，因此它们在软件中处理，并且处理器不会停滞不前。在发生磁盘访问时，处理器通常会切换到其他任务。从高水平的角度来看，对参考的局部性以及大小和每位缓存相对成本的相对关系与主内存的相对成本相似，与主内存与磁盘的相对成本相似。

Figure B.1 The typical levels in the hierarchy slow down and get larger as we move away from the processor for a large workstation or small server. Embedded computers might have no disk storage and much smaller memories and caches. Increasingly, FLASH is replacing magnetic disks, at least for first level file storage. The access times increase as we move to lower levels of the hierarchy, which makes it feasible to manage the transfer less responsively. The implementation technology shows the typical technology used for these functions. The access time is given in nanoseconds for typical values in 2017; these times will decrease over time. Bandwidth is given in megabytes per second between levels in the memory hierarchy. Bandwidth for disk/FLASH storage includes both the media and the buffered interfaces.

> 图 B.1 层次结构中的典型级别放慢速度，并随着我们离开处理器的大型工作站或小服务器而变得更大。嵌入式计算机可能没有磁盘存储，并且记忆和缓存。越来越多的闪光灯替换磁盘，至少用于第一级文件存储。随着我们转移到层次结构的较低水平，访问时间增加，这使得管理转移的响应性降低。实施技术显示了用于这些功能的典型技术。访问时间在 2017 年的典型值中给出了纳秒秒；这些时间会随着时间的流逝而减少。在内存层次结构中的级别之间，每秒给出带宽。磁盘/闪存存储的带宽同时包括媒体和缓冲接口。

[Figure B.1](#_bookmark439) shows the range of sizes and access times of each level in the mem- ory hierarchy for computers ranging from high-end desktops to low-end servers.

> [图 B.1](#_bookmark439) 显示了在内存层次结构中每个级别的大小和访问时间的范围，用于从高端台式机到低端服务器的计算机。

### Cache Performance Review

Because of locality and the higher speed of smaller memories, a memory hierarchy can substantially improve performance. One method to evaluate cache perfor- mance is to expand our processor execution time equation from [Chapter 1](#_bookmark2). We now account for the number of cycles during which the processor is stalled waiting for a memory access, which we call the _memory stall cycles_. The performance is then the product of the clock cycle time and the sum of the processor cycles and the memory stall cycles:

> 由于局部性和较小存储器的较高速度，存储器层次结构可以显着提高性能。评估高速缓存性能的一种方法是扩展[第 1 章](#_bookmark2) 中的处理器执行时间方程。我们现在计算处理器停止等待内存访问的周期数，我们称之为 _内存停止周期_。性能是时钟周期时间与处理器周期和内存停顿周期之和的乘积：

CPU execution time = (CPU clock cycles + Memory stall cycles) ×Clock cycle time

This equation assumes that the CPU clock cycles include the time to handle a cache hit and that the processor is stalled during a cache miss. [Section B.2](#cache-performance) reexamines this simplifying assumption.

> 该方程式假定 CPU 时钟周期包括处理缓存命中的时间，并且在缓存失误期间处理器停滞不前。[B.2 节]重新回顾此简化的假设。

The number of memory stall cycles depends on both the number of misses and the cost per miss, which is called the _miss penalty:_

> 内存停顿周期的数量取决于未命中次数和每次未命中的成本，这称为 _miss penalty:_

The advantage of the last form is that the components can be easily measured. We already know how to measure instruction count (IC). (For speculative processors, we only count instructions that commit.) Measuring the number of memory refer- ences per instruction can be done in the same fashion; every instruction requires an instruction access, and it is easy to decide if it also requires a data access.

> 最后形式的优点是可以轻松测量组件。我们已经知道如何衡量指导计数(IC)。(对于投机处理器，我们只计算提交的指令。)可以以相同的方式测量每个指令的内存参考数量；每个指令都需要指令访问，并且很容易决定是否还需要数据访问。

Note that we calculated miss penalty as an average, but we will use it herein as if it were a constant. The memory behind the cache may be busy at the time of the miss because of prior memory requests or memory refresh. The number of clock cycles also varies at interfaces between different clocks of the processor, bus, and memory. Thus, please remember that using a single number for miss penalty is a simplification.

> 请注意，我们将未命中惩罚计算为平均值，但我们将在此处将其用作常数。由于先前的内存请求或内存刷新，缓存后面的内存在未命中时可能很忙。时钟周期的数量在处理器、总线和存储器的不同时钟之间的接口处也不同。因此，请记住，使用单个数字作为未命中惩罚是一种简化。

The component _miss rate_ is simply the fraction of cache accesses that result in a miss (i.e., number of accesses that miss divided by number of accesses). Miss rates can be measured with cache simulators that take an _address trace_ of the instruction and data references, simulate the cache behavior to determine which references hit and which miss, and then report the hit and miss totals. Many microprocessors today provide hardware to count the number of misses and memory references, which is a much easier and faster way to measure miss rate.

> 组件*未命中率*只是导致未命中的高速缓存访 ​​ 问的分数(即，未命中的访问次数除以访问次数)。可以使用高速缓存模拟器测量未命中率，高速缓存模拟器获取指令和数据引用的*地址跟踪*，模拟高速缓存行为以确定哪些引用命中和哪些未命中，然后报告命中和未命中总数。今天的许多微处理器都提供硬件来计算未命中次数和内存引用次数，这是一种更简单、更快捷的测量未命中率的方法。

The preceding formula is an approximation because the miss rates and miss penalties are often different for reads and writes. Memory stall clock cycles could then be defined in terms of the number of memory accesses per instruction, miss penalty (in clock cycles) for reads and writes, and miss rate for reads and writes:

> 前面的公式是一个近似值，因为读数和写入的错率和罚款通常不同。然后可以根据每次指令的内存访问数量，读取和写入的罚款(以时钟周期)的身份定义内存摊位时钟周期，以及读取和写入的率：

Example Assume we have a computer where the cycles per instruction (CPI) is 1.0 when all memory accesses hit in the cache. The only data accesses are loads and stores, and these total 50% of the instructions. If the miss penalty is 50 clock cycles and the miss rate is 1%, how much faster would the computer be if all instructions were cache hits?

> 示例假设我们有一台计算机，当所有内存访问在缓存中击中所有内存时，每指令(CPI)的循环为 1.0。唯一的数据访问是负载和存储，这些指令的总计 50％。如果罚款是 50 个时钟周期，而错率是 1％，那么如果所有说明均为缓存命中率，计算机的速度将越快？

The computer with no cache misses is 1.75 times faster.

> 没有缓存错过的计算机快 1.75 倍。

Some designers prefer measuring miss rate as _misses per instruction_ rather than misses per memory reference. These two are related:

> 一些设计师希望将遗漏率视为 _imisses 每个指令_ 而不是每个内存参考的错过。这两个相关：

The latter formula is useful when you know the average number of memory accesses per instruction because it allows you to convert miss rate into misses per instruction, and vice versa. For example, we can turn the miss rate per memory reference in the previous example into misses per instruction:

> 当您知道每次指令的平均内存访问数量时，后一个公式很有用，因为它允许您将失误率转换为每个说明的错过，反之亦然。例如，我们可以将上一个示例中每个内存参考的误率转换为每个指令的错过：

By the way, misses per instruction are often reported as misses per 1000 instructions to show integers instead of fractions. Thus, the preceding answer could also be expressed as 30 misses per 1000 instructions.

> 顺便说一句，每次指令的错过通常被报告为每 1000 个说明，以显示整数而不是分数。因此，前面的答案也可以表示为每 1000 条说明 30 次。

The advantage of misses per instruction is that it is independent of the hardware implementation. For example, speculative processors fetch about twice as many instructions as are actually committed, which can artificially reduce the miss rate if measured as misses per memory reference rather than per instruction. The draw- back is that misses per instruction is architecture dependent; for example, the aver- age number of memory accesses per instruction may be very different for an 80x86 versus RISC V. Thus, misses per instruction are most popular with architects work- ing with a single computer family, although the similarity of RISC architectures allows one to give insights into others.

> 通过指令的错过的优点是它独立于硬件实现。例如，投机处理器获取的指示大约是实际提交的两倍，如果以每次内存参考而不是通过指示为单位，则可以人为地降低错过率。回报是，每个指令的错过取决于架构。例如，对于 80x86 与 RISC V 相比，每次指令的平均内存访问数量可能会大不相同。因此，尽管 RISC 架构的相似性允许，但每次指令的遗漏最受欢迎，但在与单个计算机系列一起工作的工程师中最受欢迎一个人深入了解他人。

Example To show equivalency between the two miss rate equations, let’s redo the preceding example, this time assuming a miss rate per 1000 instructions of 30. What is memory stall time in terms of instruction count?

> 示例显示了两个失误方程之间的等效性，让我们重做前面的示例，这次假设每 1000 个指令的错率为 30。在指导数计数方面，内存失速时间是多少？

_Answer_ Recomputing the memory stall cycles:

We get the same answer as on page B-5, showing equivalence of the two equations.

> 我们得到与 B-5 页相同的答案，显示了两个方程的等效性。

### Four Memory Hierarchy Questions

We continue our introduction to caches by answering the four common questions for the first level of the memory hierarchy:

> 我们通过回答记忆层次结构的第一层的四个常见问题来继续对缓存的介绍：

Q1: Where can a block be placed in the upper level? (_block placement_)
Q2: How is a block found if it is in the upper level? (_block identification_)
Q3: Which block should be replaced on a miss? (_block replacement_)
Q4: What happens on a write? (_write strategy_)

The answers to these questions help us understand the different trade-offs of mem- ories at different levels of a hierarchy; hence, we ask these four questions on every example.

##### _Q1: Where Can a Block be Placed in a Cache?_

[Figure B.2](#_bookmark440) shows that the restrictions on where a block is placed create three categories of cache organization:

> [图 B.2](#_bookmark440) 显示对块放置位置的限制创建了三类缓存组织：

- If each block has only one place it can appear in the cache, the cache is said to be _direct mapped_. The mapping is usually (_Block address_) MOD (_Number of blocks in cache_)
- If a block can be placed anywhere in the cache, the cache is said to be _fully associative_.

> - 如果每个块只有一个地方可以出现在缓存中，则缓存被称为*直接映射*。映射通常是 (_Block address_) MOD (_Number of blocks in cache_)
> - 如果可以将一个块放置在缓存中的任何位置，则缓存称为*相关*。

Figure B.2 This example cache has eight block frames and memory has 32 blocks. The three options for caches are shown left to right. In fully associative, block 12 from the lower level can go into any of the eight block frames of the cache. With direct mapped, block 12 can only be placed into block frame 4 (12 modulo 8). Set associative, which has some of both features, allows the block to be placed anywhere in set 0 (12 modulo 4). With two blocks per set, this means block 12 can be placed either in block 0 or in block 1 of the cache. Real caches contain thousands of block frames, and real memories contain millions of blocks. The set associative organization has four sets with two blocks per set, called _two-way set associative_. Assume that there is nothing in the cache and that the block address in question identifies lower-level block 12.

> 图 B.2 这个示例缓存有 8 个块帧，内存有 32 个块。缓存的三个选项从左到右显示。在完全关联中，来自较低级别的块 12 可以进入缓存的八个块帧中的任何一个。使用直接映射，块 12 只能放入块帧 4(12 模 8)。Set associative 具有这两种特性中的一部分，允许将块放置在 set 0(12 modulo 4)中的任何位置。每组两个块，这意味着块 12 可以放置在缓存的块 0 或块 1 中。真正的缓存包含数千个块帧，而真正的存储器包含数百万个块。集合关联组织有四个集合，每个集合有两个块，称为*双向集合关联*。假设缓存中没有任何内容，并且所讨论的块地址标识较低级别的块 12。

- If a block can be placed in a restricted set of places in the cache, the cache is _set associative_.A _set_ is a group of blocks in the cache. A block is first mapped onto a set, and then the block can be placed anywhere within that set. The set is usu- ally chosen by _bit selection_; that is, (_Block address_) MOD (_Number of sets in cache_)

> - 如果一个块可以放置在缓存中一组受限制的地方，则缓存是 *set associative*。一个 *set* 是缓存中的一组块。一个块首先被映射到一个集合上，然后这个块可以被放置在该集合中的任何地方。该集合通常由*位选择*选择；即，(_块地址_)MOD(_缓存中的集合数_)

If there are _n_ blocks in a set, the cache placement is called _n-way set associative_.

> 如果一个集合中有 _n_ 个块，则缓存放置称为 _n-way set associative_。

The range of caches from direct mapped to fully associative is really a continuum of levels of set associativity. Direct mapped is simply one-way set associative, and a fully associative cache with _m_ blocks could be called "*m-*way set associative." Equivalently, direct mapped can be thought of as having _m_ sets, and fully associa- tive as having one set.

> 从直接映射到完全关联的缓存范围实际上是集合关联性级别的连续体。直接映射只是单向集相联，具有 _m_ 个块的完全相联高速缓存可以称为 "*m-*路集相联" 。等价地，直接映射可以被认为有* m* 个集合，而完全关联有一个集合。

The vast majority of processor caches today are direct mapped, two-way set associative, or four-way set associative, for reasons we will see shortly.

> 由于我们很快会看到的原因，当今绝大多数的处理器库都是直接映射，双向套装协会或四向套件的关联。

##### _Q2: How Is a Block Found If It Is in the Cache?_

Caches have an address tag on each block frame that gives the block address. The tag of every cache block that might contain the desired information is checked to see if it matches the block address from the processor. As a rule, all possible tags are searched in parallel because speed is critical.

> 缓存在每个块框架上都有一个地址标签，可以提供块地址。每个缓存块的标签都可能包含所需的信息，以查看它是否与处理器的块地址匹配。通常，由于速度至关重要，因此并行搜索所有可能的标签。

There must be a way to know that a cache block does not have valid informa- tion. The most common procedure is to add a _valid bit_ to the tag to say whether or not this entry contains a valid address. If the bit is not set, there cannot be a match on this address.

> 必须有一种方法知道缓存块没有有效的信息。最常见的过程是在标签中添加一个 *VALID 位*，以说明此条目是否包含有效地址。如果未设置位，则该地址无法匹配。

Before proceeding to the next question, let’s explore the relationship of a pro- cessor address to the cache. [Figure B.3](#_bookmark441) shows how an address is divided. The first division is between the _block address_ and the _block offset._ The block frame address can be further divided into the _tag field_ and the _index field._ The block offset field selects the desired data from the block, the index field selects the set, and the tag field is compared against it for a hit. Although the comparison could be made on more of the address than the tag, there is no need because of the following:

> 在继续下一个问题之前，让我们探讨一下处理器地址与缓存的关系。[图 B.3](#_bookmark441) 显示了一个地址是如何划分的。第一个划分是 _ block address _ 和 _ block offset。_ block frame address 可以进一步划分为 _ tag field _ 和 _ index field。_ block offset field 从 block 中选择想要的数据，index field 选择集合 , 并将标签字段与它进行比较以获得命中。尽管可以在比标签更多的地址上进行比较，但没有必要，因为以下原因：

- The offset should not be used in the comparison, because the entire block is present or not, and hence all block offsets result in a match by definition.

> - 在比较中不应使用偏移，因为整个块是否存在，因此所有块偏移都会导致匹配根据定义。

Figure B.3 The three portions of an address in a set associative or direct- mapped cache. The tag is used to check all the blocks in the set, and the index is used to select the set. The block offset is the address of the desired data within the block. Fully associative caches have no index field.

> 图 B.3 集合关联或直接映射缓存中的地址的三个部分。标签用于检查集合中的所有块，并使用索引来选择集合。块偏移是块内所需数据的地址。完全关联的缓存没有索引字段。

- Checking the index is redundant, because it was used to select the set to be checked. An address stored in set 0, for example, must have 0 in the index field or it couldn’t be stored in set 0; set 1 must have an index value of 1; and so on. This optimization saves hardware and power by reducing the width of memory size for the cache tag.

> - 检查索引是多余的，因为它用于选择要检查的集合。例如，在集合 0 中存储的地址必须在索引字段中具有 0，否则无法存储在集合 0 中；集 1 必须具有 1 个索引值；等等。这种优化通过减少缓存标签的内存宽度来节省硬件和电源。

If the total cache size is kept the same, increasing associativity increases the num- ber of blocks per set, thereby decreasing the size of the index and increasing the size of the tag. That is, the tag-index boundary in [Figure B.3](#_bookmark441) moves to the right with increasing associativity, with the end point of fully associative caches having no index field.

> 如果总缓存大小保持不变，则增加的关联性会增加每组块的数量，从而减少索引的大小并增加标签的大小。也就是说，[图 B.3](#_bookmark441) 中的标记索引边界随着关联性的增加而向右移动，完全关联缓存的终点没有索引字段。

##### _Q3: Which Block Should be Replaced on a Cache Miss?_

When a miss occurs, the cache controller must select a block to be replaced with the desired data. A benefit of direct-mapped placement is that hardware decisions are simplified—in fact, so simple that there is no choice: only one block frame is checked for a hit, and only that block can be replaced. With fully associative or set associative placement, there are many blocks to choose from on a miss. There are three primary strategies employed for selecting which block to replace:

> 当发生错过时，缓存控制器必须选择一个要替换所需数据的块。直接映射放置的一个好处是，简化了硬件决策 - 实际上，别无选择：仅检查一个块框架以进行击球，只能更换该块。有了完全的关联或设定的关联位置，错过了许多块可供选择。有三种主要策略用于选择要替换的块：

- _Random_—To spread allocation uniformly, candidate blocks are randomly selected. Some systems generate pseudorandom block numbers to get repro- ducible behavior, which is particularly useful when debugging hardware.

> - _random_ - 为了统一分配，随机选择了候选块。一些系统会生成伪 andom 块数字以获得可辨认行为，这在调试硬件时特别有用。

- _Least recently used_ (LRU)—To reduce the chance of throwing out information that will be needed soon, accesses to blocks are recorded. Relying on the past to predict the future, the block replaced is the one that has been unused for the longest time. LRU relies on a corollary of locality: if recently used blocks are likely to be used again, then a good candidate for disposal is the least recently used block.

> - _最近最少使用_ (LRU)——为了减少丢弃即将需要的信息的机会，记录了对块的访问。靠过去预测未来，被替换的区块是最久未被使用的区块。LRU 依赖于局部性的推论：如果最近使用过的块可能会再次使用，那么最近最少使用的块是一个很好的处理候选对象。

- _First in, first out_ (FIFO)—Because LRU can be complicated to calculate, this approximates LRU by determining the _oldest_ block rather than the LRU.

> - _先进先出_(FIFO)——因为 LRU 的计算可能很复杂，这通过确定最旧的块而不是 LRU 来近似 LRU。

A virtue of random replacement is that it is simple to build in hardware. As the number of blocks to keep track of increases, LRU becomes increasingly expensive and is usually only approximated. A common approximation (often called pseudo- LRU) has a set of bits for each set in the cache with each bit corresponding to a single way (a _way_ is bank in a set associative cache; there are four ways in four-way set associative cache) in the cache. When a set is accessed, the bit corre- sponding to the way containing the desired block is turned on; if all the bits asso- ciated with a set are turned on, they are reset with the exception of the most recently turned on bit. When a block must be replaced, the processor chooses a block from the way whose bit is turned off, often randomly if more than one choice is available. This approximates LRU, because the block that is replaced will not have been accessed since the last time that all the blocks in the set were accessed. [Figure B.4](#_bookmark442) shows the difference in miss rates between LRU, random, and FIFO replacement.

> 随机替换的一个优点是在硬件中构建起来很简单。随着要跟踪的块数的增加，LRU 变得越来越昂贵并且通常只是近似的。一个常见的近似(通常称为伪 LRU)对缓存中的每个集合都有一组位，每个位对应一个路(一个*路*是集合关联缓存中的库；四路集合关联中有四个路 缓存)在缓存中。当访问一个集合时，对应于包含所需块的路的位被打开；如果与一个集合关联的所有位都已打开，则除最近打开的位外，它们都将被重置。当一个块必须被替换时，处理器从其位被关闭的方式中选择一个块，如果有多个选择可用，通常是随机的。这近似于 LRU，因为被替换的块自上次访问集合中的所有块以来将不会被访问过。[图 B.4](#_bookmark442) 显示了 LRU、随机和 FIFO 替换之间的未命中率差异。

Figure B.4 Data cache misses per 1000 instructions comparing least recently used, random, and first in, first out replacement for several sizes and associativities. There is little difference between LRU and random for the largest size cache, with LRU outperforming the others for smaller caches. FIFO generally outperforms random in the smaller cache sizes. These data were collected for a block size of 64 bytes for the Alpha architecture using 10 SPEC2000 benchmarks. Five are from SPECint2000 (gap, gcc, gzip, mcf, and perl) and five are from SPECfp2000 (applu, art, equake, lucas, and swim). We will use this computer and these benchmarks in most figures in this appendix.

> 图 B.4 每 1000 条指令的数据高速缓存未命中数比较了最近最少使用、随机和先进先出替换的几种大小和关联性。对于最大大小的缓存，LRU 和随机之间几乎没有区别，对于较小的缓存，LRU 优于其他缓存。FIFO 在较小的高速缓存大小中通常优于随机。这些数据是使用 10 个 SPEC2000 基准针对 Alpha 架构的 64 字节块大小收集的。五个来自 SPECint2000(gap、gcc、gzip、mcf 和 perl)，五个来自 SPECfp2000(applu、art、equake、lucas 和 swim)。我们将在本附录的大多数图中使用这台计算机和这些基准。

##### _Q4: What Happens on a Write?_

Reads dominate processor cache accesses. All instruction accesses are reads, and most instructions don’t write to memory. Figures A.32 and A.33 in [Appendix A](#_bookmark391) suggest a mix of 10% stores and 26% loads for RISC V programs, making writes 10%/(100% + 26% + 10%) or about 7% of the overall memory traffic. Of the _data cache_ traffic, writes are 10%/(26% + 10%) or about 28%. Making the common case fast means optimizing caches for reads, especially because processors traditionally wait for reads to complete but need not wait for writes. Amdahl’s Law (Section 1.9) reminds us, however, that high-performance designs cannot neglect the speed of writes.

> 读取主导的处理器缓存访问。所有指令访问均为读取，大多数指令都不会写入内存。图 A.32 和 A.33 在[附录 A](#_bookmark391) 中建议使用 10％的存储和 26％的 RISC V 程序负载，使其写入 10％/(100％ + 26％ + 10％)或大约 7％的整体内存流量。在 *data Cache* 流量中，写入为 10％/(26％ + 10％)或约 28％。快速使常见的情况意味着要优化读取的缓存，尤其是因为处理器传统上等待读取完成，但不必等待写作。但是，Amdahl 的定律(第 1.9 节)提醒我们，高性能设计不能忽略写作速度。

Fortunately, the common case is also the easy case to make fast. The block can be read from the cache at the same time that the tag is read and compared, so the block read begins as soon as the block address is available. If the read is a hit, the requested part of the block is passed on to the processor immediately. If it is a miss, there is no benefit—but also no harm except more power in desktop and server computers; just ignore the value read.

> 幸运的是，常见的情况也是快速制作的简单案例。可以在读取和比较标签的同时从缓存中读取该块，因此，块读取后，块读取一旦可用。如果读取是命中的，则该块的要求部分将立即传递给处理器。如果是错过的，则没有任何好处，但除了台式机和服务器计算机中的更多功率外，也没有任何伤害；只需忽略读取的值即可。

Such optimism is not allowed for writes. Modifying a block cannot begin until the tag is checked to see if the address is a hit. Because tag checking cannot occur in parallel, writes usually take longer than reads. Another complexity is that the pro- cessor also specifies the size of the write, usually between 1 and 8 bytes; only that portion of a block can be changed. In contrast, reads can access more bytes than necessary without fear.

> 不允许写入这种乐观。在检查标签以查看地址是否为命中之前，修改块无法开始。由于标签检查不能并行进行，因此写入通常需要比读取更长的时间。另一个复杂性是，专业人士还指定了写入的大小，通常在 1 到 8 个字节之间；只能更改块的部分。相比之下，读取可以访问更多的字节，而无需恐惧。

The write policies often distinguish cache designs. There are two basic options when writing to the cache:

> 写政策通常会区分缓存设计。写入缓存时有两个基本选项：

- _Write through_—The information is written to both the block in the cache _and_ to the block in the lower-level memory.
- _Write back_—The information is written only to the block in the cache. The modified cache block is written to main memory only when it is replaced.

> - _write thy_-信息均写入缓存中的两个块*和*的块至下层内存中的块。
> - _write back_-信息仅写入缓存中的块。仅在更换后，仅将修改后的缓存块写入主内存。

To reduce the frequency of writing back blocks on replacement, a feature called the _dirty bit_ is commonly used. This status bit indicates whether the block is _dirty_ (modified while in the cache) or _clean_ (not modified). If it is clean, the block is not written back on a miss, because identical information to the cache is found in lower levels.

> 为了减少替换时编写后块的频率，通常使用称为* dirty 的功能。此状态位指示块是\_dirty*(在缓存中修改)还是 *CLEAN*(未修改)。如果干净，则不会将块写回错过，因为与缓存相同的信息在较低的级别中找到。

Both write back and write through have their advantages. With write back, writes occur at the speed of the cache memory, and multiple writes within a block require only one write to the lower-level memory. Because some writes don’t go to memory, write back uses less memory bandwidth, making write back attractive in multiprocessors. Since write back uses the rest of the memory hierarchy and mem- ory interconnect less than write through, it also saves power, making it attractive for embedded applications.

> 两者都回信和写作具有优势。通过写回去，写入以缓存内存的速度出现，并且在一个块中的多个写入只需要一个写入较低级别的内存。由于有些人写的不转到内存，所以回信使用较少的内存带宽，从而在多处理器中引人入胜。由于写回去使用其余内存层次结构和内存互连，而不是写入，也可以节省功率，从而使其对嵌入式应用程序有吸引力。

Write through is easier to implement than write back. The cache is always clean, so unlike write back read misses never result in writes to the lower level. Write through also has the advantage that the next lower level has the most current copy of the data, which simplifies data coherency. Data coherency is important for multiprocessors and for I/O, which we examine in [Chapter 4](#_bookmark165) and Appendix D. Multilevel caches make write through more viable for the upper-level caches, as the writes need only propagate to the next lower level rather than all the way to main memory.

> 与写回去更容易实现。缓存总是很干净，因此与写回读物不同，从未导致写入较低级别。通过写入还具有一个优势，即下一个较低级别具有数据的最新副本，从而简化了数据相干性。数据相干性对于多处理器和 I/O 很重要，我们在[第 4 章](#_bookmark165)和附录 D.多级缓存中对此进行了研究，使得通过写作更为可行，因为该写作只需要传播到该文章中下一个较低级别，而不是一直到主内存。

As we will see, I/O and multiprocessors are fickle: they want write back for processor caches to reduce the memory traffic and write through to keep the cache consistent with lower levels of the memory hierarchy.

> 正如我们将看到的那样，I/O 和多处理器是善变的：他们希望写回处理器缓存以减少内存流量并写入以保持缓存与较低的内存层次结构一致。

When the processor must wait for writes to complete during write through, the processor is said to _write stall._ A common optimization to reduce write stalls is a _write buffer,_ which allows the processor to continue as soon as the data are written to the buffer, thereby overlapping processor execution with memory updating. As we will see shortly, write stalls can occur even with write buffers.

> 当处理器必须等待写入在写入过程中完成时，将处理器说到\_write Stall。缓冲区，从而与内存更新重叠的处理器执行。正如我们将不久所见，即使写作缓冲区也可能发生写摊位。

Because the data are not needed on a write, there are two options on a write miss:

> 由于写入不需要数据，因此写入遗漏有两个选项：

- _Write allocate_—The block is allocated on a write miss, followed by the preceding write hit actions. In this natural option, write misses act like read misses.
- _No-write allocate_—This apparently unusual alternative is write misses do _not_ affect the cache. Instead, the block is modified only in the lower-level memory.

Thus, blocks stay out of the cache in no-write allocate until the program tries to read the blocks, but even blocks that are only written will still be in the cache with write allocate. Let’s look at an example.

> 因此，在程序试图读取块之前，块在无编写的情况下远离缓存，但即使仅编写的块，也仍然会随着写入分配而在缓存中。让我们看看一个例子。

Example Assume a fully associative write-back cache with many cache entries that starts empty. Following is a sequence of five memory operations (the address is in square brackets):

> 示例假设一个完全关联的书面缓存，并具有许多开始空的缓存条目。以下是五个内存操作的序列(地址在方括号中)：

What are the number of hits and misses when using no-write allocate versus write allocate?

> 使用 NoWrite 分配与写入分配时，命中和错过的数量是多少？

_Answer_ For no-write allocate, the address 100 is not in the cache, and there is no allocation on write, so the first two writes will result in misses. Address 200 is also not in the cache, so the read is also a miss. The subsequent write to address 200 is a hit. The last write to 100 is still a miss. The result for no-write allocate is four misses and one hit.

> *ANSWER* 对于无编码，地址 100 不在缓存中，并且写入没有分配，因此前两个写入将导致错过。地址 200 也不在缓存中，因此读取也是一个错过的。随后的写入地址 200 是一个成功。最后一位写入 100 仍然是错过的。不分配的结果是四个失误和一人。

For write allocate, the first accesses to 100 and 200 are misses, and the rest are hits because 100 and 200 are both found in the cache. Thus, the result for write allocate is two misses and three hits.

> 对于写入分配，第一个访问 100 和 200 的访问是错过的，其余的是命中，因为在缓存中都发现了 100 和 200。因此，写入分配的结果是两个失误和三击。

Either write miss policy could be used with write through or write back. Usually, write-back caches use write allocate, hoping that subsequent writes to that block will be captured by the cache. Write-through caches often use no-write allo- cate. The reasoning is that even if there are subsequent writes to that block, the writes must still go to the lower-level memory, so what’s to be gained?

> 要么写策略小姐可以与写作或写回来。通常，写下的缓存使用写入分配，希望随后的写入该块将被缓存捕获。写入缓存通常不使用不介绍。原因是，即使随后写给该块的写作，写作仍然必须转到低级记忆，那么要获得什么？

### An Example: The Opteron Data Cache

To give substance to these ideas, [Figure B.5](#_bookmark443) shows the organization of the data cache in the AMD Opteron microprocessor. The cache contains 65,536 (64 K) bytes of data in 64-byte blocks with two-way set associative placement, least- recently used replacement, write back, and write allocate on a write miss.

> 为了给这些想法提供实质，[图 B.5](#_bookmark443) 显示了 AMD Opteron 微处理器中数据缓存的组织。该缓存包含 64 个字节块中的 65,536(64 K)字节，具有双向设置的关联位置，最近使用的最小使用的替换，写回来并在写入错过上写入分配。

Let’s trace a cache hit through the steps of a hit as labeled in [Figure B.5](#_bookmark443). (The four steps are shown as circled numbers.) As described in [Section B.5](#protection-and-examples-of-virtual-memory), the Opteron presents a 48-bit virtual address to the cache for tag comparison, which is simul- taneously translated into a 40-bit physical address.

> 让我们跟踪一个缓存，击中了[图 B.5]中标记的命中步骤(#\_bookmark443)。(四个步骤显示为循环数字。)如 [B.5 节](%EF%BC%83%E4%BF%9D%E6%8A%A4%E5%92%8C%E7%A4%BA%E4%BE%8B%E7%9A%84%E6%A1%88%E4%BE%8B%E8%AE%B0%E5%BF%86)中所述)，Opteron 向标记比较提供了一个 48 位虚拟地址，以进行标记比较，将其类似地翻译成 40 位物理地址。

The reason Opteron doesn’t use all 64 bits of virtual address is that its designers don’t think anyone needs that much virtual address space yet, and the smaller size simplifies the Opteron virtual address mapping. The designers plan to grow the virtual address in future microprocessors.

> Opteron 不使用所有 64 位虚拟地址的原因是，其设计师认为任何人都不需要太多的虚拟地址空间，而较小的尺寸简化了 Opteron 虚拟地址映射。设计师计划在未来的微处理器中发展虚拟地址。

The physical address coming into the cache is divided into two fields: the 34-bit block address and the 6-bit block offset (64 = 2 and 34 + 6 = 40). The block address is further divided into an address tag and cache index. Step 1 shows this division.

> 进入缓存的物理地址分为两个字段：34 位块地址和 6 位块偏移(64 = 2 和 34 + 6 = 40)。块地址进一步分为地址标签和缓存索引。步骤 1 显示了这个分区。

![](../media/image473.png)

Figure B.5 The organization of the data cache in the Opteron microprocessor. The 64 KiB cache is two-way set associative with 64-byte blocks. The 9-bit index selects among 512 sets. The four steps of a read hit, shown as circled numbers in order of occurrence, label this organization. Three bits of the block offset join the index to supply the RAM address to select the proper 8 bytes. Thus, the cache holds two groups of 4096 64-bit words, with each group containing half of the 512 sets. Although not exercised in this example, the line from lower-level memory to the cache is used on a miss to load the cache. The size of address leaving the processor is 40 bits because it is a physical address and not a virtual address. [Figure B.24](#_bookmark464) on page B-47 explains how the Opteron maps from virtual to physical for a cache access.

> 图 B.5 Opteron 微处理器中数据缓存的组织。64 KIB 缓存是带有 64 字节块的双向套件联合。9 位索引在 512 组中选择。读取的四个步骤，按照发生的顺序显示为循环数字，标记该组织。块偏移的三个位加入索引，以提供 RAM 地址以选择适当的 8 个字节。因此，缓存包含两组 4096 64 位单词，每组包含 512 组中的一半。尽管在此示例中不进行操作，但从低级内存到缓存的线路被使用在遗漏上加载缓存。离开处理器的地址的大小为 40 位，因为它是物理地址，而不是虚拟地址。[图 B.24](#_bookmark464) 在 B-47 上说明了从虚拟到物理访问的 opteron 映射如何用于缓存访问。

The cache index selects the tag to be tested to see if the desired block is in the cache. The size of the index depends on cache size, block size, and set associativity. For the Opteron cache the set associativity is set to two, and we calculate the index as follows:

> 缓存索引选择要测试的标签，以查看所需的块是否在缓存中。索引的大小取决于缓存大小，块大小和设置关联性。对于 Opteron 缓存，集合关联设置为两个，我们计算索引如下：

Hence, the index is 9 bits wide, and the tag is 34 9 or 25 bits wide. Although that is the index needed to select the proper block, 64 bytes is much more than the pro- cessor wants to consume at once. Hence, it makes more sense to organize the data portion of the cache memory 8 bytes wide, which is the natural data word of the 64- bit Opteron processor. Thus, in addition to 9 bits to index the proper cache block, 3 more bits from the block offset are used to index the proper 8 bytes. Index selection is step 2 in [Figure B.5](#_bookmark443).

> 因此，索引宽 9 位，标签为 34 9 或 25 位宽。尽管这是选择合适块所需的索引，但 64 个字节比专业人士要一次消费的索引要多得多。因此，组织缓存内存 8 字节宽的数据部分是更有意义的，这是 64 位 Opteron 处理器的自然数据单词。因此，除了 9 位索引适当的缓存块外，块偏移量的另外 3 位用于索引正确的 8 个字节。索引选择是[图 B.5]中的步骤 2(#\_bookmark443)。

After reading the two tags from the cache, they are compared with the tag por- tion of the block address from the processor. This comparison is step 3 in the figure. To be sure the tag contains valid information, the valid bit must be set or else the results of the comparison are ignored.

> 在读取高速缓存的两个标签后，将它们与处理器的块地址的标签进行了比较。此比较是图中的步骤 3。为了确保标签包含有效的信息，必须设置有效的位，否则比较的结果将被忽略。

Assuming one tag does match, the final step is to signal the processor to load the proper data from the cache by using the winning input from a 2:1 mul- tiplexor. The Opteron allows 2 clock cycles for these four steps, so the instruc- tions in the following 2 clock cycles would wait if they tried to use the result of the load.

> 假设一个标签确实匹配，最后一步是通过使用 2：1 mul-tiplexor 的获奖输入来向处理器发信号，以加载缓存的正确数据。Opteron 允许在这四个步骤中进行 2 个时钟循环，因此，如果他们试图使用负载的结果，则以下 2 个时钟周期中的仪器将等待。

Handling writes is more complicated than handling reads in the Opteron, as it is in any cache. If the word to be written is in the cache, the first three steps are the same. Because the Opteron executes out of order, only after it signals that the instruction has committed and the cache tag comparison indicates a hit are the data written to the cache.

> 处理写作比在任何缓存中像在 Opteron 中的处理读取更为复杂。如果要写的单词在缓存中，则前三个步骤是相同的。由于 Opteron 执行了订单，只有在信号表示指令已提交并且缓存标签比较表明命中是写入缓存到缓存的数据之后。

So far we have assumed the common case of a cache hit. What happens on a miss? On a read miss, the cache sends a signal to the processor telling it the data are not yet available, and 64 bytes are read from the next level of the hierarchy. The latency is 7 clock cycles to the first 8 bytes of the block, and then 2 clock cycles per 8 bytes for the rest of the block. Because the data cache is set associa- tive, there is a choice on which block to replace. Opteron uses LRU, which selects the block that was referenced longest ago, so every access must update the LRU bit. Replacing a block means updating the data, the address tag, the valid bit, and the LRU bit.

> 到目前为止，我们已经假设了缓存命中的常见情况。小姐会发生什么？在读取错误中，缓存将信号发送给处理器，告知其数据尚未可用，并且从层次结构的下一个级别读取了 64 个字节。延迟是块的前 8 个字节的 7 个时钟循环，然后在块的其余部分中每 8 个字节 2 个时钟循环。由于数据缓存设置为关联，因此可以选择替换哪个块。Opteron 使用 LRU，该 LRU 选择了最早引用的块，因此每个访问都必须更新 LRU 位。更换块意味着更新数据，地址标签，有效位和 LRU 位。

Because the Opteron uses write back, the old data block could have been mod- ified, and hence it cannot simply be discarded. The Opteron keeps 1 dirty bit per block to record if the block was written. If the "victim" was modified, its data and address are sent to the victim buffer. (This structure is similar to a _write buffer_ in other computers.) The Opteron has space for eight victim blocks. In parallel with other cache actions, it writes victim blocks to the next level of the hierarchy. If the victim buffer is full, the cache must wait.

> 由于 Opteron 使用写回去，因此旧的数据块可能已经进行了模拟，因此不能简单地丢弃它。Opteron 在每个块中保持 1 个肮脏的位，以记录该块是否编写。如果修改了 "受害者" ，则将其数据和地址发送到受害者缓冲区。(此结构类似于其他计算机中的 *write Buffer*。)Opteron 具有八个受害者块的空间。它与其他缓存动作并行，将受害者块写入层次结构的下一个层次。如果受害者缓冲区已满，则缓存必须等待。

A write miss is very similar to a read miss, because the Opteron allocates a block on a read or a write miss.

> 写作错过与读物的失误非常相似，因为 Opteron 在读取或写入错过上分配了一个块。

We have seen how it works, but the _data_ cache cannot supply all the memory needs of the processor: the processor also needs instructions. Although a single cache could try to supply both, it can be a bottleneck. For example, when a load or store instruction is executed, the pipelined processor will simultaneously request both a data word _and_ an instruction word. Hence, a single cache would present a structural hazard for loads and stores, leading to stalls. One simple way to conquer this problem is to divide it: one cache is dedicated to instructions and another to data. Separate caches are found in most recent processors, including the Opteron. Hence, it has a 64 KiB instruction cache as well as the 64 KiB data cache.

> 我们已经看到了它的工作原理，但是 *data* 缓存无法满足处理器的所有内存需求：处理器还需要说明。尽管单个缓存可以尝试两者都提供，但它可能是瓶颈。例如，执行加载或存储指令时，管道处理的处理器将同时请求两个数据词*和*指令字。因此，单个缓存将对负载和存储产生结构性危害，导致摊位。征服此问题的一种简单方法是将其划分：一个缓存专用于指令，另一个缓存用于数据。在包括 Opteron 在内的最新处理器中发现了单独的缓存。因此，它具有 64 KIB 指令缓存以及 64 KIB 数据缓存。

Figure B.6 Miss per 1000 instructions for instruction, data, and unified caches of different sizes. The percentage of instruction references is about 74%. The data are for two-way associative caches with 64-byte blocks for the same computer and bench- marks as [Figure B.4](#_bookmark442).

> 图 B.6 每 1000 个指令，有关指令，数据和不同尺寸的统一缓存的说明。指导参考的百分比约为 74％。这些数据适用于与[图 B.4](#_bookmark442) 的同一计算机和基准标记的双向关联缓存。

The processor knows whether it is issuing an instruction address or a data address, so there can be separate ports for both, thereby doubling the bandwidth between the memory hierarchy and the processor. Separate caches also offer the opportunity of optimizing each cache separately: different capacities, block sizes, and associativities may lead to better performance. (In contrast to the instruction caches and data caches of the Opteron, the terms _unified_ or _mixed_ are applied to caches that can contain either instructions or data.)

> 处理器知道它是在发出指令地址还是数据地址，因此两者都有单独的端口，从而使内存层次结构和处理器之间的带宽增加一倍。单独的缓存还提供了分别优化每个缓存的机会：不同的能力，大小和关联性可能会带来更好的性能。(与 Opteron 的指令缓存和数据缓存相反，术语 *unified* 或 *mixed* 应用于可以包含指令或数据的缓存。)

[Figure B.6](#_bookmark444) shows that instruction caches have lower miss rates than data caches. Separating instructions and data removes misses due to conflicts between instruction blocks and data blocks, but the split also fixes the cache space devoted to each type. Which is more important to miss rates? A fair comparison of separate instruction and data caches to unified caches requires the total cache size to be the same. For example, a separate 16 KiB instruction cache and 16 KiB data cache should be compared with a 32 KiB unified cache. Calculating the average miss rate with separate instruction and data caches necessitates knowing the percentage of memory references to each cache. From the data in [Appendix A](#_bookmark391) we find the split is 100%/(100% + 26% + 10%) or about 74% instruction references to (26% + 10%)/ (100% + 26% + 10%) or about 26% data references. Splitting affects performance beyond what is indicated by the change in miss rates, as we will see shortly.

> [图 B.6](#_bookmark444) 表明，指令缓存的错率低于数据缓存。分开指令和数据由于指令块和数据块之间的冲突而删除了错过，但是拆分还修复了专用于每种类型的缓存空间。哪个对错价更重要？单独的指令和数据缓存与统一缓存的公平比较要求总缓存大小相同。例如，应将单独的 16 KIB 指令缓存和 16 KIB 数据缓存与 32 KIB 统一的缓存进行比较。通过单独的指令和数据缓存来计算平均错率，需要了解每个缓存的内存引用的百分比。从[附录 a](#_bookmark391) 中的数据中，我们发现拆分为 100％/(100％ + 26％ + 10％)或约 74％的指令引用(26％ + 10％)/(100％ + 26％ + 10％)或大约 26％的数据参考。分裂会影响绩效超出我们不久的时候错过的变化所表明的变化。
