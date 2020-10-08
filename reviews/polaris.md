### What is the problem that is being solved?

There are two main goals - converge data warehousing and big data workloads i.e. lakehouse model, separate compute and state for cloud native execution. If these goals can be realized, users will be able to handle structured (warehouse) and unstructured (lake) data with one system efficiently at scale. This is the holy grail of massive data - to be able to store all the data in one place and compute on that data easily without needing several custom systems or data representations.

### What are the key results?

The authors present Polaris, an interactive relation query engine, useful for converging warehouses and lakes. Over time, compute has become progressively decoupled from data so that both can scale independently. In Polaris, compute and cache coexist, but all other state including metadata, transaction logs, and data exist on separate nodes in a stateless architecture.

To be able to handle relational and unstructured data, Polaris introduces the cell abstraction. Datasets are logically abstracted into cells which can then be flexibly assigned to compute nodes (stateless!). The compute node is then responsible for data extraction. A hash function and user defined partition function uniquely defines each cell in two dimensions, and cells can be grouped in different ways (maybe store one partition or hash together). Furthermore, separate metadata maintains the cell to compute node mappings.

Mapping cells to compute is an especially interesting problem. Say I want to join P and Q on column a in P and column b in Q. If P is hashed such that all columns with 'a' exist in one hash partition and similarly with 'b' for Q, then computing the inner join is as easy as joining only those cells of both tables, which is easy since they are partitioned already. A hash operator to rehash a table into a different space and a broadcast operator to map a table into a single replicated cell are also provided to make query plans - these are called data move enforcers. The cheapest plan minimizes this data movement.

For computation, a task DAG is created, where each task is essentially a query operator from the query plan (think rehashing or broadcast or result of a join). Tasks take input cells, execute code on these, and write results to output cells, which can then be returned to the user or computed on further.

For orchestration, the task DAG is further subdivided into state machines for each node. This allows the higher nodes to wait on their dependencies and trigger retries if their children fail at ome execution or to take custom actions in response to failures down the chain.

Workload scheduling becomes a mapping of resources that tasks need to resources compute nodes can provide, with a bias towards finishing tasks closer to the root quickly to free up intermediate state.

### What are some of the limitations and how might this work be improved?

The paper demonstrates the usefulness of Polaris for processing relational data, but does not demonstrate how Polaris fares for unstructured data as is commonly stored in data lakes. It would be interesting to see whether Polaris performs better on unstructured data rather than simply converting the unstructured data to structured data as in the warehouse model and then computing over it. Another limitation of this work is that Polaris requires a ton of internal dependencies specific to the Azure ecosystem and cannot be easily generalized to open source implementations - which is it's main weakness compared to Delta Lake.

### How might this work have long term impact?

This work is certainly useful for Microsoft, but until it can be shown to work effectively at in open source, it won't be able to take advantage of the network effects needed to distribute it to the rest of the world. The design of Polaris is very clean, but whether or not it is widely used will likely depend on if it can solve the unstructured data problem effectively.