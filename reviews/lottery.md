### What is the problem that is being solved?

Scheduling many processes over one CPU is a difficult problem which affects system throughput and response time, both of which are very important to a well functioning computer system. The authors argue that current scheduling algorithms do not provide flexible control over process execution rates, and in the case of fair share schedulers are too coarse grained.

### What are the key results?

The authors present lottery scheduling, a mechanism which probabilistically guarantees fairness. The core idea behind lottery scheduling is tickets. Each process gets a share of tickets, t, out of a total T. Then the algorithm randomly picks a ticket at each quantum. The probability a process gets picked for that quantum is t/T, which in the long term ensures that the process will run proportional to its share of tickets. Moreover, the lotteries a process must wait for before running is 1/p where p = t/T - geometrically distributed - this makes the system response time pretty fast too.

There are a few more ideas which round out the algorithm, such as ticket inflation which allows mutually trusting processes to issue more tickets to themselves (can be useful in interactive applications), ticket currencies which help encapsulate different sets of processes in their own micro lottery, and compensation tickets which allow a process which doesn't use up its entire quantum to still get the proportional amount of the resource. 

The interface developed also allows easy reconfiguration of tickets, which makes the system much more responsive to process needs and also makes it easier to reason about resource allocation. For example, if process A holds 1 ticket for every 2 tickets process B has, then process A will get 1/3 of the resource in the long term compared to 2/3 for process B.

### What are some of the limitations and how might this work be improved?

While the authors do present adaptations for lottery scheduling in contexts other than scheduling processes on a uniprocessor system, such as inverse lotteries for memory and mutexes lotteries, it's not clear how such a system would work in a multiprocessor system and how all these sets of lotteries will interact. A system isn't going to have just a few processes running which all execute similarly, but rather numerous processes, heterogeneous in resource needs. It would have added to the work if the paper demonstrated allocation ratio guarantees on real workloads. 

### How might this work have long term impact?

Lottery scheduling is certainly easier to understand than say priority scheduling and does seem to provide better guarantees of performance as well as greater control over scheduling than more opaque mechanisms. On the other hand, in a system running a large number of processes, the scheduling decision has to be even quicker than lottery scheduling can allow, which means a system such as MLFQ, although approximate, may work better simply due to decent semantics encoded in the algorithm.
