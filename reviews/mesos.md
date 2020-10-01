### What is the problem that is being solved?

- Clusters of computers have become major computing platform
- Engineers have developed many different specialized frameworks to utilize cluster resources
- No one of these frameworks is optimal for all applications
- Therefore, we want to be able to multiplex multiple frameworks on the same cluster
- This will allow better use of resources and allow sharing of data
- MAIN GOALS - 1: HIGH UTILIZATION, 2: EFFICIENT DATA SHARING
- right now - partition cluster or allocate set of VMs per framework
- mismatch of allocation granularity between frameworks and these strategies

### What are the key results?

- Mesos - thin resource sharing layer enabling fine grained resource sharing across frameworks
	- each framework has different scheduling needs
	- programming model, communication pattern, task dependencies, data placement
	- must scale to tens of thousands of nodes
	- fault tolerant and highly available

- One design option - global scheduler
- optimal scheduling, but several drawbacks
- hard to design expressive API
- complexity makes it hard to scale, less resilient to failure
- frameworks already implement their own scheduling

- Mesos design - resource offers
- send offers of resources (CPU, memory) to frameworks (fair sharing)
- framework can accept or reject then use resources however they want
- resource offers count against framework's allocation, so framework scheduler is incentivized to respond quickly

Architecture
- master process which manages set of slave daemons on cluster nodes
- allocation through pluggable policy (usually fair sharing, priority)
- frameworks must implement a scheduler and an executor module
- mesos master gathers resource information from slaves daemons
- sends resource offers to framework scheduler
- scheduler can accept or reject offer
  - if accept, then it sends the mesos master spec of tasks to run
- master passes tasks along to correct node, which runs them using framework executor
- frameworks can also use filters to specify that they will always reject a set of resource offers
- only offer nodes from list L (whitelist), only offer nodes with at least R resources free
- isolation through Linux containers
- resource allocation - guaranteed allocation, if exceeded, mesos can kill cany tasks

Fault tolerance 
- master node maintains soft state - active slave, active frameworks, running tasks
- use ZooKeeper with hot standby masters
- when master fails, standby master state populated by schedulers and slaves
- master reports node failures and executor crashes to frameworks

Mesos Behavior
- performs well with frameworks which scale elastically, homogeneous task duration, prefer all nodes equally
- Definitions:
  - Framework ramp-up time: time it takes a new framework to achieve its allocation (e.g., fair share)
  - Job completion time: time it takes a job to complete, assuming one job per framework
  - System utilization: total cluster utilization
  - elastic framework can start using nodes immediately and release upon task completion (Hadoop, Dryad)
  - rigid framework needs full resource allocation before begin work, cannot scale up/down
  - mandatory vs. preferred resources (GPUs etc.)
  - assume mandatory resources requested by framework doesn't exceed its guranteed share

- Homogeneous tasks
  - n slots in cluster, framework f entitled to k slots
  - T is mean task duration, constant and exponential distribution
  - f runs job which requires bkT total computation time
  - Ramp up time
    - T if constant, TlnK if expo (follows from how long it takes k slots to open up)
  - Job completion time
    - (1 + b)T at worst for elastic (bT for job to finish + T for worst case ramp up)
    - (lnk + b)T at worst for rigid (bT for job to finish + Tlnk for worst case ramp up)
  - System utilization
    - elastic can achieve full U due to using slots as soon as they are free
    - rigid cannot bc they require a certain amount of alloc slots first

- Placement preferences
  - there exists a config in which each framework gets all preferred slots
    - within T interval preferred slots will be free
  - no such config exists
    - lottery scheduling here will allow alloc to proceed

- Heterogeneous tasks
  - maintain some slots for short tasks to avoid convoy effect

- Framework incentives
  - prefer short tasks (can take advantage of reserved slots for short tasks)
  - prefer elastic scaling (grab resources opportunistically)
  - dont accept unknown resources (counts against alloc of framework)

- Limitations of Distributed scheduling
  - large tasks may starve behind several small ones
    - minimum offer size to prevent slaves from offering small resources
  - Interdependent framework constraints (rare in practice)
  - framework complexity

Implementation
- not too hard to port Hadoop, Torque, MPI, and Spark to Mesos

Evaluation
- Macrobenchmark - 96 node mesos cluster, (4 CPU 15GB RAM) compare to static partioning (24 nodes per framework)
    - Hadoop mix of small and large
    - Hadoop large batch jobs
    - Spark running ML
    - Torque running MPI
  - Mesos achieves higher utilization and mostly faster job completion (except for MPI which makes sense)
  - delay scheduling allows greater data locality, improves job completion time

### What are some of the limitations and how might this work be improved?

While the abstraction provided by Mesos is useful, not much is said in the paper about long running services, which were later scheduled on Mesos with Marathon, a framework developed by AirBnB for this purpose. Moreover, since Mesos is in fact targetted towards a cloud cluster environment, it fails to address some usability problems that Kubernetes later would - for example easy monitoring, logging, deployment. Mesos also assumes that scheduling has to be done by each framework individually, which may or may not be an issue depending on whether a scheduling framework can then be easily run on top of Mesos.

### How might this work have long term impact?

Mesos has already been widely adopted at large tech companies such as Twitter and AirBnB, making it a very successful framework. Moreover, the design and architecture of Mesos directly influences how many distributed systems are structured. For example, see the GCS, scheduler and nodes with drivers and workers in Ray.