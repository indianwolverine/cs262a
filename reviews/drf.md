### What is the problem that is being solved?

There has been a massive research focus on scheduling for single resource types (CPU, memory, I/O, network), but not a lot of work has gone into developing scheduling for workloads with multiple heterogeneous resource needs. In the cloud world, there are several such heterogeneous workloads, which, when suboptimally scheduled, lead to wasted resources. The goal of this paper is to present an algorithm which generalizes max-min fairness to multiple resource types. 

### What are the key results?

The authors first lay out a series of requirements they want their algorithm to meet:
1. Sharing Incentive - In a system of n users, a user should be better off sharing resources than simply using 1/n of the cluster.
2. Strategy Proofness - Users shouldn't be able to benefit by lying about their resource needs.
3. Envy-Freeness - A user should not prefer another user's allocation.
4. Pareto Efficiency - It shouldn't be possible to increase one user's allocation without decreasing another user's allocation.

The core of the dominant resource fairness (DRF) algorithm is simple: each task in a system has some share of a resource (CPU, memory etc.) which it needs to run. The share of the resource which is highest is the dominant share for that task. For example, if a task needs 1 out of 4 CPUs and 2 out 10 GB of memory to run, we can see that CPU is it's dominant resource (1/4 > 2/10). The algorithm is as follows - given a system with a set of resources and a set of tasks each with their own resource demands, select the task with the minimum dominant share; allocate resources to fulfill that task's demands and update counts for shares for each task; repeat until a resource is exhausted or tasks are finished.

We can see that DRF is simply a generalization of max-min fairness to multiple resource types - at each step of the algorithm we try to maximize the minimum dominant share and so on until we're done. DRF also meets all 4 requirements laid out earlier.

The paper goes on to talk about Asset Fairness and CEEI, other multiple resource allocation policies, and which properties they violate. More importantly, evaluation of DRF implemented in Mesos compared to the Hadoop slot-based fair scheduler shows much better efficiency for the DRF based approach. 

### What are some of the limitations and how might this work be improved?

It would be interesting to see how DRF scales in the data center - what level of granularity can it handle before it breaks? In a mutually trusting environment, is it better to run CEEI?

### How might this work have long term impact?

DRF is used in all sorts of cloud contexts right now, not only due to its simplicity and generalizability to a host of other problems (such as weighted DRF) but also because it provides guarantees of performance isolation (through sharing incentive), defense against bad actors, and decent utilization. 
