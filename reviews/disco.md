### What is the problem that is being solved?

With the introduction of new multiprocessor systems with cache coherent NUMA architectures came the problem of adapting previously uniprocessor operating systems to this new hardware. This is difficult because modification of existing OS's takes significant implementation time and efforts, not to mention that different application workloads may require different OS support.

### What are the key results?

The authors introduce Disco, a virtual machine monitor which sits between the raw hardware of the machine and a set of virtual machines running on top of it which can each support a separate commodity OS. This allows any OS developed to run on top of Disco, even if it doesn't know how to handle a multprocessor system or the NUMA architecture - this in turn reduces development effort and leads to less buggy code.

Disco tackles several problems which had previously plagued virtual machines: 

1. Overhead of handling privileged instructions, I/O, and replicated memory in each OS
2. Effective resource management without knowledge of higher level OS semantics
3. Communication and sharing of data between VMs

Disco handles overhead by assigning each VM a virtual CPU which runs in supervisor mode (one level above kernel mode). Most instructions execute directly on hardware, but privileged instructions are trapped to Disco, which can then handle them appropriately. By interposing between disk and the VMs, Disco is also able to make good decisions regarding data replication across processors as well as dynamic page migration to the processor which is using it most.

Disco also manages memory transparently by maintaining mappings from VA to PA to MA for each VM. Using this simple mapping it can then provide a series of nice abstractions, for example to share a page between VMs, it simply maps this page in the physical address space for each VM. Similar tricks are used to allow communication between VMs (through a virtual subnet).

### What are some of the limitations and how might this work be improved?

It's not clear that it's necessary to provide such a broad interface to hardware for each OS running on top of Disco. Since Disco here is essentially acting as a more minimal operating system layer under several other OS's, maybe it makes sense for Disco to provide a subset of functionality and then require guest OS's to adhere to this interface. On the other hand, this approach would definitely increase the development overhead necessary to run an OS on Disco.

### How might this work have long term impact?

Since this work led directly to the foundation of VMWare and a new era of virtualization in computing systems (see Amazon EC2), it's clear that this work was immensely impactful. The developmental burden that existed before technologies such as Disco were a significant blocker to innovation, and VMMs allowed creation of OS's which ran on top of virtualized hardware. This led to a host of other features such as the ability to transparently migrate VM's across VMM's and provide security and isolation for VM's running in a multi-tenant environment.