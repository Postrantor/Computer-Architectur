# G Vector Processors in More Depth

1. Introduction [G-2](#introduction-12)
2. Vector Performance in More Depth [G-2](#vector-performance-in-more-depth)
3. Vector Memory Systems in More Depth [G-9](#vector-memory-systems-in-more-depth)
4. Enhancing Vector Performance [G-11](#enhancing-vector-performance)
5. Effectiveness of Compiler Vectorization [G-14](#effectiveness-of-compiler-vectorization)
6. Putting It All Together: Performance of Vector Processors [G-15](#_bookmark695)
7. A Modern Vector Supercomputer: The Cray X1 [G-21](#a-modern-vector-supercomputer-the-cray-x1)
8. Concluding Remarks [G-25](#concluding-remarks-12)
9. Historical Perspective and References [G-26](#historical-perspective-and-references-7)

Exercises [G-29](#exercises-9) Revised by Krste Asanovic Massachusetts Institute of Technology

I’m certainly not inventing vector processors. There are three kinds that I know of existing today. They are represented by the Illiac-IV, the (CDC) Star processor, and the TI (ASC) processor. Those three were all pioneering processors.…One of the problems of being a pioneer is you always make mistakes and I never, never want to be a pioneer. It’s always best to come second when you can look at the mistakes the pioneers made.

> 我当然不是在发明矢量处理器。我知道今天存在三种。它们的代表是 Illiac-IV、(CDC) Star 处理器和 TI (ASC) 处理器。那三个都是开创性的处理器。......成为先驱者的一个问题是你总是犯错误，而我永远，永远不想成为先驱者。当您可以看到先驱者所犯的错误时，最好排在第二位。

Seymour Cray
_Public lecture at Lawrence Livermore Laboratorieson on the_
_introduction of the Cray-1 (1976)_
