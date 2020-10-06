### What is the problem that is being solved?

Amazon needs a key-value store which is highly available for their business needs. Due to it's scale, Amazon hardware fails all the time, so any key-value store they build needs to be fault tolerant as well. Due to Amazon's specific needs, they need a key-value store which is highly available for writes. This of course means that it will have to sacrifice consistency to some degree. 

### What are the key results?

The authors present Dynamo, a highly write available key-value store. In addition to the properties mentioned above, there are a few additional design considerations:

1. Scalability
2. Symmetry - each node plays the same role
3. Decentralization
4. Heterogeneity - be able to exploit heterogeneous infrastructure effectively

Partitioning: Dynamo uses consistent hashing (like Chord) to partition keys across nodes. To support infrastructure heterogeneity, one node can contain multiple virtual nodes, and these virtual nodes are the ones responsible for the keys on the clock.

Replication: A coordinator node is responsible for replicating all it's owned keys across N-1 other nodes.

Data Versioning: Dynamo uses vector clocks to track versions of objects and automatically reconcile divergent branches if a common ancestor can be determined. If not, it simply returns all versions of the object to the client and the client is then responsible for reconciling the history. 

Executing get() and put(): Dynamo uses quorums to do reads and writes to the N replicas. A write must propagate to W replicas and a read to R such that R + W > N. This ensures at least one node which is in both the read and write quorum.

### What are some of the limitations and how might this work be improved?

Dynamo tries to sell the idea that it's a highly available key-value store which is eventually consistent, but it leaves clients to handle data conflicts! While this is useful for certain Amazon services which use Dynamo, it seems that Dynamo as a system is mostly a collection of architectural and design lessons from academic literature in the field. Essentially, what's new here, other than the fact that a big company did it and encountered some production issues they needed to design for?

### How might this work have long term impact?

Perhaps what's most important in this work is the demonstration that academic ideas can be applied usefully to a production setting. Amazon's lessons from running Dynamo in a production environment are valuable in the sense that they show us that eventual consistency is often enough, because clients can reconcile reads based on application semantics in a wide variety of cases.
