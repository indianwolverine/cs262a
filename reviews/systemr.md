### What is the problem that is being solved?

System R is built around the idea of data independence - decouple data from computation.

If the user has to worry about how data is stored, then queries on the data depend on how the data is stored (graph, table, tree, DAG etc.). The relational database model instead seeks to provide a high level language which the user can use to declaratively query the data and the DBMS will take care of forming the appropriate algorithm.

All this raises the question of whether a DBMS could perform queries as efficiently as a programmer could do, which is the standard that System R sought to meet while acheiving the following goals laid out by the authors:

- high level user interface to ensure data independence
- support several different types of queries (on the fly, batched etc.)
- support changing underlying data environment
- support concurrent usage while ensuring data consistency and integrity
- data recovery
- different views of data per ACL
- as high performance as existing lower level db systems

### What are the key results?

Essentially System R makes significant progress in accomplishing all the goals the authors have laid out.

1. The SQL user interface is completely decoupled from the internal data representation - user's are able to declaratively query.
2. System R supports both interactive and programmed queries, first compiling the query to machine code then executing.
3. The data independence further supports adaptability in the face of a rapidly changing environment. For example, the authors describe using CPU time as a factor in optimizing queries, but we know that their system will soon have to evolve as CPU speeds far outstrip I/O speeds. However, due to the design of System R, they can simply make that change in the System R query optimizer without worrying about physical storage and while maintaining compatibility with applications running on top of the db.
4. Support for concurrency through transactions and fine grain locks.
5. Data recovery facilitated through shadow pages and WAL.
6. Authorization primitives are built in, allowing different configurations.
7. Seem to perform as well as existing systems, but the authors use an ideal dataset so results may be skewed.

### What are some of the limitations and how might this work be improved?

The shadow page concept is not necessary. The author's should migrate the system to using a WAL. For checkpointing, the page on disk could be used as the 'old' page and then the log could be applied to an old page for recovery.

The optimizer function is also not ideal. Given that the authors tested on data which was uniformly distributed per column with each column independently distributed, the simple optimization scheme seems to work. However, on data which does not conform to these expectations, the optimizer will use statistics which are not entirely useful in forming the query. 

This is not System R's fault, but the scheduling abstraction is faulty (Convoy effect) due to lack of communication between the db and the OS. If the OS and db were able to communicate that the transaction holding a lock should always be prioritized, the problem persists. A real fix for this likely involves a change in the OS (user visible locks!).

The authorization scheme is not very robust. For example, there is not concept of anonymized or obfuscated data.

### How might this work have long term impact?

System R met almost all of the goals it set out to acheive and the designs used in meeting those goals form the basis for modern RDBMSs today. For example, the abstraction of the high level SQL language has led to the proliferation of several competing RDBMSs today, almost too many to count, each optimizing for one factor of the other such as high availability or fault tolerance. 
