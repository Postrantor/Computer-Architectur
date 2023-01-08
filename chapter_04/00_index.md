This page intentionally left blank

1. [Introduction](#introduction-2) 282
2. [Vector Architecture](#vector-architecture) 283
3. [SIMD Instruction Set Extensions for
   Multimedia](#simd-instruction-set-extensions-for-multimedia) 304
4. [Graphics Processing Units](#graphics-processing-units) 310
5. [Detecting and Enhancing Loop-Level
   Parallelism](#detecting-and-enhancing-loop-level-parallelism) 336
6. [Cross-Cutting Issues](#cross-cutting-issues-1) 345
7. [Putting It All Together: Embedded Versus Server
   GPUs](#_bookmark198)
   [and Tesla Versus Core i7](#_bookmark198) 346
8. [Fallacies and Pitfalls](#_bookmark204) 353
9. [Concluding Remarks](#concluding-remarks-2) 357
10. [Historical Perspective and
    References](#historical-perspective-and-references-1) 357
    [Case Study and Exercises by Jason D. Bakos](#case-study-and-exercises-by-jason-d.-bakos) 357

We call these algorithms _data parallel_ algorithms because their parallelism comes from simultaneous operations across large sets of data rather than from multiple threads of control.

W. Daniel Hillis and Guy L. Steele,

*“*Data parallel algorithms,_” Commun. ACM_ (1986)

If you were plowing a field, which would you rather use: two strong oxen or 1024 chickens?

Seymour Cray, Father of the Supercomputer _(arguing for two powerful vector processors versus many simple processors)_

Computer Architecture. [https://doi.org/10.1016/B978-0-12-811905-1.00004-3](https://doi.org/10.1016/B978-0-12-811905-1.00004-3)

© 2019 Elsevier Inc. All rights reserved.
