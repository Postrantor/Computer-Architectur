---
data: 2023-01-15 11:25
---
<img src="../media/image1.jpeg" style="width:7.5in;height:4.79in" />
![](../media/image2.png)

<img src="../media/image4.png" style="width:0.465in;height:0.475in" />

## **Computer Architecture Formulas**

<img src="../media/image5.png" style="width:0.12014in;height:0.14982in" />
<img src="../media/image6.png" style="width:0.12015in;height:0.14982in" />

### Sixth Edition

“Although important concepts of architecture are timeless, this edition has been thoroughly updated with the latest technology developments, costs, examples, and references. Keeping pace with recent developments in open-sourced architecture, the instruction set architecture used in the book has been updated to use the RISC-V ISA.”

> “尽管重要的建筑概念是永恒的，但该版本已通过最新的技术发展，成本，示例和参考进行了彻底的更新。与最新的开源架构相关的发展，本书中使用的指令集体系结构已更新以使用 RISC-V ISA。”

—from the foreword by Norman P. Jouppi, Google

“_Computer Architecture: A Quantitative Approach_ is a classic that, like fine wine, just keeps getting better. I bought my first copy as I finished up my undergraduate degree and it remains one of my most frequently referenced texts today.”

> “ *计算机体系结构：定量方法*是一种经典，就像优质的葡萄酒一样，它一直在变得更好。完成本科学位时，我购买了第一本，这仍然是我今天最常引用的文本之一。”

—James Hamilton, Amazon Web Service

“Hennessy and Patterson wrote the first edition of this book when graduate students built computers with 50,000 transistors. Today, warehouse-size computers contain that many servers, each consisting of dozens of independent processors and billions of transistors. The evolution of computer architecture has been rapid and relentless, but _Computer Architecture: A Quantitative Approach_ has kept pace, with each edition accurately explaining and analyzing the important emerging ideas that make this field so exciting.”

> “轩尼诗和帕特森(Hennessy)和帕特森(Patterson)写了本书的第一版，当时研究生的学生建造了具有 50,000 个晶体管的计算机。如今，仓库大小的计算机包含许多服务器，每个服务器由数十个独立的处理器和数十亿个晶体管组成。计算机体系结构的演变既快速又无情，但是\_计算机体系结构：一种定量方法一直保持步伐，每个版本都准确地解释和分析了使该领域如此令人兴奋的重要新兴想法。”

—James Larus, Microsoft Research

“Another timely and relevant update to a classic, once again also serving as a window into the relentless and exciting evolution of computer architecture! The new discussions in this edition on the slowing of Moore's law and implications for future systems are must-reads for both computer architects and practitioners working on broader systems.”

> “对经典的另一个及时及相关的更新，再次成为计算机架构的无情而令人兴奋的演变的风向！关于摩尔定律放缓的新讨论以及对未来系统的影响是计算机架构师和从业者从事更广泛系统的必读内容。”

—Parthasarathy (Partha) Ranganathan, Google

“I love the ‘Quantitative Approach’ books because they are written by engineers, for engineers. John Hennessy and Dave Patterson show the limits imposed by mathematics and the possibilities enabled by materials science. Then they teach through real-world examples how architects analyze, measure, and compromise to build working systems. This sixth edition comes at a critical time: Moore’s Law is fading just as deep learning demands unprecedented compute cycles. The new chapter on domain-specific architectures documents a number of promising approaches and prophesies a rebirth in computer architecture. Like the scholars of the European Renaissance, computer architects must understand our own history, and then combine the lessons of that history with new techniques to remake the world.”

> “我喜欢'定量方法'书籍，因为它们是由工程师编写的工程师。约翰·轩尼诗(John Hennessy)和戴夫·帕特森(Dave Patterson)展示了数学和材料科学实现的可能性的限制。然后，他们通过现实世界中的示例来教授工程师如何分析，衡量和妥协以建立工作系统。第六版在一个关键时刻出现：摩尔的定律正在逐渐消失，就像深度学习的要求是前所未有的计算周期一样。有关领域特定体系结构的新章节记录了许多宣传方法和预言计算机体系结构的重生。就像欧洲文艺复兴时期的学者一样，计算机工程师必须了解我们自己的历史，然后将历史的教训与新技术相结合，以重塑世界。”

—Cliff Young, Google

John L. Hennessy is a Professor of Electrical Engineering and Computer Science at Stanford University, where he has been a member of the faculty since 1977 and was, from 2000 to 2016, its 10th President. He currently serves as the Director of the Knight-Hennessy Fellowship, which provides graduate fellowships to potential future leaders. Hennessy is a Fellow of the IEEE and ACM, a member of the National Academy of Engineering, the National Academy of Science, and the American Philosophical Society, and a Fellow of the American Academy of Arts and Sciences. Among his many awards are the 2001 Eckert-Mauchly Award for his contributions to RISC technology, the 2001 Seymour Cray Computer Engineering Award, and the 2000 John von Neumann Award, which he shared with David Patterson. He has also received 10 honorary doctorates.

> 约翰·轩尼诗(John L. Hennessy)是斯坦福大学电气工程和计算机科学教授，自 1977 年以来，他一直是该教师的成员，从 2000 年到 2016 年是其第十任校长。他目前是骑士汉尼斯(Knight-Hennessy)同胞的董事，该公司为潜在的未来领导者提供研究生奖学金。轩尼诗(Hennessy)是 IEEE 和 ACM 的院士，是美国国家工程学院，国家科学学院和美国哲学学会的成员，也是美国艺术与科学学院的研究员。在他的许多奖项中，他的奖项是 2001 年 Eckert-Mauchly 奖，他的 RISC 技术，2001 年 Seymour Cray 计算机工程奖和 2000 年 John von Neumann 奖，他与 David Patterson 分享了 2000 年。他还获得了 10 个荣誉博士学位。

In 1981, he started the MIPS project at Stanford with a handful of graduate students. After completing the project in 1984, he took a leave from the university to cofound MIPS Computer Systems, which developed one of the first commercial RISC microprocessors. As of 2017, over 5 billion MIPS microprocessors have been shipped in devices ranging from video games and palmtop computers to laser printers and network switches. Hennessy subsequently led the DASH (Director Architecture for Shared Memory) project, which prototyped the first scalable cache coherent multiprocessor; many of the key ideas have been adopted in modern multiprocessors. In addition to his technical activities and university responsibilities, he has continued to work with numerous start-ups, both as an early-stage advisor and an investor.

> 1981 年，他与少数研究生一起在斯坦福大学开始了 MIPS 项目。在 1984 年完成该项目后，他从大学请假前往 Cofound Mips-Compoter Systems，该系统开发了最早的商业 RISC 微处理器之一。截至 2017 年，已经在设备中运送了超过 50 亿个 MIPS 微处理器，从视频游戏和掌上计算机到激光打印机和网络交换机。轩尼诗(Hennessy)统治了 DASH(共享内存的导演架构)项目，该项目原型制作了第一个可扩展的高速缓存相干多处理器；现代多处理器已经采用了许多关键思想。除了他的技术活动和大学责任外，他还继续与许多初创企业合作，无论是作为早期顾问还是投资者。

David A. Patterson became a Distinguished Engineer at Google in 2016 after 40 years as a UC Berkeley professor. He joined UC Berkeley immediately after graduating from UCLA. He still spends a day a week in Berkeley as an Emeritus Professor of Computer Science. His teaching has been honored by the Distinguished Teaching Award from the University of California, the Karlstrom Award from ACM, and the Mulligan Education Medal and Undergraduate Teaching Award from IEEE. Patterson received the IEEE Technical Achievement Award and the ACM Eckert-Mauchly Award for contributions to RISC, and he shared the IEEE Johnson Information Storage Award for contributions to RAID. He also shared the IEEE John von Neumann Medal and the C & C Prize with John Hennessy. Like his co-author, Patterson is a Fellow of the American Academy of Arts and Sciences, the Computer History Museum, ACM, and IEEE, and he was elected to the National Academy of Engineering, the National Academy of Sciences, and the Silicon Valley Engineering Hall of Fame. He served on the Information Technology Advisory Committee to the President of the United States, as chair of the CS division in the Berkeley EECS department, as chair of the Computing Research Association, and as President of ACM. This record led to Distinguished Service Awards from ACM, CRA, and SIGARCH. He is currently Vice-Chair of the Board of Directors of the RISC-V Foundation.

> 大卫·帕特森(David A. Patterson)担任加州大学伯克利分校(UC Berkeley)教授 40 年后，于 2016 年成为 Google 的杰出工程师。从 UCLA 毕业后，他立即加入了加州大学伯克利分校。他仍然每周在伯克利度过一天，担任计算机科学名誉教授。加利福尼亚大学的杰出教学奖，ACM 的 Karlstrom 奖以及 IEEE 的 Mulligan 教育奖章和少年研究生教学奖，他的教学得到了尊敬。帕特森(Patterson)因对 RISC 的贡献而获得了 IEEE 技术成就奖和 ACM Eckert-Mauchly 奖，并分享了 IEEE Johnson 信息存储奖，以贡献 RAID。他还与约翰·轩尼诗(John Hennessy)分享了 IEEE John von Neumann 奖章和 C＆C 奖。工程名人堂。他曾担任伯克利 EEC 部门的 CS 部主席，担任计算机研究协会主席，并担任 ACM 主席。该记录导致了 ACM，CRA 和 Sigarch 的杰出服务奖。他目前是 RISC-V 基金会董事会副主席。

