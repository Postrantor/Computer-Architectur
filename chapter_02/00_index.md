1. [Introduction](#introduction-1) 78
2. [Memory Technology and Optimizations](#memory-technology-and-optimizations) 84
3. [Ten Advanced Optimizations of Cache Performance](#ten-advanced-optimizations-of-cache-performance) 94
4. [Virtual Memory and Virtual Machines](#virtual-memory-and-virtual-machines) 118
5. [Cross-Cutting Issues: The Design of Memory Hierarchies](#cross-cutting-issues-the-design-of-memory-hierarchies) 126
6. [Putting It All Together: Memory Hierarchies in the](#_bookmark71) [ARM Cortex-A53 and Intel Core i7 6700](#_bookmark71) 129
7. [Fallacies and Pitfalls](#_bookmark81) 142
8. [Concluding Remarks: Looking Ahead](#concluding-remarks-looking-ahead) 146
9. [Historical Perspectives and References](#historical-perspectives-and-references-1) 148

[Case Studies and Exercises by Norman P. Jouppi,](#case-studies-and-exercises-by-norman-p.-jouppi-rajeev-balasubramonian-naveen-muralimanohar-and-sheng-li) [Rajeev Balasubramonian, Naveen Muralimanohar,](#case-studies-and-exercises-by-norman-p.-jouppi-rajeev-balasubramonian-naveen-muralimanohar-and-sheng-li)

[and Sheng Li](#case-studies-and-exercises-by-norman-p.-jouppi-rajeev-balasubramonian-naveen-muralimanohar-and-sheng-li) 148

Ideally one would desire an indefinitely large memory capacity such that any particular… word would be immediately available… We are… forced to recognize the possibility of constructing a hierarchy of memories each of which has greater capacity than the preceding but which is less quickly accessible.

A. W. Burks, H. H. Goldstine, and J. von Neumann, _Preliminary Discussion of the Logical Design of an Electronic Computing Instrument_ (1946).

Computer Architecture. [https://doi.org/10.1016/B978-0-12-811905-1.00002-X](https://doi.org/10.1016/B978-0-12-811905-1.00002-X)

© 2019 Elsevier Inc. All rights reserved.
