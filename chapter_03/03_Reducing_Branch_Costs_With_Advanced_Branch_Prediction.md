## Reducing Branch Costs With Advanced Branch Prediction(通过高级分支预测降低分支机构成本)

Because of the need to enforce control dependences through branch hazards and stalls, branches will hurt pipeline performance. Loop unrolling is one way to reduce the number of branch hazards; we can also reduce the performance losses of branches by predicting how they will behave. In [Appendix C](#_bookmark481), we examine simple branch predictors that rely either on compile-time information or on the observed dynamic behavior of a single branch in isolation. As the number of instructions in flight has increased with deeper pipelines and more issues per clock, the importance of more accurate branch prediction has grown. In this section, we examine techniques for improving dynamic prediction accuracy. This section makes extensive use of the simple 2-bit predictor covered in Section C.2, and it is critical that the reader understand the operation of that predictor before proceeding.

> 由于需要通过分支危害和摊位来执行控制依赖，因此分支会损害管道性能。循环展开是减少分支危害数量的一种方法；我们还可以通过预测它们的行为方式来减少分支机构的绩效损失。在[附录 C](#_bookmark481) 中，我们检查了依赖编译时间信息或单个分支的观察到的动态行为的简单分支预测指标。随着飞行中的指示数量随着更深的管道和每个时钟的更多问题的增加而增加，更准确的分支预测的重要性已经增长。在本节中，我们研究了提高动态预测准确性的技术。本节广泛使用 C.2 节中涵盖的简单 2 位预测变量，至关重要的是，读者在继续操作之前了解该预测变量的操作。

### Correlating Branch Predictors(关联分支预测指标)

The 2-bit predictor schemes in [Appendix C](#_bookmark481) use only the recent behavior of a single branch to predict the future behavior of that branch. It may be possible to improve the prediction accuracy if we also look at the recent behavior of _other_ branches rather than just the branch we are trying to predict. Consider a small code fragment from the eqntott benchmark, a member of early SPEC benchmark suites that displayed particularly bad branch prediction behavior:

> [附录 C](#_bookmark481) 中的 2 位预测器方案仅使用单个分支的最新行为来预测该分支的未来行为。如果我们还查看\_other\*分支的最新行为，而不仅仅是我们要预测的分支，则可以提高预测准确性。考虑 Eqntott Benchmark 的小型代码片段，Eqntott Benchmark 是早期规格基准套件的成员，显示出特别不良的分支预测行为：

> ===

Here is the RISC-V code that we would typically generate for this code fragment assuming that aa and bb are assigned to registers x1 and x2:

> 这是我们通常会为此代码片段生成的 RISC-V 代码，假设 AA 和 BB 分配给寄存器 X1 和 X2：

> ===

Let’s label these branches b1, b2, and b3. The key observation is that the behavior of branch b3 is correlated with the behavior of branches b1 and b2. Clearly, if neither branches b1 nor b2 are taken (i.e., if the conditions both evaluate to true and aa and bb are both assigned 0), then b3 will be taken, because aa and bb are clearly equal. A predictor that uses the behavior of only a single branch to predict the outcome of that branch can never capture this behavior.

> 让我们标记这些分支 B1，B2 和 B3。关键观察是，分支 B3 的行为与分支 B1 和 B2 的行为相关。显然，如果分支 b1 和 b2 均未采用(即，如果条件均评估为 true，则分配了 AA 和 BB，则将进行 B3，因为 AA 和 BB 显然是相等的。仅使用单个分支的行为来预测该分支的结果的预测因子永远无法捕获这种行为。

Branch predictors that use the behavior of other branches to make a prediction are called _correlating predictors_ or _two-level predictors_. Existing correlating predictors add information about the behavior of the most recent branches to decide how to predict a given branch. For example, a (1,2) predictor uses the behavior of the last branch to choose from among a pair of 2-bit branch predictors in predicting a particular branch. In the general case, an (_m,n_) predictor uses the behavior of the last _m_ branches to choose from 2*<sup>m</sup>* branch predictors, each of which is an _n_-bit predictor for a single branch. The attraction of this type of correlating branch predictor is that it can yield higher prediction rates than the 2-bit scheme and requires only a trivial amount of additional hardware.

> 使用其他分支的行为进行预测的分支预测指标称为* correal preadiveors *或* two 级预测器*。现有的相关预测因素添加了有关最新分支的行为的信息，以决定如何预测给定的分支。例如，A(1,2)预测指标使用最后一个分支的行为，以在预测特定分支的一对 2 位分支预测因子中选择。在一般情况下，(_m，n_)预测变量使用了最后一个 _m_ 分支的行为，可从 2* <sup> m </sup>*分支预测变量中进行选择，每个分支预测变量是单个分支的* n*-bit 预测变量。这种类型的相关分支预测器的吸引力是，它可以产生比 2 位方案更高的预测率，并且仅需要一定数量的其他硬件。

The simplicity of the hardware comes from a simple observation: the global history of the most recent _m_ branches can be recorded in an _m_-bit shift register, where each bit records whether the branch was taken or not taken. The branchprediction buffer can then be indexed using a concatenation of the low-order bits from the branch address with the _m_-bit global history. For example, in a (2,2) buffer with 64 total entries, the 4 low-order address bits of the branch (word address) and the 2 global bits representing the behavior of the two most recently executed branches form a 6-bit index that can be used to index the 64 counters. By combining the local and global information by concatenation (or a simple hash function), we can index the predictor table with the result and get a prediction as fast as we could for the standard 2-bit predictor, as we will do very shortly.

> 硬件的简单性来自一个简单的观察：最新 _m_ 分支的全局历史记录可以记录在 _m_-bit shift 寄存器中，每个位记录了分支是否被拍摄。然后，可以使用分支地址与 _M_-BIT 全局历史记录的低阶位置串联来对分支机构的缓冲区进行索引。例如，在具有 64 个条目的(2,2)缓冲区中，分支的 4 个低阶地址位(单词地址)和 2 个代表两个最近执行的分支的行为的全局位，形成了 6 位 6 位可用于索引 64 个计数器的索引。通过通过串联(或简单的哈希函数)组合局部和全局信息，我们可以将预测表索引与结果索引，并尽可能快地获得标准 2 位预测变量的预测，因为我们很快就会做。

How much better do the correlating branch predictors work when compared with the standard 2-bit scheme? To compare them fairly, we must compare predictors that use the same number of state bits. The number of bits in an (_m_,_n_) predictor is 2*<sup>m</sup>* × _n_ × Number of prediction entries selected by the branch address A 2-bit predictor with no global history is simply a (0,2) predictor.

> 与标准的 2 位方案相比，相关分支预测因子的工作方式更好？要公平地比较它们，我们必须比较使用相同数量的状态位的预测指标。(_m _，_ n_)预测变量的位数为 2* <sup> m </sup>*×*n*× 由分支地址选择的 2 位预测器，没有全局历史记录仅为 a(0)(0)，2)预测指标。

Example How many bits are in the (0,2) branch predictor with 4K entries? How many entries are in a (2,2) predictor with the same number of bits?

> 示例(0,2)分支预测器中有多少位具有 4K 条目？(2,2)的预测指标中有多少个条目？

> ===

[Figure 3.3](#_bookmark100) compares the misprediction rates of the earlier (0,2) predictor with 4K entries and a (2,2) predictor with 1K entries. As you can see, this correlating predictor not only outperforms a simple 2-bit predictor with the same total number of state bits, but it also often outperforms a 2-bit predictor with an unlimited number of entries.

> [图 3.3](#_bookmark100) 将早期(0,2)预测的错误预测率与 4K 条目和(2,2)的预测变量进行了 1K 条目。如您所见，这种相关的预测变量不仅优于具有相同总数的简单 2 位预测变量，而且通常也经常优于具有无限数量条目的 2 位预测变量。

Perhaps the best-known example of a correlating predictor is McFarling’s gshare predictor. In gshare the index is formed by combining the address of the branch and the most recent conditional branch outcomes using an exclusiveOR, which essentially acts as a hash of the branch address and the branch history. The hashed result is used to index a prediction array of 2-bit counters, as shown in [Figure 3.4](#_bookmark101). The gshare predictor works remarkably well for a simple predictor, and is often used as the baseline for comparison with more sophisticated predictors. Predictors that combine local branch information and global branch history are also called _alloyed predictors or hybrid predictors_.

> 相关预测变量的最著名的例子也许是 McFarling 的 Gshare 预测指标。在 Gshare 中，该索引是通过使用独家组合分支的地址和最新条件分支结果组合来形成的，该分支机构本质上是分支地址和分支历史记录的哈希。哈希结果用于索引 2 位计数器的预测数组，如[图 3.4](#_bookmark101) 所示。Gshare 预测器对于一个简单的预测指标非常有效，并且通常用作与更复杂的预测指标进行比较的基线。将本地分支信息和全球分支历史记录相结合的预测因素也称为 "合金预测变量" 或 "混合预测因子" 。

### Tournament Predictors: Adaptively Combining Local and Global Predictors(锦标赛预测指标：自适应结合本地和全球预测指标)

The primary motivation for correlating branch predictors came from the observation that the standard 2-bit predictor, using only local information, failed on some important branches. Adding global history could help remedy this situation. _Tournament predictors_ take this insight to the next level, by using multiple predictors, usually a global predictor and a local predictor, and choosing between them

> 关联分支预测因子的主要动机来自这样的观察结果，即仅使用局部信息的标准 2 位预测指标在某些重要分支上失败。增加全球历史可以帮助解决这种情况。*tournament 预测因素*通过使用多个预测指标，通常是全局预测指标和局部预测变量，并在它们之间选择，将此洞察力提升到一个新的水平

> ===

Figure 3.3 Comparison of 2-bit predictors. A noncorrelating predictor for 4096 bits is first, followed by a noncorrelating 2-bit predictor with unlimited entries and a 2-bit predictor with 2 bits of global history and a total of 1024 entries. Although these data are for an older version of SPEC, data for more recent SPEC benchmarks would show similar differences in accuracy.

> 图 3.3 2 位预测因子的比较。首先是 4096 位的非相关预测指标，其次是无与伦比的 2 位预测指标，带有无限条目和 2 位预测指标，具有 2 位全球历史记录和总共 1024 个条目。尽管这些数据适用于旧版本的规格，但最新规格基准测试的数据在准确性上会显示出类似的差异。

with a selector, as shown in [Figure 3.5](#_bookmark102). A _global predictor_ uses the most recent branch history to index the predictor, while a _local predictor_ uses the address of the branch as the index. Tournament predictors are another form of hybrid or alloyed predictors.

> 使用选择器，如[图 3.5](#* Bookmark102)所示。\_global predivitor *使用最新的分支历史记录来索引预测因子，而* local preadiveor *则使用分支的地址作为索引。锦标赛预测因子是混合或合金预测变量的另一种形式。

Tournament predictors can achieve better accuracy at medium sizes (8K–32K bits) and also effectively use very large numbers of prediction bits. Existing tournament predictors use a 2-bit saturating counter per branch to choose among two different predictors based on which predictor (local, global, or even some timevarying mix) was most effective in recent predictions. As in a simple 2-bit predictor, the saturating counter requires two mispredictions before changing the identity of the preferred predictor.

> 比赛预测因素可以在中等大小(8k – 32k 位)上实现更好的准确性，并且还可以有效地使用大量的预测位。现有的比赛预测因素使用每个分支的 2 位饱和计数器在两个不同的预测指标中进行选择，基于哪个预测因子(本地，全球甚至某种时间变化的混合)在最近的预测中最有效。与简单的 2 位预测指标中一样，饱和计数器在更改首选预测因子的身份之前需要两个错误预测。

The advantage of a tournament predictor is its ability to select the right predictor for a particular branch, which is particularly crucial for the integer benchmarks.

> 锦标赛预测指标的优点是它可以为特定分支选择正确的预测指标，这对于整数基准特别重要。

> ===

Figure 3.4 A gshare predictor with 1024 entries, each being a standard 2-bit predictor.

Figure 3.5 A tournament predictor using the branch address to index a set of 2-bit selection counters, which choose between a local and a global predictor. In this case, the index to the selector table is the current branch address. The two tables are also 2-bit predictors that are indexed by the global history and branch address, respectively. The selector acts like a 2-bit predictor, changing the preferred predictor for a branch address when two mispredicts occur in a row. The number of bits of the branch address used to index the selector table and the local predictor table is equal to the length of the global branch history used to index the global prediction table. Note that misprediction is a bit tricky because we need to change both the selector table and either the global or local predictor.

> 图 3.5 使用分支地址的锦标赛预测器来索引一组 2 位选择计数器，该计数器在本地和全局预测器之间进行选择。在这种情况下，选择器表的索引是当前的分支地址。这两个表也是 2 位预测指标，分别由全球历史和分支地址索引。选择器的作用像 2 位预测指标，当两个错误预测的行为中发生时，更改了分支地址的首选预测指标。用于索引选择器表和本地预测表的分支地址的位数等于用于索引全局预测表的全局分支历史记录的长度。请注意，错误预测有点棘手，因为我们需要更改选择器表以及全局或本地预测指标。

A typical tournament predictor will select the global predictor almost 40% of the time for the SPEC integer benchmarks and less than 15% of the time for the SPEC FP benchmarks. In addition to the Alpha processors that pioneered tournament predictors, several AMD processors have used tournament-style predictors.

> 典型的比赛预测指标将在 SPEC Integer 基准测试中为全球预测变量，而对于 SPEC FP 基准测试的时间不到 15％。除了开创了锦标赛预测器的 Alpha 处理器外，几个 AMD 处理器还使用了锦标赛风格的预测指标。

[Figure 3.6](#_bookmark103) looks at the performance of three different predictors (a local 2-bit predictor, a correlating predictor, and a tournament predictor) for different numbers of bits using SPEC89 as the benchmark. The local predictor reaches its limit first. The correlating predictor shows a significant improvement, and the tournament predictor generates a slightly better performance. For more recent versions of the SPEC, the results would be similar, but the asymptotic behavior would not be reached until slightly larger predictor sizes.

> [图 3.6](#_bookmark103) 着眼于使用 SPEC89 作为基准的三个不同数量的位列的三个不同预测指标的性能(局部 2 位预测指标，相关预测指标和锦标赛预测指标)。本地预测因子首先达到其极限。相关的预测指标显示出显着的改进，并且比赛预测器的性能稍好一些。对于规格的最新版本，结果将是相似的，但是直到预测尺寸稍大，才能达到渐近行为。

The local predictor consists of a two-level predictor. The top level is a local history table consisting of 1024 10-bit entries; each 10-bit entry corresponds to the most recent 10 branch outcomes for the entry. That is, if the branch is taken 10 or more times in a row, the entry in the local history table will be all 1s. If the branch is alternately taken and untaken, the history entry consists of alternating 0s and 1s. This 10-bit history allows patterns of up to 10 branches to be discovered and predicted. The selected entry from the local history table is used to index a table of 1K entries consisting of 3-bit saturating counters, which provide the local prediction. This combination, which uses a total of 29K bits, leads to high accuracy in branch prediction while requiring fewer bits than a single level table with the same prediction accuracy.

> 局部预测指标由两级预测指标组成。顶级是一个本地历史表，该表由 1024 个 10 位条目组成；每个 10 位条目对应于该条目的最新 10 个分支结果。也就是说，如果分支连续 10 次或更多次，则本地历史记录表中的条目将全部为 1。如果分支是交替的，没有被取消，则历史记录输入包括交替的 0 和 1。这种 10 位历史允许发现和预测多达 10 个分支的模式。从本地历史表中选出的条目用于索引一个由 3 位饱和计数器组成的 1K 条目的表，这些表提供了本地预测。这种组合使用总共 29k 位，可导致分支预测的高精度，同时需要比单级表较少的钻头，具有相同的预测精度。

Figure 3.6 The misprediction rate for three different predictors on SPEC89 versus the size of the predictor in kilobits. The predictors are a local 2-bit predictor, a correlating predictor that is optimally structured in its use of global and local information at each point in the graph, and a tournament predictor. Although these data are for an older version of SPEC, data for more recent SPEC benchmarks show similar behavior, perhaps converging to the asymptotic limit at slightly larger predictor sizes.

> 图 3.6 SPEC89 的三个不同预测指标的错误预测率与千位预测变量的大小。预测变量是局部的 2 位预测指标，它是一个相关的预测指标，在图表中每个点使用全球和本地信息以及锦标赛预测指标方面是最佳构建的。尽管这些数据适用于旧版本的规格，但最新规格基准测试的数据显示出相似的行为，也许会在稍大的预测器尺寸下融合到渐近极限。

### Tagged Hybrid Predictors(标记的混合预测指标)

The best performing branch prediction schemes as of 2017 involve combining multiple predictors that track whether a prediction is likely to be associated with the current branch. One important class of predictors is loosely based on an algorithm for statistical compression called PPM (Prediction by Partial Matching). PPM (see Jim´enez and Lin, 2001), like a branch prediction algorithm, attempts to predict future behavior based on history. This class of branch predictors, which we call _tagged hybrid predictors_ (see Seznec and Michaud, 2006), employs a series of global predictors indexed with different length histories.

> 截至 2017 年的最佳性能分支预测方案涉及组合多个预测指标，这些预测指标跟踪预测是否可能与当前分支相关联。一类重要的预测因子是基于称为 PPM 的统计压缩算法(通过部分匹配的预测)。PPM(参见 Jim´enez 和 Lin，2001)，就像分支预测算法一样，试图根据历史来预测未来的行为。这类分支预测指标，我们称为 _tagged Hybrid Predictors_(参见 Seznec 和 Michaud，2006 年)，采用了一系列具有不同长度历史的全局预测指标。

For example, as shown in [Figure 3.7](#_bookmark104), a five-component tagged hybrid predictor has five prediction tables: P(0), P(1), . . . P(4), where P(_i_) is accessed using a hash of the PC and the history of the most recent _i_ branches (kept in a shift register, h, just as in gshare). The use of multiple history lengths to index separate predictors is the first critical difference. The second critical difference is the use of tags in tables P(1) through P(4). The tags can be short because 100% matches are not required: a small tag of 4–8 bits appears to gain most of the advantage. A prediction from P(1), . . . P(4) is used only if the tags match the hash of the branch address and global branch history. Each of the predictors in P(0…_n_) can be a standard 2-bit predictor. In practice a 3-bit counter, which requires three mispredictions to change a prediction, gives slightly better results than a 2-bit counter.

> 例如，如[图 3.7](#_bookmark104) 所示，五个组件标记的混合预测器具有五个预测表：p(0)，p(1)，。。。p(4)，其中 p(\_i*)使用 PC 的哈希访问 p(\_i*)和最新 _i_ 分支的历史记录(保存在移位寄存器中，H，就像在 GShare 中一样)。使用多个历史记录长度索引单独的预测指标是第一个关键差异。第二个临界区域是在表 P(1)至 P(4)中使用标签。标签可以很短，因为不需要 100％匹配：4-8 位的小标签似乎获得了大部分优势。p(1)，。。。仅当标签与分支地址和全局分支历史记录的哈希相匹配时，才会使用 p(4)。p(0…_n_)中的每个预测变量都可以是标准的 2 位预测指标。在实践中，一个 3 位计数器需要三个错误预测才能改变预测，从而比 2 位计数器可以更好地效果。

![](../media/image199.png)

Figure 3.7 A five-component tagged hybrid predictor has five separate prediction tables, indexed by a hash of the branch address and a segment of recent branch history of length 0–4 labeled  "h"  in this figure. The hash can be as simple as an exclusive-OR, as in gshare. Each predictor is a 2-bit (or possibly 3-bit) predictor. The tags are typically 4–8 bits. The chosen prediction is the one with the longest history where the tags also match.

> 图 3.7 五组分标记的混合预测器具有五个单独的预测表，该表由分支地址的哈希(Hash)和该图中标记为 " H" 的最新分支历史记录段。哈希可以像 gshare 一样简单。每个预测变量是 2 位(或可能是 3 位)预测指标。标签通常为 4-8 位。选定的预测是标签也匹配的历史最长的预测。

The prediction for a given branch is the predictor with the longest branch history that also has matching tags. P(0) always matches because it uses no tags and becomes the default prediction if none of P(1) through P(_n_) match. The tagged hybrid version of this predictor also includes a 2-bit use field in each of the history-indexed predictors. The use field indicates whether a prediction was recently used and therefore likely to be more accurate; the use field can be periodically reset in all entries so that old predictions are cleared. Many more details are involved in implementing this style of predictor, especially how to handle mispredictions. The search space for the optimal predictor is also very large because the number of predictors, the exact history used for indexing, and the size of each predictor are all variable.

> 给定分支的预测是具有最长分支历史记录的预测指标，该预测指标也具有匹配的标签。p(0)总是匹配的，因为它不使用标签，并且如果 p(1)通过 p(_n_)匹配，则成为默认预测。该预测变量的标记混合版本还包括每个历史记录指数预测指标中的 2 位使用字段。使用字段表示最近是否使用了预测，因此可能更准确。使用字段可以在所有条目中定期重置，以便清除旧的预测。实施这种预测变量，尤其是如何处理错误预测的方式，还涉及更多细节。最佳预测变量的搜索空间也很大，因为预测变量的数量，用于索引的确切历史记录以及每个预测变量的大小都是可变的。

Tagged hybrid predictors (sometimes called TAGE—TAgged GEometic— predictors) and the earlier PPM-based predictors have been the winners in recent annual international branch-prediction competitions. Such predictors outperform gshare and the tournament predictors with modest amounts of memory (32– 64 KiB), and in addition, this class of predictors seems able to effectively use larger prediction caches to deliver improved prediction accuracy.

> 标记的混合预测因子(有时称为 tage-tage-tage-tage tage tage-geometic-预测指标)和较早的基于 ppm 的预测因子一直是最近年度国际分支预测竞赛的胜利者。此类预测因子的表现优于 GSHARE 和具有适度的内存量(32-64 KIB)的比赛预测变量，此外，此类的预测变量似乎能够有效地使用较大的预测库来提供改进的预测准确性。

Another issue for larger predictors is how to initialize the predictor. It could be initialized randomly, in which case, it will take a fair amount of execution time to fill the predictor with useful predictions. Some predictors (including many recent predictors) include a valid bit, indicating whether an entry in the predictor has been set or is in the  "unused state."  In the latter case, rather than use a random prediction, we could use some method to initialize that prediction entry. For example, some instruction sets contain a bit that indicates whether an associated branch is expected to be taken or not. In the days before dynamic branch prediction, such hint bits _were_ the prediction; in recent processors, that hint bit can be used to set the initial prediction. We could also set the initial prediction on the basis of the branch direction: forward going branches are initialized as not taken, while backward going branches, which are likely to be loop branches, are initialized as taken. For programs with shorter running times and processors with larger predictors, this initial setting can have a measurable impact on prediction performance.

> 较大预测指标的另一个问题是如何初始化预测变量。它可以随机初始化，在这种情况下，将需要大量的执行时间才能填充预测器有用的预测。一些预测因素(包括许多最近的预测指标)包括一个有效的位，表明预测因子中的条目已设置或处于 "未使用状态" 。在后一种情况下，我们可以使用一些方法来初始化该预测条目。例如，某些指令集包含一些指示是否预期采用关联分支。在动态分支预测的前几天，这种提示位 _were_ 预测；在最近的处理器中，该提示位可用于设置初始预测。我们还可以根据分支方向设置初始预测：向前进行分支的初始化为未采用，而向后进行的分支(可能是循环分支)被初始化为采用。对于具有较短的运行时间和带有较大预测变量的处理器的程序，此初始设置可以对预测性能产生可衡量的影响。

[Figure 3.8](#_bookmark105) shows that a hybrid tagged predictor significantly outperforms gshare, especially for the less predictable programs like SPECint and server applications. In this figure, performance is measured as mispredicts per thousand instructions; assuming a branch frequency of 20%–25%, gshare has a mispredict rate (per branch) of 2.7%–3.4% for the multimedia benchmarks, while the tagged hybrid predictor has a misprediction rate of 1.8%–2.2%, or roughly one-third fewer mispredicts. Compared to gshare, tagged hybrid predictors are more complex to implement and are probably slightly slower because of the need to check multiple tags and choose a prediction result. Nonetheless, for deeply pipelined processors with large penalties for branch misprediction, the increased accuracy outweighs those disadvantages. Thus many designers of higher-end processors have opted to include tagged hybrid predictors in their newest implementations.

> [图 3.8](#_bookmark105) 表明，混合标记的预测变量明显胜过 gshare，尤其是对于诸如 Specint 和 Server 应用程序(Specint 和 Server 应用程序)较低的程序。在这个数字中，绩效被衡量为每千说明的错误预测。假设分支机构频率为 20％–25％，则 GSHARE 的多媒体基准的错误预测率(每个分支)为 2.7％–3.4％，而标记的混合预测率的错误预测率为 1.8％–2.2％，或大约一个。- 错误预测的三分之一。与 Gshare 相比，标记的混合预测变量更为复杂，并且由于需要检查多个标签并选择预测结果，因此可能会稍慢一些。尽管如此，对于对分支机构错误预测的严重罚款的深度管道处理器，其准确性的提高超过了这些缺点。因此，许多高端处理器的设计人员选择在其最新实现中包括标记的混合预测指标。

Figure 3.8 A comparison of the misprediction rate (measured as mispredicts per 1000 instructions executed) for tagged hybrid versus gshare. Both predictors use the same total number of bits, although tagged hybrid uses some of that storage for tags, while gshare contains no tags. The benchmarks consist of traces from SPECfp and SPECint, a series of multimedia and server benchmarks. The latter two behave more like SPECint.

> 图 3.8 对标记的混合动力与 GSHARE 的错误预测率(每 1000 个指令)的错误预测率进行比较。两个预测器都使用相同的总数，尽管标记的混合动力车将其中的一些存储空间用于标签，而 Gshare 则不包含标签。这些基准由 SpecFP 和 Specint 的痕迹组成，这是一系列多媒体和服务器基准。后两个行为更像是规格。

### The Evolution of the Intel Core i7 Branch Predictor(Intel Core i7 分支预测器的演变)

As mentioned in the previous chapter, there were six generations of Intel Core i7 processors between 2008 (Core i7 920 using the Nehalem microarchitecture) and 2016 (Core i7 6700 using the Skylake microarchitecture). Because of the combination of deep pipelining and multiple issues per clock, the i7 has many instructions in-flight at once (up to 256, and typically at least 30). This makes branch prediction critical, and it has been an area where Intel has been making constant improvements. Perhaps because of the performance-critical nature of the branch predictor, Intel has tended to keep the details of its branch predictors highly secret.

> 如上一章中所述，2008 年之间有六代英特尔核心 i7 处理器(使用 Nehalem Microharchitecture 的 Core i7 920)和 2016 年(使用 Skylake Microharchitecture)(Core i7 6700)。由于每个时钟的深层管道和多个问题的结合，i7 一次有许多指示(最多 256 个，通常至少 30 个)。这使分支预测至关重要，并且它一直是英特尔一直在不断改进的领域。也许是由于分支预测指标的性能至关重要的性质，英特尔倾向于将其分支预测因子的细节保持高度秘密。

Even for older processors such as the Core i7 920 introduced in 2008, they have released only limited amounts of information. In this section, we briefly describe what is known and compare the performance of predictors of the Core i7 920 with those in the latest Core i7 6700.

> 即使对于 2008 年推出的核心 i7 920 等较旧的处理器，它们也仅发布了有限的信息。在本节中，我们简要描述了核心 i7 920 的预测指标的性能与最新核心 i7 6700 中的预测指标的性能。

The Core i7 920 used a two-level predictor that has a smaller first-level predictor, designed to meet the cycle constraints of predicting a branch every clock cycle, and a larger second-level predictor as a backup. Each predictor combines three different predictors: (1) the simple 2-bit predictor, which is introduced in [Appendix C](#_bookmark481) (and used in the preceding tournament predictor); (2) a global history predictor, like those we just saw; and (3) a loop exit predictor. The loop exit predictor uses a counter to predict the exact number of taken branches (which is the number of loop iterations) for a branch that is detected as a loop branch. For each branch, the best prediction is chosen from among the three predictors by tracking the accuracy of each prediction, like a tournament predictor. In addition to this multilevel main predictor, a separate unit predicts target addresses for indirect branches, and a stack to predict return addresses is also used.

> Core i7 920 使用了具有较小第一级预测变量的两级预测指标，旨在满足每个时钟周期预测分支的周期约束，而较大的二级预测变量作为备份。每个预测指标结合了三个不同的预测指标：(1)简单的 2 位预测器，该预测指标在[附录 C](#_bookmark481) 中引入(并在前面的比赛预测器中使用)；(2)像我们刚刚看到的那样的全球历史预测指标；(3)循环出口预测指标。循环出口预测器使用计数器来预测被检测为循环分支的分支的确切数量(这是循环迭代的数量)。对于每个分支，最佳预测是通过跟踪每个预测的准确性(如锦标赛预测指标)来从三个预测指标中选择的。除了此多级主预测指标外，一个单独的单元预测间接分支的目标地址，还使用了预测返回地址的堆栈。

Although even less is known about the predictors in the newest i7 processors, there is good reason to believe that Intel is employing a tagged hybrid predictor. One advantage of such a predictor is that it combines the functions of all three second-level predictors in the earlier i7. The tagged hybrid predictor with different history lengths subsumes the loop exit predictor as well as the local and global history predictor. A separate return address predictor is still employed.

> 尽管对最新 i7 处理器中的预测变量的了解少，但有充分的理由相信英特尔正在采用标记的混合预测指标。这种预测因子的一个优点是，它结合了早期 i7 中所有三个二级预测指标的功能。具有不同历史长度的标记的混合预测指标集成了循环退出预测指标以及局部和全球历史预测指标。仍然采用单独的返回地址预测指标。

As in other cases, speculation causes some challenges in evaluating the predictor because a mispredicted branch can easily lead to another branch being fetched and mispredicted. To keep things simple, we look at the number of mispredictions as a percentage of the number of successfully completed branches (those that were not the result of misspeculation). [Figure 3.9](#_bookmark107) shows these data for SPECPUint2006 benchmarks. These benchmarks are considerably larger than SPEC89 or SPEC2000, with the result being that the misprediction rates are higher than those in [Figure 3.6](#_bookmark103) even with a more powerful combination of predictors. Because branch misprediction leads to ineffective speculation, it contributes to the wasted work, as we will see later in this chapter.

> 与其他情况一样，投机在评估预测变量时会引起一些挑战，因为错误预测的分支很容易导致另一个分支被取消和错误预测。为了使事情变得简单，我们将错误预测的数量视为成功完成分支机构数量的百分比(不是违法的结果)。[图 3.9](#_bookmark107) 显示了 Specpuint2006 基准测试的这些数据。这些基准比 Spec89 或 Spec2000 大得多，结果是，即使预测率更强大，预测率也高于[图 3.6](#_bookmark103) 中的错误率。由于分支错误预测会导致猜测无效，因此这有助于浪费的工作，正如我们将在本章后面看到的那样。
