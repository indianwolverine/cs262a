### What is the problem that is being solved?

The authors argue that current definitions of isolation in the database are lacking - they are either ambiguous or implementation dependent, and thus the standard definitions for isolation are not as useful in a wide range of environments. The paper therefore defines a standard, implementation independent scheme for reasoning about isolation levels in a database which is compatible with several existing implementation types (locking, optimistic, multi-version) and also, in contrast to previous systems, handles predicate locking.

### What are the key results?

The authors first lay the formal groundwork for the work they are going to describe, specifically defining transactions, predicates, histories, as well as direct read, write, and anti dependencies. They then define the direct serialization graph, where nodes are committed transactions and edges are direct read, write, and anti dependencies between transactions. 

With this framework in place, the authors proceed to define a new set of generalized isolation phenomena (G*) and isolation levels (PL-*). Without going into the details of these phenomena and levels, the new definitions for isolation allow a greater set of histories, such as those that may be valid in non-locking based systems, to be considered valid. The authors claim that these new definitions are implementation independent, and can be used to reason about isolation semantics even in systems with previously unseen ways of coping with isolation.

### what are some of the limitations and how might this work be improved?

While the authors do go through a great deal of print to define a formal set of definitions with the goal of making an implementation independent isolation framework, it's not clear whether their definitions will always hold true for isolation management systems which do not conform to a locking, optimistic, or multi-version strategy. Either that or I'm missing something here and they do in fact prove this. 

Perhaps more important, it would be interesting to see an sketch of how the defined isolation levels are achieved in real system to get a more concrete idea of the aims of the paper. Theory is always good, but in my opinion even better with some motivating examples, especially in this field.

### how might this work have long term impact?

This work definitely provides a formal treatment of a core topic in transactional logic, and in that sense can almost be used verbatim as a definitive text on isolation semantics. Furthermore, when thinking of new ways to design systems which must cope with transactions, this paper provides a very nice foundation to construct the design with. It's often very difficult to think of abstract ideas like this, even more so to design a system which actually delivers on the guarantees it claims, so to have a text like this to constrain and test a design is extremely useful.
