### What is the problem that is being solved?

Kernels are the most privileged software running on a system and as such form the TCB of the system. If a kernel exhibits undefined or unsafe behavior, then this can be catastrophic for the entire system. Formal verification of a kernel implementation based on a high level spec can provide guarantees regarding safety. 

### What are the key results?

The authors present seL4, a microkernel which has been formally verified. Essentially, the kernel is first specified in a high level, abstract spec. This spec is implemented in Haskell and formally verified using invariants and Hoare logic techiniques. Finally, once the Haskell implementation has been verified, it is translated by hand to C code. Finally, this C code is verified, completing the formal verification.

### What are some of the limitations and how might this work be improved?

There is a major caveat to the formal verification, which goes back to Ken Thompson's "Refelctions on Trusting Trust" i.e. the proof assumes that the compiler, assembly code, boot code, cache management, and hardware is all correct! Without this, the formal verification doesn't mean much. In addition, seL4 is a uniprocessor kernel and given that formal verification becomes much harder in a multiprocessor model, how far is anyone willing to take this?

### How might this work have long term impact?

seL4 has seen use, specifically in secure hardware enclaves where the properties it provides are needs. However, formal verification is not a feasible technique outside of small codebases and relatively simple designs, especially when a lot of shared state is involved.
