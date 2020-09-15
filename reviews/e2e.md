### What is the problem that is being solved?

This paper proposes a design principle which can be used to determine where to place certain functionality in software. Specifically, some of the problems in response to which this paper was written include encryption, duplicate message detection, message sequencing, guaranteed message delivery over a data communication network etc.

### What are the key results?

Taking the example of reliable file transfer across a network, the authors argue that not only can the network not ensure that a file is reliably received by a target host, but also that this functionality is very difficult to implement in the network. Moreover, implementing this functionality in the network imposes a performance burden to applications relying on the network which do not require reliable delivery. Finally, even if the network guarantees reliable delivery, the end hosts must check that the file was reliably transmitted by mechanisms at the application layer since any number of things can go wrong before and after the network transfer phase. Thus, implementing something like reliable message delivery is not sufficient for reliable delivery semantics at the application level, and this functionality should instead be implemented at the application level, with any implementation of functionality in the network layer treated as a performance enhancement rather than a guarantee.

### What are some of the limitations and how might this work be improved?

One argument against a general approach like this is that the layers of architecture can lead to increased complexity in the system such that each application using the network has to implement its own reliability mechanisms. In a distributed application where one doesn't have control over all the components, this idea makes sense. However, the paradigm might change in a situation where we have highly specialized applications running on a network which we control completely, such as the internal network of a data center. At that point, it might be worth the effort to implement some functions in the network which would make writing certain types of applications simpler.

### How might this work have long term impact?

For the most part, this paper defines how computer networks are constructed now, with the basic idea that functionality that cannot be implemented in the network is pushed out to end hosts and implemented in the network only as a performance enhancement.