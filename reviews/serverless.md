### What is the problem that is being solved?

This paper explores the ecosystem of serverless computing, diving into existing serverless implementations in industry, academia, and the open source community, as well as discussing use cases for the technology as well as open problems.

### What are the key results?

Serverless computing refers to a paradigm where the developer only writes bits of stateless, functional code which are then deployed to a cloud platform and executed agnostically on any number of resources invisible to the developer upon invocation (think webhooks for one).

The overall architecture of serverless platforms resembles an event dispatching system. Events are sent to the system, the service determines which function(s) to trigger as a result of an event, find an existing server running a function (with all the software to run the function) or spin up a new one, execute the function on the server, gather and return the results. All this needs to happen while maintaining fault tolerance and scaling up and down as needed to minimize cost.

Applications which need to deal with bursty, compute intensive workloads are especially suited to a serverless model since a developer doesn't have to worry about scaling the service and is only charged for compute time. I/O bound functions suffer from the compute time cost calculation however, and do not fare well in serverless setups.

### What are some of the limitations and how might this work be improved?

This paper itself doesn't have any limitations since it just provides a broad overview of serverless computing.

### How might this work have long term impact?

Clearly serverless paradigms are becoming more and more impactful. Some of the mobile examples that the authors list help illustrate why - serverless computing greatly reduces application eoperational complexity and lowers the barrier to entry for developers. Of course, as an application grows in popularity, it remains to be seen whether serverless platforms indeed provide easy cost savings, or whether they actually just necessitate a new kind of complexity - the complexity of writing software to be serverless and to optimize for costs on a specific serverless platform. There definitely is a use for serverless computing for stateless tasks, but for stateful it probably still makes more performance sense to control the entire pipeline.