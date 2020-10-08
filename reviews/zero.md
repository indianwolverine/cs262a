### What is the problem that is being solved?

Similar problem space to Megatron-LM, essentially we need to support training deep learning models with trillions of parameters. They point out early that the Megetron-LM model parallelism idea doesn't scale beyond one node since it depends on efficient communication between GPUs on the same server.

### What are the key results?

The authors present ZeRO, a new optimizer for memory efficiency in large language models. They point out two types of states which consume memory:

1. Model states - optimizer states, gradients, parameters (things we probably want to keep around)
2. Residual states - temporary buffer and fragmented memory used during computation

To optimize model states, ZeRO partitions everything which constitutes model state across parallel processes observing that not all model states are needed at each step of the model training. For residual states, a variety of strategies are employed including removing activiation replication, defining an appropriate size for temp buffers, and cleaning up fragmented memory.

Essentially, these optimizations allow ZeRO to parallelize training effectively across several nodes with linear increases in trainable model size as the number of nodes increases.

### What are some of the limitations and how might this work be improved?

This work is very specific to transformer networks. A more robust system would be able to automatically determine and exploit various opportunities for parallelism in various models. For example, a simple scheme would involve tracking dataflow through a model using dummy variables with versions. The transformations on these dummy variables could then be used to create a graph of dependencies, and by observing points in this graph where synchronization among nodes is not required, we could determine opportunities for parallelism.

### How might this work have long term impact?

This work is especially interesting now that GPT-3 has become available to researchers and performs much better than even the language models mentioned in this paper. This ongoing field of research obviously has a lot going on and it will be interesting to see what kinds of systems innovations result. This paper describes a modification which is highly specific to transformer and might not survive as models evolve. A system which can automatically determine model parallelism and optimize for it will be more useful.