At Berkeley, Patterson led the design and implementation of RISC I, likely the first VLSI reduced instruction set computer, and the foundation of the commercial SPARC architecture. He was a leader of the Redundant Arrays of Inexpensive Disks (RAID) project, which led to dependable storage systems from many companies. He was also involved in the Network of Workstations (NOW) project, which led to cluster technology used by Internet companies and later to cloud computing. His current interests are in designing domain-specific architectures for machine learning, spreading the word on the open RISC-V instruction set architecture, and in helping the UC Berkeley RISELab (Real-time Intelligent Secure Execution).

> 在伯克利，帕特森(Patterson)领导了 RISC I 的设计和实施，这可能是第一个 VLSI 减少的指令集计算机以及商业 Sparc Architecture 的基础。他是廉价磁盘(RAID)项目的冗余阵列的负责人，这导致了许多公司的可靠存储系统。他还参与了工作站(现在)项目的网络，这导致了互联网公司使用的集群技术，后来导致了云计算。他目前的兴趣是设计用于机器学习的特定领域的档案，在开放的 RISC-V 指令设置档案中传播单词，并帮助 UC Berkeley Riselab(实时智能安全执行)。

![](../media/image7.png)

© 2019 Elsevier Inc. All rights reserved.

> ===

No part of this publication may be reproduced or transmitted in any form or by any means, electronic or mechanical, including photocopying, recording, or any information storage and retrieval system, without permission in writing from the publisher. Details on how to seek permission, further information about the Publisher’s permissions policies and our arrangements with organizations such as the Copyright Clearance Center and the Copyright Licensing Agency, can be found at our website: [www.elsevier.com/permissions](http://www.elsevier.com/permissions).

This book and the individual contributions contained in it are protected under copyright by the Publisher (other than as may be noted herein).

> ===

Much of the improvement in computer performance over the last 40 years has been provided by computer architecture advancements that have leveraged Moore’s Law and Dennard scaling to build larger and more parallel systems. Moore’s Law is the observation that the maximum number of transistors in an integrated circuit doubles approximately every two years. Dennard scaling refers to the reduction of MOS supply voltage in concert with the scaling of feature sizes, so that as transistors get smaller, their power density stays roughly constant. With the end of Dennard scaling a decade ago, and the recent slowdown of Moore’s Law due to a combination of physical limitations and economic factors, the sixth edition of the preeminent textbook for our field couldn’t be more timely. Here are some reasons. First, because domain-specific architectures can provide equivalent performance and power benefits of three or more historical generations of Moore’s Law and Dennard scaling, they now can provide better implementations than may ever be possible with future scaling of general-purpose architectures. And with the diverse application space of computers today, there are many potential areas for architectural innovation with domain-specific architectures. Second, high-quality implementations of open-source architectures now have a much longer lifetime due to the slowdown in Moore’s Law. This gives them more opportunities for continued optimization and refinement, and hence makes them more attractive. Third, with the slowing of Moore’s Law, different technology components have been scaling heterogeneously. Furthermore, new technologies such as 2.5D stacking, new nonvolatile memories, and optical interconnects have been developed to provide more than Moore’s Law can supply alone. To use these new technologies and nonhomogeneous scaling effectively, fundamental design decisions need to be reexamined from first principles. Hence it is important for students, professors, and practitioners in the industry to be skilled in a wide range of both old and new architectural techniques. All told, I believe this is the most exciting time in computer architecture since the industrial exploitation of instruction-level parallelism in microprocessors 25 years ago.

> 在过去 40 年中，计算机性能的大部分改进是由计算机架构的进步提供了，这些进步利用了摩尔定律和丹纳德缩放来构建更大，更平行的系统。摩尔定律的观察是，集成电路中最大的晶体管数量大约每两年加倍。Dennard 缩放是指与特征大小的缩放缩放的 MOS 电源电压减少，因此随着晶体管变小，它们的功率密度大致保持恒定。随着十年前丹纳德(Dennard)的结尾，由于身体局限性和经济因素的结合，摩尔定律最近的放缓，我们领域的杰出教科书的第六版不会更及时。这是一些原因。首先，由于特定于域的架构可以为摩尔法律和丹纳德缩放的三个或更多历史世代提供同等的性能和权力益处，因此它们现在可以提供更好的实现，而不是将来的通用体系结构缩放。随着当今计算机的不同应用空间，具有许多潜在的建筑创新领域，并具有特定于领域的体系结构。其次，由于摩尔法律的放缓，开源体系结构的高质量实施现在已经有了很多寿命。这为他们提供了更多的选择，以持续优化和精致，因此使它们更具吸引力。第三，随着摩尔定律的放缓，不同的技术组成部分一直在异质上扩展。此外，已经开发了诸如 2.5D 堆叠，新的非易失性记忆和光学互连之类的新技术，以提供超出摩尔法律可以提供的更多技术。要有效地使用这些新技术和非均匀缩放，需要从第一原则中重新回顾基本设计决策。因此，对于行业中的学生，教授和从业人员来说，在各种各样的新旧建筑技术方面都很重要。总而言之，我相信这是自 25 年前在微处理器中对教学级并行的工业开发以来的计算机架构中最激动人心的时期。

The largest change in this edition is the addition of a new chapter on domainspecific architectures. It’s long been known that customized domain-specific architectures can have higher performance, lower power, and require less silicon area than general-purpose processor implementations. However when general-purpose processors were increasing in single-threaded performance by 40% per year (see Fig. 1.11), the extra time to market required to develop a custom architecture vs. using a leading-edge standard microprocessor could cause the custom architecture to lose much of its advantage. In contrast, today single-core performance is improving very slowly, meaning that the benefits of custom architectures will not be made obsolete by general-purpose processors for a very long time, if ever. [Chapter 7](#_bookmark322) covers several domain-specific architectures. Deep neural networks have very high computation requirements but lower data precision requirements – this combination can benefit significantly from custom architectures. Two example architectures and implementations for deep neural networks are presented: one optimized for inference and a second optimized for training. Image processing is another example domain; it also has high computation demands and benefits from lower-precision data types. Furthermore, since it is often found in mobile devices, the power savings from custom architectures are also very valuable. Finally, by nature of their reprogrammability, FPGA-based accelerators can be used to implement a variety of different domain-specific architectures on a single device. They also can benefit more irregular applications that are frequently updated, like accelerating internet search.

> 此版本中最大的变化是增加了有关域特定体系结构的新章节。长期以来，众所周知，与通用处理器实施相比，定制域特异性档案的性能可以具有更高的性能，更低的功率且需要更少的硅面积。但是，当通用处理器的单线程性能每年增加 40％(见图 1.11)时，开发自定义体系结构所需的额外市场时间与使用前沿标准的微处理器可能会导致自定义体系结构失去大量优势。相比之下，今天的单核性能正在非常缓慢地提高，这意味着很长一段时间(如果有的话)将不会被通用处理器淘汰。[第 7 章](#_bookmark322)涵盖了几个特定领域的体系结构。深神经网络具有很高的计算要求，但数据精度要求较低 这种组合可以从自定义体系结构中受益匪浅。提出了两个针对深神经网络的示例架构和实现：一种针对推理进行了优化，第二个对培训进行了优化。图像处理是另一个示例域；它还具有较低精确数据类型的高计算需求和收益。此外，由于通常在移动设备中找到它，因此自定义体系结构节省的功率也非常有价值。最后，从其重编程性的性质上，基于 FPGA 的加速器可用于在单个设备上实现各种不同域特异性架构。它们还可以使经常更新的更不规则的应用程序受益，例如加速 Internet 搜索。

Although important concepts of architecture are timeless, this edition has been thoroughly updated with the latest technology developments, costs, examples, and references. Keeping pace with recent developments in open-sourced architecture, the instruction set architecture used in the book has been updated to use the RISC-V ISA.

> 尽管重要的建筑概念是永恒的，但该版本已通过最新的技术发展，成本，示例和参考进行了彻底的更新。与开源体系结构的最新发展保持同步，本书中使用的指令集体系结构已更新以使用 RISC-V ISA。

On a personal note, after enjoying the privilege of working with John as a graduate student, I am now enjoying the privilege of working with Dave at Google. What an amazing duo!

> 就个人而言，在享受了与约翰一起担任研究生的特权之后，我现在享受与 Dave 在 Google 合作的特权。多么了不起的二人组！

#### Chapter 1 Fundamentals of Quantitative Design and Analysis

> ####第 1 章定量设计和分析的基础知识

1. Introduction 2

> 1.简介 2

2. Classes of Computers 6

> 2.计算机类别 6

3. Defining Computer Architecture 11

> 3.定义计算机架构 11

4. Trends in Technology 18

> 4.技术趋势 18

5. Trends in Power and Energy in Integrated Circuits 23

> 5.集成电路中的功率和能源趋势 23

6. Trends in Cost 29

> 6.成本趋势 29

7. Dependability 36

> 7.可靠性 36

8. Measuring, Reporting, and Summarizing Performance 39

> 8.测量，报告和总结性能 39

9. Quantitative Principles of Computer Design 48

> 9.计算机设计的定量原理 48

10. Putting It All Together: Performance, Price, and Power 55

> 10.将所有内容汇总在一起：性能，价格和力量 55

11. Fallacies and Pitfalls 58

> 11.谬论和陷阱 58

12. Concluding Remarks 64

> 12.结论言论 64

13. Historical Perspectives and References 67

> 13.历史观点和参考文献 67

14. Case Studies and Exercises by Diana Franklin 67

> 14.戴安娜·富兰克林(Diana Franklin)的案例研究和练习 67

#### Chapter 2 Memory Hierarchy Design

> ####第 2 章内存层次结构设计

1. Instruction-Level Parallelism: Concepts and Challenges 168

> 1.指导级并行性：概念和挑战 168

2. Basic Compiler Techniques for Exposing ILP 176

> 2.暴露 ILP 176 的基本编译器技术

3. Reducing Branch Costs With Advanced Branch Prediction 182

> 3.通过高级分支预测降低分支机构成本 182

4. Overcoming Data Hazards With Dynamic Scheduling 191

> 4.通过动态调度克服数据危害 191

5. Dynamic Scheduling: Examples and the Algorithm 201

> 5.动态调度：示例和算法 201

6. Hardware-Based Speculation 208

> 6.基于硬件的推测 208

7. Exploiting ILP Using Multiple Issue and Static Scheduling 218

> 7.使用多个问题和静态调度 218 利用 ILP

8. Exploiting ILP Using Dynamic Scheduling, Multiple Issue, and

> 8.使用动态调度，多重问题和

9. Advanced Techniques for Instruction Delivery and Speculation 228

> 9.指导交付和猜测的高级技术 228

10. Cross-Cutting Issues 240

> 10.横切问题 240

11. Multithreading: Exploiting Thread-Level Parallelism to Improve Uniprocessor Throughput 242

> 11.多线程：利用线程级并行性以改善单层处理器吞吐量 242

12. Putting It All Together: The Intel Core i7 6700 and ARM Cortex-A53 247

> 12.将所有内容放在一起：Intel Core i7 6700 和 Arm Cortex-A53 247

13. Fallacies and Pitfalls 258

> 13.谬论和陷阱 258

14. Concluding Remarks: What’s Ahead? 264

> 14.总结说：未来会怎样？264

15. Historical Perspective and References 266

> 15.历史观点和参考文献 266

[Case Studies and Exercises by Jason D. Bakos and Robert P. Colwell 266](#case-studies-and-exercises-by-jason-d.-bakos-and-robert-p.-colwell)

> [Jason D. Bakos 和 Robert P. Colwell 的案例研究和练习 266](＃案例研究和锻炼 逐行 jason-d.-bakos-and-bakos-and-Robert-P.-Colwell)

#### Chapter 4 Data-Level Parallelism in Vector, SIMD, and GPU Architectures

> ####第 4 章向量，SIMD 和 GPU 体系结构中的数据级并行性

1. Introduction 282

> 1.简介 282

2. Vector Architecture 283

> 2.矢量体系结构 283

3. SIMD Instruction Set Extensions for Multimedia 304

> 3.多媒体 304 的 SIMD 指令集扩展

4. Graphics Processing Units 310

> 4.图形处理单元 310

5. Detecting and Enhancing Loop-Level Parallelism 336

> 5.检测和增强循环级并行性 336

6. Cross-Cutting Issues 345

> 6.横切问题 345

7. Putting It All Together: Embedded Versus Server GPUs and Tesla Versus Core i7 346

> 7.将所有内容放在一起：嵌入式服务器 GPU 和 Tesla 与 Core i7 346

8. Fallacies and Pitfalls 353

> 8.谬论和陷阱 353

9. Concluding Remarks 357

> 9.总结说明 357

10. Historical Perspective and References 357

> 10.历史观点和参考文献 357

[Case Study and Exercises by Jason D. Bakos 357](#case-study-and-exercises-by-jason-d.-bakos)

> [Jason D. Bakos 357 的案例研究和练习]

#### Chapter 5 Thread-Level Parallelism

> ####第 5 章线程级并联

1. Introduction 368

> 1.简介 368

2. Centralized Shared-Memory Architectures 377

> 2.集中式共享内存架构 377

3. Performance of Symmetric Shared-Memory Multiprocessors 393

> 3.对称共享内存多处理器的性能 393

4. Distributed Shared-Memory and Directory-Based Coherence 404

> 4.分布式共享内存和基于目录的连贯性 404

5. Synchronization: The Basics 412

> 5.同步：基础 412

6. Models of Memory Consistency: An Introduction 417

> 6.内存一致性的模型：简介 417

> [!note]：内存一致性
> 这个内容可以加注一下！

7. Cross-Cutting Issues 422

> 7.横切问题 422

8. Putting It All Together: Multicore Processors and Their Performance 426

> 8.将所有内容放在一起：多核处理器及其性能 426

9. Fallacies and Pitfalls 438

> 9.谬论和陷阱 438

10. The Future of Multicore Scaling 442

> 10.多机缩放的未来 442

11. Concluding Remarks 444

> 11.总结 444

12. Historical Perspectives and References 445

> 12.历史观点和参考文献 445

[Case Studies and Exercises by Amr Zaky and David A. Wood 446](#case-studies-and-exercises-by-amr-zaky-and-david-a.-wood)

> [Amr Zaky 和 David A. Wood 的案例研究和练习 446](＃案例研究和锻炼，逐态 amr-zaky-zaky-and-david-a.-wood)

#### Chapter 6 Warehouse-Scale Computers to Exploit Request-Level and Data-Level Parallelism

> ####第 6 章仓库规模计算机利用请求级和数据级并行性

1. Introduction 466

> 1.简介 466

2. Programming Models and Workloads for Warehouse-Scale Computers 471

> 2.仓库规模计算机的编程模型和工作负载 471

3. Computer Architecture of Warehouse-Scale Computers 477

> 3.仓库规模计算机的计算机架构 477

4. The Efficiency and Cost of Warehouse-Scale Computers 482

> 4.仓库规模计算机的效率和成本 482

5. Cloud Computing: The Return of Utility Computing 490

> 5.云计算：实用程序计算的返回 490

6. Cross-Cutting Issues 501

> 6.横切问题 501

7. Putting It All Together: A Google Warehouse-Scale Computer 503

> 7.将所有内容放在一起：Google 仓库规模计算机 503

8. Fallacies and Pitfalls 514

> 8.谬论和陷阱 514

9. Concluding Remarks 518

> 9.总结说明 518

10. Historical Perspectives and References 519

> 10.历史观点和参考文献 519

Chapter 7 Domain-Specific Architectures

> 第 7 章特定领域的体系结构

1. Introduction 540

> 1.简介 540

2. Guidelines for DSAs 543

> 2. DSAS 543 指南

3. Example Domain: Deep Neural Networks 544

> 3.示例域：深神经网络 544

4. Google’s Tensor Processing Unit, an Inference Data Center Accelerator 557

> 4. Google 的张量处理单元，推理数据中心加速器 557

5. Microsoft Catapult, a Flexible Data Center Accelerator 567

> 5. Microsoft 弹射器，灵活的数据中心加速器 567

6. Intel Crest, a Data Center Accelerator for Training 579

> 6.英特尔·克雷斯特(Intel Crest)，一个用于培训的数据中心加速器 579

7. Pixel Visual Core, a Personal Mobile Device Image Processing Unit 579

> 7.像素视觉核心，个人移动设备图像处理单元 579

8. Cross-Cutting Issues 592

> 8.横切问题 592

9. Putting It All Together: CPUs Versus GPUs Versus DNN Accelerators 595

> 9.将所有内容放在一起：CPU 与 GPU 与 DNN 加速器 595

10. Fallacies and Pitfalls 602

> 10.谬论和陷阱 602

11. Concluding Remarks 604

> 11.总结言论 604

12. Historical Perspectives and References 606

> 12.历史观点和参考文献 606

### Why We Wrote This Book

> ###为什么我们写这本书

Through six editions of this book, our goal has been to describe the basic principles underlying what will be tomorrow’s technological developments. Our excitement about the opportunities in computer architecture has not abated, and we echo what we said about the field in the first edition: “It is not a dreary science of paper machines that will never work. No! It’s a discipline of keen intellectual interest, requiring the balance of marketplace forces to cost-performance-power, leading to glorious failures and some notable successes.”

> 通过本书的六个版本，我们的目标是描述明天的技术发展的基本原则。我们对计算机架构机遇的兴奋并没有减轻，我们回应了第一版中对该领域的看法：“这不是纸机的沉闷科学，永远不会起作用。不！这是一门敏锐的智力兴趣的学科，要求市场力量平衡成本绩效，从而导致光荣的失败和一些显着的成功。”

Our primary objective in writing our first book was to change the way people learn and think about computer architecture. We feel this goal is still valid and important. The field is changing daily and must be studied with real examples and measurements on real computers, rather than simply as a collection of definitions and designs that will never need to be realized. We offer an enthusiastic welcome to anyone who came along with us in the past, as well as to those who are joining us now. Either way, we can promise the same quantitative approach to, and analysis of, real systems.

> 我们写第一本书的主要目标是改变人们学习和思考计算机架构的方式。我们认为这个目标仍然有效和重要。该领域每天都在变化，必须在真实计算机上使用真实的示例和测量进行研究，而不是简单地作为定义和设计的集合，这些定义和设计无需实现。我们为过去以及现在加入我们的人提供了热情的欢迎。无论哪种方式，我们都可以承诺对真实系统的相同定量方法和分析。

As with earlier versions, we have strived to produce a new edition that will continue to be as relevant for professional engineers and architects as it is for those involved in advanced computer architecture and design courses. Like the first edition, this edition has a sharp focus on new platforms—personal mobile devices and warehouse-scale computers—and new architectures—specifically, domainspecific architectures. As much as its predecessors, this edition aims to demystify computer architecture through an emphasis on cost-performance-energy trade-offs and good engineering design. We believe that the field has continued to mature and move toward the rigorous quantitative foundation of long-established scientific and engineering disciplines.

> 与较早的版本一样，我们一直在努力制作一个新版本，该版本将继续与专业工程师和工程师一样重要，就像那些参与高级计算机架构和设计课程的人一样。像第一版一样，此版本非常关注新平台(个人移动设备和仓库规模计算机)和新的体系结构，特别是特定于域的体系结构。与其前辈一样，此版本旨在通过强调成本效果 能量折衷和良好的工程设计来揭开计算机架构的神秘面纱。我们认为，该领域继续成熟，并朝着长期以来的科学和工程学科的严格定量基础迈进。

### This Edition

The ending of Moore’s Law and Dennard scaling is having as profound effect on computer architecture as did the switch to multicore. We retain the focus on the extremes in size of computing, with personal mobile devices (PMDs) such as cell phones and tablets as the clients and warehouse-scale computers offering cloud computing as the server. We also maintain the other theme of parallelism in all its forms: _data-level parallelism (DLP)_ in Chapters [1](#_bookmark2) and [4](#_bookmark165), _instruction-level parallelism (ILP)_ in [Chapter 3](#_bookmark93), _thread-level parallelism_ in [Chapter 5](#_bookmark213), and _requestlevel parallelism_ (RLP) in [Chapter 6](#_bookmark268).

> 摩尔定律和丹纳德缩放的结束对计算机架构的影响与转向多核心一样。我们将关注计算大小的极端关注，其中包括个人移动设备(PMD)，例如手机和平板电脑，例如客户和仓库级计算机，这些计算机将云计算作为服务器。我们还以各种形式维持另一个并行性的主题：*data 级并行性(dlp)*在第 [1](#_bookmark2) 和 [4](#_bookmark165) 中[第 3 章](#_bookmark93)，\_ thread-level Parallelism* [第 5 章](#* bookmark213)和 *request-级别的平行*(rlp)在[第 6 章](#_bookmark268)中。

The most pervasive change in this edition is switching from MIPS to the RISCV instruction set. We suspect this modern, modular, open instruction set may become a significant force in the information technology industry. It may become as important in computer architecture as Linux is for operating systems.

> 此版本中最普遍的变化是从 MIPS 切换到 RISC-V 指令集。我们怀疑这种现代，模块化的开放指导集可能会成为信息技术行业的重要力量。它在计算机体系结构中可能与 Linux 一样重要。

The newcomer in this edition is [Chapter 7](#_bookmark322), which introduces domain-specific architectures with several concrete examples from industry.

> 此版本中的新来者是[第 7 章](#_bookmark322)，它引入了特定于领域的体系结构，其中包括来自行业的几个具体示例。

As before, the first three appendices in the book give basics on the RISC-V instruction set, memory hierarchy, and pipelining for readers who have not read a book like _Computer Organization and Design_. To keep costs down but still supply supplemental material that is of interest to some readers, available online at [https://www.elsevier.com/books-and-journals/book-companion/9780128119051](https://www.elsevier.com/books-and-journals/book-companion/9780128119051) are nine more appendices. There are more pages in these appendices than there are in this book!

> 和以前一样，本书中的前三个附录为 RISC-V 指令集，内存层次结构和管道提供了基础知识，这些读者尚未阅读像 *Computer 组织和 Design* 这样的书。为了降低成本，但仍然支持一些读者感兴趣的补充材料，请网上在线获得，网址为[[https://www.elsevier.com/books-and-journals/book-companion/9780128119051](https://www.elsevier.com/books-and-journals/book-companion/9780128119051)]([https：///[www.elsevier.com/books-and-journals/book-companion/9780128119051](http://www.elsevier.com/books-and-journals/book-companion/9780128119051)](https%EF%BC%9A///%5B[www.elsevier.com/books-and-journals/book-companion/9780128119051%5D(http://www.elsevier.com/books-and-journals/book-companion/9780128119051)](http://www.elsevier.com/books-and-journals/book-companion/9780128119051%5D(http://www.elsevier.com/books-and-journals/book-companion/9780128119051))))是九个附录。这些附录中的页面比本书中的更多！

This edition continues the tradition of using real-world examples to demonstrate the ideas, and the “Putting It All Together” sections are brand new. The “Putting It All Together” sections of this edition include the pipeline organizations and memory hierarchies of the ARM Cortex A8 processor, the Intel core i7 processor, the NVIDIA GTX-280 and GTX-480 GPUs, and one of the Google warehouse-scale computers.

> 该版本延续了使用现实世界示例来证明这些想法的传统，而“将其整合在一起”部分是全新的。此版本的“将所有内容放在一起”部分包括管道组织和记忆层的 ARM Cortex A8 处理器，Intel Core i7 处理器，NVIDIA GTX-280 和 GTX-480 GPU，以及 Google Warehouse 之一 规模计算机。

### Topic Selection and Organization

> ###主题选择和组织

As before, we have taken a conservative approach to topic selection, for there are many more interesting ideas in the field than can reasonably be covered in a treatment of basic principles. We have steered away from a comprehensive survey of every architecture a reader might encounter. Instead, our presentation focuses on core concepts likely to be found in any new machine. The key criterion remains that of selecting ideas that have been examined and utilized successfully enough to permit their discussion in quantitative terms.

> 和以前一样，我们采取了一种保守的主题选择方法，因为该领域的想法比对基本原则的理解可以合理涵盖的更多有趣的想法。我们已经摆脱了对读者可能遇到的每个建筑的全面调查。取而代之的是，我们的演示重点是在任何新机器中都可能找到的核心概念。关键标准仍然是选择已经成功地检查和利用的想法，以允许他们以定量术语进行讨论。

Our intent has always been to focus on material that is not available in equivalent form from other sources, so we continue to emphasize advanced content wherever possible. Indeed, there are several systems here whose descriptions cannot be found in the literature. (Readers interested strictly in a more basic introduction to computer architecture should read _Computer Organization and Design: The Hardware/Software Interface_.)

> 我们的意图一直是专注于其他来源不可用的材料，因此我们继续在可能的情况下强调先进的内容。确实，这里有几个系统可以在文献中找到它们的描述。(严格对计算机体系结构的更基本介绍感兴趣的读者应阅读*计算机组织和设计：硬件/软件接口*。)。

### An Overview of the Content

[Chapter 1](#_bookmark2) includes formulas for energy, static power, dynamic power, integrated circuit costs, reliability, and availability. (These formulas are also found on the front inside cover.) Our hope is that these topics can be used through the rest of the book. In addition to the classic quantitative principles of computer design and performance measurement, it shows the slowing of performance improvement of general-purpose microprocessors, which is one inspiration for domain-specific architectures.

> [第 1 章](#_bookmark2)包括能源，静态功率，动态功率，集成的能力，可靠性和可用性的公式。(这些公式也在封面内部的前面。)我们希望这些主题可以通过本书的其余部分使用。除了经典的计算机设计和性能测量的定量原理外，它还显示了通用微处理器的性能改善的放缓，这是针对域特异性体系结构的灵感。

Our view is that the instruction set architecture is playing less of a role today than in 1990, so we moved this material to [Appendix A](#_bookmark391). It now uses the RISC-V architecture. (For quick review, a summary of the RISC-V ISA can be found on the back inside cover.) For fans of ISAs, Appendix K was revised for this edition and covers 8 RISC architectures (5 for desktop and server use and 3 for embedded use), the 80 86, the DEC VAX, and the IBM 360/370.

> 我们的观点是，与 1990 年的指令集体系结构相比，今天的角色较少，因此我们将此材料移至[附录 A](#_bookmark391)。现在，它使用 RISC-V 架构。(要进行快速回顾，可以在内部的封面上找到 RISC-V ISA 的摘要。)对于 ISAS 的粉丝，该版本对附录 K 进行了修订，并涵盖了 8 个 RISC 架构(5 用于桌面和服务器使用 5，而 3 则 3 for 嵌入式使用)，80 86，DEC VAX 和 IBM 360/370。

We then move onto memory hierarchy in [Chapter 2](#_bookmark46), since it is easy to apply the cost-performance-energy principles to this material, and memory is a critical resource for the rest of the chapters. As in the past edition, [Appendix B](#_bookmark436) contains an introductory review of cache principles, which is available in case you need it. [Chapter 2](#_bookmark46) discusses 10 advanced optimizations of caches. The chapter includes virtual machines, which offer advantages in protection, software management, and hardware management, and play an important role in cloud computing. In addition to covering SRAM and DRAM technologies, the chapter includes new material both on Flash memory and on the use of stacked die packaging for extending the memory hierarchy. The PIAT examples are the ARM Cortex A8, which is used in PMDs, and the Intel Core i7, which is used in servers.

> 然后，我们在[第 2 章](#_bookmark46)中移至内存层次结构，因为很容易将成本效果 能源原理应用于此材料，而内存是其余章节的关键资源。与过去的版本一样，[附录 B](#_bookmark436) 包含对缓存原理的介绍性评论，如果您需要它，可以使用。[第 2 章](#_bookmark46)讨论了 10 个缓存的高级优化。本章包括虚拟机，可在保护，软件管理和硬件管理方面具有优势，并在云计算中发挥重要作用。除了涵盖 SRAM 和 DRAM 技术外，本章还包括闪存上的新材料以及使用堆叠的模具包装来扩展内存层次结构。PIAT 示例是用于 PMD 中的 ARM Cortex A8，以及用于服务器中的 Intel Core i7。

[Chapter 3](#_bookmark93) covers the exploitation of instruction-level parallelism in highperformance processors, including superscalar execution, branch prediction (including the new tagged hybrid predictors), speculation, dynamic scheduling, and simultaneous multithreading. As mentioned earlier, [Appendix C](#_bookmark481) is a review of pipelining in case you need it. [Chapter 3](#_bookmark93) also surveys the limits of ILP. Like [Chapter 2](#_bookmark46), the PIAT examples are again the ARM Cortex A8 and the Intel Core i7. While the third edition contained a great deal on Itanium and VLIW, this material is now in Appendix H, indicating our view that this architecture did not live up to the earlier claims.

> [第 3 章](#_bookmark93)涵盖了高性能处理器中指令级并行的开发，包括超级执行，分支预测(包括新的标记的混合预测指标)，推测，动态调度和同时的多线程。如前所述，[附录 C](#_bookmark481) 是对管道的评论，以防万一。[第 3 章](#_bookmark93)还调查了 ILP 的限制。像[第 2 章](#_bookmark46)一样，PIAT 示例再次是 ARM Cortex A8 和 Intel Core i7。虽然第三版在 iTanium 和 vliw 上包含了很多东西，但该材料现在在附录 H 中，这表明我们认为这种体系结构并没有符合较早的主张。

The increasing importance of multimedia applications such as games and video processing has also increased the importance of architectures that can exploit data level parallelism. In particular, there is a rising interest in computing using graphical processing units (GPUs), yet few architects understand how GPUs really work. We decided to write a new chapter in large part to unveil this new style of computer architecture. [Chapter 4](#_bookmark165) starts with an introduction to vector architectures, which acts as a foundation on which to build explanations of multimedia SIMD instruction set extensions and GPUs. (Appendix G goes into even more depth on vector architectures.) This chapter introduces the Roofline performance model and then uses it to compare the Intel Core i7 and the NVIDIA GTX 280 and GTX 480 GPUs. The chapter also describes the Tegra 2 GPU for PMDs.

> 多媒体应用程序(例如游戏和视频处理)的重要性越来越多，也提高了可以利用数据级并行性的体系结构的重要性。特别是，使用图形处理单元(GPU)对计算的兴趣有所增加，但是很少有工程师了解 GPU 的真正工作方式。我们决定在很大程度上写一个新的篇章，以揭露这种新型的计算机架构风格。[第 4 章](#_bookmark165)从介绍矢量体系结构开始，该介绍是建立多媒体模拟仪式扩展和 GPU 的解释的基础。(附录 G 对向量体系结构进行了更深入的深度。)本章介绍了屋顶线的性能模型，然后使用它比较 Intel Core i7 和 NVIDIA GTX 280 和 GTX 480 GPU。本章还介绍了 PMD 的 Tegra 2 GPU。

[Chapter 5](#_bookmark213) describes multicore processors. It explores symmetric and distributed-memory architectures, examining both organizational principles and performance. The primary additions to this chapter include more comparison of multicore organizations, including the organization of multicore-multilevel caches, multicore coherence schemes, and on-chip multicore interconnect. Topics in synchronization and memory consistency models are next. The example is the Intel Core i7. Readers interested in more depth on interconnection networks should read Appendix F, and those interested in larger scale multiprocessors and scientific applications should read Appendix I.

> [第 5 章](#_bookmark213)描述了多层处理器。它探索对称和分布式内存架构，研究组织原理和绩效。本章的主要补充包括对多项组织组织的更多比较，包括组织多层型粘合剂，多核心连贯方案和芯片上多层互连的组织。接下来是同步和内存一致性模型的主题。该示例是 Intel Core i7。对互连网络有更多深度感兴趣的读者应阅读附录 F，并且那些对大规模多处理器和科学应用程序感兴趣的人应阅读附录 I。

[Chapter 6](#_bookmark268) describes warehouse-scale computers (WSCs). It was extensively revised based on help from engineers at Google and Amazon Web Services. This chapter integrates details on design, cost, and performance of WSCs that few architects are aware of. It starts with the popular MapReduce programming model before describing the architecture and physical implementation of WSCs, including cost. The costs allow us to explain the emergence of cloud computing, whereby it can be cheaper to compute using WSCs in the cloud than in your local datacenter. The PIAT example is a description of a Google WSC that includes information published for the first time in this book.

> [第 6 章](#_bookmark268)描述了仓库规模计算机(WSCS)。根据 Google 和 Amazon Web Services 的工程师的帮助，对其进行了广泛的修订。本章整合了很少有档案的 WSC 的设计，成本和性能的详细信息。它从流行的 MapReduce 编程模型开始，然后再描述 WSC 的体系结构和物理实现，包括成本。成本使我们能够解释云计算的出现，因此，使用云中的 WSC 比本地数据中心中的 WSC 更便宜。PIAT 示例是对 Google WSC 的描述，其中包括本书中首次发布的信息。

The new [Chapter 7](#_bookmark322) motivates the need for Domain-Specific Architectures (DSAs). It draws guiding principles for DSAs based on the four examples of DSAs. Each DSA corresponds to chips that have been deployed in commercial settings. We also explain why we expect a renaissance in computer architecture via DSAs given that single-thread performance of general-purpose microprocessors has stalled.

> 新的[第 7 章](#_bookmark322)激发了对域特异性体系结构(DSA)的需求。根据 DSA 的四个示例，它绘制了 DSA 的指导原则。每个 DSA 对应于已在商业环境中部署的芯片。我们还解释了为什么我们期望通过 DSA 进行计算机体系结构的复兴，因为通用微处理器的单线程性能已经停滞不前。

This brings us to Appendices [A](#_bookmark391) through M. [Appendix A](#_bookmark391) covers principles of ISAs, including RISC-V, and Appendix K describes 64-bit versions of RISC V, ARM, MIPS, Power, and SPARC and their multimedia extensions. It also includes some classic architectures (80x86, VAX, and IBM 360/370) and popular embedded instruction sets (Thumb-2, microMIPS, and RISC V C). Appendix H is related, in that it covers architectures and compilers for VLIW ISAs.

> 这将我们带到了附录 [a](#_bookmark391)。，以及 SPARC 及其多媒体扩展。它还包括一些经典体系结构(80x86，VAX 和 IBM 360/370)和流行的嵌入式指令集(Thumb-2，Micromips 和 Risc V C)。附录 H 是相关的，因为它涵盖了 Vliw Isas 的架构和编译器。

As mentioned earlier, [Appendix B](#_bookmark436) and [Appendix C](#_bookmark481) are tutorials on basic caching and pipelining concepts. Readers relatively new to caching should read [Appen-](#_bookmark436) [dix B](#_bookmark436) before [Chapter 2](#_bookmark46), and those new to pipelining should read [Appendix C](#_bookmark481) before [Chapter 3](#_bookmark93).

> 如前所述，[附录 B](#_bookmark436) 和[附录 C](#_bookmark481) 是有关基本的可追溯和管道概念的教程。缓存相对较新的读者应阅读 [appen-](#_bookmark436)[dix b](#_bookmark436)[第 2 章](#_bookmark46)，而新手的管道上的读者应阅读[附录 C](#_bookmark481)[第 3 章](#_bookmark93)。

Appendix D, “Storage Systems,” has an expanded discussion of reliability and availability, a tutorial on RAID with a description of RAID 6 schemes, and rarely found failure statistics of real systems. It continues to provide an introduction to queuing theory and I/O performance benchmarks. We evaluate the cost, performance, and reliability of a real cluster: the Internet Archive. The “Putting It All Together” example is the NetApp FAS6000 filer.

> 附录 D，“存储系统”，对可靠性和可用性进行了扩展的讨论，有关 RAID 的教程，并描述了 RAID 6 个方案，并且很少发现对真实系统的故障统计。它继续为排队理论和 I/O 性能基准提供介绍。我们评估了一个真正集群的成本，演奏和可靠性：互联网档案。“将所有内容放在一起”的示例是 NetApp FAS6000 档案。

Appendix E, by Thomas M. Conte, consolidates the embedded material in one place.

> 托马斯·孔戴(Thomas M. Conte)的附录 E 将嵌入式材料合并在一个地方。

Appendix F, on interconnection networks, is revised by Timothy M. Pinkston and Jos´e Duato. Appendix G, written originally by Krste Asanovi´c, includes a description of vector processors. We think these two appendices are some of the best material we know of on each topic.

> 附录 F 在互连网络上，由蒂莫西·平克斯顿(Timothy M.附录 G，最初由 Krste Asanovi´c 撰写，包括对矢量处理器的描述。我们认为，这两个附录是我们每个主题中了解的最好的材料。

Appendix H describes VLIW and EPIC, the architecture of Itanium.

> 附录 H 描述了 Itanium 架构的 Vliw 和 Epic。

Appendix I describes parallel processing applications and coherence protocols for larger-scale, shared-memory multiprocessing. Appendix J, by David Goldberg, describes computer arithmetic.

> 附录一世描述了并行处理应用程序和相干协议，用于大规模共享的内存多处理。David Goldberg 的附录 J 描述了计算机算术。

Appendix L, by Abhishek Bhattacharjee, is new and discusses advanced techniques for memory management, focusing on support for virtual machines and design of address translation for very large address spaces. With the growth in clouds processors, these architectural enhancements are becoming more important. Appendix M collects the “Historical Perspective and References” from each chapter into a single appendix. It attempts to give proper credit for the ideas in each chapter and a sense of the history surrounding the inventions. We like to think of this as presenting the human drama of computer design. It also supplies references that the student of architecture may want to pursue. If you have time, we recommend reading some of the classic papers in the field that are mentioned in these sections. It is both enjoyable and educational to hear the ideas directly from the creators. “Historical Perspective” was one of the most popular sections of prior editions.

> Abhishek Bhattacharjee 的附录 L 是新的，并讨论了用于内存管理的高级技术，重点介绍了对虚拟机的支持和非常宽敞的地址空间的地址翻译设计。随着云处理器的增长，这些建筑增强功能变得越来越重要。附录 M 将每章的“历史观点和参考”收集到一个附录中。它试图为每章中的想法以及对发明的历史有适当的荣誉。我们喜欢将其视为展示人类的计算机设计戏剧。它还提供了建筑学生可能想要追求的参考文献。如果您有时间，我们建议阅读这些部分中提到的领域中一些经典论文。直接听到创作者的想法，这既愉快又教育意义。“历史观点”是先前版本中最受欢迎的部分之一。

### Navigating the Text

There is no single best order in which to approach these chapters and appendices, except that all readers should start with [Chapter 1](#_bookmark2). If you don’t want to read everything, here are some suggested sequences:

> 没有一个最佳顺序可以接近这些章节和附录，除了所有读者都应以[第 1 章](#_bookmark2)开头。如果您不想阅读所有内容，这里有一些建议的序列：

_Memory Hierarchy:_ [Appendix B](#_bookmark436), [Chapter 2](#_bookmark46), and Appendices D and M.

> _Memory 层次结构：_ [附录 B](#_bookmark436)，[第 2 章](#_bookmark46)和附录 D 和 M。

_Instruction-Level Parallelism:_ [Appendix C](#_bookmark481), [Chapter 3](#_bookmark93), and Appendix H

> _ Instructuction-level 并行性：_ [附录 C](#_bookmark481)，[第 3 章](#_bookmark93)和附录 H

_Data-Level Parallelism:_ Chapters [4](#_bookmark165), [6](#_bookmark268), and [7](#_bookmark322), Appendix G

> *data 级并行性：*章节 [4](#_bookmark165)，[6](#_bookmark268) 和 [7](#_bookmark322)，附录 G

_Thread-Level Parallelism:_ [Chapter 5](#_bookmark213), Appendices F and I

> _ thread 级并行性：_ [第 5 章](#_bookmark213)，附录 F 和 i

_Request-Level Parallelism:_ [Chapter 6](#_bookmark268)

> _request 级并行性：_ [第 6 章](#_bookmark268)

_ISA:_ Appendices A and K Appendix E can be read at any time, but it might work best if read after the ISA and cache sequences. Appendix J can be read whenever arithmetic moves you. You should read the corresponding portion of Appendix M after you complete each chapter.

> *isa：*附录 A 和 K 附录 E 可以随时读取，但是如果在 ISA 和缓存序列后读取，它可能会效果最好。每当算术移动您时，都可以阅读附录 J。完成每章后，您应该阅读附录 M 的相应部分。

### Chapter Structure

> ###章节结构

The material we have selected has been stretched upon a consistent framework that is followed in each chapter. We start by explaining the ideas of a chapter. These ideas are followed by a “Crosscutting Issues” section, a feature that shows how the ideas covered in one chapter interact with those given in other chapters. This is followed by a “Putting It All Together” section that ties these ideas together by showing how they are used in a real machine.

> 我们选择的材料已在每章中遵循的一致框架上拉伸。我们首先解释一章的想法。这些想法之后是“横切问题”部分，该部分显示了一章中涵盖的想法如何与其他章节中给出的想法相互作用。其次是“将所有内容放在一起”部分，通过显示它们在真实机器中的使用方式将这些想法联系在一起。

Next in the sequence is “Fallacies and Pitfalls,” which lets readers learn from the mistakes of others. We show examples of common misunderstandings and architectural traps that are difficult to avoid even when you know they are lying in wait for you. The “Fallacies and Pitfalls” sections is one of the most popular sections of the book. Each chapter ends with a “Concluding Remarks” section.

> 序列的下一个是“谬论和陷阱”，它使读者可以从他人的错误中学习。我们展示了常见的误解和建筑陷阱的例子，即使您知道他们正在等待您，这些例子也很难避免。“谬论和陷阱”部分是本书中最受欢迎的部分之一。每章以“结论说明”部分结尾。

### Case Studies With Exercises

Each chapter ends with case studies and accompanying exercises. Authored by experts in industry and academia, the case studies explore key chapter concepts and verify understanding through increasingly challenging exercises. Instructors should find the case studies sufficiently detailed and robust to allow them to create their own additional exercises.

> 每章以案例研究和随附的练习结尾。该案例研究由行业和学术界的专家撰写，探讨了关键章节概念，并通过越来越具有挑战性的练习来验证理解。讲师应发现案例研究足够详细且鲁棒，以使他们能够创建自己的其他练习。

Brackets for each exercise (<chapter.section>) indicate the text sections of primary relevance to completing the exercise. We hope this helps readers to avoid exercises for which they haven’t read the corresponding section, in addition to providing the source for review. Exercises are rated, to give the reader a sense of the amount of time required to complete an exercise:

> 每次练习的支架(＆lt;章节。节＆gt;)表示完成练习的主要相关性的文本部分。我们希望这可以帮助读者避免练习，除了提供回顾源外，他们还没有阅读相应的部分。对练习进行了评分，以使读者了解完成练习所需的时间：

### Supplemental Materials

A variety of resources are available online at [https://www.elsevier.com/books/](https://www.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1) > [computer-architecture/hennessy/978-0-12-811905-1](https://www.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1), including the following:

> 可以在线提供各种资源)> [Computer-Architecture/Hennessy/978-0-12-811905-1](%5Bhttps://www.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1%5D(https://www.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1))，包括下列：

Reference appendices, some guest authored by subject experts, covering a range of advanced topics

> 参考附录，一些由主题专家撰写的来宾，涵盖了一系列高级主题

Historical perspectives material that explores the development of the key ideas presented in each of the chapters in the text

> 历史观点材料，探讨了文本中每章中介绍的关键思想的发展

Instructor slides in PowerPoint

> 在 PowerPoint 中滑动教练

Figures from the book in PDF, EPS, and PPT formats

> PDF，EPS 和 PPT 格式的书中的数字

Links to related material on the Web

> 网络上相关材料的链接

List of errata New materials and links to other resources available on the Web will be added on a regular basis.

> 将定期添加 Errrata 新材料列表和网络上其他资源的链接。

### Helping Improve This Book

Finally, it is possible to make money while reading this book. (Talk about cost performance!) If you read the Acknowledgments that follow, you will see that we went to great lengths to correct mistakes. Since a book goes through many printings, we have the opportunity to make even more corrections. If you uncover any remaining resilient bugs, please contact the publisher by electronic mail ([ca6bugs](mailto:ca6bugs@mkp.com)@[mkp.com](mailto:ca6bugs@mkp.com)).

> 最后，可以在阅读这本书时赚钱。(谈论成本的能力！)如果您阅读了以下确认，您会发现我们竭尽全力纠正错误。由于一本书经历了许多印刷品，因此我们有机会进行更多的更正。如果您发现剩余的弹性错误，请通过电子邮件([Ca6bugs](mailto%EF%BC%9Aca6bugs@mkp.com) 与发布者联系，@[mkp.com](mailto%EF%BC%9Aca6bugs@mkp.com))。

We welcome general comments to the text and invite you to send them to a separate email address at [ca6comments](mailto:ca6comments@mkp.com)@[mkp.com](mailto:ca6comments@mkp.com).

> 我们欢迎对文本的一般性评论，并邀请您将其发送到 [CA6COMMENTS](mailto%EF%BC%9Aca6comments@mkp.com)@[mkp.com](mailto%EF%BC%9Aca6comments@mkp.com) 的单独电子邮件地址。

### Concluding Remarks

Once again, this book is a true co-authorship, with each of us writing half the chapters and an equal share of the appendices. We can’t imagine how long it would have taken without someone else doing half the work, offering inspiration when the task seemed hopeless, providing the key insight to explain a difficult concept, supplying over-the-weekend reviews of chapters, and commiserating when the weight of our other obligations made it hard to pick up the pen.

> 这本书再次是一本真正的共同作者，我们每个人都写了一半的章节和附录的平等份额。我们无法想象如果没有其他人做一半的工作，这将花费多长时间，当任务似乎没有希望时提供灵感，提供了一个关键的见解，以解释一个困难的概念，供应章节的周末评论和当我们的其他义务的重量使笔很难拿起时，他会同意。

Thus, once again, we share equally the blame for what you are about to read.

Although this is only the sixth edition of this book, we have actually created ten different versions of the text: three versions of the first edition (alpha, beta, and final) and two versions of the second, third, and fourth editions (beta and final). Along the way, we have received help from hundreds of reviewers and users. Each of these people has helped make this book better. Thus, we have chosen to list all of the people who have made contributions to some version of this book.

> 尽管这只是本书的第六版，但我们实际上创建了文本的十个不同版本：第一版的三个版本(Alpha，beta 和 finas)以及第二版，第二版，第三版和第四版的两个版本(Beta 和最终)。一路上，我们收到了数百名审阅者和用户的帮助。这些人中的每一个都帮助这本书变得更好。因此，我们选择列出所有为本书某种版本做出贡献的人。

### Contributors to the Sixth Edition

Like prior editions, this is a community effort that involves scores of volunteers. Without their help, this edition would not be nearly as polished.

##### _Reviewers_

Jason D. Bakos, University of South Carolina; Rajeev Balasubramonian, University of Utah; Jose Delgado-Frias, Washington State University; Diana Franklin, The University of Chicago; Norman P. Jouppi, Google; Hugh C. Lauer, Worcester Polytechnic Institute; Gregory Peterson, University of Tennessee; Bill Pierce, Hood College; Parthasarathy Ranganathan, Google; William H. Robinson, Vanderbilt University; Pat Stakem, Johns Hopkins University; Cliff Young, Google; Amr Zaky, University of Santa Clara; Gerald Zarnett, Ryerson University; Huiyang Zhou, North Carolina State University.

Members of the University of California-Berkeley Par Lab and RAD Lab who gave frequent reviews of Chapters [1](#_bookmark3), [4](#_bookmark166), and [6](#_bookmark269) and shaped the explanation of GPUs and WSCs: Krste Asanovi´c, Michael Armbrust, Scott Beamer, Sarah Bird, Bryan Catanzaro, Jike Chong, Henry Cook, Derrick Coetzee, Randy Katz, Yunsup Lee, Leo Meyervich, Mark Murphy, Zhangxi Tan, Vasily Volkov, and Andrew Waterman.

##### _Appendices_

Krste Asanovi´c, University of California, Berkeley [(Appendix](#_bookmark685) G); Abhishek Bhattacharjee, Rutgers University ([Appendix L](#_bookmark902)); Thomas M. Conte, North Carolina State University ([Appendix E](#_bookmark574)); Jos´e Duato, Universitat Politècnica de

##### _Case Studies With Exercises_

Jason D. Bakos, University of South Carolina (Chapters [3](#_bookmark94) and [4](#_bookmark166)); Rajeev Balasubramonian, University of Utah ([Chapter 2](#_bookmark47)); Diana Franklin, The University of Chicago ([Chapter 1](#_bookmark3) and [Appendix](#_bookmark482) C); Norman P. Jouppi, Google, ([Chapter 2](#_bookmark47)); Naveen Muralimanohar, HP Labs ([Chapter 2](#_bookmark47)); Gregory Peterson, University of Tennessee ([Appendix A](#_bookmark392)); Parthasarathy Ranganathan, Google [(Chapter](#_bookmark269) 6); Cliff Young, Google ([Chapter 7](#_bookmark323)); Amr Zaky, University of Santa Clara [(Chapter 5](#_bookmark214) and [Appendix B](#_bookmark437)).

Jichuan Chang, Junwhan Ahn, Rama Govindaraju, and Milad Hashemi assisted in the development and testing of the case studies and exercises for [Chapter 6](#_bookmark269).

##### _Additional Material_

John Nickolls, Steve Keckler, and Michael Toksvig of NVIDIA (Chapter 4 NVIDIA GPUs); Victor Lee, Intel ([Chapter 4](#_bookmark166) comparison of Core i7 and GPU); John Shalf, LBNL ([Chapter 4](#_bookmark166) recent vector architectures); Sam Williams, LBNL (Roofline model for computers in [Chapter 4](#_bookmark166)); Steve Blackburn of Australian National University and Kathryn McKinley of University of Texas at Austin (Intel performance and power measurements in [Chapter 5](#_bookmark214)); Luiz Barroso, Urs H€olzle, Jimmy Clidaris, Bob Felderman, and Chris Johnson of Google (the Google WSC in [Chapter 6](#_bookmark269)); James Hamilton of Amazon Web Services (power distribution and cost model in [Chapter 6](#_bookmark269)).

Jason D. Bakos of the University of South Carolina updated the lecture slides for this edition.

This book could not have been published without a publisher, of course. We wish to thank all the Morgan Kaufmann/Elsevier staff for their efforts and support. For this fifth edition, we particularly want to thank our editors Nate McFadden and Steve Merken, who coordinated surveys, development of the case studies and exercises, manuscript reviews, and the updating of the appendices.

We must also thank our university staff, Margaret Rowland and Roxana Infante, for countless express mailings, as well as for holding down the fort at Stanford and Berkeley while we worked on the book.

Our final thanks go to our wives for their suffering through increasingly early mornings of reading, thinking, and writing.

### Contributors to Previous Editions

##### _Reviewers_

George Adams, Purdue University; Sarita Adve, University of Illinois at UrbanaChampaign; Jim Archibald, Brigham Young University; Krste Asanovi´c, Massachusetts Institute of Technology; Jean-Loup Baer, University of Washington; Paul Barr, Northeastern University; Rajendra V. Boppana, University of Texas, San Antonio; Mark Brehob, University of Michigan; Doug Burger, University of Texas, Austin; John Burger, SGI; Michael Butler; Thomas Casavant; Rohit Chandra; Peter Chen, University of Michigan; the classes at SUNY Stony Brook, Carnegie Mellon, Stanford, Clemson, and Wisconsin; Tim Coe, Vitesse Semiconductor; Robert P. Colwell; David Cummings; Bill Dally; David Douglas; Jos´e Duato, Universitat Politècnica de València and Simula; Anthony Duben, Southeast Missouri State University; Susan Eggers, University of Washington; Joel Emer; Barry Fagin, Dartmouth; Joel Ferguson, University of California, Santa Cruz; Carl Feynman; David Filo; Josh Fisher, Hewlett-Packard Laboratories; Rob Fowler, DIKU; Mark Franklin, Washington University (St. Louis); Kourosh Gharachorloo; Nikolas Gloy, Harvard University; David Goldberg, Xerox Palo Alto Research Center; Antonio González, Intel and Universitat Politècnica de Catalunya; James Goodman, University of Wisconsin-Madison; Sudhanva Gurumurthi, University of Virginia; David Harris, Harvey Mudd College; John Heinlein; Mark Heinrich, Stanford; Daniel Helman, University of California, Santa Cruz; Mark D. Hill, University of Wisconsin-Madison; Martin Hopkins, IBM; Jerry Huck, Hewlett-Packard Laboratories; Wen-mei Hwu, University of Illinois at UrbanaChampaign; Mary Jane Irwin, Pennsylvania State University; Truman Joe; Norm Jouppi; David Kaeli, Northeastern University; Roger Kieckhafer, University of Nebraska; Lev G. Kirischian, Ryerson University; Earl Killian; Allan Knies, Purdue University; Don Knuth; Jeff Kuskin, Stanford; James R. Larus, Microsoft Research; Corinna Lee, University of Toronto; Hank Levy; Kai Li, Princeton University; Lori Liebrock, University of Alaska, Fairbanks; Mikko Lipasti, University of Wisconsin-Madison; Gyula A. Mago, University of North Carolina, Chapel Hill; Bryan Martin; Norman Matloff; David Meyer; William Michalson, Worcester Polytechnic Institute; James Mooney; Trevor Mudge, University of Michigan; Ramadass Nagarajan, University of Texas at Austin; David Nagle, Carnegie Mellon University; Todd Narter; Victor Nelson; Vojin Oklobdzija, University of California, Berkeley; Kunle Olukotun, Stanford University; Bob Owens, Pennsylvania State University; Greg Papadapoulous, Sun Microsystems; Joseph Pfeiffer; Keshav Pingali, Cornell University; Timothy M. Pinkston, University of Southern California; Bruno Preiss, University of Waterloo; Steven Przybylski; Jim Quinlan; Andras Radics; Kishore Ramachandran, Georgia Institute of Technology; Joseph Rameh, University of Texas, Austin; Anthony Reeves, Cornell University; Richard Reid, Michigan State University; Steve Reinhardt, University of Michigan; David Rennels, University of California, Los Angeles; Arnold L. Rosenberg, University of Massachusetts, Amherst; Kaushik Roy, Purdue University; Emilio Salgueiro, Unysis; Karthikeyan Sankaralingam, University of Texas at Austin; Peter Schnorf; Margo Seltzer; Behrooz Shirazi, Southern Methodist University; Daniel Siewiorek, Carnegie Mellon University; J. P. Singh, Princeton; Ashok Singhal; Jim Smith, University of Wisconsin-Madison; Mike Smith, Harvard University; Mark Smotherman, Clemson University; Gurindar Sohi, University of Wisconsin-Madison; Arun Somani, University of Washington; Gene Tagliarin, Clemson University; Shyamkumar Thoziyoor, University of Notre Dame; Evan Tick, University of Oregon; Akhilesh Tyagi, University of North Carolina, Chapel Hill; Dan Upton, University of Virginia; Mateo Valero, Universidad Polit´ecnica de Cataluña, Barcelona; Anujan Varma, University of California, Santa Cruz; Thorsten von Eicken, Cornell University; Hank Walker, Texas A&M; Roy Want, Xerox Palo Alto Research Center; David Weaver, Sun Microsystems; Shlomo Weiss, Tel Aviv University; David Wells; Mike Westall, Clemson University; Maurice Wilkes; Eric Williams; Thomas Willis, Purdue University; Malcolm Wing; Larry Wittie, SUNY Stony Brook; Ellen Witte Zegura, Georgia Institute of Technology; Sotirios G. Ziavras, New Jersey Institute of Technology.

##### _Appendices_

The vector appendix was revised by Krste Asanovi´c of the Massachusetts Institute of Technology. The floating-point appendix was written originally by David Goldberg of Xerox PARC.

##### _Exercises_

George Adams, Purdue University; Todd M. Bezenek, University of WisconsinMadison (in remembrance of his grandmother Ethel Eshom); Susan Eggers; Anoop Gupta; David Hayes; Mark Hill; Allan Knies; Ethan L. Miller, University of California, Santa Cruz; Parthasarathy Ranganathan, Compaq Western Research Laboratory; Brandon Schwartz, University of Wisconsin-Madison; Michael Scott; Dan Siewiorek; Mike Smith; Mark Smotherman; Evan Tick; Thomas Willis.

##### _Case Studies With Exercises_

Andrea C. Arpaci-Dusseau, University of Wisconsin-Madison; Remzi H. ArpaciDusseau, University of Wisconsin-Madison; Robert P. Colwell, R&E Colwell & Assoc., Inc.; Diana Franklin, California Polytechnic State University, San Luis Obispo; Wen-mei W. Hwu, University of Illinois at Urbana-Champaign; Norman P. Jouppi, HP Labs; John W. Sias, University of Illinois at Urbana-Champaign; David A. Wood, University of Wisconsin-Madison.

##### _Special Thanks_

Duane Adams, Defense Advanced Research Projects Agency; Tom Adams; Sarita Adve, University of Illinois at Urbana-Champaign; Anant Agarwal; Dave Albonesi, University of Rochester; Mitch Alsup; Howard Alt; Dave Anderson; Peter Ashenden; David Bailey; Bill Bandy, Defense Advanced Research Projects Agency; Luiz Barroso, Compaq’s Western Research Lab; Andy Bechtolsheim; _C. Gordon_ Bell; Fred Berkowitz; John Best, IBM; Dileep Bhandarkar; Jeff Bier, BDTI; Mark Birman; David Black; David Boggs; Jim Brady; Forrest Brewer; Aaron Brown, University of California, Berkeley; E. Bugnion, Compaq’s Western Research Lab; Alper Buyuktosunoglu, University of Rochester; Mark Callaghan; Jason F. Cantin; Paul Carrick; Chen-Chung Chang; Lei Chen, University of Rochester; Pete Chen; Nhan Chu; Doug Clark, Princeton University; Bob Cmelik; John Crawford; Zarka Cvetanovic; Mike Dahlin, University of Texas, Austin; Merrick Darley; the staff of the DEC Western Research Laboratory; John DeRosa; Lloyd Dickman; J. Ding; Susan Eggers, University of Washington; Wael El-Essawy, University of Rochester; Patty Enriquez, Mills; Milos Ercegovac; Robert Garner; K. Gharachorloo, Compaq’s Western Research Lab; Garth Gibson; Ronald Greenberg; Ben Hao; John Henning, Compaq; Mark Hill, University of WisconsinMadison; Danny Hillis; David Hodges; Urs H€olzle, Google; David Hough; Ed Hudson; Chris Hughes, University of Illinois at Urbana-Champaign; Mark Johnson; Lewis Jordan; Norm Jouppi; William Kahan; Randy Katz; Ed Kelly; Richard Kessler; Les Kohn; John Kowaleski, Compaq Computer Corp; Dan Lambright; Gary Lauterbach, Sun Microsystems; Corinna Lee; Ruby Lee; Don Lewine; Chao-Huang Lin; Paul Losleben, Defense Advanced Research Projects Agency; Yung-Hsiang Lu; Bob Lucas, Defense Advanced Research Projects Agency; Ken Lutz; Alan Mainwaring, Intel Berkeley Research Labs; Al Marston; Rich Martin, Rutgers; John Mashey; Luke McDowell; Sebastian Mirolo, Trimedia Corporation; Ravi Murthy; Biswadeep Nag; Lisa Noordergraaf, Sun Microsystems; Bob Parker, Defense Advanced Research Projects Agency; Vern Paxson, Center for Internet Research; Lawrence Prince; Steven Przybylski; Mark Pullen, Defense Advanced Research Projects Agency; Chris Rowen; Margaret Rowland; Greg Semeraro, University of Rochester; Bill Shannon; Behrooz Shirazi; Robert Shomler; Jim Slager; Mark Smotherman, Clemson University; the SMT research group at the University of Washington; Steve Squires, Defense Advanced Research Projects Agency; Ajay Sreekanth; Darren Staples; Charles Stapper; Jorge Stolfi; Peter Stoll; the students at Stanford and Berkeley who endured our first attempts at creating this book; Bob Supnik; Steve Swanson; Paul Taysom; Shreekant Thakkar; Alexander Thomasian, New Jersey Institute of Technology; John Toole, Defense Advanced Research Projects Agency; Kees A. Vissers, Trimedia Corporation; Willa Walker; David Weaver; Ric Wheeler, EMC; Maurice Wilkes; Richard Zimmerman.

## Index

> ＃＃ 指数

1. [Introduction](#introduction) 2

> 1. [简介](%EF%BC%83%E7%AE%80%E4%BB%8B) 2

2. [Classes of Computers](#classes-of-computers) 6

> 2. [计算机类](%EF%BC%83%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%B1%BB) 6

3. [Defining Computer Architecture](#defining-computer-architecture) 11

> 3. [定义计算机体系结构](%EF%BC%83%E5%AE%9A%E4%B9%89%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%9E%B6%E6%9E%84) 11

4. [Trends in Technology](#trends-in-technology) 18

> 4. [技术趋势](%EF%BC%83%E8%B6%8B%E5%8A%BF%E6%8A%80%E6%9C%AF) 18

5. [Trends in Power and Energy in Integrated Circuits](#trends-in-power-and-energy-in-integrated-circuits) 23

> 5. [综合电路中的功率和能量趋势](%EF%BC%83%E8%B6%8B%E5%8A%BF%E5%9C%A8%E5%8A%9F%E7%8E%87%E5%92%8C%E8%83%BD%E6%BA%90%E4%B8%AD%E7%9A%84%E7%94%B5%E8%B7%AF%E4%B8%AD) 23

6. [Trends in Cost](#trends-in-cost) 29

> 6. [成本趋势](%EF%BC%83%E6%88%90%E6%9C%AC%E8%B6%8B%E5%8A%BF) 29

7. [Dependability](#dependability) 36

> 7. [可靠性](%EF%BC%83%E5%8F%AF%E9%9D%A0%E6%80%A7) 36

8. [Measuring, Reporting, and Summarizing Performance](#measuring-reporting-and-summarizing-performance) 39

> 8. [测量，报告和汇总性能](%EF%BC%83%E6%B5%8B%E9%87%8F%E6%8A%A5%E5%91%8A%E5%92%8C%E5%A4%8F%E5%AD%A3%E5%8C%96%E8%A1%A8%E7%8E%B0) 39

9. [Quantitative Principles of Computer Design](#quantitative-principles-of-computer-design) 48

> 9. [计算机设计的定量原理](%EF%BC%83%E5%AE%9A%E9%87%8F%E5%8E%9F%E7%90%86%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%AE%BE%E8%AE%A1) 48

10. [Putting It All Together: Performance, Price, and Power](#_bookmark30) 55

> 10. [将所有内容放在一起：性能，价格和权力](#_bookmark30) 55

11. [Fallacies and Pitfalls](#_bookmark33) 58

> 11. [谬论和陷阱](#_bookmark33) 58

12. [Concluding Remarks](#concluding-remarks-1) 64

> 12. [结论言论](＃结论 remarks-1)64

13. [Historical Perspectives and References](#historical-perspectives-and-references) 67

> 13. [历史观点和参考](%EF%BC%83%E5%8E%86%E5%8F%B2%E8%A7%82%E5%AF%9F%E5%92%8C%E5%8F%82%E8%80%83) 67

© 2019 Elsevier Inc. All rights reserved.
