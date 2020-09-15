### What is the problem that is being solved?

This paper introduces two new methods for analyzing journaling filesystems - semantic block-level analysis (SBA) and semantic trace playback (STP) and uses these to analyze the performance of several journaling filesystems (ext3, ReiserFS, JFS, NTFS).

### What are the key results?

SBA --> Traditional methods of profiling either apply workloads and measure FS performance or collect FS traces. SBA involves adding a layer between FS and disk driver which captures quantity of reads/writes, block numbers (useful for determining extent of seq/random I/O), and timing when block I/O occurs. In addition to this, SBA understands the semantic meaning of blocks because it understands FS layout (need to write one SBA driver per FS) on disk - can distinguish between writes to journal, different transactions etc.

SBA Workloads - amount of data written, seq/random I/O, interval between calls to fsync, concurrency

STP --> Runs as a user-level process which takes a FS access trace (generated from SBA and library between application and FS) to simulate access patterns to disk. STP can then use these traces to simulate timed disk access and understand performance implications of changes to the FS that have yet to be implemented. For example, one might want to simulate a different FS layout on disk, which can be done by using STP with a trace.

Analysis using SBA --> SBA on ext3 reveals that data journaling mode performs better than writeback and ordered modes for random writes since the transaction is committed in the log before data is written to a fixed location - thus the writes can happen anytime in the future and writes to the journal are logically sequential. In addition, the authors find that ext3 limits parallelism by waiting for data writes to flush to disk before starting the corresponding journal writes, which is not needed for correctness. 

Analysis using STP --> Putting the journal in the middle of the FS leads to higher bandwidth because seeks to the FS are minimized from either end. Dynamically changing the journaling policy to optimize for transaction characteristics also leads to lower latency. Finally, the author's show that differential journaling is only really useful when data is also journaled.

### What are some of the limitations and how might this work be improved?

SBA can have high overhead at high I/O rates, although the author's don't encounter this condition in their study. Furthermore, SBA must be modified for each new FS (due to its semantic nature) which presents some overhead in using it to profile a new filesystem. In addition, the authors don't go into too much detail about how STP works for previously unseen traces (other than the trivial case of block placement and FS layout).

### How might this work have long term impact?

This work is immediately useful in verification and profiling of complex systems. As the author's note, if software such as SBA is integrated within the software it is profiling, then there are liable to be calls which are missed. On the other hand, if software such as SBA acts as a MITM, it can intercept and verify all application traffic independently, ensuring that the system works as intended under complex workflows and exposing any implementation bugs while also uncovering areas for performance enhancement.

