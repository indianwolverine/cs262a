### What is the problem that is being solved?

Sharing mutable data at scale is a difficult problem due to the CAP theorem, and efforts to support such semantics sacrifice either consistency (last-writer-wins) or availability (serialize all updates). The authors propose Commutative Replicated Data Types, which ensure no conflicts on data if all updates commute, therefore obviating the need for consensus algorithms for concurrency control.

### What are the key results?

The authors illustrate the concept of CRDTs using the example of Treedoc to implement an ordered set abstraction. The main idea here is that only two operations are defined on the set - insert and delete - with the tree structure establishing a total order among elements in the ordered set. Essentially the idea here is that for replicas storing this ordered set, if all replicas receive the same set of updates, regardless of order or duplication, they will all converge to the same state. This is useful, because no coordination has to occur between the replicas, such as Raft, Paxos, or 2PC, thus avoiding the communication overhead such consensus protocols entail.

The authors proceed to describe a protocol for flattening the Treedoc to remove deleted nodes, and a system for maintaining only a core set of nodes which must converge on the same flattened structure through traditional consensus protocols, with other nodes designated as Nebula nodes which send and receive updates to the core source of truth nodes periodically.

### What are some of the limitations and how might this work be improved?

While the idea presented is quite clever, it would've been useful to see how the author's actually use Treedoc in practice. What are the series of atoms in the ordered set and how do they relate to concurrent document editing, as is the intended use case for Treedoc. Moreover, it would be useful to see what kinds of problems CRDTs cannot solve, so as to establish the range of environments they are actually useful within.

### How might this work have long term impact?

By providing a guarantee of strong eventual consistency by relaxing some of the constraints imposed by strong consistency definitions, CRDTs provide a new way of framing an application's needs and provide more room for design. For example, not all applications need strong consistency, but may find the CRDT model useful for their needs. CRDTs are already being used in a range of environments where strong eventual consistency is needed such as with Figma's collaborative wireframe editor, which goes to show that some of the scalability concerns are tractable.
