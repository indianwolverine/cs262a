### What is the problem that is being solved?

As software engineering has evolved to incorporate multiprocessors, clusters (and their scale), and networks, programming languages developed before this era haven't been able to provide clean abstractions to handle these new challenges. This is especially problematic at Google, where thousands of engineers work on the same codebase and need a language which does smart things when it comes to dependency management, concurrency, and modularity when running at Google scale.

### What are the key results?

Rob Pike presents his latest invention, the Go programming language, which is open source, compiled, concurrent, garbage-collected, and statically typed. Interestingly, Go was developed in response to software engineering needs and not as a research endeavor in programming languages, which makes it different from previous language work.

Specifically, Go focuses on solving the following problems: 

1. slow builds
2. uncontrolled dependencies
3. each programmer using a different subset of the language
4. poor program understanding (code hard to read, poorly documented, and so on)
5. duplication of effort
6. cost of updates
7. version skew
8. difficulty of writing automatic tools
9. cross-language builds

Dependency Management
- Requires use of ifndef compilation guards in C, C++, scales badly when lots of extraneous includes in large project -> makes compiler scan files doing no useful work
- This is especially bad for Google scale, as building a binary can take very long
- In Go, an unused import leads to a compiler error
- Furthermore, imports are read from the beginning of the imported object file, as export data, greatly speeding up compilation

Packages
- import path must be unique, but can name package anything within file
- URL's can be used as package imports

Syntax
- nearly regular grammar - very easy to parse
- no objects, but methods can be defined which operate on specific structs

Naming
- public starts with uppercase, private with lower
- simple scope hierarchy
- always clear which object is being used, no use of this
- no method name overloading

Semantics
- mostly C like
- include langauge support for concurrency, GC, interfaces, reflection, type switches

Concurrency and Garbage Collection
- support for channels, go is garbage collected

Composition
- no type hierarchy, instead there are interfaces which types can implement

Errors
- functions simply return errors as well as the normal value, programmer checks for error

Tools
- simple language design enable host of tools
- gofmt, gofix etc.

### What are some of the limitations and how might this work be improved?

The paper doesn't go into too much detail regarding Go's concurrency model, but from experience, the language's built-in support of user level threads is not great. Other than that and garbage collecting, which will always lead to nondeterminism in systems, Go is a very well designed language.

### How might this work have long term impact?

Go has had a massive impact, especially in the systems community, due to its clean design, great decisions in a number of different areas such as composition, dependency management, naming, and error handling. Most new systems which are built are done in Go or C is performance is required, and Rust to some degree. Despite its faults, Go does start the conversation of language design for the new systems paradigm of cloud computing i.e. how can a language provide clean abstractions while also providing good control of the hardware while otherwise staying out of the way.