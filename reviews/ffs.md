### What is the problem that is being solved?

UNIX filesystem is inherently slow and unable to support appplications which process large amounts of data. For example, directories, inodes, and data are all allocated in different parts of the disk, leading to high seek times to read all the files in a directory. Furthermore, data blocks for a file are often not stored sequentially on disk, and the FS only transfers 512 byte blocks per disk transaction. All of these design choices combine to severely limit I/O throughput and increase disk latency.

The author's have created a new filesystem (FFS) which has increased throughput by exploiting locality of reference and optimizing for large sequential data transfers.

### What are the key results?

FFS introduces the design of storing files in cylinder groups, with all of the files and inodes for files in a directory ideally all lying in the same cylinder group. The idea here is that access patterns are usually for all the files in a directory, so if we can store these files close together we can see higher disk bandwidth due to sequential reads. Moreover, data blocks are not stored consecutively but rather at rotationally optimal positions i.e. by the time one data block has been transferred, the disk head is above the next and ready to transfer it.

In addition to these locality of reference ideas, FFS also adds some enhancements for end users such as longer file names, advisory file locks, symlinks, a disk quota system (precursor to cgroups?), and atomic renaming of files.

These performance enhancements increase disk bandwidth from 2-4% of maximum to 20-40% for large reads and writes.

### What are some of the limitations and how might this work be improved?

Despite all the design work, FFS is still only able to achieve 20-40% of disk bandwidth for large reads and writes - the workload is CPU bound in all cases. Given that the trend was towards faster and faster processors in relation to disk I/O as well as higher capacity RAM, perhaps the focus of the design should have been towards exploiting increased CPU and RAM. 

For example, most reads and writes could be directly to RAM with batched sequential writes when a certain threshold of dirty pages are accumulated in RAM.

Additionally, since directories are stored as far away from each other as possible, accesses to subdirectories within a directory will be slow.

### How might this work have long term impact?

The locality of reference ideas can be applied to any similar filesystem. Moreover, the paper makes extensive use of profiling the hardware before deciding on configuration which is a useful lesson for any performance critical system. 
