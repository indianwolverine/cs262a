### What is the problem that is being solved?

Google handles a lot of structured data for it's many services. These services need an abstraction to quickly store and access structured data. Essentially some sort of map like data structure which scales to petabytes of data. 

### What are the key results?

To address these problems, Google came up with Bigtable, a "sparse, distributed, persistent multi-dimensional sorted map", which is exactly what it says it is. This is useful for accessing structured data along many dimensions. For example, rows may be different URLs, columns could be for example the contents stored at the URL, and in the time dimension we could have multiple versions of the content stored at a particular URL.

Row keys can be arbitrary strings. Updates to a single row are atomic, and row keys are lexicographically sorted dynamically partitioned into tablets, which are just a range of row keys. For columns we have column families which can contain multiple column keys. The final dimension for the map is a timestamp per cell, which allows multiple entries per cell ordered by time (most recent to least). 

Bigtables are stored using the SSTable file format at Google. Essentially, the SSTable index is loaded in memory and points to blocks storing key/value pairs on disk. Furthermore, Bigtable relies on Google's distributed lock service, Chubby, for coordination.

Similar to GFS, Bigtable is implemented as a master node which manages tablet servers and schema changes and a set of tablet servers which serve tablet data directly to clients. Moreover, clients cache the B+Tree-like structure of tablet location indices in memory, so all fetches of tablet data can go directly to tablet servers.

### What are some of the limitations and how might this work be improved?

Bigtable provides a useful abstraction, but the time dimension in a row+column seems restrictive. Why not have support for custom dimensions instead of just timestamp to sort data - this may be useful for certain applications. Moreover, why stop at 3 dimensions for the table - it might be interesting to support even more dimensions in the map.

### How might this work have long term impact?

Bigtable, in the general sense with multiple dimensions of data, provides an abstraction that can be used for a variety of applications. Moreover, it has influenced the design of systems such as HBase which have had enormous impact in structured data storage. The lessons which are important here are similar to the lessons from GFS, namely that it is important to first figure out what kinds of workloads a system actually needs to support and only then design a simple, minimal system to address these needs - usually this is enough. 
