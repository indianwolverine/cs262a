### What is the problem that is being solved?

The ability to virtualize OS's on hypervisors efficiently (see Xen) opens up the possibility of migrating VMs across hypervisors. This could be useful for maintenence of hosts for example, because of the decoupling between hardware and software that VMMs provide. The key problem this paper addresses is that of live migration of VMs i.e. seamless migration of VMs which are actively running processes which require liveness while not disrupting other VMs running on the hypervisor. 

### What are the key results?

Previously developed migration mechanisms for VMs are described. Notably, previous work focused on either stopping a VM entirely, copying it over to the target VMM, and then resuming it or migrating a small subset of the VM to the target VMM and then faulting on pages still on the source VMM. Both of these approaches are suboptimal since they either cause significant downtime or degrade performance of the VM. 

The key idea presented by the authors is that only a certain subset of pages in a VM will be hot at any time - the writable working set. Given this, first all the cold pages can be migrated to the target VMM, with further colder pages being iteratively copied over until all we have left is the wws. At that point, the VM can be suspended, the remaining pages copied over, and the target VM started. This reduces performance degradation degradation for the VM since all pages are on the target host when the VM starts again, and it also reduces downtime by copying over colder pages first, thus reducing the length of the wws stop-and-copy phase.

The author's also describe mechanisms for migrating active network connections and disk resources, although they make a mojor assumption that disk resources are accessed over a NAS (disk is decoupled from local storage). 

### What are some of the limitations and how might this work be improved?

Because of the assumption of a NAS for disk access (which is actually reasonable now in clusters) the work has a significant limitation in migrating disk resources to the target host. This makes it very difficult for the system to work across wide area networks and in situations where this assumption does not hold. However, since disk is rarely accessed compared to memory, maybe a pull migration mechanism would make most sense here as well.

### How might this work have long term impact?

This work enabled the development of cluster orchestration software and efficiently managing software resources in cluster computing environments, which is something the authors actually hint at in the paper. Ideas from this paper form the basis for allowing systems like Kubernetes and Tupperware to function properly, allowing effective load balancing of software and maintenence of hardware, which leads to cost savings and less greater service availability. Moreover, the local disk migration complication the authors run into actually presages the concept of decoupling disk storage from compute, a major design decision prevalent in data centers today. 
