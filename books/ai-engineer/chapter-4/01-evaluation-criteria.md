# Evaluation Criteria

Which is worse—an application that has never been deployed or an application that is deployed but no one knows whether it’s working? When I asked this question at conferences, most people said the latter. An application that is deployed but can’t be evaluated is worse. It costs to maintain, but if you want to take it down, it might cost even more.

AI applications with questionable returns on investment are, unfortunately, quite common. This happens not only because the application is hard to evaluate but also because application developers don’t have visibility into how their applications are being used. An ML engineer at a used car dealership told me that his team built a model to predict the value of a car based on the specs given by the owner. A year after the model was deployed, their users seemed to like the feature, but he had no idea if the model’s predictions were accurate. At the beginning of the ChatGPT fever, companies rushed to deploy customer support chatbots. Many of them are still unsure if these chatbots help or hurt their user experience.

Before investing time, money, and resources into building an application, it’s important to understand how this application will be evaluated. I call this approach  _evaluation-driven development_. The name is inspired by  [_test-driven development_](https://en.wikipedia.org/wiki/Test-driven_development)  in software engineering, which refers to the method of writing tests before writing code. In AI engineering, evaluation-driven development means defining evaluation criteria before building.

#### Evaluation-Driven Development

While some companies chase the latest hype, sensible business decisions are still being made based on returns on investment, not hype. Applications should demonstrate value to be deployed. As a result, the most common enterprise applications in production are those with clear evaluation criteria:

-   Recommender systems are common because their successes can be evaluated by an increase in engagement or purchase-through rates.[1](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch04.html#id989)
    
-   The success of a fraud detection system can be measured by how much money is saved from prevented frauds.
    
-   Coding is a common generative AI use case because, unlike other generation tasks, generated code can be evaluated using functional correctness.
    
-   Even though foundation models are open-ended, many of their use cases are close-ended, such as intent classification, sentiment analysis, next-action prediction, etc. It’s much easier to evaluate classification tasks than open-ended tasks.
    

While the evaluation-driven development approach makes sense from a business perspective, focusing only on applications whose outcomes can be measured is similar to looking for the lost key under the lamppost (at night). It’s easier to do, but it doesn’t mean we’ll find the key. We might be missing out on many potentially game-changing applications because there is no easy way to evaluate them.

I believe that evaluation is the biggest bottleneck to AI adoption. Being able to build reliable evaluation pipelines will unlock many new applications.

An AI application, therefore, should start with a list of evaluation criteria specific to the application. In general, you can think of criteria in the following buckets: domain-specific capability, generation capability, instruction-following capability, and cost and latency.

Imagine you ask a model to summarize a legal contract. At a high level, domain-specific capability metrics tell you how good the model is at understanding legal contracts. Generation capability metrics measure how coherent or faithful the summary is. Instruction-following capability determines whether the summary is in the requested format, such as meeting your length constraints. Cost and latency metrics tell you how much this summary will cost you and how long you will have to wait for it.

The last chapter started with an evaluation approach and discussed what criteria a given approach can evaluate. This section takes a different angle: given a criterion, what approaches can you use to evaluate it?