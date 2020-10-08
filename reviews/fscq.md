### What is the problem that is being solved?

Filesystems are notorious for saynig that they provide certain guarantees under various scenarios, including crashes, but sometimes fail to deliver on those promises, leaving user data in an inconsistent, unpredictable state. It might be useful to formally verify a filesystem to make sure that it performs as specced under all circumstances.

### What are the key results?

The authors present FSCQ, a filesystem which has been machine verified to meet its spec under all possible workload sequences. Importantly, this means that FSCQ will never lose data given any sequences of crashes and reboots. 

The idea of Crash Hoare logic (CHL) is introduced by this paper. CHL allows programmers to specify invariants which must hold in case of crashes and supports encoding recovery mechanisms. This CHL can then be proved automatically.

Essentially FSCQ is a journaling file system, to which changes can be made and then automatically verified using Coq. The author's show that FSCQ does not show much performance degradation when compared to other similar filesystems.

### What are some of the limitations and how might this work be improved?

One of the biggest limitations of the automatic proof method described in this paper is that it allows for checking only filesystems which are built in a similar way as FSCQ i.e. they are log based, journaling filesystems. This kind of work is really only useful in the sense that it can guide discussion of how to build more reliable filesystems and what kinds of common pitfalls to look out for.

### How might this work have long term impact?

Perhaps the most important thing to take away from this paper is a set of guidelines on what kinds of design decisions lead to a more reliable filesystem rather than actually machine verify each and every change made to a filesystem during development. Like for Histar, the impact here is not likely to be very much since people want performant software, not necessarily always correct software. Besides, file system corruption, which matters at scale, can easily be corrected using erasure coding, parity, and replication of data. Nobody really cares if one filesystem is corrupted since replicas are often lying around somewhere.