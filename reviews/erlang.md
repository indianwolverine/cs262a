### What is the problem that is being solved?

Telecom switching systems have very strict requirements for reliability and fault-tolerance and are expected to operate for long periods of time while exhibiting soft real-time behavior and handling software and hardware errors gracefully. Although the paper is framed as a solution to the telecom domain, the results can easily be applied to internet connected systems, as these require generally the same guarantees. The goal here is to create a software system that can meet all of these needs.

### What are the key results?

The author presents Erlang, a language meant to address the problems listed above. In Erlang, everything runs as a process, isolated from every other process (and therefore fault tolerant), and there exists a hierarchy of such processes which can only communicate with each other via message passing if they know the name of the reciever process (usually parent processes know child process names). These processes are very lightweight, and obviate the need for a complex underlying OS - thus Erlang programs can run pretty much anywhere and are especially useful in embedded systems.

These simple abstractions, which give the language runtime full control of a lightweight process tree, where a predefined communication protocol allows isolated process to concurrently communicate with each other and respond to errors gracefully, enable a host of programming paradigms. For example, in a client-server model, the concurrent aspects of a server implementation can be factored out into a separate module, while the sequential aspects of it exist separately.

Error handling is also greatly simplified in an Erlang system. When a process can't do what it wants to do, it simply exits and sends a message to another process, usually its supervisor, indicating that it falied - the supervisor is subsequently left ot take care of the error.

### What are some of the limitations and how might this work be improved?

The main limitation here is that when concurrency handled fully by the language runtime is successful when that runtime is the only thing running on a system. What happens when we have a system with multiple non-Erlang processes competing for the same resources - in such a situation can the language runtime make good decisions regarding scheduling and Erlang process interaction? However, if the underlying operating system is an exokernel, these assumptions actually work very well. 

### How might this work have long term impact?

Erlang is used throughout industry, including, most notably, at WhatsApp, where it has been a massive success in building a fault-tolerant, distributed service with all the guarantees promised by Erlang. In an ideal world, perhaps process scheduling should be done by some sort of language runtime, since the runtime has the greatest knowledge of application semantics and can thus avoid many of the problems encountered when relying on an OS to provide concurrency. The exokernel approach is especially attractive - in this system, an Erlang runtime could implement all the primitives required for process management while relying on a thin LibOS to provide hardware virtualization.