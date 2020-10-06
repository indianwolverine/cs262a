### What is the problem that is being solved?

In distributed systems, a common operation is to map keys to nodes (whether for data or roles etc.). Efficient lookup for these keys is difficult. In the naive case, every node in the system contains all the key location information for all other nodes which leads to wasted space, or a single node contains key to node mappings for each other node - making that node a single point of failure and defeating the purpose of a P2P system. The authors of this paper describe a protocol to make key lookups fast. 

### What are the key results?

The authors present Chord, a protocol which supports efficient lookup of keys in a distributed P2P system. In doing so Chord also provides the following 
guarantees:

1. Load Balancing keys across nodes
2. Decentralization (no central directory concept)
3. Scalability - cost of lookup grows as O(log n) where n in number of nodes
4. Availability - robustness under dynamic network and nodes
5. Flexible naming

At it's core, Chord divides keys among nodes in a modulo fashion such that a key is mapped to the node which has the smallest id larger than it. Furthermore, each node maintains routing information for log(n) nodes ahead of it on the clock, so when a node gets a lookup request it proceeds quickly. Each node also maintains which node preceeds it in the clock. Node joining and leaving is now easy - when a node joins, it finds its successor and updates that node's predecessor to itself and also find the successors predecessor and update's that node's successor to itself. Finally, it transfers the needed state from its successor to itself.

### What are some of the limitations and how might this work be improved?

The authors describe Chord in the context of P2P system, but how would this work in a cloud environment? Clearly many of the guarantees Chord provides are useful for cloud-based key value stores, but a focus on cloud systems instead of P2P systems would have made the paper stronger. This is of course an unfair criticism because cloud environments weren't big when this paper was published.

### How might this work have long term impact?

This work was done in the context of P2P system, assuming network bandwidth is scarce. The hardware story is very different now, but the use of consistent hashing in this work demonstrates the power of the scheme in large scale environments. For example, similar mechanisms as are employed in Chord can be used for load balancing in a network. Overall, the key-value store abstraction which Chord enables at scale is a very powerful one and one that we see in all sorts of systems in industry today such as Dynamo, Cassandra, etc.
