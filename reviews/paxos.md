### What is the problem that is being solved?

Reaching consensus on a value in a distributed environment with failing machines is a fundamentally hard problem. The Paxos protocol seeks to provide a solution to this problem, but is difficult to understand - so we need Leslie Lamport to explain it to us. 

### What are the key results?

Fundamentally, three results fall out of the Paxos protocol:

1. Only proposed values can be chosen
2. Only a single value is chosen
3. A process cannot learn a value unless it has been chosen

There are essentially two phases to the protocol - an initial phase where leader election takes place, which Lamport calls the prepare phase. In this phase, only value numbers are agreed upon by all participants - proposers and acceptors. 

After this, the accept phase happens, where the proposer can actually attach values to the proposals it got acceptors to agree to in the prepare phase.

As long as a majority of the nodes are working in a cluster, the Paxos protocol ensures the safety of the data and that progress can be made. 

The key idea here is that given this distributed consensus, a distributed state machine can be implemented such that values of proposals correspond to actions in a state machine. Thus, we can guarantee that all nodes will eventually reach the same state.

### What are some of the limitations and how might this work be improved?

Seeing as this is the second paper on Paxos published by Lamport to more fully explain ideas from the first paper, The Part Time Parliament, it's clear that this protocol is complex and difficult to implement in practice. Another problem is something the author points out in the paper: that Paxos does not guarantee liveness of data. Paxos can get stuck in an infinite loop of actions such as two competing proposers trying to get acceptors to accept their proposals in turn. Perhaps timeouts and an explicit leader election process could be useful in this case.

### How might this work have long term impact?

This work is useful in the sense that it presents a consensus protocol which can actually be used in practice to build fault-tolerant distributed systems. Other than Paxos 2PC, and Raft, I have not actually heard of any other consensus protocols, so it may be safe to say that this work has been very impactful in the field of distributed systems.