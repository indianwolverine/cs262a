### What is the problem that is being solved?

Previously, services and batch jobs run on two different systems at Google. The goal with Borg is to manage both under a single system to increase resource utilization (just like Mesos). Although the Mesos paper doesn't mention long running services as a target framework, often used with Marathon to achieve same effect. Omega and Kubernetes (k8s) are second and third systems developed to address problems in Borg.

### What are the key results?

There is a move towards managing applications in containers, not only due to the isolation and resource utiilzation wins, but also because application logic can be encapsulated in the runtime isolation and container image. 

The state reconciliation design which lies at the heart of k8s is the key design result here. While other frameworks can provide the initial resource allocation functionality, it is very difficult for each framework to individually manage it's own failure recovery. This is something that k8s decides to provide, which makes application development very simple. Moreover, state is accessed through the API server abstraction, which is probably the only part of the k8s design which will outlast all the rest.

This design in turn simplifies all the other features necessary in an orchestration system used in an environment at scale - monitoring, logging, easy communication with other services.

### What are some of the limitations and how might this work be improved?

It could be argued that k8s is in fact too complex. For example, one of the problems the authors mention with the system is the difficulty in expressing application configuration leading to the development of an opaque DSL for config management. This may signal that k8s does too many things and doesn't allow the application enough control in how to use its resources, instead forcing it to fit into the box allowed by the config DSL. Maybe a better design here, drawing from the lessons of the e2e principle, would be to only provide functionality in k8s which is absolutely necessary and fully able to be realized at the layer. For example, a common complaint levied against k8s is difficulty in managing stateful services. Debugging such a complex system is very difficult.

### How might this work have long term impact?

k8s is already being used widely throughout industry, for better or for worse. I would argue that the design is too complex and doesn't actually provide many benefits until an organization approaches Google scale (at least at the time Google started needing Borg). On the other hand, a system such as Mesos is more flexible and provides a minimal design which can then be built on top of to compose more complex abstractions. Maybe what k8s actually shows is that hype can go a long way towards impact, the best designs don't always win, and that we should not care so much, from an academic perspective at least, about industry impact and instead focus on clean designs which solve real problems.