## Models of Memory Consistency: An Introduction

Cache coherence ensures that multiple processors see a consistent view of mem- ory. It does not answer the question of _how_ consistent the view of memory must be. By "how consistent," we are really asking when a processor must see a value that has been updated by another processor. Because processors communicate through shared variables (used both for data values and for synchronization), the question boils down to this: In what order must a processor observe the data writes of another processor? Because the only way to "observe the writes of another proces- sor" is through reads, the question becomes what properties must be enforced among reads and writes to different locations by different processors?

> 缓存连贯性可确保多个处理器看到对内存的一致视图。它没有回答 *how* 的问题一致的内存视图。通过 "多么一致" ，我们真的在询问处理器何时必须看到另一个处理器更新的值。由于处理器通过共享变量进行通信(既用于数据值和同步)，所以问题归结为：处理器必须以什么顺序观察另一个处理器的数据写入？因为 "观察另一个过程的写作" 的唯一方法是通过阅读，所以问题变成了在读取和写入不同处理器向不同位置写入的属性？

Although the question of how consistent memory must be seems simple, it is remarkably complicated, as we can see with a simple example. Here are two code segments from processes P1 and P2, shown side by side:

> 尽管我们可以通过一个简单的示例看到的是，尽管一致的内存似乎必须很简单，但它非常复杂，但它非常复杂。这是来自过程 P1 和 P2 的两个代码段，并排显示：

Assume that the processes are running on different processors, and that locations A and B are originally cached by both processors with the initial value of 0. If writes always take immediate effect and are immediately seen by other processors, it will be impossible for _both_ IF statements (labeled L1 and L2) to evaluate their condi- tions as true, since reaching the IF statement means that either A or B must have been assigned the value 1. But suppose the write invalidate is delayed, and the processor is allowed to continue during this delay. Then it is possible that both P1 and P2 have not seen the invalidations for B and A (respectively) _before_ they attempt to read the values. The question now is should this behavior be allowed, and, if so, under what conditions?

> 假设这些过程正在在不同的处理器上运行，并且位置 A 和 B 最初由两个处理器均可缓存为 0。语句(标记为 L1 和 L2)将其条件评估为真实，因为到达 IF 语句意味着必须分配了 A 或 B。在此延迟期间。那么，P1 和 P2 都可能没有看到 B 和 A(分别为 A)\_BEFORE 的无效，他们试图读取值。现在的问题是应该允许这种行为，如果是，在什么条件下？

The most straightforward model for memory consistency is called _sequential consistency_. Sequential consistency requires that the result of any execution be the same as though the memory accesses executed by each processor were kept in order and the accesses among different processors were arbitrarily interleaved. Sequential consistency eliminates the possibility of some nonobvious execution in the previous example because the assignments must be completed before the IF statements are initiated.

> 内存一致性的最直接模型称为* sequinential Ontermency*。顺序一致性要求任何执行的结果都相同，就像每个处理器执行的内存访问权限保持顺序，并且不同处理器之间的访问是任意交织的。顺序一致性消除了上一个示例中某些不明显执行的可能性，因为必须在启动 IF 语句之前完成作业。

The simplest way to implement sequential consistency is to require a processor to delay the completion of any memory access until all the invalidations caused by that access are completed. Of course, it is equally effective to delay the next mem- ory access until the previous one is completed. Remember that memory consis- tency involves operations among different variables: the two accesses that must be ordered are actually to different memory locations. In our example, we must delay the read of A or B (A ==0 or B == 0) until the previous write has completed (B= 1 or A= 1). Under sequential consistency, we cannot, for example, simply place the write in a write buffer and continue with the read.

