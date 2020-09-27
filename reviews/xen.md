### What is the problem that is being solved?

Given powerful hardware, there needs to exist a mechanism to effectively partition this hardware for multiple users/customer (think multiple unrelated users using the same underlying AWS server) while providing performance and isolation guarantees to each user. Having all processes from many users running in the same OS provides neither performance nor isolation guarantees, running user tasks in separate tasks may provide some degree of isolation, but due to conflicting resource needs, does not guarantee performance. The authors seek to build a VMM upon which 100's of VM's can be run and provide both performance and isolation guarantees to guest OS's. 

### What are the key results?

Instead of supporting full virtualization, Xen acts as a paravirtualized system. In this scheme, guest OS's must be modified to use the Xen virtualized hardware interface, but for many operations they still have direct or quick access to hardware with limited or no context switching overhead with Xen. 

For memory management, Xen makes Guest OS's manage their own hardware page tabiles, with updates batched to reduce overhead of context switching to Xen. 

To speed up interrupts, Xen allows the Guest OS to install custom syscall handlers to avoid going through Xen, but page faults, being privileged instructions, still require check by Xen to ensure safety.

Device I/O is exposed through a custom Xen driver which writes puts all Guest OS writes on a shared-memory ring buffer, again allowing the OS to skip Xen for writing to devices and by installing custom interrupt handlers.

CPU and timer are virtualized as well, with CPU multiplexed among VM's using the Borrowed Virtual Time algorithm. Virtual memory management is simple in Xen as well, as a Guest OS only has to go through Xen for page table writes and Xen simply validates the write.

Physical memory is statically at VM instantiation, which makes management very simple. Furthermore, Guest OS's can read physical to hardware page mappings to optimize memory access. 

### What are some of the limitations and how might this work be improved?

Since disk is exposed to Guest OS's through virtual block devices and writes to disk can be reordered and optimized internally by Xen, it may be harder to provide performance guarantees for disk accesses. For example, a Guest OS running a log-structured file system requires what it assumes to be sequential disk writes to actually be sequential to meet it's performance guarantees. Much like Xen allows transparent access of memory to Guest OS's, couldn't a similar mechanism be applied to disk?

### How might this work have long term impact?

One of the many important things Xen accomplishes is that due to the strict division between resources used by each VM, the system is able to control QoS at a fine granularity. This is very useful for enterprises such as AWS which need to account for resource consumption to bill customers. Another important design consideration in Xen is that VM's may belong to separate untrusted users - which is the primary assumption in a cloud computing platform, and clearly Xen has had a massive influence on how cloud computing works today (see EC2 for example). Moreover, Xen displays some wisdom from the E2E principle, by trying to do only those tasks at the VMM level which can be done completely at that level, and providing a simple service which doesn't get in the way of higher level applications, but only provides certain transparent guarantees while seeking to give the Guest OS's as much control of the hardware as possible.
