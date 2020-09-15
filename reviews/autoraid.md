### What is the problem that is being solved?

With increasing data storage needs, RAID is often employed as a solution to protext data from disk failure while still providing good performance. However, RAID comes with many configuration options which are difficult to tune and once a disk array is made into a RAID array by a controller, it it very difficult to change the array configuration - involves copying data out to secondary device and then back into a reconfigured array. For all of these reasons, RAID is difficult to use and reason about for most people. 

### What are the key results?

The authors propose a RAID storage hierarchy, with the hot, working set of data being kept in a performant but expensive RAID 1 array and cold data in a cost effective and less performant RAID 5 array. This solution assumes, of course, that only a subset of the disk data is in the working set at any time and that the working set changes slowly to allow data migration from RAID 5 to RAID 1 and vice versa.

In essence, the design of the system involves splitting Physical Extent Groups, which can belong to either RAID 1 or RAID 5 storage. Hot data is stored in RAID 1 with mirroring across two disks and cold data in RAID 5 in stripes across all disks. The controller tries to serve read and write requests from its own caches, and if these fail, then a complex series of decisions have to be made: if the block is in RAID 1, a disk has to be picked to read from, if in RAID 5, reading is again pretty simple; writes are more difficult as they can involve a casacde of actions such as evicting the least recently written blocks from RAID 1 to RAID 5, and moreover writes to RAID 5 are log structured to make them more efficient and avoid the small write problem.

In addition, the solution proposed by the authors allows for easy expansion of the array with a new disk, as this new disk is split up into Physical Extent Groups and background processes migrate data over from existing disks.

### What are some of the limitations and how might this work be improved?

Overall, the solution is very interesting, but the complexity of the design is concerning. For example, there could be any number of issues with the implementation, which involves complex bookkeeping and keeping track of a number of different failure scenarios, which could be latent. Moreover, this solution is limited to an array containing a maximum of 12 disks, likely due to hardware constraints. 

It's quite difficult for the application developer to reason about disk access times in this system as well, and therefore difficult to design around an expected disk access time. Depending on internal policy, a write may return immediately, or may involve a cascade of actions to the disk to clear enough space in NVRAM for the write. Overall, the system does not provide latency guarantees. However, this may be an unfair criticism given that it does provide decent performance and high availability.

### How might this work have long term impact?

This paper proposes an elegant design and implementation of the existing RAID architecture, by dividing up the disk into Physical Extent Groups and using these and the logical unit for achieving a RAID configuration. In a computing cluster setting, this may be quite useful for certain workloads which only work on a certain subset of data at a time.