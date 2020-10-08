### What is the problem that is being solved?

Traditional kernels aren't very secure, and frankly, aren't really built with security as a priority. Role based access controls provided by these controls are usually not enough to prevent information leakage and system compromise. If there was a small TCB in the form of a kernel, applications running on top of this kernel could then be untrusted but we could still guarantee security - Histar explains how.

### What are the key results?

The authors present Histar, a new kernel which provides strict information flow control, presenting a Unix-like environment to applications, with decent performance. Instead of focusing on access control, Histar focuses on securing information flow i.e. ensuring that sensitive information does not make its way to an unprivileged entity by tracking information flow in the system. 

All OS abstractions are layered on top of 6 low level kernel objects:

1. threads
2. address spaces
3. segments
4. gates
5. containers
6. devices

Every object has a label associated with it which specifies if it has untainting privileges, and if not, what level of taint it has. In general, objects with higher taint cannot write to lower taint objects, and lower taint objects cannot read from a higher taint object.

This system is useful for restricting access to resources. For example, a user can define two categories, one for read access to their files and one for write access and then use taints to specify what other applications can do with the files with respect to reads and writes.

### What are some of the limitations and how might this work be improved?

While the work on creating a secure microkernel is useful, unfortunately there exist well understood hardware level attacks which nevertheless can compromise the system. Without enough knowledge of hardware, it's hard to say how to prevent those attacks well.

### How might this work have long term impact?

This work, like most security work in systems, in likely not to have much impact - people are willing to sacrifice security to some degree if it guarantees much better performance. Moreover, most of the time, software security is good enough, and the effort that goes into developing and then maintaining a system such as Histar simply requires too much operational knowledge to work at scale. This type of security is really only useful and necessary at the nation-state level, and at that level there are likely to be many more physical security barriers which function much better than software security barriers can. If I have an airgapped machine sitting in a vault guarded by the military, this machine will be much more secure than a network connected Histar machine no matter what OS the airgapped machine runs and no matter how well engineered Histar is.