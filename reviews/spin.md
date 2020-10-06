### What is the problem that is being solved?

Operating systems often expose a set interface to applications, which is useful in a general sense as many applications are able to use this interface to accomplish their goals. However, for performance, certain applications such as media, databases, and parallel computing require specialized semantics in order to function. Their performance suffers when having to wrestle the underlying OS mechanisms for memory management, scheduling etc. A more extensible OS environment is needed to support applications which want full control over their own memory, processor, and I/O needs.

### What are the key results?

The authors present SPIN, an extensible microkernel which gives applications control of the machine's logical and physical resources through runtime adaptations of the microkernel - much like eBPF in Linux now.

Essentially, applications can install bits of code called spindles right into the kernel as handlers which will be invoked in response to certain hardware events, say a page fault or incoming packet from the network. Since these spindles are part of the kernel, they eliminate the context switching overhead of event handling and allow applications to define custom methods of dealing with interrupts. These spindles are compiled and statically analyzed to make sure they meet the integrity checks for running in kernelspace. 

The spindle concept enables a host of optimizations for applications. For example, an application can install a spindle which is invoked in response to a scheduling event. This would enable the application to select the best application thread to run next, which is helpful for synchronization among several threads.

### What are some of the limitations and how might this work be improved?

There is quite a bit of overhead in restructuring applications to take advantage of the extensible microkernel approach. There are several competing ideas in the same space, such as VMs, containers etc. which offer different tradeoffs. I'm guessing what happened here was that people found it easier to just toss their applications into a VM or container and not worry about writing it in a more efficient way to take advantage of kernel extensibility, while waiting for processors to speed up and provide the performance boost.

### How might this work have long term impact?

This work has actually had impact in Linux, a monolithic kernel! For example, the eBPF interface exposed by the Linux kernel closely resembles the spindle idea, and lots of people are using eBPF because it offers extensibility in the kernel. The idea of an extensible microkernel is definitely interesting, but it probably doesn't have that much traction now just because systems like SPIN didn't have a lot of active maintainers to keep the project going. But clearly people find the extensibility features useful, as eveidenced by eBPF.