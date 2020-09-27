### What is the problem that is being solved?

Similar to the Paxos paper, the goal here is to create a consensus algorithm - in this case something which is more understandable than Paxos and therefore easier to use in building practical distributed systems.

### What are the key results?

To achieve the self stated goal of understandability, the Raft algorithm splits the consensus process into Leader Election and Log Replication subproblems while guranteeing safety of the data.

In the beginning, all servers in a Raft cluster start off in follower mode. After the election timeout, the followers become candidates and request votes from other servers in the cluster. The server recieving the most votes becomes the leader for the term. 

The leader periodically sends heartbeat events to the followers in order to maintain its leadership. If these heartbeat events cease for some reason, after the election timeout, followers again become candidates and follow the election procedure.

The leader is solely responsible for maintaining the log replication. In order to do this, it takes client requests, writes them to its own log, sends this request to all followers, and upon recieving confirmation from a majority, it considers the request committed and returns this response to the client. There are a few more details regarding leaders and followers crashing, but these are covered clearly in the paper and don't really add to the core explanation.

### What are some of the limitations and how might this work be improved?

It's difficult to point out limitations in this work. The authors designed a modular, simple to understand solution with clean abstractions, and were very successful in doing so. There might be an issue where the log mechanism may be too tightly coupled to the core abstraction i.e. maybe Paxos is more general and useful for some other use cases, however, given the simplicity of Raft, it's likely that other use cases can be adapted to the log centric one.

### How might this work have long term impact?

Given the beauty and simplicity of the solution as well as the fact that it targets a core problem in distributed systems, this work is already having immense impact and will continue to be used as the core distributed consensus algorithm for many systems. We are already seeing it used in numerous systems, as well as in the database world, which tranditionally favors the monolithic single system approach to design (of course with the internals being composed of several separate modules). It would be interesting to implement this in say a replicated database system with the log entries corresponding to a write ahead log.