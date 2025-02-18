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

## Domain-Specific Capability

To build a coding agent, you need a model that can write code. To build an application to translate from Latin to English, you need a model that understands both Latin and English. Coding and English–Latin understanding are domain-specific capabilities. A model’s domain-specific capabilities are constrained by its configuration (such as model architecture and size) and training data. If a model never saw Latin during its training process, it won’t be able to understand Latin. Models that don’t have the capabilities your application requires won’t work for you.

To evaluate whether a model has the necessary capabilities, you can rely on domain-specific benchmarks, either public or private. Thousands of public benchmarks have been introduced to evaluate seemingly endless capabilities, including code generation, code debugging, grade school math, science knowledge, common sense, reasoning, legal knowledge, tool use, game playing, etc. The list goes on.

Domain-specific capabilities are commonly evaluated using exact evaluation. Coding-related capabilities are typically evaluated using functional correctness, as discussed in  [Chapter 3](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#ch03a_evaluation_methodology_1730150757064067). While functional correctness is important, it might not be the only aspect that you care about. You might also care about efficiency and cost. For example, would you want a car that runs but consumes an excessive amount of fuel? Similarly, if an SQL query generated by your text-to-SQL model is correct but takes too long or requires too much memory to run, it might not be usable.

Efficiency can be exactly evaluated by measuring runtime or memory usage.  [BIRD-SQL](https://oreil.ly/mOAjn)  (Li et al., 2023) is an example of a benchmark that takes into account not only the generated query’s execution accuracy but also its efficiency, which is measured by comparing the runtime of the generated query with the runtime of the ground truth SQL query.

You might also care about code readability. If the generated code runs but nobody can understand it, it will be challenging to maintain the code or incorporate it into a system. There’s no obvious way to evaluate code readability exactly, so you might have to rely on subjective evaluation, such as using AI judges.

Non-coding domain capabilities are often evaluated with close-ended tasks, such as multiple-choice questions. Close-ended outputs are easier to verify and reproduce. For example, if you want to evaluate a model’s ability to do math, an open-ended approach is to ask the model to generate the solution to a given problem. A close-ended approach is to give the model several options and let it pick the correct one. If the expected answer is option C and the model outputs option A, the model is wrong.

This is the approach that most public benchmarks follow. In April 2024, 75% of  the tasks  in Eleuther’s  [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness/blob/master/docs/task_table.md)  are multiple-choice, including  [UC Berkeley’s MMLU (2020)](https://arxiv.org/abs/2009.03300),  [Microsoft’s AGIEval (2023)](https://arxiv.org/abs/2304.06364), and the  [AI2 Reasoning Challenge (ARC-C) (2018)](https://oreil.ly/d3ggH). In their paper, AGIEval’s authors explained that they excluded open-ended tasks on purpose to avoid inconsistent assessment.

Here’s an example of a multiple-choice question in the MMLU benchmark:

-   Question: One of the reasons that the government discourages and regulates monopolies is that
    
    -   (A) Producer surplus is lost and consumer surplus is gained.
        
    -   (B) Monopoly prices ensure productive efficiency but cost society allocative efficiency.
        
    -   (C) Monopoly firms do not engage in significant research and development.
        
    -   (D) Consumer surplus is lost with higher prices and lower levels of output.
        
    -   Label: (D)
        

A multiple-choice question (MCQ) might have one or more correct answers. A common metric is accuracy—how many questions the model gets right. Some tasks use a point system to grade a model’s performance—harder questions are worth more points. You can also use a point system when there are multiple correct options. A model gets one point for each option it gets right.

Classification is a special case of multiple choice where the choices are the same for all questions. For example, for a tweet sentiment classification task, each question has the same three choices: NEGATIVE, POSITIVE, and NEUTRAL. Metrics for classification tasks, other than accuracy, include F1 scores, precision, and recall.

MCQs are popular because they are easy to create, verify, and evaluate against the random baseline. If each question has four options and only one correct option, the random baseline accuracy would be 25%. Scores above 25% typically, though not always, mean that the model is doing better than random.

A drawback of using MCQs is that a model’s performance on MCQs can vary with small changes in how the questions and the options are presented.  [Alzahrani et al. (2024)](https://arxiv.org/abs/2402.01781)  found that the introduction of an extra space between the question and answer or an addition of an additional instructional phrase, such as “Choices:” can cause the model to change its answers. Models’ sensitivity to prompts and prompt engineering best practices are discussed in  [Chapter 5](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch05.html#ch05a_prompt_engineering_1730156991195551).

Despite the prevalence of close-ended benchmarks, it’s unclear if they are a good way to evaluate foundation models. MCQs test the ability to differentiate good responses from bad responses (classification), which is different from the ability to generate good responses. MCQs are best suited for evaluating knowledge (“does the model know that Paris is the capital of France?”) and reasoning (“can the model infer from a table of business expenses which department is spending the most?”). They aren’t ideal for evaluating generation capabilities such as summarization, translation, and essay writing. Let’s discuss how generation capabilities can be evaluated in the next section.