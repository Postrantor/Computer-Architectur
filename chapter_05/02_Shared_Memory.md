## Centralized Shared-Memory Architectures

The observation that the use of large, multilevel caches can substantially reduce the memory bandwidth demands of a processor is the key insight that motivates centralized memory multiprocessors. Originally, these processors were all single-core and often took an entire board, and memory was located on a shared bus. With more recent, higher-performance processors, the memory demands have outstripped the capability of reasonable buses, and recent microprocessors directly connect memory to a single chip, which is sometimes called a _backside_ or _memory bus_ to distinguish it from the bus used to connect to I/O. Accessing a chip’s local memory whether for an I/O operation or for an access from another chip requires going through the chip that "owns" that memory. Thus access to memory is asymmetric: faster to the local memory and slower to the remote memory. In a multicore that memory is shared among all the cores on a single chip, but the asymmetric access to the memory of one multicore from the memory of another usually remains.

> 观察到大型多级缓存可以大大减少处理器的内存带宽需求的观察是激励集中记忆多处理器的关键见解。最初，这些处理器都是单核，经常拿起整个板，并且内存位于共享的 Bus 上。凭借最新的，高性能的处理器，**内存需求超过了合理的总线的能力**，而最近的微处理器直接将内存连接到单个芯片，有时称为 *backside* 或 *memory Bus*，以将其与用于连接到连接到连接的总线我/o。无论是用于 I/O 操作还是从另一个芯片访问芯片的本地内存，都需要经过 "拥有" 该内存的芯片。因此，对内存的访问是不对称的：对本地内存的速度更快，对远程内存较慢。在一个多核心中，单个芯片上的所有内核之间共享内存，但是通常仍然存在对一个多核心的内存的不对称访问，通常仍然存在。

Symmetric shared-memory machines usually support the caching of both shared and private data. _Private data_ are used by a single processor, while _shared data_ are used by multiple processors, essentially providing communication among the processors through reads and writes of the shared data. When a private item is cached, its location is migrated to the cache, reducing the average access time as well as the memory bandwidth required. Because no other processor uses the data, the program behavior is identical to that in a uniprocessor. When shared data are cached, the shared value may be replicated in multiple caches. In addition to the reduction in access latency and required memory bandwidth, this replication also provides a reduction in contention that may exist for shared data items that are being read by multiple processors simultaneously. Caching of shared data, however, introduces a new problem: cache coherence.

> 对称共享内存机器通常支持共享和私人数据的缓存。* private data *由单个处理器使用，而* SHARED 数据*则由多个处理器使用，从本质上讲，通过读取和写入共享数据，从而在处理器之间提供通信。当私人物品被缓存时，其位置会迁移到缓存，减少了平均访问时间以及所需的内存带宽。由于没有其他处理器使用数据，因此程序行为与单层处理器中的行为相同。缓存共享数据时，共享值可以在多个缓存中复制。除了减少访问延迟和所需的内存带宽外，此复制还提供了降低的争论，这对于共享数据项可能存在，这些数据项同时读取了多个处理器。但是，共享数据的缓存引入了一个新问题：**缓存连贯性**。

### What Is Multiprocessor Cache Coherence?

