### What is the problem that is being solved?

We've read about microkernels, but do they go far enough? With microkernels, we still have IPC and virtual memory implemented by the kernel, and it can be argued that even these are not extensible enough and stand in the way of application performance. What is the bare minimum abstraction that a kernel can provide, and what does that kernel look like?

### What are the key results?

The authors present Exokernel, a minimal kernel which merely multiplexes hardware resources among untrusted library OS's running on top of it. The point here is to separate protection of resources from the management of these resources.

Motivating exokernels is the E2E principle - traditional kernels provide all sorts of abstractions in very specific ways that not every application needs, so all applications end up paying the cost of these abstractions even if they don't use them.

The exokernel exposes raw hardware attributes such as the TLB, page numbers, and devices directly to applications, but enforces allocation and revocation of these resources from applications by partitioning the hardware among applications. In this way, the exokernel approach is for a single computer what Mesos is for the cluster!

Much like SPIN, exokernel allows application code written in a type-safe language to be installed directly in the kernel to handle hardware events such as incoming packets.

Importantly, exokernels are very effiecient, secure, do everything a traditional OS can (with the help of LibOS's), and allow a large degree of extensibility to applications. 

### What are some of the limitations and how might this work be improved?

In the age of cluster computing where performance can be increased often by throwing more resources at the problem, it seems that no one cares about basic OS research and changing the fundamental assumptions of OS design. I don't think there are any substantial limitations in this work, but I think exokernels in general have not taken off due to intertia in the industry, coupled with terrible development practices and lazy developers who don't know anything about their system beyond a high level API.

### How might this work have long term impact?

Exokernels haven't really been successful in the wild, and this is probably due to competing visions of VMs, containers, and microkernels. For example, if lazy developers can just push their code into a VM and ship it off to the cloud where there is access to boundless resources, who cares about optimizing the OS for performance and extensibility anyway? There isn't really a huge ecosystem around exokernels either, and the idea of thinking about all the abstractions that a traditional OS provides for free for each application can be daunting, even when using a LibOS. Maybe what's needed in this area is a proof of concept LibOS which current applications can run on top of with similar or increased performace compared to current OS's, with ports of other common applications showing marked improvements in performance and extensibility when ported to an exokernel.