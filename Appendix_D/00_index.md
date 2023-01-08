# D Storage Systems

1. Introduction [D-2](#introduction-9)
2. Advanced Topics in Disk Storage
   [D-2](#advanced-topics-in-disk-storage)
3. Definition and Examples of Real Faults and Failures
   [D-10](#definition-and-examples-of-real-faults-and-failures)
4. I/O Performance, Reliability Measures, and Benchmarks
   [D-15](#io-performance-reliability-measures-and-benchmarks)
5. A Little Queuing Theory [D-23](#a-little-queuing-theory)
6. Crosscutting Issues [D-34](#crosscutting-issues)
7. Designing and Evaluating an I/O System—The
   Internet Archive Cluster [D-36](#designing-and-evaluating-an-io-system-the-internet-archive-cluster)
8. Putting It All Together: NetApp FAS6000 Filer [D-41](#_bookmark561)
9. Fallacies and Pitfalls [D-43](#_bookmark562)
10. Concluding Remarks [D-47](#concluding-remarks-9)
11. Historical Perspective and References
    [D-48](#historical-perspective-and-references-5) Case Studies with
    Exercises by Andrea C. Arpaci-Dusseau
    and Remzi H. Arpaci-Dusseau [D-48](#case-studies-with-exercises-by-andrea-c.-arpaci-dusseau-and-remzi-h.-arpaci-dusseau)

I think Silicon Valley was misnamed. If you look back at the dollars shipped in products in the last decade, there has been more revenue from magnetic disks thanfrom silicon. They ought torename the place Iron Oxide Valley.

Al Hoagland

_A pioneer of magnetic disks (1982)_

Combining bandwidth and storage … enables swift and reliable access to the ever expanding troves of content on the proliferating disks and

… repositories of the Internet … the capacity of storage arrays of all kinds is rocketing ahead of the advance of computer performance.

George Gilder _“The End Is Drawing Nigh,” Forbes ASAP (April 4, 2000)_
