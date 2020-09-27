### What is the problem that is being solved?

The unique scale of Google workloads presents a data processing challenge: relatively simple computations must be distibuted across multiple machines to run in a reasonable amount of time. With this distributed system come all the problems of parallelization, failure, and fault-tolerance which application programmers now also have to write in order to get a simple computation to run.

### What are the key results?

The author's present a simple programming model in which computations must be divided into map and reduce phases - similar to the synonymous functions in functional programming models. A user simply has to specify a computation using these primitives, and the underlying MapReduce system takes care of the distributed system executing the computation.

A master node is designated for each MapReduce operation. This master node sets up a set of worker nodes which operate on the input data partitioned across M sets. The master node tries to schedule partitions on workers such that the data is close to the worker nodes due to network bandwidth bottlenecks. Once the workers complete the map operation, they write the data to local disk. From here the data is streamed out to one of the set of R reduce workers given a partition key. The reduce workers then operate on the data and store the result in a set of R output files specified by the user in the Google File System.

There are a series of optimizations and important design decisions that the author's make. For example, to mitigate against straggling worker nodes, once a certain amount of tasks are done, backup tasks are started for all tasks in progress to increase the probability that at least one of the tasks will finish fast. In addition, there is a mechanism for flagging bad records which cause the system to crash so that workers can skip over these dynamically. These mitigations are important in a system of such scale since tail latency can be high with so many worker nodes.

The system also responds well to failures - if a worker fails, the master node can detect it (because the worker stops sending heartbeats) and can start the tasks mapped to the worker in another worker node; if the master fails, a new master can be spun up using checkpointing data the master periodically writes.

### What are some of the limitations and how might this work be improved?

Why aren't a subset of Map workers resused as Reduce workers to lower the network latency? In the case that data doesn't really need to be shuffled much to different partitions, it may be advantageous to simply designate a map worker as a reduce worker next. How does the design change with greater available network bandwidth? The author's operate in an environment where network bandwidth is the bottleneck, which leads one to wonder how this model would change if this constraint were relaxed. Is there a mechanism to test a small batch of data on a small set of machines automatically to ensure correctness before accidently wasting thousands of machines on a faulty computation? This would be a useful facility to provide end users.

Finally, MapReduce is constrained to batch workloads - how can we modify the model to allow interactive streaming queries?

### How might this work have long term impact?

By restricting the programming model to a small set of primitives (see the Unix syscall interface), while providing guarantees of fault-tolerance, correct distributed execution, and efficiency, the MapReduce framework is a beautiful design which again demonstrates that good designs come from small, well-defined interfaces. Clearly the framework has been immensely successful, being widely adopted at companies everywhere and leading to the development of similar execution models such as Spark in the future.