## Case Studies and Exercises by Norman P. Jouppi, Rajeev Balasubramonian, Naveen Muralimanohar, and Sheng Li

### Case Study 1: Optimizing Cache Performance via Advanced Techniques

##### _Concepts illustrated by this case study_

- Nonblocking Caches
- Compiler Optimizations for Caches
- Software and Hardware Prefetching
- Calculating Impact of Cache Performance on More Complex Processors
  The transpose of a matrix interchanges its rows and columns; this concept is illustrated here:

A11
A12

A13 A14

A11 A21 A31 A41

A21
A22 A23 A24

⇒
A12 A22 A32 A42

A31 A32 A33 A34 A41 A42 A43 A44

A13 A23 A33 A43 A14 A24 A34 A44

Here is a simple C loop to show the transpose:

for (i = 0; i &lt; 3; i++) { for (j = 0; j &lt; 3; j++) {

output\[j\]\[i\] = input\[i\]\[j\];

}

}

Assume that both the input and output matrices are stored in the row major order (_row major order_ means that the row index changes fastest). Assume that you are executing a 256 256 double-precision transpose on a processor with a 16 KB fully associative (don’t worry about cache conflicts) least recently used (LRU) replace- ment L1 data cache with 64-byte blocks. Assume that the L1 cache misses or pre- fetches require 16 cycles and always hit in the L2 cache, and that the L2 cache can process a request every 2 processor cycles. Assume that each iteration of the pre- ceding inner loop requires 4 cycles if the data are present in the L1 cache. Assume that the cache has a write-allocate fetch-on-write policy for write misses. Unreal- istically, assume that writing back dirty cache blocks requires 0 cycles.

1. 15/15/12/20\] &lt;2.3&gt; For the preceding simple implementation, this execution order would be nonideal for the input matrix; however, applying a loop interchange
   optimization would create a nonideal order for the output matrix. Because loop interchange is not sufficient to improve its performance, it must be blocked instead.
2. &lt;2.3&gt; What should be the minimum size of the cache to take advantage of blocked execution?
3. &lt;2.3&gt; How do the relative number of misses in the blocked and unblocked versions compare in the preceding minimum-sized cache?
4. &lt;2.3&gt; Write code to perform a transpose with a block size parameter _B_
   that uses _B_· _B_ blocks.
5. &lt;2.3&gt; What is the minimum associativity required of the L1 cache for consistent performance independent of both arrays’ position in memory?
6. &lt;2.3&gt; Try out blocked and nonblocked 256 256 matrix transpositions on a computer. How closely do the results match your expectations based on what
   you know about the computer’s memory system? Explain any discrepancies if possible.
7. &lt;2.3&gt; Assume you are designing a hardware prefetcher for the
   preceding
   _unblocked_ matrix transposition code. The simplest type of hardware prefetcher only

prefetches sequential cache blocks after a miss. More complicated “nonunit stride” hardware prefetchers can analyze a miss reference stream and detect and prefetch nonunit strides. In contrast, software prefetching can determine nonunit strides as eas- ily as it can determine unit strides. Assume prefetches write directly into the cache and that there is no “pollution” (overwriting data that must be used before the data that are prefetched). For best performance given a nonunit stride prefetcher, in the steady state of the inner loop, how many prefetches must be outstanding at a given time?

1. 20\] &lt;2.3&gt; With software prefetching, it is important to be
   careful to have the prefetches occur in time for use but also to
   minimize the number of outstanding
   prefetches to live within the capabilities of the microarchitecture and minimize cache pollution. This is complicated by the fact that different processors have dif- ferent capabilities and limitations.
2. &lt;2.3&gt; Create a blocked version of the matrix transpose with software prefetching.
3. &lt;2.3&gt; Estimate and compare the performance of the blocked and unblocked transpose codes both with and without software prefetching.

### Case Study 2: Putting It All Together: Highly Parallel Memory Systems

##### _Concept illustrated by this case study_

