### What is the problem that is being solved?

There is a fundamental tradeoff between concurrency and overhead when designing locking protocol for a database management system. On the one hand, if locks are too granular, this increases the overhead of acquiring and keeping track of locks for transactions which process a large amount of records. On the other hand, coarse locks prevent smaller transactions from running concurrently, which decreases concurrency when the transactions are processing mutually exclusive data. The authors describe a locking system which addresses these issues and discuss the degrees of consistency in a DBMS. 

### What are the key results?

The authors present a granular, hierarchical scheme for lock management in DBMSs. In particular, this system makes use of Shared and Exclusive locks (S, X) for reading and writing records, intent locks (IS, IX) which must be acquired on ancestors in a db graph before reading or writing, and a hybrid shared/intent exclusive lock (SIX) which allows a transaction to read all descendents of a node and write some of them. This scheme allows a greater degree ofconcurrency in the system, as transactions can proceed while maintaining degrees of consistency - which the authors elaborate on next.

The authors describe the notion of serializability in transaction schedules, namely the idea of determining if the actions of concurrent transactions are equivalent to a serial execution of the transactions. They present the two phase locking protocol (2PL) and describe 4 different levels of consistency that can be achieved depending on read/write semantics.

### What are some of the limitations and how might this work be improved?

The work, while very thorough and mathematically rigorous, is very hard to implement on a real DBMS. Moreover, most applications don't actually need the strict isolation and consistency semantics described in the paper. It might be fine in practice to concurrently run transactions which violate some consistency and isolation constraints while still returning satisfactory results.

### How might this work have long term impact?

In practice, if DBMSs want to implement a hierarchal, granular lock structure, they have to look no further than this paper. This work also introduces ideas such as serializability which are useful for reasoning about read/write semantics. It also raises the question - what degree of consistency is tolerable - and in doing so presages the modern eventually consistent systems being built today.
