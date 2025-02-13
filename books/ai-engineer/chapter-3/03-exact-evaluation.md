# Exact Evaluation

When evaluating models’ performance, it’s important to differentiate between exact and subjective evaluation. Exact evaluation produces judgment without ambiguity. For example, if the answer to a multiple-choice question is A and you pick B, your answer is wrong. There’s no ambiguity around that. On the other hand, essay grading is subjective. An essay’s score depends on who grades the essay. The same person, if asked twice some time apart, can give the same essay different scores. Essay grading can become more exact with clear grading guidelines. As you’ll see in the next section, AI as a judge is subjective. The evaluation result can change based on the judge model and the prompt.

I’ll cover two evaluation approaches that produce exact scores: functional correctness and similarity measurements against reference data. Note that this section focuses  on evaluating  open-ended responses (arbitrary text generation) as opposed to  close-ended  responses (such as classification). This is not because foundation models aren’t being used for close-ended tasks. In fact, many foundation model systems have at least a classification component, typically for intent classification or scoring. This section focuses on open-ended evaluation because close-ended evaluation is already well understood.

## Functional Correctness

Functional correctness evaluation means evaluating a system based on whether it performs the intended functionality. For example, if you ask a model to create a website, does the generated website meet your requirements? If you ask a model to make a reservation at a certain restaurant, does the model succeed?

Functional correctness is the ultimate metric for evaluating the performance of any application, as it measures whether your application does what it’s intended to do. However, functional correctness isn’t always straightforward to measure, and its measurement can’t be easily automated.

Code generation is an example of a task where functional correctness measurement can be automated. Functional correctness in coding is sometimes  _execution accuracy_. Say you ask the model to write a Python function,  `gcd(num1, num2)`, to find the greatest common denominator (gcd) of two numbers, num1 and num2. The generated code can then be input into a Python interpreter to check whether the code is valid and if it is, whether it outputs the correct result of a given pair  `(num1, num2)`. For example, given the pair  `(num1=15, num2=20)`, if the function  `gcd(15, 20)`  doesn’t return 5, the correct answer, you know that the function is wrong.

Long before AI was used for writing code, automatically verifying code’s functional correctness was standard practice in software engineering. Code is typically validated with  [unit tests](https://en.wikipedia.org/wiki/Unit_testing)  where code is executed in different scenarios to ensure that it generates the expected outputs. Functional correctness evaluation is how coding platforms like LeetCode and HackerRank validate the submitted solutions.

Popular benchmarks for evaluating AI’s code generation capabilities, such as  [OpenAI’s HumanEval](https://oreil.ly/CjYs9)  and  [Google’s MBPP](https://github.com/google-research/google-research/tree/master/mbpp)  (Mostly Basic Python Problems Dataset) use functional correctness as their metrics.  Benchmarks for text-to-SQL (generating SQL queries from natural languages) like Spider ([Yu et al., 2018](https://oreil.ly/ijU20)), BIRD-SQL (Big Bench for Large-scale Database Grounded Text-to-SQL Evaluation) ([Li et al., 2023](https://oreil.ly/rrSS9)), and WikiSQL ([Zhong, et al., 2017](https://arxiv.org/abs/1709.00103)) also rely on functional  correctness.

When evaluating a model, for each problem a number of code samples, denoted as  _k_, are generated. A model solves a problem if any of the  _k_  code samples it generated pass all of that problem’s test cases. The final score, called  _pass@k_, is the fraction of the solved problems out of all problems. If there are 10 problems and a model solves 5 with  _k_  = 3, then that model’s pass@3 score is 50%. The more code samples a model generates, the more chance the model has at solving each problem, hence the greater the final score. This means that in expectation, pass@1 score should be lower than pass@3, which, in turn, should be lower than pass@10.

Another category of tasks whose functional correctness can be automatically evaluated is game bots. If you create a bot to play  _Tetris_, you can tell how good the bot is by the score it gets. Tasks with measurable objectives can typically be evaluated using functional correctness. For example, if you ask AI to schedule your workloads to optimize energy consumption, the AI’s performance can be measured by how much energy it saves.[11](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#id906)

## Similarity Measurements Against Reference Data

If the task you care about can’t be automatically evaluated using functional correctness, one common approach is to evaluate AI’s outputs against reference data. For example, if you ask a model to translate a sentence from French to English, you can evaluate the generated English translation against the correct English translation.

Each example in the reference data follows the format (input, reference responses). An input can have multiple reference responses, such as multiple possible English translations of a French sentence. Reference responses are also called  _ground truths_  or  _canonical responses_. Metrics that require references are  _reference-based_, and metrics that don’t are _reference-free_.

Since this evaluation approach requires reference data, it’s bottlenecked by how much and how fast reference data can be generated. Reference data is generated typically by humans and increasingly by AIs. Using human-generated data as the reference means that we treat human performance as the gold standard, and AI’s performance is measured against human performance. Human-generated data can be expensive and time-consuming to generate, leading many to use AI to generate reference data instead. AI-generated data might still need human reviews, but the labor needed to review it is much less than the labor needed to generate reference data from scratch.

Generated responses that are more similar to the reference responses are considered better. There are four ways to measure the similarity between two open-ended texts:

1.  Asking an evaluator to make the judgment whether two texts are the same
    
2.  Exact match: whether the generated response matches one of the reference responses exactly
    
3.  Lexical similarity: how similar the generated response looks to the reference responses
    
4.  Semantic similarity: how close the generated response is to the reference responses in meaning (semantics)
    

Two responses can be compared by human evaluators or AI evaluators. AI evaluators are increasingly common and will be the focus of the next section.

This section focuses on hand-designed metrics: exact match, lexical similarity, and semantic similarity. Scores by exact matching are binary (match or not), whereas the other two scores are on a sliding scale (such as between 0 and 1 or between –1 and 1). Despite the ease of use and flexibility of the AI as a judge approach, hand-designed similarity measurements are still widely used in the industry for their exact nature.

###### Note

> This section discusses how you can use similarity measurements to
> evaluate the quality of a generated output. However, you can also use
> similarity measurements for many other use cases, including but not
> limited to the following:
> 
> Retrieval and search
> 
> find items similar to a query
> 
> Ranking
> 
> rank items based on how similar they are to a query
> 
> Clustering
> 
> cluster items based on how similar they are to each other
> 
> Anomaly detection
> 
> detect items that are the least similar to the rest
> 
> Data deduplication
> 
> remove items that are too similar to other items
> 
> Techniques discussed in this section will come up again throughout the
> book.