### What is the problem that is being solved?

The paper argues that emerging reinforcement learning (RL) workloads demand a new framework to unify model training, simulation, and serving, which are parts unique to RL applications. Frameworks exist which can accomplish 1 or even 2 of these stages individually, but the case is made that due to tight coupling among all three stages, it's not viable to chain together existing systems, and that researchers waste a lot of effort right now building distributed RL systems from scratch. Ray is meant to abstract away the infrastructure needs of RL workloads, providing a single framework for training, simulation, and testing.

### What are the key results?

To accomplish the goal of supporting RL applications, Ray exposes a simple interface with asynchronous functions which return futures as well as synchronous functions to collect results to computations. 

Computation is subdivided into stateless tasks (which may be used for example for model simulation) and stateful actors (which may be used for model training). Internally, Ray creates a task graphs with directed edges from dependencies to their dependent tasks or actors. 

To support this computation model, Ray employs a set of components in a highly scalable architecture. Drivers and Workers run on nodes and form the application layer - the core work happens here (tasks and actors). On each node there is a local object store containing inputs and outputs for tasks and actors executing on the node. This object store exists in shared memory so that all executing computation on a node can access it. Furthermore, if data is on another node, it is first replicated to the local node before execution begins. 

A local scheduler picks up tasks from the driver and workers and either tries to schedule these locally or offloads them to the global scheduler via the 
Global Control Store (GCS). The GCS acts as a sharded key-value store storing all object metadata. In fact the global scheduler is also sharded, allowing all components of the system to scale horizontally.

### What are some of the limitations and how might this work be improved?

It's not clear in the paper how exactly the global scheduler and GCS are sharded. How is this sharding balanced and how does it scale up/down in response to workload needs? Furthermore, how does infrastructure provisioning work? Is Ray integrated into a cluster orchestration system or does it run bare metal? What kinds of tradeoffs are important in these scenarios? For example, managing infrastructure of the scale Ray is built for is no easy task. I'm guessing it follows a serverless model contacting cloud providers directly, but there should be an easy way to integrate Ray on top of cluster orchestration systems to forego the overhead of effectively managing all the infrastructure resources.

### How might this work have long term impact?

There are many important design decisions made here that may be useful in other systems. For example, the decoupling of the global state from the global scheduler as well as the sharding of both of these components is a great way to design scalable, fault-tolerant distributed systems. In addition, the bottom-up scheduling, making local decisions before and only offloading if not enough information or resources are available is something that can probably be applied to a cluster orchestration system such as Kubernetes. In addition to all this, there is the obvious impact of this work on RL applications. However, I would argue that the task and actor abstractions along with the task graph representation could be useful for any number of applications. In short - design decisions made in Ray could be widely adopted in other systems, and Ray itself could be used as a system for heterogeneous workloads besides just RL. 
