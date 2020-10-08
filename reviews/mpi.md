### What is the problem that is being solved?

The Message Passing Interface (MPI) is a spec for a stdlib for message passing (useful for parallel computing) which has many implementations for different hardware (think different supercomputers). Because of this tight coupling, which is needed to ensure performance, these implementations are not portable, and a lot of duplicated effort goes into creating them for new hardware. There is a need for an implementation which is both portable and performant.

### What are the key results?

The authors present MPICH, a portable and performant implementation of MPI. MPICH groups processes in contexts and allows them to use a communicator to send messages to each other. These processes can be mapped anywhere in the parallel ecosystem - same processor or different machine altogether. 

MPICH ships with testing libraries which are useful for benchmarking any MPI framework, and MPICH specifically performs well compared to existing frameworks.

To achieve portability, MPICH is built on top of an abstract device interface (ADI). The hardware specific part of the implementation abstracts the hardware to expose the ADI to the rest of MPICH, which can then be ported trivially to new hardware as long as an ADI is available for that hardware.

Perhaps most importantly, out of the numerous functions defined in the MPI spec, MPICH only implements a small subset of these on top of the ADI. This subset can then be used to implement every other function in the MPI spec. This strategy maximizes the amount of code reuse that can happen in MPICH through composition.

### What are some of the limitations and how might this work be improved?

The limitations aren't with the work per se but with the MPI model which gets complex very fast due to the low level abstraction it provides, and requires great expertise in the programming model to get right. In a distributed system built via MPI, issues such as fault tolerance and non-idempotency must be dealt with directly by the programmer for each new application, which adds a lot of overhead to even seemingly easy tasks on other frameworks such as Spark.

### How might this work have long term impact?

MPICH has been used to program supercomputers, so in it's specific domain it has had massive impact due to its focus on portability and performance.