Unfortunately, caching shared data introduces a new problem. Because the view of memory held by two different processors is through their individual caches, the processors could end up seeing different values for the same memory location, as [Figure 5.3](#_bookmark219) illustrates. This difficulty is generally referred to as the _cache coherence problem_. Notice that the coherence problem exists because we have both a global state, defined primarily by the main memory, and a local state, defined by the individual caches, which are private to each processor core. Thus, in a multicore where some level of caching may be shared (e.g., an L3), although some levels are private (e.g., L1 and L2), the coherence problem still exists and must be solved. Informally, we could say that a memory system is coherent if any read of a data item returns the most recently written value of that data item. This definition, although intuitively appealing, is vague and simplistic; the reality is much more complex. This simple definition contains two different aspects of memory system behavior, both of which are critical to writing correct shared-memory programs. The first aspect, called _coherence_, defines what values can be returned by a read. The second aspect, called _consistency_, determines when a written value will be returned by a read. Let’s look at coherence first.

> 不幸的是，缓存共享数据引入了一个新问题。由于两个不同处理器持有的内存视图是通过其各自的缓存，因此处理器最终可能会看到同一内存位置的不同值，如[图 5.3](#_bookmark219) 所示。这个困难通常称为 *cache 连贯性问题*。请注意，连贯问题之所以存在，是因为我们既有一个全球状态，主要由主内存定义，也是由每个处理器核心私有的单个缓存定义的局部状态。因此，在可以共享某种级别的缓存(例如，L3)的多核心中，尽管某些级别是私有的(例如 L1 和 L2)，但仍然存在相干问题，必须解决。非正式地，我们可以说，如果任何读取数据项返回该数据项的最新书面值，则存储系统是连贯的。这个定义虽然具有直观的吸引力，但却是模糊而简单的。现实要复杂得多。这个简单的定义包含内存系统行为的两个不同方面，这两个方面对于编写正确的共享内存程序至关重要。第一个方面称为 *coherence*，定义了读取可以返回的值。第二个方面称为 *consistency*，确定读取何时将由读取返回书面值。让我们先看看连贯性。

Figure 5.3 The cache coherence problem for a single memory location (X), read and written by two processors (A and B). We initially assume that neither cache contains the variable and that X has the value 1. We also assume a writethrough cache; a write-back cache adds some additional but similar complications. After the value of X has been written by A, A’s cache and the memory both contain the new value, but B’s cache does not, and if B reads the value of X it will receive 1!

> 图 5.3 单个内存位置(X)的缓存相干问题，由两个处理器(A 和 B)读取和编写。我们最初假设两个缓存都不包含变量，并且 x 具有值 1。我们还假设一个 writeThrough 缓存。写下缓存增加了一些其他但类似的并发症。在由 A 编写 X 的值之后，A 的缓存和内存都包含新值，但是 B 的缓存却没有，并且如果 B 读取 X 的值，则将获得 1 个！

A memory system is coherent if

1. A read by processor P to location X that follows a write by P to X, with no writes of X by another processor occurring between the write and the read by P, always returns the value written by P.
2. A read by a processor to location X that follows a write by another processor to X returns the written value if the read and write are sufficiently separated in time and no other writes to X occur between the two accesses.
3. Writes to the same location are _serialized_; that is, two writes to the same location by any two processors are seen in the same order by all processors. For example, if the values 1 and then 2 are written to a location, processors can never read the value of the location as 2 and then later read it as 1.

> 如果存储系统连贯
>
> 1. 由处理器 P 到位置 X 的读取，遵循 P 到 X 的写入，而在写入和读取之间的另一个处理器的写入和 P 的读写中，请始终返回 P。
> 2. 处理器读取位置 X 的读取，遵循另一个处理器的写入 X 的写入，如果读取和写入足够分开，并且在两个访问之间没有其他写入 x。
> 3. 写入同一位置是* serialized*;也就是说，所有处理器都以相同顺序看到两个处理器的两个处理器写入同一位置。例如，如果值 1，然后将 2 写入位置，则处理器永远无法将位置的值读取为 2，然后将其读取为 1。

The first property simply preserves program order—we expect this property to be true even in uniprocessors. The second property defines the notion of what it means to have a coherent view of memory: if a processor could continuously read an old data value, we would clearly say that memory was incoherent.

> 第一个属性只是保留了程序订单 - 我们期望该属性即使在单层处理器中也是如此。第二个属性定义了具有连贯的内存视图含义的概念：如果处理器可以不断读取旧数据值，我们会清楚地说内存是不连贯的。

The need for write serialization is more subtle, but equally important. Suppose we did not serialize writes, and processor P1 writes location X followed by P2 writing location X. Serializing the writes ensures that every processor will see the write done by P2 at some point. If we did not serialize the writes, it might be the case that some processors could see the write of P2 first and then see the write of P1, maintaining the value written by P1 indefinitely. The simplest way to avoid such difficulties is to ensure that all writes to the same location are seen in the same order; this property is called _write serialization_.

> 写入序列化的需求更加微妙，但同样重要。假设我们没有序列化写入，而处理器 P1 则写入位置 X，然后是 P2 写作位置 X。序列化该写作确保每个处理器都会在某个时候看到 P2 完成的写入。如果我们不序列化写作，则可能是某些处理器可以先看到 P2 的写入，然后看到 P1 的写入，并无限期地维护 P1 写的值。避免这种困难的最简单方法是确保所有写入相同位置的写入都以相同的顺序看到；此属性称为 *write serialization*。

Although the three properties just described are sufficient to ensure coherence, the question of when a written value will be seen is also important. To see why, observe that we cannot require that a read of X instantaneously see the value written for X by some other processor. If, for example, a write of X on one processor precedes a read of X on another processor by a very small time, it may be impossible to ensure that the read returns the value of the data written, since the written data may not even have left the processor at that point. The issue of exactly _when_ a written value must be seen by a reader is defined by a _memory consistency model_—a topic discussed in [Section 5.6](#models-of-memory-consistency-an-introduction).

> 尽管刚刚描述的三个属性足以确保连贯性，但何时将看到书面值的问题也很重要。要了解原因，请注意，我们不需要读取 X 即时看到其他处理器为 X 编写的值。例如，如果一个处理器上的 x 写入 x 在另一个处理器上的 X 读取之前，则可能无法确保读取返回书面数据的值，因为书面数据甚至可能不会那时离开了处理器。*when *的问题必须由读者看到书面值，由* memory 一致性模型*定义 - [第 5.6 节]中讨论的主题(＃符合模型 - 符合性的 and introduction)。

Coherence and consistency are complementary: _Coherence defines the behavior of reads and writes to the same memory location, while consistency defines the behavior of reads and writes with respect to accesses to other memory locations_. For now, make the following two assumptions. First, a write does not complete (and allow the next write to occur) until all processors have seen the effect of that write. Second, the processor does not change the order of any write with respect to any other memory access. These two conditions mean that, if a processor writes location A followed by location B, any processor that sees the new value of B must also see the new value of A. These restrictions allow the processor to reorder reads, but forces the processor to finish a write in program order. We will rely on this assumption until we reach [Section 5.6](#models-of-memory-consistency-an-introduction), where we will see exactly the implications of this definition, as well as the alternatives.

> 连贯性和一致性是互补的：_coherence 定义了读取和写入相同的内存位置的行为，而一致性则定义了读取和写入有关访问其他内存位置的行为。现在，做以下两个假设。
> 首先，在所有处理器都看到该写作的效果之前，写入还没有完成(并允许下一个写作)。
> 其次，处理器不会更改有关任何其他内存访问的任何写入顺序。这两个条件意味着，如果处理器写入位置 A 后面是位置 B，则任何看到 B 的新值的处理器也必须看到 A 的新值 A。这些限制允许处理器重新排序，但迫使处理器完成按程序顺序写入。我们将依靠这个假设，直到我们达到[第 5.6 节](＃内存 - 符合性模型 - 引言)，我们将准确地看到该定义的含义以及替代方案。

### Basic Schemes for Enforcing Coherence

The coherence problem for multiprocessors and I/O, although similar in origin, has different characteristics that affect the appropriate solution. Unlike I/O, where multiple data copies are a rare event—one to be avoided whenever possible—a program running on multiple processors will normally have copies of the same data in several caches. In a coherent multiprocessor, the caches provide both _migration_ and _replication_ of shared data items.

> 多处理器和 I/O 的连贯性问题虽然原点相似，但具有影响适当解决方案的不同特征。与 I/O 不同的是，多个数据副本是一个罕见的事件(可以在可能的情况下避免使用)，在多个处理器上运行的程序通常会在几个缓存中具有相同数据的副本。在连贯的多处理器中，缓存同时提供共享数据项的* migration *和* replication*。

Coherent caches provide migration because a data item can be moved to a local cache and used there in a transparent fashion. This migration reduces both the latency to access a shared data item that is allocated remotely and the bandwidth demand on the shared memory.

> 连贯的缓存提供迁移，因为数据项可以移至本地缓存并以透明的方式使用。这种迁移均减少了访问远程分配的共享数据项的延迟以及对共享内存的带宽需求。

Because the caches make a copy of the data item in the local cache, coherent caches also provide replication for shared data that are being read simultaneously. Replication reduces both latency of access and contention for a read shared data item. Supporting this migration and replication is critical to performance in accessing shared data. Thus, rather than trying to solve the problem by avoiding it in software, multiprocessors adopt a hardware solution by introducing a protocol to maintain coherent caches.

> 由于缓存在本地缓存中制作了数据项的副本，因此连贯的缓存还为正在同时读取的共享数据提供了复制。复制减少了读取共享数据项的访问和争议的延迟。支持此迁移和复制对于访问共享数据的性能至关重要。因此，多处理器通过引入协议以维护连贯的缓存来采用硬件解决方案，而不是试图通过避免使用软件来解决问题。

The protocols to maintain coherence for multiple processors are called _cache coherence protocols_. Key to implementing a cache coherence protocol is tracking the state of any sharing of a data block. The state of any cache block is kept using status bits associated with the block, similar to the valid and dirty bits kept in a uniprocessor cache. There are two classes of protocols in use, each of which uses different techniques to track the sharing status:

> 维持多个处理器连贯性的协议称为 *Cache Cooherence 协议*。实施高速缓存相干协议的关键是跟踪数据块共享的任何状态。任何缓存块的状态都使用与块关联的状态位保持，类似于在单层缓存中保存的有效和肮脏位。使用两类协议，每种协议都使用不同的技术来跟踪共享状态：

_Directory based_—The sharing status of a particular block of physical memory is kept in one location, called the _directory_. There are two very different types of directory-based cache coherence. In an SMP, we can use one centralized directory, associated with the memory or some other single serialization point, such as the outermost cache in a multicore. In a DSM, it makes no sense to have a single directory because that would create a single point of contention and make it difficult to scale to many multicore chips given the memory demands of multicores with eight or more cores. Distributed directories are more complex than a single directory, and such designs are the subject of [Section 5.4](#distributed-shared-memory-and-directory-based-coherence).

> _ directory 基于_-特定物理内存块的共享状态保存在一个位置，称为 *directory*。有两种非常不同类型的基于目录的高速缓存相干性。在 SMP 中，我们可以使用一个与内存或其他单个序列化点相关联的集中式目录，例如多核心中的最外部缓存。在 DSM 中，拥有一个单个目录是没有意义的，因为这会产生一个单一的争论，并且由于具有八个或更多核心的 Multicores 的内存需求，因此难以扩展到许多多项芯片。分布式目录比单个目录更为复杂，此类设计是[第 5.4 节](＃分布式共享 - 记忆和基于直接指导的 coherence)的主题。

_Snooping_—Rather than keeping the state of sharing in a single directory, every cache that has a copy of the data from a block of physical memory could track the sharing status of the block. In an SMP, the caches are typically all accessible via some broadcast medium (e.g., a bus connects the per-core caches to the shared cache or memory), and all cache controllers monitor or _snoop_ on the medium to determine whether they have a copy of a block that is requested on a bus or switch access. Snooping can also be used as the coherence protocol for a multichip multiprocessor, and some designs support a snooping protocol on top of a directory protocol within each multicore.

> *snoop\_\_-而不是将共享状态保持在单个目录中，每个具有从物理内存块中的数据副本的缓存都可以跟踪块的共享状态。在 SMP 中，通常可以通过某些广播介质访问缓存(例如，总线将每核缓存连接到共享的缓存或内存)，并且所有缓存控制器在介质上的监视器或\_snoop* 在总线或开关访问中请求的块。侦听也可以用作多级多处理器的连贯协议，并且某些设计支持每个多项式目录协议之上的侦听协议。

Snooping protocols became popular with multiprocessors using microprocessors (single-core) and caches attached to a single shared memory by a bus. The bus provided a convenient broadcast medium to implement the snooping protocols. Multicore architectures changed the picture significantly because all multicores share some level of cache on the chip. Thus some designs switched to using directory protocols, since the overhead was small. To allow the reader to become familiar with both types of protocols, we focus on a snooping protocol here and discuss a directory protocol when we come to DSM architectures.

> 使用微处理器(单核)和通过总线附加到单个共享内存上的缓存者，窥探协议在多处理器中变得很受欢迎。公共汽车提供了一种方便的广播媒介来实施窥探协议。多层体系结构极大地改变了图片，因为所有 Multicores 在芯片上共享一定级别的高速缓存。因此，由于开销很小，因此一些设计切换到使用目录协议。为了让读者熟悉两种类型的协议，我们在此处关注窥探协议，并在进入 DSM 体系结构时讨论目录协议。

### Snooping Coherence Protocols

There are two ways to maintain the coherence requirement described in the prior section. One method is to ensure that a processor has exclusive access to a data item before writing that item. This style of protocol is called a _write invalidate protocol_ because it invalidates other copies on a write. It is by far the most common protocol. Exclusive access ensures that no other readable or writable copies of an item exist when the write occurs: all other cached copies of the item are invalidated.

> 有两种方法可以维持上一节中描述的相干要求。一种方法是确保处理器在编写该项目之前可以独家访问数据项。这种协议样式称为 *write 协议无效*，因为它使写入其他副本无效。到目前为止，这是最常见的协议。独家访问可确保在写入时没有其他可读或可写的项目的副本：该项目的所有其他缓存副本都是无效的。

[Figure 5.4](#_bookmark220) shows an example of an invalidation protocol with write-back caches in action. To see how this protocol ensures coherence, consider a write followed by a read by another processor: because the write requires exclusive access, any copy held by the reading processor must be invalidated (thus the protocol name). Therefore when the read occurs, it misses in the cache and is forced to fetch a new copy of the data. For a write, we require that the writing processor has exclusive access, preventing any other processor from being able to write simultaneously. If two processors do attempt to write the same data simultaneously, one of them wins the race (we’ll see how we decide who wins shortly), causing the other processor’s copy to be invalidated. For the other processor to complete its write, it must obtain a new copy of the data, which must now contain the updated value. Therefore this protocol enforces write serialization.

> [图 5.4](#_bookmark220) 显示了带有写作缓存的无效协议的示例。要查看该协议如何确保连贯性，请考虑写入另一个处理器的读写：因为写入需要独家访问，因此必须将阅读处理器保存的任何副本无效(因此是协议名称)。因此，当读取发生时，它会错过缓存，并被迫获取新的数据副本。对于写作，我们要求写作处理器具有独家访问权限，从而阻止任何其他处理器能够同时写入。如果两个处理器确实试图同时编写相同的数据，那么其中一个赢得了比赛(我们将看到如何决定谁赢得胜利)，从而导致另一个处理器的副本无效。为了使其他处理器完成其写入，它必须获得数据的新副本，该副本现在必须包含更新的值。因此，该协议执行编写序列化。

Figure 5.4 An example of an invalidation protocol working on a snooping bus for a single cache block (X) with write-back caches. We assume that neither cache initially holds X and that the value of X in memory is 0. The processor and memory contents show the value after the processor and bus activity have both completed. A blank indicates no activity or no copy cached. When the second miss by B occurs, processor A responds with the value canceling the response from memory. In addition, both the contents of B’s cache and the memory contents of X are updated. This update of memory, which occurs when a block becomes shared, simplifies the protocol, but it is possible to track the ownership and force the write-back only if the block is replaced. This requires the introduction of an additional status bit indicating ownership of a block. The ownership bit indicates that a block may be shared for reads, but only the owning processor can write the block, and that processor is responsible for updating any other processors and memory when it changes the block or replaces it. If a multicore uses a shared cache (e.g., L3), then all memory is seen through the shared cache; L3 acts like the memory in this example, and coherency must be handled for the private L1 and L2 caches for each core. It is this observation that led some designers to opt for a directory protocol within the multicore. To make this work, the L3 cache must be inclusive; recall from [Chapter 2](#_bookmark0), that a cache is inclusive if any location in a higher level cache (L1 and L2 in this case) is also in L3. We return to the topic of inclusion on page 423.

> 图 5.4 无效协议的一个示例，可在单个缓存块(x)上使用带写下的缓存。我们假设最初都不容纳 x，并且内存中的 x 值都为 0。处理器和内存内容显示了处理器和总线活动都完成后的值。空白表示没有活动或没有缓存的副本。当第二次失误发生 B 时，处理器 A 会响应值，以取消内存响应的值。此外，B 的内容都更新了 B 的 CACHE 和 X 的内存内容。这种内存的更新是在共享块时发生的，它简化了协议，但是只有在更换块时，才有可能跟踪所有权并强制写入。这需要引入额外的状态位，表明块的所有权。所有权位表明可以共享一个块用于读取，但是只有拥有处理器才能编写块，并且该处理器负责在更改块或替换块时更新任何其他处理器和内存。如果多层使用共享缓存(例如 L3)，则通过共享缓存可以看到所有内存；在此示例中，L3 的作用类似于内存，并且必须为每个核心的私有 L1 和 L2 缓存处理相干性。正是这一观察结果使一些设计师选择了多项式内部的目录协议。为了进行这项工作，L3 缓存必须具有包含在内；从[第 2 章](#_bookmark0)回来，如果较高级别的缓存中的任何位置(在这种情况下为 L1 和 L2)也在 L3 中。我们回到第 423 页的包容性主题。

The alternative to an invalidate protocol is to update all the cached copies of a data item when that item is written. This type of protocol is called a _write update_ or _write broadcast_ protocol. Because a write update protocol must broadcast all writes to shared cache lines, it consumes considerably more bandwidth. For this reason, virtually all recent multiprocessors have opted to implement a write invalidate protocol, and we will focus only on invalidate protocols for the rest of the chapter.

> 无效协议的替代方法是在编写该项目时更新数据项的所有缓存副本。这种类型的协议称为 *write update* 或 *write broadcast* 协议。由于写入更新协议必须广播所有写入共享的缓存线，因此它会消耗更多的带宽。因此，几乎所有最近的多处理器都选择实现写入无效协议，我们将仅关注本章其余部分的无效协议。

### Basic Implementation Techniques

The key to implementing an invalidate protocol in a multicore is the use of the bus, or another broadcast medium, to perform invalidates. In older multiple-chip multiprocessors, the bus used for coherence is the shared-memory access bus. In a single-chip multicore, the bus can be the connection between the private caches (L1 and L2 in the Intel i7) and the shared outer cache (L3 in the i7). To perform an invalidate, the processor simply acquires bus access and broadcasts the address to be invalidated on the bus. All processors continuously snoop on the bus, watching the addresses. The processors check whether the address on the bus is in their cache. If so, the corresponding data in the cache are invalidated.

> 在多项式中实现无效协议的关键是使用总线或其他广播介质执行无效。在较旧的多芯片多处理器中，用于连贯性的总线是共享记忆访问总线。在单芯片多核心中，总线可以是私有缓存(Intel I7 中的 L1 和 L2)与共享外部缓存(I7 中的 L3)之间的连接。为了执行无效，处理器只需获取总线访问权限，并广播在公共汽车上无效的地址。所有处理器都在公共汽车上不断窥探，观看地址。处理器检查公共汽车上的地址是否在其缓存中。如果是这样，缓存中的相应数据将无效。

When a write to a block that is shared occurs, the writing processor must acquire bus access to broadcast its invalidation. If two processors attempt to write shared blocks at the same time, their attempts to broadcast an invalidate operation will be serialized when they arbitrate for the bus. The first processor to obtain bus access will cause any other copies of the block it is writing to be invalidated. If the processors were attempting to write the same block, the serialization enforced by the bus would also serialize their writes. One implication of this scheme is that a write to a shared data item cannot actually complete until it obtains bus access. All coherence schemes require some method of serializing accesses to the same cache block, either by serializing access to the communication medium or to another shared structure. In addition to invalidating outstanding copies of a cache block that is being written into, we also need to locate a data item when a cache miss occurs. In a writethrough cache, it is easy to find the recent value of a data item because all written data are always sent to the memory, from which the most recent value of a data item can always be fetched. (Write buffers can lead to some additional complexities and must effectively be treated as additional cache entries.)

> 当写入共享的块时，书写处理器必须获取总线访问才能广播其无效。如果两个处理器试图同时编写共享块，则他们试图广播无效操作的尝试将在他们为总线仲裁时序列化。第一个获得总线访问的处理器将导致其编写的任何其他块的副本无效。如果处理器试图编写相同的块，则公共汽车执行的序列化也将序列化其写入。该方案的一个含义是，在获得总线访问之后，写入共享数据项才能实际完成。所有相干方案都需要通过序列化对通信介质的访问或其他共享结构的访问来序列化访问相同的缓存块的方法。除了使正在写入的高速缓存块的未成年副本无效外，我们还需要在发生缓存错过时找到数据项。在 WriteThrough 缓存中，很容易找到数据项的最新值，因为所有书面数据始终发送到内存，并且可以始终从中获取数据项的最新值。(写缓冲区可以导致一些其他复杂性，并且必须有效地将其视为其他缓存条目。)

For a write-back cache, the problem of finding the most recent data value is harder because the most recent value of a data item can be in a private cache rather than in the shared cache or memory. Fortunately, write-back caches can use the same snooping scheme both for cache misses and for writes: each processor snoops every address placed on the shared bus. If a processor finds that it has a dirty copy of the requested cache block, it provides that cache block in response to the read request and causes the memory (or L3) access to be aborted. The additional complexity comes from having to retrieve the cache block from another processor’s private cache (L1 or L2), which can often take longer than retrieving it from L3. Because write-back caches generate lower requirements for memory bandwidth, they can support larger numbers of faster processors. As a result, all multicore processors use write-back at the outermost levels of the cache, and we will examine the implementation of coherence with write-back caches.

> 对于写下缓存，找到最新数据值的问题更难，因为数据项的最新值可以在私有缓存中而不是在共享缓存或内存中。幸运的是，写下的缓存可以将同一侦探方案用于缓存错过和写入：每个处理器 snoops 在共享总线上的每个地址。如果处理器发现其具有所请求的缓存块的肮脏副本，则规定响应读取请求的缓存块，并导致内存(或 L3)访问被中止。额外的复杂性来自必须从另一个处理器的私人缓存(L1 或 L2)中检索缓存块，这通常比从 L3 中检索它可能需要更长的时间。由于写下贴件对内存带宽的要求较低，因此它们可以支持大量更快的处理器。结果，所有多层处理器都在缓存的最外层级别使用写下，我们将研究与写下 caches 相干性的实现。

The normal cache tags can be used to implement the process of snooping, and the valid bit for each block makes invalidation easy to implement. Read misses, whether generated by an invalidation or by some other event, are also straightforward because they simply rely on the snooping capability. For writes, we want to know whether any other copies of the block are cached because, if there are no other cached copies, then the write does not need to be placed on the bus in a write-back cache. Not sending the write reduces both the time to write and the required bandwidth.

> 普通的缓存标签可用于实现侦听过程，每个块的有效位使无效易于实现。阅读失误，无论是通过无效的还是其他事件产生的，也很简单，因为它们只是依靠侦听能力。对于写信，我们想知道该块的其他任何副本是否被缓存，因为，如果没有其他缓存副本，则不需要将写入放置在书面缓存中。不发送写作会减少写作时间和所需的带宽。

To track whether or not a cache block is shared, we can add an extra state bit associated with each cache block, just as we have a valid bit and a dirty bit. By adding a bit indicating whether the block is shared, we can decide whether a write must generate an invalidate. When a write to a block in the shared state occurs, the cache generates an invalidation on the bus and marks the block as _exclusive_. No further invalidations will be sent by that core for that block. The core with the sole copy of a cache block is normally called the _owner_ of the cache block.

> 要跟踪是否共享缓存块，我们可以添加与每个缓存块关联的额外状态位，就像我们有一个有效的位和肮脏的位一样。通过添加一些指示是否共享块，我们可以决定写入是否必须生成无效。当出现共享状态中的一个块写入块时，缓存会在总线上生成无效，并将块标记为 *Exclusive*。该核心将不会为该块发送进一步的无效。带有缓存块唯一副本的核心通常称为缓存块的 *owner*。

When an invalidation is sent, the state of the owner’s cache block is changed from shared to unshared (or exclusive). If another processor later requests this cache block, the state must be made shared again. Because our snooping cache also sees any misses, it knows when the exclusive cache block has been requested by another processor and the state should be made shared.

> 发送无效后，所有者的缓存块的状态将从共享更改为 Unshared(或独家)。如果另一个处理器稍后请求此缓存块，则必须再次共享状态。因为我们的窥探缓存也看到了任何错过，所以它知道何时何时由另一个处理器要求独家缓存块，并应共享状态。

Every bus transaction must check the cache-address tags, which could potentially interfere with processor cache accesses. One way to reduce this interference is to duplicate the tags and have snoop accesses directed to the duplicate tags. Another approach is to use a directory at the shared L3 cache; the directory indicates whether a given block is shared and possibly which cores have copies. With the directory information, invalidates can be directed only to those caches with copies of the cache block. This requires that L3 must always have a copy of any data item in L1 or L2, a property called _inclusion_, which we will return to in [Section 5.7](#cross-cutting-issues-2).

> 每个总线交易都必须检查缓存地址标签，这可能会干扰处理器高速缓存访问。减少这种干扰的一种方法是复制标签，并将侦探访问针对重复标签。另一种方法是在共享 L3 缓存处使用目录；该目录指示给定块是否共享，可能是哪些内核具有副本。有了目录信息，无效的人只能将其引导到具有缓存块副本的那些缓存。这要求 L3 必须始终具有 L1 或 L2 中的任何数据项的副本，该属性称为 *Clusion*，我们将在[5.7]中返回(＃Cross-Cutting-issues-2)。

### An Example Protocol

A snooping coherence protocol is usually implemented by incorporating a finitestate controller in each core. This controller responds to requests from the processor in the core and from the bus (or other broadcast medium), changing the state of the selected cache block, as well as using the bus to access data or to invalidate it. Logically, you can think of a separate controller as being associated with each block; that is, snooping operations or cache requests for different blocks can proceed independently. In actual implementations, a single controller allows multiple operations to distinct blocks to proceed in interleaved fashion (i.e., one operation may be initiated before another is completed, even though only one cache access or one bus access is allowed at a time). Also, remember that, although we refer to a bus in the following description, any interconnection network that supports a broadcast to all the coherence controllers and their associated private caches can be used to implement snooping.

> 通常通过在每个核心中纳入一个载体控制器来实现窥探相干协议。该控制器响应核心和总线(或其他广播介质)的处理器的请求，更改所选的缓存块的状态，并使用总线访问数据或使其无效。从逻辑上讲，您可以将一个单独的控制器视为与每个块相关联；也就是说，可以独立进行窥探操作或缓存请求。在实际实现中，单个控制器允许多个操作以交错方式进行不同的块(即，即使仅允许一次缓存或一个总线访问，也可以在完成另一个操作之前启动一个操作)。另外，请记住，尽管我们在以下说明中指的是总线，但任何支持向所有连贯控制器及其相关的私有卡车广播的互连网络都可以用于实现侦探。

The simple protocol we consider has three states: invalid, shared, and modified. The shared state indicates that the block in the private cache is potentially shared, whereas the modified state indicates that the block has been updated in the private cache; note that the modified state _implies_ that the block is exclusive. [Figure 5.5](#_bookmark221) shows the requests generated by a core (in the top half of the table) as well as those coming from the bus (in the bottom half of the table). This protocol is for a write-back cache but is easily changed to work for a write-through cache by reinterpreting the modified state as an exclusive state and updating the cache on writes in the normal fashion for a write-through cache. The most common extension of this basic protocol is the addition of an exclusive state, which describes a block that is unmodified but held in only one private cache. We describe this and other extensions on page 388.

> 我们考虑的简单协议具有三个状态：无效，共享和修改。共享状态表明私有缓存中的块可能会共享，而修改后的状态表示该块已在私有缓存中进行了更新；请注意，修改后的状态 *implies* 块是独有的。[图 5.5](#_bookmark221) 显示了由核心生成的请求(在表格的上半部分)以及来自公共汽车(表的下半部分)的请求。该协议适用于书面缓存，但可以通过重新解释修改后的状态作为独家状态并以正常方式以写入缓存的方式来更新缓存，从而可以轻松地更改为通过写入缓存的工作。该基本协议的最常见扩展是添加一个独家状态，该状态描述了一个未修改但仅保存在一个私人缓存中的块。我们在第 388 页上描述了这一点和其他扩展。

Figure 5.5 The cache coherence mechanism receives requests from both the core’s processor and the shared bus and responds to these based on the type of request, whether it hits or misses in the local cache, and the state of the local cache block specified in the request. The fourth column describes the type of cache action as normal hit or miss (the same as a uniprocessor cache would see), replacement (a uniprocessor cache replacement miss), or coherence (required to maintain cache coherence); a normal or replacement action may cause a coherence action depending on the state of the block in other caches. For read, misses, write misses, or invalidates snooped from the bus, an action is required _only_ if the read or write addresses match a block in the local cache and the block is valid.

> 图 5.5 缓存连贯机制从核心的处理器和共享总线接收请求，并根据请求的类型(无论是在本地缓存中命中还是错过)以及请求中指定的本地缓存块的状态对这些请求进行响应。。第四列将缓存操作的类型描述为正常命中或错过的(与单层缓存相同)，替换(UniProcessor Cache 替换失误)或连贯性(需要保持缓存相干性)；正常或替换作用可能会根据其他缓存中的块状态引起连贯作用。对于从总线上窃取的读物，错过，写入错过或无效，如果读取或写地址匹配本地缓存中的一个块并且该块是有效的，则需要采取措施。

When an invalidate or a write miss is placed on the bus, any cores whose private caches have copies of the cache block invalidate it. For a write miss in a writeback cache, if the block is exclusive in just one private cache, that cache also writes back the block; otherwise, the data can be read from the shared cache or memory. [Figure 5.6](#_bookmark222) shows a finite-state transition diagram for a single private cache block using a write invalidation protocol and a write-back cache. For simplicity, the three states of the protocol are duplicated to represent transitions based on processor requests (on the left, which corresponds to the top half of the table in [Figure 5.5](#_bookmark221)), as opposed to transitions based on bus requests (on the right, which corresponds to the bottom half of the table in [Figure 5.5](#_bookmark221)). Boldface type is used to distinguish the bus actions, as opposed to the conditions on which a state transition depends. The state in each node represents the state of the selected private cache block specified by the processor or bus request.

> 当将无效或写入错过放置在公共汽车上时，任何私有缓存的内核都有缓存块无效的副本。对于在写下缓存中的写入错过，如果仅在一个私有缓存中块是独有的，则该缓存还会写回块。否则，可以从共享的缓存或内存中读取数据。[图 5.6](#_bookmark222) 使用写入无效协议和写入后的缓存显示了单个私有缓存块的有限状态过渡图。为简单起见，协议的三个状态被复制以根据处理器请求表示过渡(在左侧，对应于[图 5.5](#_bookmark221) 中表的上半部分)，而不是基于总线的过渡请求(在右侧，对应于[图 5.5](#_bookmark221) 中表的下半部分)。与状态过渡所依赖的条件相比，Boldface 类型用于区分总线操作。每个节点中的状态代表处理器或总线请求指定的选定私有缓存块的状态。

Figure 5.6 A write invalidate, cache coherence protocol for a private write-back cache showing the states and state transitions for each block in the cache. The cache states are shown in circles, with any access permitted by the local processor without a state transition shown in parentheses under the name of the state. The stimulus causing a state change is shown on the transition arcs in regular type, and any bus actions generated as part of the state transition are shown on the transition arc in _bold_. The stimulus actions apply to a block in the private cache, not to a specific address in the cache. Thus a read miss to a block in the shared state is a miss for that cache block but for a different address. The left side of the diagram shows state transitions based on actions of the processor associated with this cache; the right side shows transitions based on operations on the bus. A read miss in the exclusive or shared state and a write miss in the exclusive state occur when the address requested by the processor does not match the address in the local cache block. Such a miss is a standard cache replacement miss. An attempt to write a block in the shared state generates an invalidate. Whenever a bus transaction occurs, all private caches that contain the cache block specified in the bus transaction take the action dictated by the right half of the diagram. The protocol assumes that memory (or a shared cache) provides data on a read miss for a block that is clean in all local caches. In actual implementations, these two sets of state diagrams are combined. In practice, there are many subtle variations on invalidate protocols, including the introduction of the exclusive unmodified state, as to whether a processor or memory provides data on a miss. In a multicore chip, the shared cache (usually L3, but sometimes L2) acts as the equivalent of memory, and the bus is the bus between the private caches of each core and the shared cache, which in turn interfaces to the memory.

> 图 5.6 写入无效的私人写入缓存的无效，缓存相干协议，显示缓存中每个块的状态和状态过渡。缓存状态以圆圈显示，本地处理器允许的任何访问权，没有括号中以状态名称显示的状态过渡。在常规类型的过渡弧上显示了引起状态变化的刺激，并且在 *bold* 中的过渡弧上显示了任何作为状态过渡的一部分产生的总线动作。刺激动作适用于私有缓存中的一个块，而不是缓存中的特定地址。因此，在共享状态中的一个读数是对该缓存块的遗漏，但对于不同的地址而言。图的左侧显示了基于与此缓存相关的处理器的动作的状态过渡。右侧显示了基于巴士操作的过渡。当处理器请求的地址与本地缓存块中的地址不匹配时，在独家状态或共享状态中的读取错误以及在独家状态中的写入错过。这样的错过是标准的缓存替换失误。试图在共享状态中编写一个块会产生无效。每当发生总线交易时，所有包含总线交易中指定的高速缓存块的私人缓存都采用图表的右半部分。该协议假设内存(或共享缓存)为在所有本地缓存中都干净的块上的读取失误提供数据。在实际实现中，将这两组状态图组合在一起。实际上，关于处理器或内存是否提供了失误的数据，包括引入独家未修饰状态，包括引入独家未修饰状态的无效协议。在多层芯片中，共享的缓存(通常是 L3，但有时 L2)充当内存的等效物，并且总线是每个核心的私人缓存与共享缓存之间的总线，而共享缓存又是与内存接口的。

All of the states in this cache protocol would be needed in a uniprocessor cache, where they would correspond to the invalid, valid (and clean), and dirty states. Most of the state changes indicated by arcs in the left half of [Figure 5.6](#_bookmark222) would be needed in a write-back uniprocessor cache, with the exception being the invalidate on a write hit to a shared block. The state changes represented by the arcs in the right half of [Figure 5.6](#_bookmark222) are needed only for coherence and would not appear at all in a uniprocessor cache controller.

> 该缓存协议中的所有状态都将在单层缓存中需要，它们将与无效，有效(干净)和肮脏状态相对应。在书面式 Uniprocessor 缓存中，需要在[图 5.6]的左半部分(#\_bookmark222)的左半部分指示的大多数状态更改，除了对共享块的写入打击中的无效。[图 5.6]的右半(#\_bookmark222)在右半中表示的状态变化仅是为了连贯性，并且根本不会出现在单层缓存控制器中。

As mentioned earlier, there is only one finite-state machine per cache, with stimuli coming either from the attached processor or from the bus. [Figure 5.7](#_bookmark223) shows how the state transitions in the right half of [Figure 5.6](#_bookmark222) are combined with those in the left half of the figure to form a single state diagram for each cache block.

> 如前所述，每个缓存只有一台有限状态的机器，刺激来自连接的处理器或总线。[图 5.7](#_bookmark223) 显示了[图 5.6](#_bookmark222) 中右半中的状态转换如何与图的左半部分结合在一起，以形成每个缓存块的单个状态图。

To understand why this protocol works, observe that any valid cache block is either in the shared state in one or more private caches or in the exclusive state in exactly one cache. Any transition to the exclusive state (which is required for a processor to write to the block) requires an invalidate or write miss to be placed on the bus, causing all local caches to make the block invalid. In addition, if some other local cache had the block in exclusive state, that local cache generates a writeback, which supplies the block containing the desired address. Finally, if a read miss occurs on the bus to a block in the exclusive state, the local cache with the exclusive copy changes its state to shared.

> 要了解该协议为何起作用，请观察到任何有效的缓存块在一个或多个私人缓存中的共享状态处于共享状态，或者在一个缓存中处于独家状态。任何向独家状态(处理器写入块所需的过渡)都要求将其放置在总线上或写入误差，从而导致所有本地缓存使块无效。此外，如果其他某些本地缓存在独家状态下具有块，则本地缓存会生成一个写入，该记录将提供包含所需地址的块。最后，如果在公共汽车到独家状态下的一个块上发生了读取误差，则具有独家副本的本地缓存将其状态更改为共享。

The actions in gray in [Figure 5.7](#_bookmark223), which handle read and write misses on the bus, are essentially the snooping component of the protocol. One other property that is preserved in this protocol, and in most other protocols, is that any memory block in the shared state is always up to date in the outer shared cache (L2 or L3, or memory if there is no shared cache), which simplifies the implementation. In fact, it does not matter whether the level out from the private caches is a shared cache or memory; the key is that all accesses from the cores go through that level.

> [图 5.7](#_bookmark223) 中的灰色动作(在总线上读写错过)本质上是协议的窥探组件。在此协议中保留的另一个属性，在大多数其他协议中，共享状态中的任何内存块始终在外部共享缓存中(L2 或 l3 或内存，如果没有共享的缓存)，则始终是最新的。这简化了实施。实际上，私人缓存的级别是共享的缓存还是内存都没有关系。关键是，所有核心的访问都可以通过该级别。

Although our simple cache protocol is correct, it omits a number of complications that make the implementation much trickier. The most important of these is that the protocol assumes that operations are _atomic_—that is, an operation can be done in such a way that no intervening operation can occur. For example, the protocol described assumes that write misses can be detected, acquire the bus, and receive a response as a single atomic action. In reality this is not true. In fact, even a read miss might not be atomic; after detecting a miss in the L2 of a multicore, the core must arbitrate for access to the bus connecting to the shared L3. Nonatomic actions introduce the possibility that the protocol can _deadlock_, meaning that it reaches a state where it cannot continue. We will explore these complications later in this section and when we examine DSM designs.

> 尽管我们的简单缓存协议是正确的，但它忽略了许多并发症，这些并发症使实现更加棘手。其中最重要的是该协议假设操作是 *ATOMIC*，也就是说，可以以不可能发生中间操作的方式完成操作。例如，所描述的协议假定可以检测到写入，获取总线并作为单个原子动作接收响应。实际上，这不是真的。实际上，即使是读小姐也可能不是原子。在检测到多核心的 L2 中的失误之后，核心必须仲裁以访问连接到共享 L3 的总线。非原子动作介绍了协议可以 *deadlock* 的可能性，这意味着它达到了无法继续的状态。我们将在本节稍后探索这些并发症，并在检查 DSM 设计时。

Figure 5.7 Cache coherence state diagram with the state transitions induced by the local processor shown in _black_ and by the bus activities shown in _gray_. As in [Figure 5.6](#_bookmark222), the activities on a transition are shown in _bold_.

> 图 5.7 缓存连贯状态图与 *black* 中所示的本地处理器以及 *gray* 中所示的总线活动所引起的状态转换。如[图 5.6](#_bookmark222) 中，过渡中的活动在 *bold* 中显示。

With multicore processors, the coherence among the processor cores is all implemented on chip, using either a snooping or simple central directory protocol. Many multiprocessor chips, including the Intel Xeon and AMD Opteron, support multichip multiprocessors that could be built by connecting a high-speed interface already incorporated in the chip. These next-level interconnects are not just extensions of the shared bus, but use a different approach for interconnecting multicores. A multiprocessor built with multiple multicore chips will usually have a distributed memory architecture and will need an interchip coherency mechanism above and beyond the one within the chip. In most cases, some form of directory scheme is used.

> 使用多核处理器，使用窥探或简单的中央目录协议，在芯片上实现了处理器内核之间的连贯性。许多多处理器芯片，包括 Intel Xeon 和 AMD Opteron，都支持可以通过连接芯片中已经合并的高速接口来构建的多芯片多处理器。这些下一级的互连不仅是共享总线的扩展，而且使用不同的方法来互连多核。使用多个多层芯片构建的多处理器通常将具有分布式的内存体系结构，并且需要芯片上方和之外的芯片连贯机制。在大多数情况下，使用某种形式的目录方案。

### Extensions to the Basic Coherence Protocol

The coherence protocol we have just described is a simple three-state protocol and is often referred to by the first letter of the states, making it a MSI (Modified, Shared, Invalid) protocol. There are many extensions of this basic protocol, which we mention in the captions of figures in this section. These extensions are created by adding additional states and transactions that optimize certain behaviors, possibly resulting in improved performance. Two of the most common extensions are

> 我们刚刚描述的连贯协议是一个简单的三州协议，通常由状态的首字母提及，使其成为 MSI(修改，共享，无效)协议。该基本协议有许多扩展，我们在本节中的数字标题中提到。这些扩展是通过添加优化某些行为的其他状态和交易来创建的，可能会改善性能。最常见的两个扩展是

1. _MESI_ adds the state Exclusive to the basic MSI protocol, yielding four states (Modified, Exclusive, Shared, and Invalid). The exclusive state indicates that a cache block is resident in only a single cache but is clean. If a block is in the E state, it can be written without generating any invalidates, which optimizes the case where a block is read by a single cache before being written by that same cache. Of course, when a read miss to a block in the E state occurs, the block must be changed to the S state to maintain coherence. Because all subsequent accesses are snooped, it is possible to maintain the accuracy of this state. In particular, if another processor issues a read miss, the state is changed from exclusive to shared. The advantage of adding this state is that a subsequent write to a block in the exclusive state by the same core need not acquire bus access or generate an invalidate, since the block is known to be exclusively in this local cache; the processor merely changes the state to modified. This state is easily added by using the bit that encodes the coherent state as an exclusive state and using the dirty bit to indicate that a bock is modified. The Intel i7 uses a variant of a MESI protocol, called MESIF, which adds a state (Forward) to designate which sharing processor should respond to a request. It is designed to enhance performance in distributed memory organizations.

> 1. *mesi* 将状态独家添加到基本 MSI 协议中，得出四个状态(修改，独家，共享和无效)。独家状态表明，缓存块仅居住在一个缓存中，但干净。如果一个块处于 E 状态，则可以在不生成任何无效的情况下编写它，从而优化了在由同一缓存书写之前，单个缓存读取块的情况。当然，当发生 E 状态中的一个块读数时，必须将块更改为 S 状态以保持连贯性。由于所有后续访问均已窥探，因此可以保持该状态的准确性。特别是，如果另一个处理器发行了读物，则该状态将从独占性更改为共享。添加此状态的优点是，随后通过同一核心在独家状态的块上写入一个块不需要获取总线访问或生成无效的，因为该块已知该块仅在此本地缓存中仅限于此。处理器只是更改状态进行修改。通过使用编码连贯状态作为独家状态的位并使用肮脏位来表示修改 BOCK 的位，可以轻松添加此状态。英特尔 i7 使用了称为 MESIF 的 MESI 协议的变体，该变体添加了一个状态(向前)来指定共享处理器应响应请求。它旨在提高分布式内存组织的性能。

2. _MOESI_ adds the state Owned to the MESI protocol to indicate that the associated block is owned by that cache and out-of-date in memory. In MSI and MESI protocols, when there is an attempt to share a block in the Modified state, the state is changed to Shared (in both the original and newly sharing cache), and the block must be written back to memory. In a MOESI protocol, the block can be changed from the Modified to Owned state in the original cache without writing it to memory. Other caches, which are newly sharing the block, keep the block in the Shared state; the O state, which only the original cache holds, indicates that the main memory copy is out of date and that the designated cache is the owner. The owner of the block must supply it on a miss, since memory is not up to date and must write the block back to memory if it is replaced. The AMD Opteron processor family uses the MOESI protocol.

> 2. *moesi* 将拥有的状态添加到 MESI 协议中，以指示关联的块由该缓存拥有并在存储器中脱离。在 MSI 和 MESI 协议中，当尝试在修改状态下共享一个块时，状态已更改为共享(在原始和新共享的缓存中)，并且必须将块写回内存。在 MOESI 协议中，可以将块从原始缓存中的修改状态更改为原始状态，而无需将其写入内存。新近共享该区块的其他缓存将块保持在共享状态；仅原始高速缓存的 o 状态表明主存储器副本已过时，指定的缓存是所有者。该块的所有者必须在遗漏上提供它，因为内存不是最新的，并且如果更换了内存，则必须将块写回内存。AMD Opteron 处理器家族使用 MOESI 协议。

The next section examines the performance of these protocols for our parallel and multiprogrammed workloads; the value of these extensions to a basic protocol will be clear when we examine the performance. But, before we do that, let’s take a brief look at the limitations on the use of a symmetric memory structure and a snooping coherence scheme.

> 下一部分将检查这些协议的性能，以实现我们的并行和多编程工作负载；当我们检查性能时，这些扩展对基本协议的价值将很清楚。但是，在我们这样做之前，让我们简要介绍一下使用对称内存结构和窥探连贯方案的限制。

### Limitations in Symmetric Shared-Memory Multiprocessors and Snooping Protocols

As the number of processors in a multiprocessor grows, or as the memory demands of each processor grow, any centralized resource in the system can become a bottleneck. For multicores, a single shared bus became a bottleneck with only a few cores. As a result, multicore designs have gone to higher bandwidth interconnection schemes, as well as multiple, independent memories to allow larger numbers of cores. The multicore chips we examine in [Section 5.8](#_bookmark247) use three different approaches:

> 随着多处理器中的处理器数量的增长，或者随着每个处理器的内存需求的增长，系统中的任何集中资源都可以成为瓶颈。对于 Multicors 而言，一辆共享的巴士成为只有几个核心的瓶颈。结果，多核心设计已经进入了更高的带宽互连方案，以及多个独立的记忆，以允许大量核心。我们在[第 5.8 节](#_bookmark247)中检查的多芯芯片使用三种不同的方法：

1. The IBM Power8, which has up to 12 processors in a single multicore, uses 8 parallel buses that connect the distributed L3 caches and up to 8 separate memory channels.

> 1. IBM Power8 在单个多功能中最多具有 12 个处理器，使用 8 个并行总线连接分布式 L3 缓存和多达 8 个单独的内存通道。

2. The Xeon E7 uses three rings to connect up to 32 processors, a distributed L3 cache, and two or four memory channels (depending on the configuration).

> 2. Xeon E7 使用三个戒指连接最多 32 个处理器，一个分布式 L3 缓存和两个或四个内存通道(取决于配置)。

3. The Fujitsu SPARC64 X+ uses a crossbar to connect a shared L2 to up to 16 cores and multiple memory channels.

> 3. Fujitsu SPARC64 X+ 使用横杆将共享的 L2 连接到最多 16 个内核和多个存储通道。

The SPARC64 X+ is a symmetric organization with uniform access time. The Power8 has nonuniform access time for both L3 and memory. Although the uncontended access time differences among memory addresses within a single Power8 multicore are not large, with contention for memory, the access time differences can become significant even within one chip. The Xeon E7 can operate as if access times were uniform; in practice, software systems usually organize memory so that the memory channels are associated with a subset of the cores.

> SPARC64 X+ 是一个具有统一访问时间的对称组织。Power8 对于 L3 和内存都有非均匀的访问时间。尽管单个功率 8 多核算中内存地址之间的无访问访问时间差异并不大，并且具有内存的争论，但即使在一个芯片中，访问时间差异也可能变得很大。Xeon E7 可以像访问时间均匀一样运行。实际上，软件系统通常会组织内存，使内存通道与核心的子集相关联。

Snooping bandwidth at the caches can also become a problem because every cache must examine every miss, and having additional interconnection bandwidth only pushes the problem to the cache. To understand this problem, consider the following example.

> 缓存的窥探带宽也可能会成为一个问题，因为每个缓存都必须检查每个错过，并且具有其他互连带宽只会将问题推向缓存。要了解此问题，请考虑以下示例。

Example Consider an 8-processor multicore where each processor has its own L1 and L2 caches, and snooping is performed on a shared bus among the L2 caches. Assume the average L2 request, whether for a coherence miss or other miss, is 15 cycles. Assume a clock rate of 3.0 GHz, a CPI of 0.7, and a load/store frequency of 40%. If our goal is that no more than 50% of the L2 bandwidth is consumed by coherence traffic, what is the maximum coherence miss rate per processor?

> 示例考虑了一个 8 处理器多核，每个处理器都有自己的 L1 和 L2 缓存，并且在 L2 缓存之间的共享总线上执行侦听。假设平均的 L2 请求，无论是连贯性小姐还是其他错过，都是 15 个周期。假设时钟速率为 3.0 GHz，CPI 为 0.7，负载/存储频率为 40％。如果我们的目标是连贯流量消耗了 L2 带宽的 50％，那么每个处理器的最大连贯性失误率是多少？

_Answer_ Start with an equation for the number of cache cycles that can be used (where CMR is the coherence miss rate):

> *answer* 从可以使用的缓存周期数的方程式开始(其中 CMR 是连贯的失误率)：

This means that the coherence miss rate must be 0.73% or less. In the next section, we will see several applications with coherence miss rates in excess of 1%. Alternatively, if we assume that CMR can be 1%, then we could support just under 6 processors. Clearly, even small multicores will require a method for scaling snoop bandwidth.

> 这意味着连贯的失误率必须为 0.73％或更低。在下一部分中，我们将看到多个连贯率超过 1％的申请。另外，如果我们假设 CMR 可以为 1％，那么我们可以支持仅 6 个处理器。显然，即使是小型的多大盘也需要一种缩放史努比带宽的方法。

There are several techniques for increasing the snoop bandwidth:

> 有几种提高史努比带宽的技术：

1. As mentioned earlier, the tags can be duplicated. This doubles the effective cache-level snoop bandwidth. If we assume that half the coherence requests do not hit on a snoop request and the cost of the snoop request is only 10 cycles (versus 15), then we can cut the average cost of a CMR to 12.5 cycles. This reduction allows the coherence miss rate to be 0.88, or alternatively to support one additional processor (7 versus 6).

> 1.如前所述，标签可以重复。这将有效的缓存级别带宽加倍。如果我们假设一半的连贯性请求不会按照侦探请求达到目标，而史努比请求的成本仅为 10 个周期(对 15 个)，那么我们可以将 CMR 的平均成本降低至 12.5 个周期。这种降低使连贯的损失率为 0.88，或者可以支持一个额外的处理器(7 对 6)。

2. If the outermost cache on a multicore (typically L3) is shared, we can distribute that cache so that each processor has a portion of the memory and handles snoops for that portion of the address space. This approach, used by the IBM 12-core Power8, leads to a NUCA design, but effectively scales the snoop bandwidth at L3 by the number of processors. If there is a snoop hit in L3, then we must still broadcast to all L2 caches, which must in turn snoop their contents. Since L3 is acting as a filter on the snoop requests, L3 must be inclusive.

> 2.如果共享多项式(通常是 L3)上的最外面的高速缓存，我们可以分发该缓存，以便每个处理器都有一部分内存并为地址空间的该部分处理 SNOOPS。IBM 12 核功率 8 使用的方法导致了 NUCA 设计，但通过处理器数量有效地在 L3 处缩放了 Snoop 带宽。如果在 L3 中遇到窥探，那么我们仍然必须向所有 L2 缓存广播，这反过来又必须窥探其内容。由于 L3 充当 Snoop 请求的过滤器，因此 L3 必须具有包含在内。

3. We can place a directory at the level of the outermost shared cache (say, L3). L3 acts as a filter on snoop requests and must be inclusive. The use of a directory at L3 means that we need not snoop or broadcast to all the L2 caches, but only those that the directory indicates may have a copy of the block. Just as L3 may be distributed, the associated directory entries may also be distributed. This approach is used in the Intel Xeon E7 series, which supports from 8 to 32 cores.

> 3.我们可以将目录放置在最外部共享缓存的级别(例如，L3)。L3 充当 Snoop 请求的过滤器，必须具有包含。在 L3 上使用目录意味着我们不需要窥探或广播到所有 L2 缓存，但只有该目录指示的那些可以具有该块的副本。正如可以分发 L3 一样，相关目录条目也可以分布。这种方法用于英特尔 Xeon E7 系列，该系列支持 8 至 32 个核心。

[Figure 5.8](#_bookmark224) shows how a multicore with a distributed cache system, such as that used in schemes 2 or 3, might look. If additional multicore chips were added to form a larger multiprocessor, an off-chip network would needed, as well as a method to extend the coherence mechanisms (as we will see in [Section 5.8](#_bookmark247)).

> [图 5.8](#_bookmark224) 显示了具有分布式高速缓存系统的多项(例如 2 或 3 中使用的)如何看待。如果添加了其他多层芯片以形成较大的多处理器，则需要一个外片网络，以及扩展相干机制的方法(正如我们将在 [5.8](#_bookmark247) 中看到的那样)。

Figure 5.8 A single-chip multicore with a distributed cache. In current designs, the distributed shared cache is usually L3, and levels L1 and L2 are private. There are typically multiple memory channels (2–8 in today’s designs). This design is NUCA, since the access time to L3 portions varies with faster access time for the directly attached core. Because it is NUCA, it is also NUMA.

> 图 5.8 具有分布式缓存的单芯片多核。在当前的设计中，分布式共享缓存通常为 L3，L1 和 L2 级别是私有的。通常有多个内存通道(当今设计中 2-8 个)。该设计是核心，因为对 L3 部分的访问时间随着直接连接的核心的访问时间而变化。因为它是核，所以也是 numa。

The AMD Opteron represents another intermediate point in the spectrum between a snooping and a directory protocol. Memory is directly connected to each multicore chip, and up to four multicore chips can be connected. The system is a NUMA because local memory is somewhat faster. The Opteron implements its coherence protocol using the point-to-point links to broadcast up to three other chips. Because the interprocessor links are not shared, the only way a processor can know when an invalid operation has completed is by an explicit acknowledgment. Thus the coherence protocol uses a broadcast to find potentially shared copies, like a snooping protocol, but uses the acknowledgments to order operations, like a directory protocol. Because local memory is only somewhat faster than remote memory in the Opteron implementation, some software treats the Opteron multiprocessor as having uniform memory access.

> AMD Opteron 代表侦听和目录协议之间光谱中的另一个中间点。存储器直接连接到每个多核心芯片，最多可以连接四个多芯片芯片。系统是 NUMA，因为本地内存速度更快。Opteron 使用点对点链接实现其连贯协议，以广播最多三个芯片。由于未共享处理器链接，因此处理器知道何时完成操作的唯一方法是通过明确的确认。因此，连贯协议使用广播来查找潜在共享的副本，例如侦听协议，但使用确认来订购操作，例如目录协议。由于本地内存仅比 Opteron 实现中的远程内存快一些，因此某些软件将 Opteron 多处理器视为具有统一的内存访问权限。

In [Section 5.4](#distributed-shared-memory-and-directory-based-coherence), we examine directory-based protocols, which eliminate the need for broadcast to all caches on a miss. Some multicore designs use directories within the multicore (Intel Xeon E7), while others add directories when scaling beyond a multicore. Distributed directories eliminate the need for a single point to serialize all accesses (typically a single shared bus in a snooping scheme), and any scheme that removes the single point of serialization must deal with many of the same challenges as a distributed directory scheme.

> 在[第 5.4 节](#distributed-shared-memory-and-directory-based-coherence)中，我们检查了基于目录的协议，这消除了对失误上所有缓存的广播的需求。一些多项设计使用多项式(Intel Xeon E7)中的目录，而另一些多项设计则在超越多项方面时添加了目录。分布式目录消除了单个点以序列化所有访问的需求(通常是侦听方案中的单个共享总线)，任何删除单个序列化点的方案都必须应对与分布式目录方案相同的许多挑战。

### Implementing Snooping Cache Coherence

When we wrote the first edition of this book in 1990, our final "Putting It All Together" was a 30-processor, single-bus multiprocessor using snoop-based coherence; the bus had a capacity of just over 50 MiB/s, which would not be enough bus bandwidth to support even one core of an Intel i7 in 2017! When we wrote the second edition of this book in 1995, the first cache coherence multiprocessors with more than a single bus had recently appeared, and we added an appendix describing the implementation of snooping in a system with multiple buses. In 2017, _every_ multicore multiprocessor system that supports 8 or more cores uses an interconnect other than a single bus, and designers must face the challenge of implementing snooping (or a directory scheme) without the simplification of a bus to serialize events.

> 当我们在 1990 年撰写本书的第一版时，我们的最终 "将其放在一起" 是使用基于史努比的连贯性的 30 个处理器，单夫人多处理器。公共汽车的容量刚好超过 50 MIB/S，这将是足够的公共汽车带宽，即使在 2017 年，即使是 Intel I7 的一个核心！当我们在 1995 年撰写本书的第二版时，最近出现了一个超过单个总线的 Cache 连贯性多处理器，我们添加了一个附录，描述了在具有多个总线的系统中的 Snooping 实现。在 2017 年，支持 8 个或更多核心的多层多处理器系统使用单个总线以外的其他互连，设计师必须面对实施窥探(或目录方案)的挑战，而无需简化总线以序列化事件。

As we observed on page 386, the major complication in actually implementing the snooping coherence protocol we have described is that write and upgrade misses are not atomic in any recent multiprocessor. The steps of detecting a write or upgrade miss, communicating with the other processors and memory, getting the most recent value for a write miss and ensuring that any invalidates are processed, and updating the cache cannot be done as though they took a single cycle.

> 正如我们在第 386 页上观察到的那样，实际实施我们描述的窥探连贯协议的主要复杂性是，在任何最近的多处理器中，写入和升级误差都不是原子。检测写入或升级失误，与其他处理器和内存进行通信，获得写入失误的最新价值并确保处理任何无效的任何无效的步骤，并且更新缓存无法完成。

In a multicore with a single bus, these steps can be made effectively atomic by arbitrating for the bus to the shared cache or memory first (before changing the cache state) and not releasing the bus until all actions are complete. How can the processor know when all the invalidates are complete? In early designs, a single line was used to signal when all necessary invalidates had been received and were being processed. Following that signal, the processor that generated the miss could release the bus, knowing that any required actions would be completed before any activity related to the next miss. By holding the bus exclusively during these steps, the processor effectively made the individual steps atomic.

> 在具有单个总线的多核心中，可以通过将总线仲裁到共享的缓存或内存(在更改高速缓存状态之前)，而不是释放总线直到所有操作完成之前，可以有效地制定这些步骤。处理器如何知道所有无效的人何时完成？在早期设计中，使用一条线来发出信号，当收到所有必要的无效并正在处理时。在该信号之后，生成失误的处理器可能会释放公交车，知道在与下一次失误有关的任何活动之前都将完成任何必需的操作。通过在这些步骤中仅持有公共汽车，处理器有效地使单个步骤原子化。

In a system without a single, central bus, we must find some other method of making the steps in a miss atomic. In particular, we must ensure that two processors that attempt to write the same block at the same time, a situation which is called a _race_, are strictly ordered: one write is processed and precedes before the next is begun. It does not matter which of two writes in a race wins the race, just that there be only a single winner whose coherence actions are completed first. In a multicore using multiple buses, races can be eliminated if each block of memory is associated with only a single bus, ensuring that two attempts to access the same block must be serialized by that common bus. This property, together with the ability to restart the miss handling of the loser in a race, are the keys to implementing snooping cache coherence without a bus. We explain the details in Appendix I.

> 在没有单个中央总线的系统中，我们必须找到其他原子能中的步骤。特别是，我们必须确保严格订购两个试图同时编写相同块的处理器，即称为 *race* 的情况：处理一个写入并在下一个开始之前先进行了。比赛中的两个写作中的哪个赢得比赛并不重要，只是只有一个赢家首先完成了连贯的行动。在使用多个总线的多核心中，如果每个内存的每个内存仅与单个总线相关联，则可以消除种族，从而确保两次尝试访问相同块的尝试必须由该普通总线序列化。该属性以及能够在比赛中重新启动失败者的错过处理的能力，是实施无公共汽车的窥探缓存连贯性的关键。我们解释了附录 I 中的细节。

It is possible to combine snooping and directories, and several designs use snooping within a multicore and directories among multiple chips or a combination of directories at one cache level and snooping at another level.

> 可以将窥探和目录结合起来，并且几种设计在多个芯片中使用侦探和目录中的侦探，或者在一个缓存级别上的目录组合并在另一个级别上窥探。
