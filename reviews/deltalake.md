### What is the problem that is being solved?

Cloud object stores provide a mechanism for storing vast amounts of data, which makes them ideal candidates for data warehouse and data lake applications. However, these object stores operate on eventually consistent semantics, and do not guarantee ACID properties. Moreover, due to the lack of a hierarchical structure in cloud object stores, metadata actions are not performant. The goal of this paper is to present a system which satisfies the needs of data warehouses and data lakes (lakehouses) by using object stores as a storage layer and adding a middle layer to guarantee ACID transactions.

### What are the key results?

The authors present Delta Lake, a storage format and set of protocols to implement ACID transactions over a cloud object store or distributed file system. The idea here is that cloud object stores provide cheap storage; moreover, many enterprises maintain separate data pipeline for OLTP and OLAP workloads, which involve many moving parts (read operational complexity) and duplication of data. Delta Lake is meant to address this problem by combining OLTP and OLAP functionality in one protocol + storage format, so that users can access all their data from a central store, for whatever custom data pipeline they require, without having to maintain separate pipeline infrastructure.

Delta Lake delivers on this vision by defining a format such that objects are written to the object store (partitioned by column), with a separate log and checkpoint mechanism which allows users to read different versions of objects. Each monotonically increasing log entry specifies a committed transaction, and all data is immutable - these design choices enable ACID guarantees as well as rollback, time travel etc. 

The authors also describe a series of features which they have implemented to help with typical workflows (Z-ordering for OLAP, streaming optimizations).

### What are some of the limitations and how might this work be improved?

Because Delta Lake is designed for cloud object stores, assumptions of eventual consistency and slow read and write latencies are baked into the protocol. Because of this, objects written to the object store must be large, which is difficult for ML workflows which try to store small objects. Perhaps there are either some design changes or backing storage changes which can enable smaller object read and writes to become performant.

### How might this work have long term impact?

The key design decisions made in this paper are useful. The idea of using cheap storage as a single source of truth for a variety of different workloads is certainly very attractive. Moreover, providing ACID guarantees over an store which doesn't is an impressive result in itself. However, this work will be successful if it can truly provide a viable alternative to unite data processing needs. If the industry values simplicity of maintenance over performance, or if a Delta Lake system can become performant enough that operational costs for maintaining custom, brittle data pipeline can no longer be justified, the protocol should see widespread adoption. It remains to be seen whether the assumption of a cheap cloud object store is too ingrained in the design, or whether Delta Lake can be adapted to other mediums. Regardless, it's a very clean design.