> 实现顺序一致性的最简单方法是要求处理器延迟任何内存访问的完成，直到完成该访问引起的所有无效。当然，延迟下一个 mem 访问到上一个访问权限同样有效。请记住，内存的同意涉及不同变量之间的操作：必须订购的两个访问实际上是在不同的内存位置。在我们的示例中，我们必须延迟 A 或 B 的读取(A == 0 或 B == 0)，直到上一篇写入完成(B = 1 或 A = 1)。在顺序的一致性下，我们不能只需将写入放置在写缓冲区中，然后继续读取。

Although sequential consistency presents a simple programming paradigm, it reduces potential performance, especially in a multiprocessor with a large number of processors or long interconnect delays, as we can see in the following example.

> 尽管顺序一致性显示了一个简单的编程范式，但它降低了潜在的性能，尤其是在具有大量处理器或长时间互连延迟的多处理器中，正如我们可以在以下示例中看到的那样。

Example Suppose we have a processor where a write miss takes 50 cycles to establish own- ership, 10 cycles to issue each invalidate after ownership is established, and 80 cycles for an invalidate to complete and be acknowledged once it is issued. Assum- ing that four other processors share a cache block, how long does a write miss stall the writing processor if the processor is sequentially consistent? Assume that the invalidates must be explicitly acknowledged before the coherence controller knows they are completed. Suppose we could continue executing after obtaining ownership for the write miss without waiting for the invalidates; how long would the write take?

> 示例假设我们有一个处理器，在该处理器中，写作小姐需要 50 个周期来建立自己的企业，建立所有权后的 10 个循环，并在发行后完成了 80 个周期，并在发行后被确认。假设其他四个处理器共享一个缓存块，如果处理器顺序一致，则写作 Miss Miss Witter Backal tall Miss stall 多长时间？假设必须在连贯控制器知道完成之前明确确认无效的人。假设我们可以在不等待无效的情况下获得写作失误的所有权后继续执行；写作需要多长时间？

_Answer_ When we wait for invalidates, each write takes the sum of the ownership time plus the time to complete the invalidates. Because the invalidates can overlap, we need only worry about the last one, which starts 10 + 10 + 10 + 10 40 cycles after own- ership is established. Therefore the total time for the write is 50 + 40 + 80 170 cycles. In comparison, the ownership time is only 50 cycles. With appropriate write buffer implementations, it is even possible to continue before ownership is established.

> *answer* 当我们等待无效时，每个写入都有所有权时间的总和以及完成无效的时间。由于无效的人可能会重叠，因此我们只需要担心最后一个，这是在建立自己的企业后开始 10 + 10 + 10 + 10 40 周期。因此，写入的总时间为 50 + 40 + 80 170 周期。相比之下，所有权时间仅为 50 个周期。通过适当的写缓冲区实现，甚至可以在建立所有权之前继续进行。

