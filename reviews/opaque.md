### What is the problem that is being solved?

Big data workloads in the cloud often handle lots of sensitive information and currently, even with hardware enclaves, there is no way to secure these workloads without adversely affecting performance. Importantly, even hardware enclaves leak access patterns at the network and memory level. A new framework is needed which provides stronger security guarantees in the cloud.

### What are the key results?

The authors present Opaque, an oblivious distributed data analytics platform which hides access pattern leakage. Opaque does this by providing distributed relation operators which protect against access pattern leakage as well as query planning for rules and costs. Opaque can be run in the following modes:

1. encryption mode - guarantees data encryption and authentication as well as correct execution
2. oblivious mode - additionally protects against access pattern leakage
3. oblivious pad mode - additionally protects against size leakage

To support the encryption mode, common data encryption techniques are used to provide the guarantees. To support the oblivious modes, Opaque develops a set of oblivious operators:

1. oblivious sort
2. oblivious aggregate
3. oblivious filter
4. oblivious sort-merge join

Using these oblivious primitives, Opaque then introduces the idea of a cost model associated with oblivious operators during query planning. Using this novel cost model, Opaque is able to optimize query plans using oblivious operators.

To account for mixed sensitivity i.e. computations where at least one table is sensitive and at least one isn't, Opaque borrows ideas of tainting from the information flow model. This prevents sensitive information from inadvertantly being leaked via an unprivileged table operation.

### What are some of the limitations and how might this work be improved?

Obviously, there are the performance hits that the system takes due to the security concerns, but that shouldn't really be a problem as long as the sensitive datasets are small. More importantly, there is a tight coupling between Opaque and Spark SQL, so it remains to be seen whether the work can be replicated on other distributed data analytics platforms or whether Spark provides guarantees which are necessary for the correct operation of Opaque.

### How might this work have long term impact?

This work has the potential to be immensely impactful if performance degradation can be improved and if industry finds value in protecting certain sensitive information. Certainly this seems to be the case since hardware enclaves are already widely available, and Opaque only improves on their usage by providing even stronger security guarantees.