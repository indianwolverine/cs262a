### What is the problem that is being solved?

Serverless computing brings with it a host of new architectural challenges including but not limited to debugging in a serverless context, monitoring, developer tooling and test environments etc. An open platform is needed to develop and test serverless architectures easily and promote research and development in the area.

### What are the key results?

Essentially the authors are creating OpenLambda, a platform for building and evaluating all the subsystems involved in a serverless system. Some of the research problems they bring up include:

1. Execution Engine - A fast execution engine is needed which has lower startup latency than serverless platforms provide now
2. Interpreted Languages - Profiling information must be shared between lambda workers to facilitate JIT compilation
3. Package Support - Better support for third party packages is needed
4. Cookies and Sessions - Support for long lived sessions is needed in the context of the web
5. Databases - Custom integration with serverless specific data stores and colocation of data with workers is needed
6. Data aggregators - Serverless platoforms need to support tree like dependency structures among lambdas
7. Load Balancers - Scheduling must be aware of data locality as well as code locality
8. Cost Debugging - Custom support is needed to allow developers to debug the cost of their functions
9. Legacy decomposition - It remains to be seen whether current monolithic web apps can even be modularized to use serverless platforms

### What are some of the limitations and how might this work be improved?

What about security? The biggest hole in all of this research is that in a web paradigm, attackers have an enormous TCB to attack and exploit with serverless platforms. Because of this, any useful serverless platform has to be built extremely securely from the start or risk DDOS attacks, which could either cost the cloud provider, thus making the serverless venture less attractive, or hurt the developers, thus taking them off the platform.

### How might this work have long term impact?

Unfortunately, as the count of lazy developers and short sighted corporations increases daily, we are sure to see a rise in serverless platforms, and therefore, probably an undue amount of research effort in this area. I think any real applications worth anything will not migrate to a serverless paradigm and instead continue to focus on low level performance tuning, but I am sure serverless platforms will continue to be successful as AWS eats up the VC money of wannabe startups running their webscale workloads on the cash incinerator cloud.