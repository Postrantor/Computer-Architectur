## Case Studies and Exercises by Diana Franklin

### Case Study 1: Chip Fabrication Cost

##### *Concepts illustrated by this case study*

- Fabrication Cost
- Fabrication Yield
- Defect Tolerance Through Redundancy
  Many factors are involved in the price of a computer chip. Intel is spending $7 billion to complete its Fab 42 fabrication facility for 7 nm technology. In this case study, we explore a hypothetical company in the same situation and how different design deci- sions involving fabrication technology, area, and redundancy affect the cost of chips.

1. \[10/10\] &lt;1.6&gt; [Figure 1.26](#_bookmark41) gives hypothetical relevant chip statistics that influence the cost of several current chips. In the next few exercises, you will be exploring the
   effect of different possible design decisions for the Intel chips.

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 24%" />
<col style="width: 6%" />
<col style="width: 19%" />
<col style="width: 15%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th><span id="_bookmark41" class="anchor"></span>Chip</th>
<th><blockquote>
<p>Die Size (mm<sup>2</sup>)</p>
</blockquote></th>
<th><blockquote>
<p>Estimated defect rate (per cm<sup>2</sup>)</p>
</blockquote></th>
<th><blockquote>
<p><em>N</em></p>
</blockquote></th>
<th><blockquote>
<p>Manufacturing size (nm)</p>
</blockquote></th>
<th><blockquote>
<p>Transistors (billion)</p>
</blockquote></th>
<th><blockquote>
<p>Cores</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>BlueDragon</td>
<td><blockquote>
<p>180</p>
</blockquote></td>
<td>0.03</td>
<td><blockquote>
<p>12</p>
</blockquote></td>
<td><blockquote>
<p>10</p>
</blockquote></td>
<td>7.5</td>
<td><blockquote>
<p>4</p>
</blockquote></td>
</tr>
<tr class="even">
<td>RedDragon</td>
<td><blockquote>
<p>120</p>
</blockquote></td>
<td>0.04</td>
<td><blockquote>
<p>14</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td>7.5</td>
<td><blockquote>
<p>4</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>Phoenix<sup>8</sup></td>
<td><blockquote>
<p>200</p>
</blockquote></td>
<td>0.04</td>
<td><blockquote>
<p>14</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td>12</td>
<td><blockquote>
<p>8</p>
</blockquote></td>
</tr>
</tbody>
</table>
 Figure 1.26 Manufacturing cost factors for several hypothetical current and future processors.

1. \[10\] &lt;1.6&gt; What is the yield for the Phoenix chip?
2. \[10\] &lt;1.6&gt; Why does Phoenix have a higher defect rate than
   BlueDragon?

   1. \[20/20/20/20\] &lt;1.6&gt; They will sell a range of chips from
      that factory, and they need to decide how much capacity to
      dedicate to each chip. Imagine that they will
      sell two chips. Phoenix is a completely new architecture designed with 7 nm tech- nology in mind, whereas RedDragon is the same architecture as their 10 nm Blue- Dragon. Imagine that RedDragon will make a profit of $15 per defect-free chip. Phoenix will make a profit of $30 per defect-free chip. Each wafer has a 450 mm diameter.
3. \[20\] &lt;1.6&gt; How much profit do you make on each wafer of
   Phoenix chips?
4. \[20\] &lt;1.6&gt; How much profit do you make on each wafer of RedDragon chips?
5. \[20\] &lt;1.6&gt; If your demand is 50,000 RedDragon chips per
   month and 25,000 Phoenix chips per month, and your facility can
   fabricate 70 wafers a month, how
   many wafers should you make of each chip?
6. \[20/20\] &lt;1.6&gt; Your colleague at AMD suggests that, since the
   yield is so poor, you might make chips more cheaply if you released
   multiple versions of the same
   chip, just with different numbers of cores. For example, you could sell Phoenix<sup>8</sup>, Phoenix<sup>4</sup>, Phoenix<sup>2</sup>, and Phoenix<sup>1</sup>, which contain 8, 4, 2, and 1 cores on each chip, respectively. If all eight cores are defect-free, then it is sold as Phoenix<sup>8</sup>. Chips with four to seven defect-free cores are sold as Phoenix<sup>4</sup>, and those with two or three defect-free cores are sold as Phoenix<sup>2</sup>. For simplification, calculate the yield for a single core as the yield for a chip that is 1/8 the area of the original Phoenix chip. Then view that yield as an independent probability of a single core being defect free. Calculate the yield for each configuration as the probability of at the corre- sponding number of cores being defect free.
7. \[20\] &lt;1.6&gt; What is the yield for a single core being defect free as well as the yield for Phoenix<sup>4</sup>, Phoenix<sup>2</sup> and Phoenix<sup>1</sup>?
8. \[5\] &lt;1.6&gt; Using your results from part a, determine which chips you think it would be worthwhile to package and sell, and why.
9. \[10\] &lt;1.6&gt; If it previously cost $20 dollars per chip to
   produce Phoenix<sup>8</sup>, what will be the cost of the new
   Phoenix chips, assuming that there are no additional
   costs associated with rescuing them from the trash?
10. \[20\] &lt;1.6&gt; You currently make a profit of $30 for each defect-free Phoenix<sup>8</sup>, and you will sell each Phoenix<sup>4</sup> chip for $25. How much is your profit per Phoenix<sup>8</sup>
    chip if you consider (i) the purchase price of Phoenix<sup>4</sup> chips to be entirely profit and (ii) apply the profit of Phoenix<sup>4</sup> chips to each Phoenix<sup>8</sup> chip in proportion to how many are produced? Use the yields calculated from part Problem 1.3a, not from problem 1.1a.

### Case Study 2: Power Consumption in Computer Systems

##### *Concepts illustrated by this case study*

- Amdahl’s Law
- Redundancy
- MTTF
- Power Consumption
  Power consumption in modern systems is dependent on a variety of factors, includ- ing the chip clock frequency, efficiency, and voltage. The following exercises explore the impact on power and energy that different design decisions and use scenarios have.

1. \[10/10/10/10\] &lt;1.5&gt; A cell phone performs very different tasks, including stream- ing music, streaming video, and reading email. These tasks perform very different
   computing tasks. Battery life and overheating are two common problems for cell phones, so reducing power and energy consumption are critical. In this problem, we consider what to do when the user is not using the phone to its full computing capacity. For these problems, we will evaluate an unrealistic scenario in which the cell phone has no specialized processing units. Instead, it has a quad-core, general- purpose processing unit. Each core uses 0.5 W at full use. For email-related tasks,

the quad-core is 8× as fast as necessary.

1. \[10\] &lt;1.5&gt; How much dynamic energy and power are required compared to running at full power? First, suppose that the quad-core operates for 1/8 of the time and is idle for the rest of the time. That is, the clock is disabled for
   7/8 of the time, with no leakage occurring during that time. Compare total dynamic energy as well as dynamic power while the core is running.
2. \[10\] &lt;1.5&gt; How much dynamic energy and power are required using fre- quency and voltage scaling? Assume frequency and voltage are both reduced
   to 1/8 the entire time.
3. \[10\] &lt;1.6, 1.9&gt; Now assume the voltage may not decrease below 50% of the original voltage. This voltage is referred to as the *voltage floor*, and any voltage
   lower than that will lose the state. Therefore, while the frequency can keep decreasing, the voltage cannot. What are the dynamic energy and power savings in this case?
4. \[10\] &lt;1.5&gt; How much energy is used with a dark silicon approach? This involves creating specialized ASIC hardware for each major task and power
   gating those elements when not in use. Only one general-purpose core would be provided, and the rest of the chip would be filled with specialized units. For email, the one core would operate for 25% the time and be turned completely off with power gating for the other 75% of the time. During the other 75% of the time, a specialized ASIC unit that requires 20% of the energy of a core would be running.
5. \[10/10/10\] &lt;1.5&gt; As mentioned in Exercise 1.4, cell phones
   run a wide variety of applications. We’ll make the same assumptions
   for this exercise as the previous one, that it is 0.5 W per core and
   that a quad core runs email 3× as fast.

   1. \[10\] &lt;1.5&gt; Imagine that 80% of the code is
      > parallelizable. By how much would the frequency and voltage on
      > a single core need to be increased in order to exe-
      > cute at the same speed as the four-way parallelized code?
      >
6. \[10\] &lt;1.5&gt; What is the reduction in dynamic energy from using frequency and voltage scaling in part a?
7. \[10\] &lt;1.5&gt; How much energy is used with a dark silicon
   approach? In this approach, all hardware units are power gated,
   allowing them to turn off entirely
   (causing no leakage). Specialized ASICs are provided that perform the same computation for 20% of the power as the general-purpose processor. Imagine that each core is power gated. The video game requires two ASICS and two cores. How much dynamic energy does it require compared to the baseline of parallelized on four cores?
8. \[10/10/10/10/10/20\] &lt;1.5,1.9&gt; General-purpose processes are
   optimized for general-purpose computing. That is, they are optimized
   for behavior that is gener-
   ally found across a large number of applications. However, once the domain is restricted somewhat, the behavior that is found across a large number of the target applications may be different from general-purpose applications. One such appli- cation is deep learning or neural networks. Deep learning can be applied to many different applications, but the fundamental building block of inference—using the learned information to make decisions—is the same across them all. Inference operations are largely parallel, so they are currently performed on graphics proces- sing units, which are specialized more toward this type of computation, and not to inference in particular. In a quest for more performance per watt, Google has cre- ated a custom chip using tensor processing units to accelerate inference operations in deep learning.[<sup>1</sup>](#_bookmark42) This approach can be used for speech recognition and image recognition, for example. This problem explores the trade-offs between this pro- cess, a general-purpose processor (Haswell E5-2699 v3) and a GPU (NVIDIA K80), in terms of performance and cooling*.* If heat is not removed from the com- puter efficiently, the fans will blow hot air back onto the computer, not cold air. Note: The differences are more than processor—on-chip memory and DRAM also come into play. Therefore statistics are at a system level, not a chip level.

<span id="_bookmark42" class="anchor"></span><sup>1</sup>Cite paper at this website: [https://drive.google.com/file/d/0Bx4hafXDDq2EMzRNcy1vSUxtcEk/view](https://drive.google.com/file/d/0Bx4hafXDDq2EMzRNcy1vSUxtcEk/view).

1. \[10\] &lt;1.9&gt; If Google’s data center spends 70% of its time on workload A and 30% of its time on workload B when running GPUs, what is the speedup of the
   TPU system over the GPU system?
2. \[10\] &lt;1.9&gt; If Google’s data center spends 70% of its time on workload A and 30% of its time on workload B when running GPUs, what percentage of Max
   IPS does it achieve for each of the three systems?
3. \[15\] &lt;1.5, 1.9&gt; Building on (b), assuming that the power scales linearly from idle to busy power as IPS grows from 0% to 100%, what is the performance per
   watt of the TPU system over the GPU system?
4. \[10\] &lt;1.9&gt; If another data center spends 40% of its time on workload A, 10% of its time on workload B, and 50% of its time on workload C, what are the
   speedups of the GPU and TPU systems over the general-purpose system?
5. \[10\] &lt;1.5&gt; A cooling door for a rack costs $4000 and dissipates 14 kW (into the room; additional cost is required to get it out of the room). How many
   Haswell-, NVIDIA-, or Tensor-based servers can you cool with one cooling door, assuming TDP in [Figures 1.27](#_bookmark43) and [1.28](#_bookmark44)?
6. \[20\] &lt;1.5&gt; Typical server farms can dissipate a maximum of 200 W per square foot. Given that a server rack requires 11 square feet (including front and back
   clearance), how many servers from part (e) can be placed on a single rack, and how many cooling doors are required?

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 27%" />
<col style="width: 16%" />
<col style="width: 19%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th><span id="_bookmark43" class="anchor"></span>System</th>
<th><blockquote>
<p>Chip</p>
</blockquote></th>
<th><blockquote>
<p>TDP</p>
</blockquote></th>
<th><blockquote>
<p>Idle power</p>
</blockquote></th>
<th><blockquote>
<p>Busy power</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>General-purpose</td>
<td><blockquote>
<p>Haswell E5-2699 v3</p>
</blockquote></td>
<td><blockquote>
<p>504 W</p>
</blockquote></td>
<td><blockquote>
<p>159 W</p>
</blockquote></td>
<td><blockquote>
<p>455 W</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Graphics processor</td>
<td><blockquote>
<p>NVIDIA K80</p>
</blockquote></td>
<td><blockquote>
<p>1838 W</p>
</blockquote></td>
<td><blockquote>
<p>357 W</p>
</blockquote></td>
<td><blockquote>
<p>991 W</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>Custom ASIC</td>
<td><blockquote>
<p>TPU</p>
</blockquote></td>
<td><blockquote>
<p>861 W</p>
</blockquote></td>
<td><blockquote>
<p>290 W</p>
</blockquote></td>
<td><blockquote>
<p>384 W</p>
</blockquote></td>
</tr>
</tbody>
</table>
 Figure 1.27 Hardware characteristics for general-purpose processor, graphical processing unit-based or custom ASIC-based system, including measured power (cite ISCA paper).

<span id="_bookmark44" class="anchor"></span>Throughput % Max IPS

<table style="width:100%;">
<colgroup>
<col style="width: 18%" />
<col style="width: 24%" />
<col style="width: 9%" />
<col style="width: 11%" />
<col style="width: 10%" />
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 6%" />
</colgroup>
<thead>
<tr class="header">
<th>System</th>
<th><blockquote>
<p>Chip</p>
</blockquote></th>
<th>A</th>
<th>B</th>
<th>C</th>
<th>A</th>
<th>B</th>
<th><blockquote>
<p>C</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>General-purpose</td>
<td><blockquote>
<p>Haswell E5-2699 v3</p>
</blockquote></td>
<td>5482</td>
<td><blockquote>
<p>13,194</p>
</blockquote></td>
<td>12,000</td>
<td>42%</td>
<td><blockquote>
<p>100%</p>
</blockquote></td>
<td><blockquote>
<p>90%</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Graphics processor</td>
<td><blockquote>
<p>NVIDIA K80</p>
</blockquote></td>
<td>13,461</td>
<td><blockquote>
<p>36,465</p>
</blockquote></td>
<td>15,000</td>
<td>37%</td>
<td><blockquote>
<p>100%</p>
</blockquote></td>
<td><blockquote>
<p>40%</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>Custom ASIC</td>
<td><blockquote>
<p>TPU</p>
</blockquote></td>
<td>225,000</td>
<td><blockquote>
<p>280,000</p>
</blockquote></td>
<td>2000</td>
<td>80%</td>
<td><blockquote>
<p>100%</p>
</blockquote></td>
<td><blockquote>
<p>1%</p>
</blockquote></td>
</tr>
</tbody>
</table>
 Figure 1.28 Performance characteristics for general-purpose processor, graphical processing unit-based or custom ASIC-based system on two neural-net workloads (cite ISCA paper). Workloads A and B are from published results. Workload C is a fictional, more general-purpose application.

### Exercises

1. \[10/15/15/10/10\] &lt;1.4, 1.5&gt; One challenge for architects is
   that the design created today will require several years of
   implementation, verification, and testing before
   appearing on the market. This means that the architect must project what the tech- nology will be like several years in advance. Sometimes, this is difficult to do.
2. \[10\] &lt;1.4&gt; According to the trend in device scaling historically observed by Moore’s Law, the number of transistors on a chip in 2025 should be how many
   times the number in 2015?
3. \[15\] &lt;1.5&gt; The increase in performance once mirrored this trend. Had perfor- mance continued to climb at the same rate as in the 1990s, approximately what
   performance would chips have over the VAX-11/780 in 2025?
4. \[15\] &lt;1.5&gt; At the current rate of increase of the mid-2000s,
   what is a more updated projection of performance in 2025?
5. \[10\] &lt;1.4&gt; What has limited the rate of growth of the clock rate, and what are architects doing with the extra transistors now to increase performance?
6. \[10\] &lt;1.4&gt; The rate of growth for DRAM capacity has also
   slowed down. For 20 years, DRAM capacity improved by 60% each year.
   If 8 Gbit DRAM was
   first available in 2015, and 16 Gbit is not available until 2019, what is the cur- rent DRAM growth rate?
7. \[10/10\] &lt;1.5&gt; You are designing a system for a real-time
   application in which specific deadlines must be met. Finishing the
   computation faster gains nothing.
   You find that your system can execute the necessary code, in the worst case, twice as fast as necessary.
8. \[10\] &lt;1.5&gt; How much energy do you save if you execute at the current speed and turn off the system when the computation is complete?
9. \[10\] &lt;1.5&gt; How much energy do you save if you set the voltage and frequency to be half as much?

<!-- -->

1. \[10/10/20/20\] &lt;1.5&gt; Server farms such as Google and Yahoo!
   provide enough compute capacity for the highest request rate of the
   day. Imagine that most of
   the time these servers operate at only 60% capacity. Assume further that the power does not scale linearly with the load; that is, when the servers are operating at 60% capacity, they consume 90% of maximum power. The servers could be turned off, but they would take too long to restart in response to more load. A new system has been proposed that allows for a quick restart but requires 20% of the maximum power while in this “barely alive” state.
2. \[10\] &lt;1.5&gt; How much power savings would be achieved by turning off 60% of the servers?
3. \[10\] &lt;1.5&gt; How much power savings would be achieved by placing 60% of the servers in the “barely alive” state?
4. \[20\] &lt;1.5&gt; How much power savings would be achieved by reducing the volt- age by 20% and frequency by 40%?
5. \[20\] &lt;1.5&gt; How much power savings would be achieved by placing 30% of the servers in the “barely alive” state and 30% off?

<!-- -->

1. \[10/10/20\] &lt;1.7&gt; Availability is the most important consideration for designing servers, followed closely by scalability and throughput.

   1. \[10\] &lt;1.7&gt; We have a single processor with a failure in

      > time (FIT) of 100. What is the mean time to failure (MTTF) for
      > this system?
      >
   2. \[10\] &lt;1.7&gt; If it takes one day to get the system running

      > again, what is the avail- ability of the system?
      >
   3. \[20\] &lt;1.7&gt; Imagine that the government, to cut costs, is

      > going to build a super- computer out of inexpensive computers
      > rather than expensive, reliable com-
      > puters. What is the MTTF for a system with 1000 processors? Assume that if one fails, they all fail.
      >
2. \[20/20/20\] &lt;1.1, 1.2, 1.7&gt; In a server farm such as that used by Amazon or eBay, a single failure does not cause the entire system to crash. Instead, it will reduce the
   number of requests that can be satisfied at any one time.
3. \[20\] &lt;1.7&gt; If a company has 10,000 computers, each with an MTTF of 35 days, and it experiences catastrophic failure only if 1/3 of the computers fail, what is
   the MTTF for the system?
4. \[20\] &lt;1.1, 1.7&gt; If it costs an extra $1000, per computer, to double the MTTF, would this be a good business decision? Show your work.
5. \[20\] &lt;1.2&gt; [Figure 1.3](#_bookmark8) shows, on average, the cost of downtimes, assuming that the cost is equal at all times of the year. For retailers, however, the Christmas
   season is the most profitable (and therefore the most costly time to lose sales). If a catalog sales center has twice as much traffic in the fourth quarter as every other quarter, what is the average cost of downtime per hour during the fourth quarter and the rest of the year?
6. \[20/10/10/10/15\] &lt;1.9&gt; In this exercise, assume that we are considering enhanc- ing a quad-core machine by adding encryption hardware to it. When computing
   encryption operations, it is 20 times faster than the normal mode of execution. We will define percentage of encryption as the percentage of time in the original execution that is spent performing encryption operations. The specialized hard- ware increases power consumption by 2%.
7. \[20\] &lt;1.9&gt; Draw a graph that plots the speedup as a percentage of the compu- tation spent performing encryption. Label the y-axis “Net speedup” and label
   the x-axis “Percent encryption.”
8. \[10\] &lt;1.9&gt; With what percentage of encryption will adding encryption hard- ware result in a speedup of 2?
9. \[10\] &lt;1.9&gt; What percentage of time in the new execution will be spent on encryption operations if a speedup of 2 is achieved?
10. \[15\] &lt;1.9&gt; Suppose you have measured the percentage of encryption to be 50%. The hardware design group estimates it can speed up the encryption hard-
    ware even more with significant additional investment. You wonder whether adding a second unit in order to support parallel encryption operations would be more useful. Imagine that in the original program, 90% of the encryption operations could be performed in parallel. What is the speedup of providing two or four encryption units, assuming that the parallelization allowed is limited to the number of encryption units?
11. \[15/10\] &lt;1.9&gt; Assume that we make an enhancement to a
    computer that improves some mode of execution by a factor of 10.
    Enhanced mode is used 50% of the time,
    measured as a percentage of the execution time *when the enhanced mode is in use*. Recall that Amdahl’s Law depends on the fraction of the original, unenhanced exe- cution time that could make use of enhanced mode. Thus we cannot directly use this 50% measurement to compute speedup with Amdahl’s Law.
12. \[15\] &lt;1.9&gt; What is the speedup we have obtained from fast
    mode?
13. \[10\] &lt;1.9&gt; What percentage of the original execution time has been converted to fast mode?

<!-- -->

1. \[20/20/15\] &lt;1.9&gt; When making changes to optimize part of a
   processor, it is often the case that speeding up one type of
   instruction comes at the cost of slowing down
   something else. For example, if we put in a complicated fast floating-point unit, that takes space, and something might have to be moved farther away from the middle to accommodate it, adding an extra cycle in delay to reach that unit. The basic Amdahl’s Law equation does not take into account this trade-off.
2. \[20\] &lt;1.9&gt; If the new fast floating-point unit speeds up floating-point opera- tions by, on average, 2x, and floating-point operations take 20% of the original
   program’s execution time, what is the overall speedup (ignoring the penalty to any other instructions)?
3. \[20\] &lt;1.9&gt; Now assume that speeding up the floating-point unit slowed down data cache accesses, resulting in a 1.5x slowdown (or 2/3 speedup). Data cache
   accesses consume 10% of the execution time. What is the overall speedup now?
4. \[15\] &lt;1.9&gt; After implementing the new floating-point
   operations, what per- centage of execution time is spent on
   floating-point operations? What percent-
   age is spent on data cache accesses?
5. \[10/10/20/20\] &lt;1.10&gt; Your company has just bought a new
   22-core processor, and you have been tasked with optimizing your
   software for this processor.
   You will run four applications on this system, but the resource requirements are not equal. Assume the system and application characteristics listed in [Table 1.1](#_bookmark45).

<table>
<colgroup>
<col style="width: 36%" />
<col style="width: 13%" />
<col style="width: 19%" />
<col style="width: 19%" />
<col style="width: 11%" />
</colgroup>
<thead>
<tr class="header">
<th><span id="_bookmark45" class="anchor"></span>Table 1.1 Four
applications</th>
<th colspan="4"></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Application</td>
<td><blockquote>
<p>A</p>
</blockquote></td>
<td><blockquote>
<p>B</p>
</blockquote></td>
<td>C</td>
<td>D</td>
</tr>
<tr class="even">
<td>% resources needed</td>
<td><blockquote>
<p>41</p>
</blockquote></td>
<td><blockquote>
<p>27</p>
</blockquote></td>
<td><blockquote>
<p>18</p>
</blockquote></td>
<td>14</td>
</tr>
<tr class="odd">
<td>% parallelizable</td>
<td><blockquote>
<p>50</p>
</blockquote></td>
<td><blockquote>
<p>80</p>
</blockquote></td>
<td><blockquote>
<p>60</p>
</blockquote></td>
<td>90</td>
</tr>
</tbody>
</table>
 The percentage of resources of assuming they are all run in serial. Assume that when you parallelize a portion of the program by X, the speedup for that portion is X.

1. \[10\] &lt;1.10&gt; How much speedup would result from running application A on the entire 22-core processor, as compared to running it serially?
2. \[10\] &lt;1.10&gt; How much speedup would result from running application D on the entire 22-core processor, as compared to running it serially?
3. \[20\] &lt;1.10&gt; Given that application A requires 41% of the resources, if we stat- ically assign it 41% of the cores, what is the overall speedup if A is run paral-
   lelized but everything else is run serially?
4. \[20\] &lt;1.10&gt; What is the overall speedup if all four applications are statically assigned some of the cores, relative to their percentage of resource needs, and
   all run parallelized?
5. \[10\] &lt;1.10&gt; Given acceleration through parallelization, what new percentage of the resources are the applications receiving, considering only active time on
   their statically-assigned cores?
6. \[10/20/20/20/25\] &lt;1.10&gt; When parallelizing an application, the ideal speedup is speeding up by the number of processors. This is limited by two things: percentage
   of the application that can be parallelized and the cost of communication. Amdahl’s Law takes into account the former but not the latter.
7. \[10\] &lt;1.10&gt; What is the speedup with *N* processors if 80% of the application is parallelizable, ignoring the cost of communication?
8. \[20\] &lt;1.10&gt; What is the speedup with eight processors if, for every processor added, the communication overhead is 0.5% of the original execution time.
9. \[20\] &lt;1.10&gt; What is the speedup with eight processors if, for every time the number of processors is doubled, the communication overhead is increased
   by 0.5% of the original execution time?
10. \[20\] &lt;1.10&gt; What is the speedup with *N* processors if, for every time the num- ber of processors is doubled, the communication overhead is increased by 0.5%
    of the original execution time?
11. \[25\] &lt;1.10&gt; Write the general equation that solves this question: What is the number of processors with the highest speedup in an application in which P% of
    the original execution time is parallelizable, and, for every time the number of processors is doubled, the communication is increased by 0.5% of the original execution time?