To provide better performance, researchers and architects have explored two dif- ferent routes. First, they developed ambitious implementations that preserve sequential consistency but use latency-hiding techniques to reduce the penalty; we discuss these in [Section 5.7](#cross-cutting-issues-2). Second, they developed less restrictive memory consistency models that allow for faster hardware. Such models can affect how the programmer sees the multiprocessor, so before we discuss these less restrictive models, let’s look at what the programmer expects.

> 为了提供更好的性能，研究人员和工程师探索了两条不同的路线。首先，他们开发了雄心勃勃的实现，以保持顺序一致性，但使用延迟隐藏技术来减少罚款。我们在[第 5.7 节](%EF%BC%83%E4%BA%A4%E5%8F%89%E5%88%87%E5%AD%97-2)中讨论这些。其次，他们开发了较少的限制性内存一致性模型，可以更快地进行硬件。这样的模型可以影响程序员如何看待多处理器，因此，在我们讨论这些限制性较小的模型之前，让我们看看程序员的期望。

### The Programmer’s View

Although the sequential consistency model has a performance disadvantage, from the viewpoint of the programmer, it has the advantage of simplicity. The challenge is to develop a programming model that is simple to explain and yet allows a high- performance implementation.

> 尽管顺序一致性模型具有性能劣势，但从程序员的角度来看，它具有简单性的优势。面临的挑战是开发一个易于解释且允许高性能实现的编程模型。

One such programming model that allows us to have a more efficient imple- mentation is to assume that programs are _synchronized_. A program is synchronized if all accesses to shared data are ordered by synchronization operations. A data ref- erence is ordered by a synchronization operation if, in every possible execution, a write of a variable by one processor and an access (either a read or a write) of that variable by another processor are separated by a pair of synchronization opera- tions, one executed after the write by the writing processor and one executed before the access by the second processor. Cases where variables may be updated without ordering by synchronization are called _data races_ because the execution outcome depends on the relative speed of the processors, and like races in hardware design, the outcome is unpredictable, which leads to another name for synchronized pro- grams: _data-race-free_.

> 这样的编程模型使我们能够具有更有效的效率，就是假设程序是 *synchronized*。如果对共享数据的所有访问均通过同步操作订购，则将程序同步。如果在每个可能的执行中，由一个处理器写入变量，而另一个处理器的该变量的访问(读取或写入)则通过同步操作来订购数据参考。操作，一个由写作处理器在写作后执行的操作，第二个处理器在访问之前执行。可以在不同步订购的情况下更新变量的情况称为 *data races*，因为执行结果取决于处理器的相对速度，并且像硬件设计中的种族一样，结果是不可预测的，这导致了同步 Pro-Grams 的另一个名称：_data-race free _。

As a simple example, consider a variable being read and updated by two dif- ferent processors. Each processor surrounds the read and update with a lock and an unlock, both to ensure mutual exclusion for the update and to ensure that the read is consistent. Clearly, every write is now separated from a read by the other processor by a pair of synchronization operations: one unlock (after the write) and one lock (before the read). Of course, if two processors are writing a variable with no inter- vening reads, then the writes must also be separated by synchronization operations. It is a broadly accepted observation that most programs are synchronized. This observation is true primarily because, if the accesses were unsynchronized, the behavior of the program would likely be unpredictable because the speed of exe- cution would determine which processor won a data race and thus affect the results of the program. Even with sequential consistency, reasoning about such programs is very difficult.

> 作为一个简单的示例，请考虑两个不同处理器正在读取和更新的变量。每个处理器都用锁定和解锁包围读取和更新，以确保更新的相互排除并确保读取是一致的。显然，现在每个写入者都通过一对同步操作与另一个处理器的读取：一个解锁(写入后)和一个锁(在读取之前)。当然，如果两个处理器编写一个没有相互读取的变量，那么书写也必须通过同步操作分开。这是一个广泛接受的观察结果，即大多数程序都是同步的。该观察结果之所以如此，主要是因为，如果访问不同步，则该程序的行为可能是不可预测的，因为速度的速度将确定哪个处理器赢得了数据竞赛，从而影响了程序的结果。即使有顺序的一致性，对此类程序的推理也非常困难。

Programmers could attempt to guarantee ordering by constructing their own synchronization mechanisms, but this is extremely tricky, can lead to buggy pro- grams, and may not be supported architecturally, meaning that they may not work in future generations of the multiprocessor. Instead, almost all programmers will choose to use synchronization libraries that are correct and optimized for the mul- tiprocessor and the type of synchronization.

> 程序员可以尝试通过构建自己的同步机制来保证秩序，但这非常棘手，可能导致货物范围，并且可能不会受到架构的支持，这意味着它们可能在多个处理器的后代不起作用。取而代之的是，几乎所有程序员都将选择使用对 Multipsersor 和同步类型进行正确且优化的同步库。

Finally, the use of standard synchronization primitives ensures that even if the architecture implements a more relaxed consistency model than sequential consis- tency, a synchronized program will behave as though the hardware implemented sequential consistency.

> 最后，使用标准同步原始功能可确保，即使体系结构实现了比顺序共识更轻松的一致性模型，同步程序也会表现得好像实现的连续一致性一样。

### Relaxed Consistency Models: The Basics and Release Consistency

The key idea in relaxed consistency models is to allow reads and writes to complete out of order, but to use synchronization operations to enforce ordering so that a synchronized program behaves as though the processor were sequentially consistent. There are a variety of relaxed models that are classified according to what read and write orderings they relax. We specify the orderings by a set of rules of the form X Y, meaning that operation X must complete before operation Y is done. Sequential consistency requires maintaining all four possible orderings: R W, R R, W R, and W W. The relaxed models are defined by the subset of four orderings they relax:

> 放松的一致性模型中的关键思想是允许读取和写入订单，但要使用同步操作来强制执行排序，以使同步程序的行为表现得好像处理器是顺序一致的。有各种各样的放松模型根据他们放松的读写顺序进行分类。我们通过表格 X y 的一组规则指定订单，这意味着操作 X 必须在完成操作 y 之前完成。顺序一致性需要维持所有四个可能的顺序：R W，R R，W R 和 W W W.松弛的模型由它们放松的四个订单的子集定义：

1. Relaxing only the W R ordering yields a model known as _total store ordering_ or _processor consistency_. Because this model retains ordering among writes, many programs that operate under sequential consistency operate under this model, without additional synchronization.

> 1.仅放松 w r 排序会产生一个称为 *total Store Ordering* 或* Processor Entermency*的模型。因为该模型保留在写作中的顺序，所以许多在该模型下运行的程序在没有其他同步的情况下运行。

2. Relaxing both the W R ordering and the W W ordering yields a model known as _partial store order_.

> 2.放宽 W r 排序和 W w 排序都会产生一个称为\_-帕特店订单的模型。

3. Relaxing all four orderings yields a variety of models including _weak ordering_, the PowerPC consistency model, and _release consistency_, the RISC V consistency model.

> 3.放松所有四个订单都会产生各种型号，包括* Weak Ordering*，PowerPC 一致性模型和 *release Enstermency*，Risc V 一致性模型。

By relaxing these orderings, the processor may obtain significant performance advantages, which is the reason that RISC V, ARMv8, as well as the C++ and C language standards chose release consistency as the model.

> 通过放松这些订单，处理器可以获得显着的性能优势，这是 RISC V，ARMV8 以及 C ++ 和 C 语言标准选择释放一致性作为模型的原因。

_Release consistency_ distinguishes between synchronization operations that are used to _acquire_ access to a shared variable (denoted S<sub>A</sub>) and those that _release_ an object to allow another processor to acquire access (denoted S<sub>R</sub>). Release consistency is based on the observation that in synchronized programs, an acquire operation must precede a use of shared data, and a release operation must follow any updates to shared data and also precede the time of the next acquire. This property allows us to slightly relax the ordering by observing that a read or write that precedes an acquire need not complete before the acquire, and also that a read or write that follows a release need not wait for the release. Thus the orderings that are preserved involve only S<sub>A</sub> and S<sub>R</sub>, as shown in [Figure 5.23](#_bookmark243); as the example in [Figure 5.24](#_bookmark244) shows, this model imposes the few- est orders of the five models.

> *release Enterioncy *区分用于* acquire*访问共享变量(表示 s <ub> a </sub>)的同步操作与 *release* *release* 对象允许另一个处理器获取访问的那些(表示 S <ub> r </sub>)。发布一致性是基于这样的观察结果：在同步程序中，获取操作必须在使用共享数据之前进行，并且释放操作必须遵循任何更新以进行共享数据，并在下一个获取时间之前。该属性使我们能够通过观察获得在获取之前的读取或写入时稍微放松订单，而在获取之前不需要完成，并且在发布后的读取或写入也不需要等待发行版。因此，保留的订单仅涉及 s <sub> a </sub>和 s <sub> r </sub>，如[图 5.23](#_bookmark243) 所示；正如[图 5.24](#_bookmark244) 中的示例所示，该模型施加了五个模型的几个订单。

Release consistency provides one of the least restrictive models that is easily checkable and ensures that synchronized programs will see a sequentially consis- tent execution. Although most synchronization operations are either an acquire or a release (an acquire normally reads a synchronization variable and atomically updates it, and a release usually just writes it), some operations, such as a barrier, act as both an acquire and a release and cause the ordering to be equivalent to weak ordering. Although synchronization operations always ensure that previous writes have completed, we may want to guarantee that writes are completed without an identified synchronization operation. In such cases, an explicit instruction, called FENCE in RISC V, is used to ensure that all previous instructions in that thread have completed, including completion of all memory writes and associated inval- idates. For more information about the complexities, implementation issues, and

> 发布一致性提供了易于检查的最小限制模型之一，并确保同步程序将看到一个顺序执行的执行。尽管大多数同步操作要么是获取或发行版本(收购通常读取同步变量并在原子上更新它，而且发行版通常只是写入它)，但某些操作(例如屏障)作为获取和释放和释放和释放，并且导致订购等同于弱排序。尽管同步操作始终确保以前的写入已经完成，但我们可能希望保证在没有确定的同步操作的情况下完成写入。在这种情况下，使用 RISC V 中称为围栏的显式指令用于确保该线程中的所有先前指令都已完成，包括完成所有内存写入和相关的无效 ID。有关复杂性，实施问题以及的更多信息

Figure 5.23 The orderings imposed by various consistency models are shown for both ordinary accesses and synchronization accesses. The models grow from most restrictive (sequential consistency) to least restrictive (release consistency), allowing increased flexibility in the implementation. The weaker models rely on fences created by syn- chronization operations, as opposed to an implicit fence at every memory operation. S<sub>A</sub> and S<sub>R</sub> stand for acquire and release operations, respectively, and are needed to define release consistency. If we were to use the notation S<sub>A</sub> and S<sub>R</sub> for each S consistently, each ordering with one S would become two orderings (e.g., S ! W becomes S<sub>A</sub> ! W, S<sub>R</sub> ! W), and each S! S would become the four orderings shown in the last line of the bottom-right table entry.

> 图 5.23 为普通访问和同步访问显示了各种一致性模型所施加的订单。这些模型从大多数限制性(顺序的一致性)增长到最少限制性(释放一致性)，从而提高了实施的灵活性。较弱的模型依赖于合成过程创建的围栏，而不是在每个内存操作中的隐式围栏。S <sub> a </sub>和 s <sub> r </sub>分别代表获取和释放操作，并且需要定义发布一致性。如果我们要使用符号 s <sub> a </sub>和 s <sub> r </sub>始终如一，则每个 s 的订购将变成两个订单(例如，s！s！> a </sub>！w，s <sub> r </sub>！S 将成为右下表条目的最后一行中显示的四个订单。

![](../media/image298.png)acquire (S);

Figure 5.24 These examples of the five consistency models discussed in this section show the reduction in the number of orders imposed as the models become more relaxed. Only the minimum orders are shown with arrows. Orders implied by transitivity, such as the write of C before the release of S in the sequential consistency model or the acquire before the release in weak ordering or release consistency, are not shown.

> 图 5.24 本节中讨论的五个一致性模型的这些示例表明，随着模型变得更加放松，施加的订单数量减少。仅显示箭头的最小订单。未显示通过传递性隐含的订单，例如在顺序一致性模型中释放 s 之前的 C 写入或以弱排序或释放一致性释放之前获得的订单。

performance potential from relaxed models, we highly recommend the excellent tutorial by [Adve and Gharachorloo (1996)](#_bookmark917).

> 从放松模型中的性能潜力，我们强烈推荐[Adve and Gharachorloo(1996)]的出色教程(#\_bookmark917)。
