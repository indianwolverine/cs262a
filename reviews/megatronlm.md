### What is the problem that is being solved?

In the domain of Natural Language Processing (NLP), very large models are required to achieve good prediction results. The parameters for these models range in the billions and cannot fit into memory. A new strategy is required to efficiently train these models.

### What are the key results?

The authors present Megatron-LM, which, with a few modifications to PyTorch, supports training models with billions of parameters by exploiting model layer parallelism - specifically in transformer networks which are used for NLP predictions by models such as BERT and GPT-2. The key result here is that there are layers of transformer networks where computation can proceed on parts of the model in parallel across GPUs. The authors exploit this parallelism to enable training of very large models.

### What are some of the limitations and how might this work be improved?

This work is very specific to transformer networks. A more robust system would be able to automatically determine and exploit various opportunities for parallelism in various models. For example, a simple scheme would involve tracking dataflow through a model using dummy variables with versions. The transformations on these dummy variables could then be used to create a graph of dependencies, and by observing points in this graph where synchronization among nodes is not required, we could determine opportunities for parallelism.

### How might this work have long term impact?

This work is especially interesting now that GPT-3 has become available to researchers and performs much better than even the language models mentioned in this paper. This ongoing field of research obviously has a lot going on and it will be interesting to see what kinds of systems innovations result. This paper describes a modification which is highly specific to transformer and might not survive as models evolve. A system which can automatically determine model parallelism and optimize for it will be more useful.