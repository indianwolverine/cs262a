### What is the problem that is being solved?

Training large scale machine learning models itself is a challenging systems issue. How do you map the computation to system resources - moreover, what happens when we want to use a distributed cluster of servers each with heterogeneous resources for model training? There is a need for a distributed system for ML model training which can deal with heterogeneity.

### What are the key results?

Google's previous work in this area, DistBelief, has a few drawbacks which necessitated the development of TF. DistBelief is based on the paramater server model, where the server is used to get the parameters for a model, these are fed through a model, and then outputs are written back to the server. It becomes difficult to use this system when building new types of models, refining currently existing ones with novel architectures, and there is a network bandwidth bottleneck due to multiple updates to the parameter server.

At a high level, TF allows the user to define a dataflow graph, with nodes as mathematical operators and mutable state flowing as tensors between nodes. Given a user script, TF first compiles it into a dataflow graph, optimizes the graph, then maps the graph to nodes in the resource pool. A subset of nodes are responsible for parameter values, these are called PS nodes, while the rest simply execute operations on sections of the graph.

TF can concurrently execute subgraphs of the dataflow graph at a time, passing tensors from one subgraph to another through the use of queues. This is all done asynchronously, and can often only provide weak consistency, which is enough for most models.

Users can explicitly specify resource requirements and mappings of tasks to resources (run this step on a GPU e.g.) to manually optimize their models. Each operation defined in TF has it's own optimized implementation for different hardware (CPU, GPU, TPU).

Fault tolerance is not needed as such since many models function fine even when reading stale inputs, but TF does provide support for a synchronous model of execution with straggler mitigation, which allows faster training due to computations being on fresh parameters.

### What are some of the limitations and how might this work be improved?

It remains to be seen whether TF can automatically adjust task to resource mappings for optimal memory, compute, and I/O. Clearly, in a large model which requires expensive training, these optimizations will become important. Perhaps TF could even learn a better placement through repeated iterations of training a model?

### How might this work have long term impact?

TF has already had a massive impact in accelerating large scale ML research pretty much everywhere. It's pretty much an industry standard at this point - which goes to show how well designed and well implemented systems can spur innovation. It's likely to see some competition as new ML workloads emerge which have different execution needs than a static dataflow representation can provide, so this will be an interesting thread to follow.