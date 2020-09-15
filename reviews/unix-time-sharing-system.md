### What is the problem that is being solved?

The goal of Unix was to create a simple, feature-rich, general purpose operating system which allowed enough surface area for application developers to easily express their ideas but no more. Many of the cool things users can do with Unix are built on top of the sparse syscall interface.

### What are the key results?

According to the authors, the most important job of Unix was to provide a file system. Unix does this very simply, providing a small set of syscalls as an interface to the kernel. The entire filesystem is a tree, with support for normal files, directories, links, I/O devices treated as files, mountable file systems from removable media, and a decoupling of file metadata from data.

In addition to this, Unix provided the interactive Shell environment, which allowed elegant composition of programs in pipe chains. Essentially everything on the system in the ideal case is treated as a file and most operations act on these files transparently whether the underlying medium is the traditional RAM or disk storage, or a network link. 

### What are some of the limitations and how might this work be improved?

Some of what I considered limitations in the Unix design are as follows:

- No links allowed across devices
  - Why not have a unified namespace for all files?
  - If everything is a file, a user should be able to access files on other hosts or across a network transparently

- The superuser concept
  - Unrestrained access to file system leads to a massive security hole for anyone gaining superuser credentials
  - Why not namespace the filesystems so that users only ever have access to files in their own namespace and can't see anything outside this namespace?
  - The permissions system is a bit lacking and easily broken
  
- No user-visible locks
  - Nowadays several programs are multithreaded and structured to do bookeeping in /tmp files and often clobber each other
  - If we had user-visible locks, multithreaded programs would not have to write elaborate locking logic when writing to files, and could write to files with the assurance that writes happen with the intended semantics

- Buffered I/O
  - There is no visibility into if data has been persisted if system crashes
  - Should be a mechanism to ensure writes have been persisted to disk (not sure but I think fsync does this nowadays?)

- Pipes
  - why not have named pipes which can be created and managed beforehand or dynamically?
  - Reads from pipes should have ability to return immediately if no data in pipe


### How might this work have long term impact?

Unix is everywhere! From Linux popularly used in DCs to MacOS (also BSD). The minimal design has proven to be much more useful for building applications than a complex syscall interface to the kernel. This leads to idiomatic ways of doing things - ensures safety.

Moreover, since Unix is written in C, it is portable to many different architectures. We can see the success of Linux today stems from the fact that it can run on pretty much any hardware and has device drivers for any device anyone cares to write a driver for - although Linux isn't quite as simple as Unix was.

On the other hand, a Unix-like operating system is pretty much taken for granted. We could be doing a better job of questioning some of the basic assumptions made in Unix and operating systems should be an active area of research. If we have novelty hardware, why not novelty operating systems to manage that hardware? Or, going the other way, in a world where we can have access to computers anywhere, why don't we have transparent access and control over all of our own data? If Facebook wants to use my data, it should ideally have to go through my own servers to serve that data on its platform. And ideally, from my laptop, I should have transparent access to all my data without having to ssh to another machine and without something like NFS.