### What is the problem that is being solved?

While MapReduce is great for batch processing of data, because of its architecture, it's not usable for iterative and interactive processing of data - two workloads which have been steadily become predominant in industry.

### What are the key results?

Spark introduces the idea of a resilient distributed dataset, which is essentially just a set of data which has a lineage attached to it. This lineage tells us how the data was produced using a set of operations from the original data on disk.

The concept of the RDD enables lazy data computation and resiliency (since lost data can simply be recomoputed using the lineage). It also enable iterative and interactive analysis by allowing users to cache RDDs in memory. Essentially, the key difference between MapReduce and Spark is that users can cache and operate on in memory dataset in an ad-hoc manner, which was impossible with MapReduce where the entire operation has to be defined beforehand.

Along with this architecture, Spark provides an expressive interface which consists of more than just map and reduce operations, which expands the scope of which applications can run on top of it. Furthermore, Spark ships with an interpreter which allows for interactive analysis, a big design flaw in the original MapReduce.

### What are some of the limitations and how might this work be improved?

There are any number of extensions one can think of given the core Spark architecture. For one, Spark was originally built on top of the Mesos cluster manager. It might be useful to be able to ship a bare-metal version of Spark which reduces the overhead of managing an orchestration system such as Mesos, but this would also drive up the complexity of the Spark codebase significantly. The authors mention this in the paper, but a SQL interface to Spark would be very useful to end users, as well as a streaming solution so that data can be lazily computed and operated upon, even in between completion of any step of the pipeline.

### How might this work have long term impact?

Spark is already seeing widespread adoption throughout industry, and as long as the workloads it is optimal for continue to flourish, Spark will continue to be a useful tool. However, we are already seeing shifts in the types of workloads that are prevalent, especially in the ML world, motivating the development of specialized tools such as Ray for RL workloads.