### What is the problem that is being solved?

This paper seeks to evaluate the performance of VMs and containers, two different means of running IaaS systems in the cloud, on a variety of different workloads.

### What are the key results?

First a discussion of VMs and containers occurs - the main idea here is that VMs provide stronger isolation guarantees than containers, but have a greater associated overhead and a difficult to reason about two tiered scheduling system (Guest OS as well as Host OS running the hypervisor). Containers provide namespaced resources in a single OS, but isolation is a bit weaker due to the Linux syscall interface and processes running inside containers are able to see all the hardware of the system, which is not ideal is the process is performance tuning based on these observations.

The paper describes a series of benchmarks, including real applications such as MySQL and Redis, using which it tests the performance of containers and VMs. The essential finding here is that both containers and VMs achieve near native performance on CPU and memory bound workloads, but due to context switching overhead and certain implementation peculiarities, I/O bound workloads experience much higher latency and less throughput than native configurations.

### What are some of the limitations and how might this work be improved?

This work seems less a true comparison of containers vs. VMs and more of just a comparison of Docker vs. KVM. It's not enough to say that Docker and KVM are the representative softwares of their class. The authors in fact demonstrate that Docker with NAT enabled suffered reduced performance, worse than KVM even. However, the use of NAT by Docker is something specific to Docker itself and not a fundamental limitation of containers. Perhaps the real lesson here is that both VMs and containers are overkill technologies which just add more complexity to an already complex system. Both technologies are attempting to provide abstractions which aren't provided in current operating systems, but are doing this by simply bolting on more layers of abstraction. They do address important problems in the current software ecosystem, but a cleaner, more sustainable solution would simply be to architect an operating system which provides strong namespace guarantees for processes.

### How might this work have long term impact?

Maybe this work could be used by enterprises to inform decisions of whether to use containers or VMs for certain workloads. However, with the constantly evolving state of the underlying container and VM software, I don't think this paper presents any findings which are truly fundamental.
