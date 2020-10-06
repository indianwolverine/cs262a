### What is the problem that is being solved?

Traditional operating systems combine modules which provide abstractions over the system hardware (which evolve at the rate of hardware), and modules which customize the user environment, such as the file system interface. This tight coupling makes it difficult to port these OS's to new hardware, evolving system services, and impose one operating system on the machine (without virtualization of course). Microkernel architectures seek to address these issues. 

### What are the key results?

The authors present the Mach microkernel, a kernel which interfaces directly with the underlying hardware and pushes system services such as the file system to userspace, and systems built on top of Mach. 

Because Mach handles the hardware and presents a simple interface to all other services, this allows operating systems running on top of Mach to evolve independently since they no longer have to worry about the specifics of hardware and can instead focus on providing programming abstractions to users. This leads to quite a few advantages:

1. Tailorability - Multiple OS's can run on Mach
2. Portability - OS code is independent of ISA specifics
3. Network accessibility - OS location is independent of clients
4. Multiprocessor and Multicomputer support
5. Security - small TCB

Mach supports the following features: tasks and threads, IPC via ports, virtual memory (memory object stores), system call redirection, device support, user multiprocessing and multicomputers.

For emulation of specific OS's, usually application instructions will trap to Mach, which will then transfer control to the emulation library in the application address space. This emulation library will then talk to say a Unix server which controls all the logic specific to Unix. In this way, OS's can be implemented on top of Mach while maintaining binary compatibility.

### What are some of the limitations and how might this work be improved?

While the idea of microkernels is attractive, it does come with a performance hit due to the IPC which must occur. When an OS is monolithic, it can make a lot of assumptions about internal mechanisms and thereby optimize performance, but with Mach, it becomes harder due to the overhead imposed by IPC. Nevertheless, Mach provides independence from the hardware, which is more valuable than performance. Eventually monolithic kernels will get too big to reason about (they already are), and the performance of a modular system such as Mach can be iteratively optimized.

### How might this work have long term impact?

Mach kernels form the basis for many OS's already such as pretty much all of Apple's OS's, which is enormous impact. More importantly, however, they present a new way of thinking about OS abstractions. Borrowing lessons from System R and the E2E argument, microkernels achieve hardware independence and form the narrow waist of the OS.
