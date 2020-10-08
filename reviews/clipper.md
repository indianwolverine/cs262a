### What is the problem that is being solved?

Most ML frameworks, such as TF, support model training, but don't have a core policy for model serving i.e. deploying a model to make real world predictions. A system is needed to interpose between applications and ML frameworks, which can select and serve the best models produced by an ML framework and specifically tailor it for the application in question. This serving system, used for inference, must be latency sensitive to fit user needs and must scale to match application load.

### What are the key results?

The authors present Clipper, a low latency online prediction serving system which acts as a serving layer between ML frameworks and end user applications. The main goals of Clipper are low latency, high throughput, and improved accuracy.

To accomplish these goals, Clipper employs a layered architecture: 

1. model abstraction - abstracts models behind a common API so applications don't have to worry about model specifics
2. model selection - select and combine predictions across models

At a high level, applications send prediction requests to Clipper through the model selection layer. This layer then send requests to be batched at the model abstraction layer. Once a batch has accumulated, it is sent to the model for prediction. Upon prediction completion of a batch, the model abstraction layer sends these back to the model selection layer to serve to the client based on specific policies. Furthermore, prediction feedback is also sent back to the model selection layer to improve further prediction.

At the model abstraction layer, a prediction cache is used to serve common queries for models. It also employs an adaptive batching mechanism to maximize throughput while still hitting latency targets through an AIMD (think TCP) mechanism. Essentially, queries to models are batched as much as possible as long as the latency is under the target.

At the model selection layer, Clipper employs two main approaches to serving predictions. One is the selection based approach, where a single model with the greatest weight is chosen, it's prediction used, then its weight adjusted based on loss feedback. This is computationally cheap since only one model has to be evaluated. The second is an ensemble approach where all models are asked for predictions and their (linear) weights adjusted based on loss feedback. This performs a bit better but is computationally expensive.

### What are some of the limitations and how might this work be improved?

It would be useful if Clipper also provided some means for updating the underlying models used to predictions i.e. we could deploy new models when they become available and decomission old ones, or even better, keep around the best model version only. Since deployment is difficult or not really a part of Clipper right now, it becomes difficult to use as a dynamic system.

### How might this work have long term impact?

As mentioned above, if there was a nice integration which was able to deploy and compare new versions of models dynamically on Clipper, this would serve as a robust way to do model deployment and serving. However, it remains to be seen whether more tightly integrated training and serving systems will be preferred over the multi-model paradigm by Clipper. I'm guessing Clipper will succeed as long as it provides clean integrations with existing frameworks and is easy to use for application developers.