- Cross-Cutting Issues: The Design of Memory Hierarchies
  The program in [Figure 2.32](#_bookmark88) can be used to evaluate the behavior of a memory sys- tem. The key is having accurate timing and then having the program stride through memory to invoke different levels of the hierarchy. [Figure 2.32](#_bookmark88) shows the code in

C. The first part is a procedure that uses a standard utility to get an accurate measure of the user CPU time; this procedure may have to be changed to work on some systems. The second part is a nested loop to read and write memory at different strides and cache sizes. To get accurate cache timing, this code is repeated many times. The third part times the nested loop overhead only so that it can be subtracted from overall measured times to see how long the accesses were. The results are output in .csv file format to facilitate importing into spreadsheets. You may need to change CACHE_MAX depending on the question you are answer- ing and the size of memory on the system you are measuring. Running the program in single-user mode or at least without other active applications will give more con- sistent results. The code in [Figure 2.32](#_bookmark88) was derived from a program written by Andrea Dusseau at the University of California-Berkeley and was based on a detailed description found in [Saavedra-Barrera (1992)](#_bookmark998). It has been modified to fix a number of issues with more modern machines and to run under Microsoft

<span id="_bookmark88" class="anchor"></span>\#include "stdafx.h" \#include &lt;stdio.h&gt; \#include &lt;time.h&gt;

\#define ARRAY_MIN (1024) /\* 1/4 smallest cache \*/ \#define ARRAY_MAX (4096\*4096) /\* 1/4 largest cache \*/ int x\[ARRAY_MAX\]; /\* array going to stride through \*/

double get_seconds() { /\* routine to read time in seconds \*/

time64_t ltime;

\_time64( &ltime ); return (double) ltime;

}

int label(int i) {/\* generate text labels \*/ if (i&lt;1e3) printf("%1dB,",i);

else if (i&lt;1e6) printf("%1dK,",i/1024); else if (i&lt;1e9) printf("%1dM,",i/1048576); else printf("%1dG,",i/1073741824);

return 0;

}

int \_tmain(int argc, \_TCHAR\* argv\[\]) { int register nextstep, i, index, stride; int csize;

double steps, tsteps;

double loadtime, lastsec, sec0, sec1, sec; /\* timing variables \*/

/\* Initialize output \*/ printf(" ,");

for (stride=1; stride &lt;= ARRAY_MAX/2; stride=stride\*2) label(stride\*sizeof(int));

printf("\\n");

/\* Main loop for each configuration \*/

for (csize=ARRAY_MIN; csize &lt;= ARRAY_MAX; csize=csize\*2) { label(csize\*sizeof(int)); /\* print cache size this loop \*/ for (stride=1; stride &lt;= csize/2; stride=stride\*2) {

/\* Lay out path of memory references in array \*/ for (index=0; index &lt; csize; index=index+stride)

x\[index\] = index + stride; /\* pointer to next \*/ x\[index-stride\] = 0; /\* loop back to beginning \*/

/\* Wait for timer to roll over \*/ lastsec = get_seconds();

sec0 = get_seconds(); while (sec0 == lastsec);

/\* Walk through path in array for twenty seconds \*/

/\* This gives 5% accuracy with second resolution \*/ steps = 0.0; /\* number of steps taken \*/

nextstep = 0; /\* start at beginning of path \*/ sec0 = get_seconds(); /\* start timer \*/

{ /\* repeat until collect 20 seconds \*/ (i=stride;i!=0;i=i-1) { /\* keep samples same \*/

nextstep = 0;

do nextstep = x\[nextstep\]; /\* dependency \*/ while (nextstep != 0);

}

steps = steps + 1.0; /\* count loop iterations \*/ sec1 = get_seconds(); /\* end timer \*/

} while ((sec1 - sec0) &lt; 20.0); /\* collect 20 seconds \*/ sec = sec1 - sec0;

/\* Repeat empty loop to loop subtract overhead \*/ tsteps = 0.0; /\* used to match no. while iterations \*/ sec0 = get_seconds(); /\* start timer \*/

{ /\* repeat until same no. iterations as above \*/ (i=stride;i!=0;i=i-1) { /\* keep samples same \*/

index = 0;

do index = index + stride; while (index &lt; csize);

}

tsteps = tsteps + 1.0;

sec1 = get_seconds(); /\* - overhead \*/

} while (tsteps&lt;steps); /\* until = no. iterations \*/ sec = sec - (sec1 - sec0);

loadtime = (sec\*1e9)/(steps\*csize);

/\* write out results in .csv format for Excel \*/ printf("%4.1f,", (loadtime&lt;0.1) ? 0.1 : loadtime);

}; /\* end of inner for loop \*/ printf("\\n");

}; /\* end of outer for loop \*/ return 0;

}

Figure 2.32 C program for evaluating memory system.

Visual C++. It can be downloaded from [http://www.hpl.hp.com/research/cacti/](http://www.hpl.hp.com/research/cacti/aca_ch2_cs2.c) [aca_ch2_cs2.c](http://www.hpl.hp.com/research/cacti/aca_ch2_cs2.c).

The preceding program assumes that program addresses track physical addresses, which is true on the few machines that use virtually addressed caches, such as the Alpha 21264. In general, virtual addresses tend to follow physical addresses shortly after rebooting, so you may need to reboot the machine in order to get smooth lines in your results. To answer the following questions, assume that the sizes of all components of the memory hierarchy are powers of 2. Assume that the size of the page is much larger than the size of a block in a second-level cache (if there is one) and that the size of a second-level cache block is greater than or equal to the size of a block in a first-level cache. An example of the output of the program is plotted in [Figure 2.33](#_bookmark89); the key lists the size of the array that is exercised.

1. 12/12/10/12\] &lt;2.6&gt; Using the sample program results in
   [Figure 2.33](#_bookmark89):
   a\. \[12\] &lt;2.6&gt; What are the overall size and block size of the second-level cache?

b\. \[12\] &lt;2.6&gt; What is the miss penalty of the second-level cache?

c\. \[12\] &lt;2.6&gt; What is the associativity of the second-level cache?

d\. \[10\] &lt;2.6&gt; What is the size of the main memory?

e\. \[12\] &lt;2.6&gt; What is the paging time if the page size is 4 KB?

![](./media/image158.png)<span id="_bookmark89"
class="anchor"></span>1000

100

10

1
4B 16B 64B 256B 1K

4K 16K 64K 256K 1M

Stride

4M 16M 64M 256M

Figure 2.33 Sample results from program in Figure 2.32.

1. 15/15/20\] &lt;2.6&gt; If necessary, modify the code in [Figure 2.32](#_bookmark88) to measure the following system characteristics. Plot the experimental results with elapsed time on
   the _y_-axis and the memory stride on the _x_-axis. Use logarithmic scales for both axes, and draw a line for each cache size.
2. &lt;2.6&gt; What is the system page size?
3. &lt;2.6&gt; How many entries are there in the TLB?
4. &lt;2.6&gt; What is the miss penalty for the TLB?
5. &lt;2.6&gt; What is the associativity of the TLB?

<!-- -->

1. 20\] &lt;2.6&gt; In multiprocessor memory systems, lower levels of the memory hierarchy may not be able to be saturated by a single processor but should be able
   to be saturated by multiple processors working together. Modify the code in [Figure 2.32](#_bookmark88), and run multiple copies at the same time. Can you determine:
2. &lt;2.6&gt; How many actual processors are in your computer system and how many system processors are just additional multithreaded contexts?
3. &lt;2.6&gt; How many memory controllers does your system have?

<!-- -->

1. &lt;2.6&gt; Can you think of a way to test some of the characteristics of an instruc- tion cache using a program? _Hint_: The compiler may generate a large number of
   nonobvious instructions from a piece of code. Try to use simple arithmetic instruc- tions of known length in your instruction set architecture (ISA).

### Case Study 3: Studying the Impact of Various Memory System Organizations

##### _Concepts illustrated by this case study_

- DDR3 memory systems
- Impact of ranks, banks, row buffers on performance and power
- DRAM timing parameters
  A processor chip typically supports a few DDR3 or DDR4 memory channels. We will focus on a single memory channel in this case study and explore how its per- formance and power are impacted by varying several parameters. Recall that the channel is populated with one or more DIMMs. Each DIMM supports one or more ranks—a rank is a collection of DRAM chips that work in unison to service a single command issued by the memory controller. For example, a rank may be composed of 16 DRAM chips, where each chip deals with a 4-bit input or output on every channel clock edge. Each such chip is referred to as a 4 (by four) chip. In other examples, a rank may be composed of 8 8 chips or 4 16 chips—note that in each case, a rank can handle data that are being placed on a 64-bit memory channel. A rank is itself partitioned into 8 (DDR3) or 16 (DDR4) banks. Each bank has a row buffer that essentially remembers the last row read out of a bank. Here’s an example of a typical sequence of memory commands when performing a read from a bank:

\(i\) The memory controller issues a Precharge command to get the bank ready to access a new row. The precharge is completed after time tRP.

\(ii\) The memory controller then issues an Activate command to read the appro- priate row out of the bank. The activation is completed after time tRCD and the row is deemed to be part of the row buffer.

\(iii\) The memory controller can then issue a column-read or CAS command that places a specific subset of the row buffer on the memory channel. After time CL, the first 64 bits of the data burst are placed on the memory channel. A burst typically includes eight 64-bit transfers on the memory channel, per- formed on the rising and falling edges of 4 memory clock cycles (referred to as transfer time).

\(iv\) If the memory controller wants to then access data in a different row of the bank, referred to as a row buffer miss, it repeats steps (i)–(iii). For now, we will assume that after CL has elapsed, the Precharge in step (i) can be issued; in some cases, an additional delay must be added, but we will ignore that delay here. If the memory controller wants to access another block of data in the same row, referred to as a row buffer hit, it simply issues another CAS command. Two back-to-back CAS commands have to be separated by at least 4 cycles so that the first data transfer is complete before the second data transfer can begin.

Note that a memory controller can issue commands to different banks in successive cycles so that it can perform many memory reads/writes in parallel and it is not sitting idle waiting for tRP, tRCD, and CL to elapse in a single bank. For the sub- sequent questions, assume that tRP tRCD CL 13 ns, and that the memory channel frequency is 1 GHz, that is, a transfer time of 4 ns.

1. &lt;2.2&gt; What is the read latency experienced by a memory
   controller on a row buffer miss?
2. &lt;2.2&gt; What is the latency experienced by a memory controller
   on a row buffer hit?
3. &lt;2.2&gt; If the memory channel supports only one bank and the
   memory access pattern is dominated by row buffer misses, what is the
   utilization of the memory
   channel?
4. &lt;2.2&gt; Assuming a 100% row buffer miss rate, what is the
   minimum number of banks that the memory channel should support in
   order to achieve a 100% mem-
   ory channel utilization?
5. &lt;2.2&gt; Assuming a 50% row buffer miss rate, what is the minimum
   number of banks that the memory channel should support in order to
   achieve a 100% memory channel utilization?
6. &lt;2.2&gt; Assume that we are executing an application with four
   threads and the threads exhibit zero spatial locality, that is, a
   100% row buffer miss rate. Every
   200 ns, each of the four threads simultaneously inserts a read operation into the

memory controller queue. What is the average memory latency experienced if the memory channel supports only one bank? What if the memory channel supported four banks?

1. &lt;2.2&gt; From these questions, what have you learned about the benefits and downsides of growing the number of banks?
2. &lt;2.2&gt; Now let’s turn our attention to memory power. Download a copy of the Micron power calculator from this link: [https://www.micron.com/](https://www.micron.com/~/media/documents/products/power-calculator/ddr3_power_calc.xlsm) [/media/](https://www.micron.com/~/media/documents/products/power-calculator/ddr3_power_calc.xlsm)
   [documents/products/power-calculator/ddr3_power_calc.xlsm](https://www.micron.com/~/media/documents/products/power-calculator/ddr3_power_calc.xlsm). This spreadsheet is preconfigured to estimate the power dissipation in a single 2 Gb 8 DDR3 SDRAM memory chip manufactured by Micron. Click on the “Summary” tab to see the power breakdown in a single DRAM chip under default usage conditions (reads occupy the channel for 45% of all cycles, writes occupy the channel for 25% of all cycles, and the row buffer hit rate is 50%). This chip consumes 535 mW, and the breakdown shows that about half of that power is expended in Activate oper- ations, about 38% in CAS operations, and 12% in background power. Next, click on the “System Config” tab. Modify the read/write traffic and the row buffer hit rate and observe how that changes the power profile. For example, what is the decrease in power when channel utilization is 35% (25% reads and 10% writes), or when row buffer hit rate is increased to 80%?
3. &lt;2.2&gt; In the default configuration, a rank consists of eight 8 2 Gb DRAM chips. A rank can also comprise16 4 chips or 4 16 chips. You can also vary the
   capacity of each DRAM chip—1 Gb, 2 Gb, and 4 Gb. These selections can be made in the “DDR3 Config” tab of the Micron power calculator. Tabulate the total power consumed for each rank organization. What is the most power-efficient approach to constructing a rank of a given capacity?

### Exercises

1. 12/15\] &lt;2.3&gt; The following questions investigate the impact of small and simple caches using CACTI and assume a 65 nm (0.065 m) technology. (CACTI
   is available in an online form at [http://quid.hpl.hp.com/cacti/.](http://quid.hpl.hp.com:9081/cacti/.))
2. &lt;2.3&gt; Compare the access times of 64 KB caches with 64-byte blocks and a single bank. What are the relative access times of two-way and four-way set
   associative caches compared to a direct mapped organization?
3. &lt;2.3&gt; Compare the access times of four-way set associative caches with 64-byte blocks and a single bank. What are the relative access times of 32 and
   64 KB caches compared to a 16 KB cache?
4. &lt;2.3&gt; For a 64 KB cache, find the cache associativity between 1 and
   8 with the lowest average memory access time given that misses per instruction

for a certain workload suite is 0.00664 for direct-mapped, 0.00366 for two-way set associative, 0.000987 for four-way set associative, and 0.000266 for eight- way set associative cache. Overall, there are 0.3 data references per instruction. Assume cache misses take 10 ns in all models. To calculate the hit time in

cycles, assume the cycle time output using CACTI, which corresponds to the maximum frequency a cache can operate without any bubbles in the pipeline.

1. 15/15/10\] &lt;2.3&gt; You are investigating the possible benefits
   of a way- predicting L1 cache. Assume that a 64 KB four-way set
   associative single-banked L1 data cache is the cycle time limiter in
   a system. For an alternative cache orga-
   nization, you are considering a way-predicted cache modeled as a 64 KB direct- mapped cache with 80% prediction accuracy. Unless stated otherwise, assume that a mispredicted way access that hits in the cache takes one more cycle. Assume the miss rates and the miss penalties in question 2.8 part (c).
2. &lt;2.3&gt; What is the average memory access time of the current cache (in cycles) versus the way-predicted cache?
3. &lt;2.3&gt; If all other components could operate with the faster way-predicted cache cycle time (including the main memory), what would be the impact on
   performance from using the way-predicted cache?
4. &lt;2.3&gt; Way-predicted caches have usually been used only for instruction caches that feed an instruction queue or buffer. Imagine that you want to try out
   way prediction on a data cache. Assume that you have 80% prediction accuracy and that subsequent operations (e.g., data cache access of other instructions, dependent operations) are issued assuming a correct way prediction. Thus a way misprediction necessitates a pipe flush and replay trap, which requires 15 cycles. Is the change in average memory access time per load instruction with data cache way prediction positive or negative, and how much is it?
5. &lt;2.3&gt; As an alternative to way prediction, many large associative L2 caches serialize tag and data access so that only the required dataset array
   needs to be activated. This saves power but increases the access time. Use CACTI’s detailed web interface for a 0.065 m process 1 MB four-way set associative cache with 64-byte blocks, 144 bits read out, 1 bank, only 1 read/write port, 30 bit tags, and ITRS-HP technology with global wires. What is the ratio of the access times for serializing tag and data access compared to parallel access?
6. 12\] &lt;2.3&gt; You have been asked to investigate the relative
   performance of a banked versus pipelined L1 data cache for a new
   microprocessor. Assume a 64 KB
   two-way set associative cache with 64-byte blocks. The pipelined cache would consist of three pipe stages, similar in capacity to the Alpha 21264 data cache. A banked implementation would consist of two 32 KB two-way set associative banks. Use CACTI and assume a 65 nm (0.065 m) technology to answer the fol- lowing questions. The cycle time output in the web version shows at what frequency a cache can operate without any bubbles in the pipeline.
7. &lt;2.3&gt; What is the cycle time of the cache in comparison to its access time, and how many pipe stages will the cache take up (to two decimal places)?
8. &lt;2.3&gt; Compare the area and total dynamic read energy per access of the pipelined design versus the banked design. State which takes up less area and
   which requires more power, and explain why that might be.
9. 15\] &lt;2.3&gt; Consider the usage of critical word first and early restart on L2 cache misses. Assume a 1 MB L2 cache with 64-byte blocks and a refill path
   that is 16 bytes wide. Assume that the L2 can be written with 16 bytes every 4 processor cycles, the time to receive the first 16 byte block from the memory con- troller is 120 cycles, each additional 16 byte block from main memory requires 16 cycles, and data can be bypassed directly into the read port of the L2 cache. Ignore any cycles to transfer the miss request to the L2 cache and the requested data to the L1 cache.
10. &lt;2.3&gt; How many cycles would it take to service an L2 cache miss with and without critical word first and early restart?
11. &lt;2.3&gt; Do you think critical word first and early restart would be more important for L1 caches or L2 caches, and what factors would contribute to their
    relative importance?
12. 12\] &lt;2.3&gt; You are designing a write buffer between a write-through L1 cache and a write-back L2 cache. The L2 cache write data bus is 16 B wide and can per-
    form a write to an independent cache address every four processor cycles.
13. &lt;2.3&gt; How many bytes wide should each write buffer entry be?
14. &lt;2.3&gt; What speedup could be expected in the steady state by using a merging write buffer instead of a nonmerging buffer when zeroing memory
    by the execution of 64-bit stores if all other instructions could be issued in parallel with the stores and the blocks are present in the L2 cache?
15. &lt;2.3&gt; What would the effect of possible L1 misses be on the number of required write buffer entries for systems with blocking and nonblocking caches?

<!-- -->

1. &lt;2.1, 2.2, 2.3&gt; A cache acts as a filter. For example, for every 1000 instruc- tions of a program, an average of 20 memory accesses may exhibit low enough
   locality that they cannot be serviced by a 2 MB cache. The 2 MB cache is said to have an MPKI (misses per thousand instructions) of 20, and this will be largely true regardless of the smaller caches that precede the 2 MB cache. Assume the fol- lowing cache/latency/MPKI values: 32 KB/1/100, 128 KB/2/80, 512 KB/4/50, 2 MB/8/40, 8 MB/16/10. Assume that accessing the off-chip memory system requires 200 cycles on average. For the following cache configurations, calculate the average time spent accessing the cache hierarchy. What do you observe about the downsides of a cache hierarchy that is too shallow or too deep?
2. KB L1; 8 MB L2; off-chip memory
3. KB L1; 512 KB L2; 8 MB L3; off-chip memory
4. KB L1; 128 KB L2; 2 MB L3; 8 MB L4; off-chip memory

<!-- -->

1. &lt;2.1, 2.2, 2.3&gt; Consider a 16 MB 16-way L3 cache that is shared by two programs A and B. There is a mechanism in the cache that monitors cache miss
   rates for each program and allocates 1–15 ways to each program such that the over- all number of cache misses is reduced. Assume that program A has an MPKI of 100 when it is assigned 1 MB of the cache. Each additional 1 MB assigned to program

A reduces the MPKI by 1. Program B has an MPKI of 50 when it is assigned 1 MB of cache; each additional 1 MB assigned to program B reduces its MPKI by 2. What is the best allocation of ways to programs A and B?

1. &lt;2.1, 2.6&gt; You are designing a PMD and optimizing it for low
   energy. The core, including an 8 KB L1 data cache, consumes 1 W
   whenever it is not in hiber- nation. If the core has a perfect L1
   cache hit rate, it achieves an average CPI of 1 for
   a given task, that is, 1000 cycles to execute 1000 instructions. Each additional cycle accessing the L2 and beyond adds a stall cycle for the core. Based on the following specifications, what is the size of L2 cache that achieves the lowest energy for the PMD (core, L1, L2, memory) for that given task?
2. core frequency is 1 GHz, and the L1 has an MPKI of 100.
3. KB L2 has a latency of 10 cycles, an MPKI of 20, a background power of
   0.2 W, and each L2 access consumes 0.5 nJ.
4. MB L2 has a latency of 20 cycles, an MPKI of 10, a background power of
   0.8 W, and each L2 access consumes 0.7 nJ.
5. memory system has an average latency of 100 cycles, a background power of 0.5 W, and each memory access consumes 35 nJ.

<!-- -->

1. &lt;2.1, 2.6&gt; You are designing a PMD that is optimized for low
   power. Qual- itatively explain the impact on cache hierarchy (L2 and
   memory) power and overall
   application energy if you design an L2 cache with:
2. ll block size
3. ll cache size
4. associativity

<!-- -->

1. \[10/10\] &lt;2.1, 2.2, 2.3&gt; The ways of a set can be viewed as a
   priority list, ordered from high priority to low priority. Every
   time the set is touched, the list can be
   reorganized to change block priorities. With this view, cache management policies can be decomposed into three sub-policies: Insertion, Promotion, and Victim Selection. Insertion defines where newly fetched blocks are placed in the priority list. Promotion defines how a block’s position in the list is changed every time it is touched (a cache hit). Victim Selection defines which entry of the list is evicted to make room for a new block when there is a cache miss.
2. Can you frame the LRU cache policy in terms of the Insertion, Promotion, and Victim Selection sub-policies?
3. Can you define other Insertion and Promotion policies that may be competitive and worth exploring further?

   1. \[15\] &lt;2.1, 2.3&gt; In a processor that is running multiple
      programs, the last-level cache is typically shared by all the
      programs. This leads to interference, where
      one program’s behavior and cache footprint can impact the cache available to other programs. First, this is a problem from a quality-of-service (QoS) perspective, where the interference leads to a program receiving fewer resources and lower

performance than promised, say by the operator of a cloud service. Second, this is a problem in terms of privacy. Based on the interference it sees, a program can infer the memory access patterns of other programs. This is referred to as a timing chan- nel, a form of information leakage from one program to others that can be exploited to compromise data privacy or to reverse-engineer a competitor’s algorithm. What policies can you add to your last-level cache so that the behavior of one program is immune to the behavior of other programs sharing the cache?

1. \[15\] &lt;2.3&gt; A large multimegabyte L3 cache can take tens of cycles to access because of the long wires that have to be traversed. For example, it may take
   20 cycles to access a 16 MB L3 cache. Instead of organizing the 16 MB cache such that every access takes 20 cycles, we can organize the cache so that it is an array of smaller cache banks. Some of these banks may be closer to the processor core, while others may be further. This leads to nonuniform cache access (NUCA), where 2 MB of the cache may be accessible in 8 cycles, the next 2 MB in 10 cycles, and so on until the last 2 MB is accessed in 22 cycles. What new policies can you introduce to maximize performance in a NUCA cache?
2. \[10/10/10\] &lt;2.2&gt; Consider a desktop system with a processor connected to a 2 GB DRAM with _error-correcting code (ECC)_. Assume that there is only one
   memory channel of width 72 bits (64 bits for data and 8 bits for ECC).
3. \[10\] &lt;2.2&gt; How many DRAM chips are on the DIMM if 1 Gb DRAM chips are used, and how many data I/Os must each DRAM have if only one DRAM
   connects to each DIMM data pin?
4. \[10\] &lt;2.2&gt; What burst length is required to support 32 B L2 cache blocks?
5. \[10\] &lt;2.2&gt; Calculate the peak bandwidth for DDR2-667 and DDR2-533 DIMMs for reads from an active page excluding the ECC overhead.

<!-- -->

1. \[10/10\] &lt;2.2&gt; A sample DDR2 SDRAM timing diagram is shown in [Figure 2.34](#_bookmark90). tRCD is the time required to activate a row in a bank, and column address
   strobe (CAS) latency (CL) is the number of cycles required to read out a column in a row. Assume that the RAM is on a standard DDR2 DIMM with ECC, having 72 data lines. Also assume burst lengths of 8 that read out 8 bits, or a total of 64 B from the DIMM. Assume tRCD = CAS (or CL) clock_frequency, and clock_frequency = transfers_per_second/2. The on-chip latency

<span id="_bookmark90" class="anchor"></span>Clock

CMD/ ADD

Data

Figure 2.34 DDR2 SDRAM timing diagram.

on a cache miss through levels 1 and 2 and back, not including the DRAM access, is 20 ns.

1. \[10\] &lt;2.2&gt; How much time is required from presentation of the activate command until the last requested bit of data from the DRAM transitions from valid to invalid for the DDR2-667 1 Gb CL 5 DIMM? Assume that
   for every request, we automatically prefetch another adjacent cache line in the same page.
2. \[10\] &lt;2.2&gt; What is the relative latency when using the DDR2-667 DIMM of a read requiring a bank activate versus one to an already open page, including the
   time required to process the miss inside the processor?
3. \[15\] &lt;2.2&gt; Assume that a DDR2-667 2 GB DIMM with CL 5 is
   available for 130 and a DDR2-533 2 GB DIMM with CL 4 is available
   for 100. Assume that two
   DIMMs are used in a system, and the rest of the system costs 800. Consider the performance of the system using the DDR2-667 and DDR2-533 DIMMs on a workload with 3.33 L2 misses per 1K instructions, and assume that 80% of all DRAM reads require an activate. What is the cost-performance of the entire system when using the different DIMMs, assuming only one L2 miss is outstanding at a time and an in-order core with a CPI of 1.5 not including L2 cache miss memory access time?
4. \[12\] &lt;2.2&gt; You are provisioning a server with eight-core 3
   GHz CMP that can execute a workload with an overall CPI of 2.0
   (assuming that L2 cache miss refills
   are not delayed). The L2 cache line size is 32 bytes. Assuming the system uses DDR2-667 DIMMs, how many independent memory channels should be provided so the system is not limited by memory bandwidth if the bandwidth required is sometimes twice the average? The workloads incur, on average, 6.67 L2 misses per 1 K instructions.
5. \[15\] &lt;2.2&gt; Consider a processor that has four memory
   channels. Should consec- utive memory blocks be placed in the same
   bank, or should they be placed in dif-
   ferent banks on different channels?
6. \[12/12\] &lt;2.2&gt; A large amount (more than a third) of DRAM
   power can be due to page activation (see
   [http://download.micron.com/pdf/technotes/ddr2/TN4704.pdf](http://download.micron.com/pdf/technotes/ddr2/TN4704.pdf) and
   [http://www.micron.com/systemcalc](http://www.micron.com/systemcalc)). Assume you are building a
   system with
   2 GB of memory using either 8-bank 2 Gb 8 DDR2 DRAMs or 8-bank 1 Gb 8 DRAMs, both with the same speed grade. Both use a page size of 1 KB, and the last-level cache line size is 64 bytes. Assume that DRAMs that are not active are in precharged standby and dissipate negligible power. Assume that

the time to transition from standby to active is not significant.

1. \[12\] &lt;2.2&gt; Which type of DRAM would be expected to provide the higher system performance? Explain why.
2. \[12\] &lt;2.2&gt; How does a 2 GB DIMM made of 1 Gb 8 DDR2 DRAMs com- pare with a DIMM with similar capacity made of 1 Gb 4 DDR2 DRAMs in
   terms of power?
3. \[20/15/12\] &lt;2.2&gt; To access data from a typical DRAM, we first have to activate the appropriate row. Assume that this brings an entire page of size 8 KB to the row
   buffer. Then we select a particular column from the row buffer. If subsequent accesses to DRAM are to the same page, then we can skip the activation step; oth- erwise, we have to close the current page and precharge the bitlines for the next activation. Another popular DRAM policy is to proactively close a page and precharge bitlines as soon as an access is over. Assume that every read or write to DRAM is of size 64 bytes and DDR bus latency (data from [Figure 2.33](#_bookmark89)) for sending 512 bits is Tddr.
4. \[20\] &lt;2.2&gt; Assuming DDR2-667, if it takes five cycles to precharge, five cycles to activate, and four cycles to read a column, for what value of the row
   buffer hit rate (_r_) will you choose one policy over another to get the best access time? Assume that every access to DRAM is separated by enough time to finish a random new access.
5. \[15\] &lt;2.2&gt; If 10% of the total accesses to DRAM happen back to back or contiguously without any time gap, how will your decision change?
6. \[12\] &lt;2.2&gt; Calculate the difference in average DRAM energy per access between the two policies using the previously calculated row buffer hit rate.
   Assume that precharging requires 2 nJ and activation requires 4 nJ and that 100 pJ/bit are required to read or write from the row buffer.
7. \[15\] &lt;2.2&gt; Whenever a computer is idle, we can either put it in standby (where DRAM is still active) or we can let it hibernate. Assume that, to hibernate, we have
   to copy just the contents of DRAM to a nonvolatile medium such as Flash. If read- ing or writing a cache line of size 64 bytes to Flash requires 2.56 J and DRAM requires 0.5 nJ, and if idle power consumption for DRAM is 1.6 W (for 8 GB), how long should a system be idle to benefit from hibernating? Assume a main memory of size 8 GB.
8. \[10/10/10/10/10\] &lt;2.4&gt; Virtual machines (VMs) have the potential for adding many beneficial capabilities to computer systems, such as improved total cost of ownership (TCO) or availability. Could VMs be used to provide the following
   capabilities? If so, how could they facilitate this?
9. \[10\] &lt;2.4&gt; Test applications in production environments using development machines?
10. \[10\] &lt;2.4&gt; Quick redeployment of applications in case of disaster or failure?
11. \[10\] &lt;2.4&gt; Higher performance in I/O-intensive applications?
12. \[10\] &lt;2.4&gt; Fault isolation between different applications, resulting in higher availability for services?
13. \[10\] &lt;2.4&gt; Performing software maintenance on systems while applications are running without significant interruption?

<!-- -->

1. \[10/10/12/12\] &lt;2.4&gt; Virtualmachinescanloseperformancefromanumberofevents, such as the execution of privileged instructions, TLB misses, traps, and I/O.

<table>
<colgroup>
<col style="width: 31%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th><span id="_bookmark91" class="anchor"></span>Benchmark</th>
<th><blockquote>
<p>Native</p>
</blockquote></th>
<th><blockquote>
<p>Pure</p>
</blockquote></th>
<th><blockquote>
<p>Para</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Null call</td>
<td>0.04</td>
<td>0.96</td>
<td>0.50</td>
</tr>
<tr class="even">
<td>Null I/O</td>
<td>0.27</td>
<td>6.32</td>
<td>2.91</td>
</tr>
<tr class="odd">
<td>Stat</td>
<td>1.10</td>
<td>10.69</td>
<td>4.14</td>
</tr>
<tr class="even">
<td>Open/close</td>
<td>1.99</td>
<td>20.43</td>
<td>7.71</td>
</tr>
<tr class="odd">
<td>Install signal handler</td>
<td>0.33</td>
<td>7.34</td>
<td>2.89</td>
</tr>
<tr class="even">
<td>Handle signal</td>
<td>1.69</td>
<td>19.26</td>
<td>2.36</td>
</tr>
<tr class="odd">
<td>Fork</td>
<td>56.00</td>
<td>513.00</td>
<td>164.00</td>
</tr>
<tr class="even">
<td>Exec</td>
<td>316.00</td>
<td><blockquote>
<p>2084.00</p>
</blockquote></td>
<td>578.00</td>
</tr>
<tr class="odd">
<td>Fork+exec sh</td>
<td>1451.00</td>
<td><blockquote>
<p>7790.00</p>
</blockquote></td>
<td>2360.00</td>
</tr>
</tbody>
</table>
 Figure 2.35 Early performance of various system calls under native execution, pure virtualization, and paravirtualization.

These events are usually handled in system code. Thus one way of estimating the slowdown when running under a VM is the percentage of application execution time in system versus user mode. For example, an application spending 10% of its execution in system mode might slow down by 60% when running on a VM. [Figure 2.35](#_bookmark91) lists the early performance of various system calls under native execu- tion, pure virtualization, and paravirtualization for LMbench using Xen on an Itanium system with times measured in microseconds (courtesy of Matthew Chapman of the University of New South Wales).

1. \[10\] &lt;2.4&gt; What types of programs would be expected to have smaller slowdowns when running under VMs?
2. \[10\] &lt;2.4&gt; If slowdowns were linear as a function of system time, given the preceding slowdown, how much slower would a program spending 20% of its
   execution in system time be expected to run?
3. \[12\] &lt;2.4&gt; What is the median slowdown of the system calls in the table above under pure virtualization and paravirtualization?
4. \[12\] &lt;2.4&gt; Which functions in the table above have the largest slowdowns? What do you think the cause of this could be?

   1. \[12\] &lt;2.4&gt; Popek and Goldberg’s definition of a virtual
      machine said that it would be indistinguishable from a real
      machine except for its performance. In this ques-
      tion, we will use that definition to find out if we have access to native execution on a processor or are running on a virtual machine. The Intel VT-x technology effec- tively provides a second set of privilege levels for the use of the virtual machine. What would a virtual machine running on top of another virtual machine have to do, assuming VT-x technology?
5. \[20/25\] &lt;2.4&gt; With the adoption of virtualization support on
   the x86 architecture, virtual machines are actively evolving and
   becoming mainstream. Compare and
   contrast the Intel VT-x and AMD’s AMD-V virtualization technologies.

(Information on AMD-V can be found at [http://sites.amd.com/us/business/it-](http://sites.amd.com/us/business/it-solutions/virtualization/Pages/resources.aspx) [solutions/virtualization/Pages/resources.aspx](http://sites.amd.com/us/business/it-solutions/virtualization/Pages/resources.aspx).)

1. \[20\] &lt;2.4&gt; Which one could provide higher performance for memory- intensive applications with large memory footprints?
2. \[25\] &lt;2.4&gt; Information on AMD’s IOMMU support for virtualized I/O can be found at [http://developer.amd.com/documentation/articles/pages/892006101.](http://developer.amd.com/documentation/articles/pages/892006101.aspx) [aspx](http://developer.amd.com/documentation/articles/pages/892006101.aspx). What do Virtualization Technology and an input/output memory manage-
   ment unit (IOMMU) do to improve virtualized I/O performance?
3. \[30\] &lt;2.2, 2.3&gt; Since instruction-level parallelism can also be effectively exploited on in-order superscalar processors and _very long instruction word_
   ![](./media/image197.png)_(VLIW)_ processors with speculation, one important reason for building an out- of-order (OOO) superscalar processor is the ability to tolerate unpredictable mem- ory latency caused by cache misses. Thus you can think about hardware supporting OOO issue as being part of the memory system. Look at the floorplan of the Alpha 21264 in [Figure 2.36](#_bookmark92) to find the relative area of the integer and floating-point issue queues and mappers versus the caches. The queues schedule instructions for issue,

<span id="_bookmark92" class="anchor"></span>Figure 2.36 Floorplan of the Alpha 21264 \[Kessler 1999\].

and the mappers rename register specifiers. Therefore these are necessary additions to support OOO issue. The 21264 only has L1 data and instruction caches on chip, and they are both 64 KB two-way set associative. Use an OOO superscalar sim- ulator such as SimpleScalar ([http://www.cs.wisc.edu/](http://www.cs.wisc.edu/~mscalar/simplescalar.html) [mscalar/simplescalar.](http://www.cs.wisc.edu/~mscalar/simplescalar.html) [html](http://www.cs.wisc.edu/~mscalar/simplescalar.html)) on memory-intensive benchmarks to find out how much performance is lost if the area of the issue queues and mappers is used for additional L1 data cache area in an in-order superscalar processor, instead of OOO issue in a model of the 21264. Make sure the other aspects of the machine are as similar as possible to make the comparison fair. Ignore any increase in access or cycle time from larger caches and effects of the larger data cache on the floorplan of the chip. (Note that this com- parison will not be totally fair, as the code will not have been scheduled for the in-order processor by the compiler.)

1. \[15\] &lt;2.2, 2.7&gt; As discussed in [Section 2.7](#_bookmark81),
   the Intel i7 processor has an aggres- sive prefetcher. What are
   potential disadvantages in designing a prefetcher that is
   extremely aggressive?
2. \[20/20/20\] &lt;2.6&gt; The Intel performance analyzer VTune can be
   used to make many measurements of cache behavior. A free evaluation
   version of VTune on
   both Windows and Linux can be downloaded from [http://software.intel.com/en-](http://software.intel.com/en-us/articles/intel-vtune-amplifier-xe/) [us/articles/intel-vtune-amplifier-xe/](http://software.intel.com/en-us/articles/intel-vtune-amplifier-xe/). The program (aca_ch2_cs2.c) used in Case Study 2 has been modified so that it can work with VTune out of the box on Microsoft Visual C++. The program can be downloaded from [http://www.](http://www.hpl.hp.com/research/cacti/aca_ch2_cs2_vtune.c) [hpl.hp.com/research/cacti/aca_ch2_cs2_vtune.c](http://www.hpl.hp.com/research/cacti/aca_ch2_cs2_vtune.c). Special VTune functions have been inserted to exclude initialization and loop overhead during the performance analysis process. Detailed VTune setup directions are given in the README sec- tion in the program. The program keeps looping for 20 seconds for every config- uration. In the following experiment, you can find the effects of data size on cache and overall processor performance. Run the program in VTune on an Intel proces- sor with the input dataset sizes of 8 KB, 128 KB, 4 MB, and 32 MB, and keep a stride of 64 bytes (stride one cache line on Intel i7 processors). Collect statistics on overall performance and L1 data cache, L2, and L3 cache performance.
3. \[20\] &lt;2.6&gt; List the number of misses per 1K instruction of L1 data cache, L2, and L3 for each dataset size and your processor model and speed. Based on the results, what can you say about the L1 data cache, L2, and L3 cache sizes on
   your processor? Explain your observations.
4. \[20\] &lt;2.6&gt; List the _instructions per clock_ (IPC) for each dataset size and your processor model and speed. Based on the results, what can you say about the
   L1, L2, and L3 miss penalties on your processor? Explain your observations.
5. \[20\] &lt;2.6&gt; Run the program in VTune with input dataset size
   of 8 KB and 128 KB on an Intel OOO processor. List the number of L1
   data cache and
   L2 cache misses per 1K instructions and the CPI for both configurations. What can you say about the effectiveness of memory latency hiding techniques in high-performance OOO processors? _Hint_: You need to find the L1 data cache miss latency for your processor. For recent Intel i7 processors, it is approxi- mately 11 cycles.
