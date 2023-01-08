# A Instruction Set Principles

<!-- -->

1. [Introduction](#introduction-6) A-2
2. [Classifying Instruction Set
   Architectures](#classifying-instruction-set-architectures) A-3
3. [Memory Addressing](#memory-addressing) A-7
4. [Type and Size of Operands](#type-and-size-of-operands) A-13
5. [Operations in the Instruction
   Set](#operations-in-the-instruction-set) A-15
6. [Instructions for Control Flow](#instructions-for-control-flow) A-16
7. [Encoding an Instruction Set](#encoding-an-instruction-set) A-21
8. [Cross-Cutting Issues: The Role of
   Compilers](#cross-cutting-issues-the-role-of-compilers) A-24
9. [Putting It All Together: The RISC-V
   Architecture](#putting-it-all-together-the-risc-v-architecture) A-33
10. [Fallacies and Pitfalls](#_bookmark431) A-42
11. [Concluding Remarks](#concluding-remarks-6) A-46
12. [Historical Perspective and
    References](#historical-perspective-and-references-2) A-47 Exercises
    by Gregory D. Peterson A-47
    A _n_ Add the number in storage location _n_ into the accumulator.

E _n_ If the number in the accumulator is greater than or equal to zero execute next the order which stands in storage location _n_; otherwise proceed serially.

Z Stop the machine and ring the warning bell.

Wilkes and Renwick, _Selection from the List of 18 Machine Instructions for the EDSAC (1949)_
