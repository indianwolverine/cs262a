### What is the problem that is being solved?

In an adversarial model, attackers can gain entry to databases by exploiting common software bugs. At that point, they will have full access to unencrypted data stored on the database. This is a common threat model and completely unaddressed in the literature so far.

### What are the key results?

The authors present CryptDB, a database in which queries are run over encrypted data such that only users with the correct credentials can actually decrypt the underlying data. This is powerful because in this model, even attackers who have gained access to the database will be unable to decrypt any of the data, and are thus prevented from compromising the underlying data.

Essentially there are two main threat models against which CryptDB tries to provide confidentiality:

1. A passive attacker who has full access to the system
2. A malicious DBA with full admin access to the system

The key design idea which makes CryptDB work is surprisingly beautiful and simple, a first for a security focused system. Since a SQL system only has to support a predefined set of operations, different levels of encryption can be used depending on the semantics of the query. Essentially, there are different levels of encryption:

1. RND - preserves no order among data, useful for simply getting all data
2. DET - Identical values have identical ciphertexts, supports group by, count, distinct
3. OPE - perserves order between values, useful for order by, min, max, sort 
4. HOM - full homomorphic encrytion, expensive but can compute arbitrarily over encrypted data
5. JOIN and OPE-JOIN - useful for joins
6. SEARCH - supports text search

All data in the system, depending on which queries need to be supported on it, is encrypted in an onion scheme i.e. it is first encrypted with weaker forms of such as OPE, and then by stronger forms such as RND. When a query needs to operate on data in a certain way, first the level of encryption needed to support the query is determined, the data is decrypted to that level, and then the query is executed.

### What are some of the limitations and how might this work be improved?

Obviously there is the performance hit that CryptDB takes compared to a normal DBMS. However, it is likely that only very small DB's protecting a subset of information will need the security guarantees provided by CryptDB, and it's likely feasible for an organization to run a few secure instances.

### How might this work have long term impact?

In cases where stronger security guarantees are needed at smaller scale, CryptDB has the potential to be very impactful. However, due to the performance hit it takes, it's unlikely performance critical applications, even if they store critical data, use CryptDB. On the other hand, if there is a way to safely encrypt only a subset of the data, that might be useful for production use.