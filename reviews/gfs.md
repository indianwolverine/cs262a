### What is the problem that is being solved?

Google's unique application workloads necessitated the development of a new distributed file system uniquely suited to these workloads. The problem here is to create a distributed file system which will support the development of distributed applications at Google scale - this means building a system reilient to component failures and able to handle very large files.

### What are the key results?

There are a number of assumptions at play here which influence the design of the Google File System (GFS).

1. Component failures are common due to scale
2. Files are large, on the order of GBs
3. Most writes are appends, and most reads sequential
4. Concurrent writes and reads are common (producer/consumer)
5. Bandwidth is more important than latency

GFS supports create, delete, open, close, read, write, snapshot, and record append (atomic append for producer/consumer) operations to files. The overall design consists of one master node which contains all file metadata and file to chunk mapping and a set of chunkservers which contain 64MB chunks of data from files. Clients contact the master node for the metadata, then talk to chunkservers directly to get the data of the file. No data is cached at the client since most workloads are streaming and working sets likely won't fit into memory - this eliminates cache coherence problems.

The master node stores all metadata in memory, with an operation log to persist the file and chunk namespaces as well as file to chunk mappings. If the master crashes, it can rebuild from the log, and also by contacting the chunkservers upon restart.

To ensure a write order at replicas, GFS employs a lease mechanism where a primary replica is designated and decides the order of all mutations. For record appends, the client simply supplies the data and GFS tells the client where the data was written. Snapshots are implemented through a copy-on-write mechanism where the master node revokes all leases to ensure writes go through it first.

### What are some of the limitations and how might this work be improved?

While this work does present interesting lessons from a real workload, much like the Dynamo paper, it's highly specific to Google's own environment. Maybe the design lesson here is to not necessarily try to provide a broad abstraction that works for everyone, but instead focus on what the workload looks like, and given these assumptions, then start designing and building the system. The usual lessons of monitoring, logging, fault tolerance at scale etc. also apply here. It's interesting that the author's decided to use only one master node as well. Why not shard the namespace across multiple masters?

### How might this work have long term impact?

Google's work is a huge departure from previous distributed file systems, and with the lessons of tailoring a system to application needs is very important to designing new systems today. For example, GFS has been very successful at Google in supporting all sorts of distributed systems workloads. In fact, there seems to be a trend here in Google's design philosophy - design work real world workloads. We now see lots of systems which are architected in this way, such as Ray for RL specific applications, and Spark for data pipelines. All these designs share the common choice of exposing a very narrow API tailored to the required appilcation semantics and doing no more than that.
