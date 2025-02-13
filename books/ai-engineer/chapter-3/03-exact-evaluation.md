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

### Exact match

It’s considered an exact match if the generated response matches one of the reference responses exactly. Exact matching works for tasks that expect short, exact responses such as simple math problems, common knowledge queries, and trivia-style questions. Here are examples of inputs that have short, exact responses:

-   “What’s 2 + 3?”
    
-   “Who was the first woman to win a Nobel Prize?”
    
-   “What’s my current account balance?”
    
-   “Fill in the blank: Paris to France is like ___ to England.”
    

There are variations to matching that take into account formatting issues. One variation is to accept any output that contains the reference response as a match. Consider the question “What’s 2 + 3?” The reference response is “5”. This variation accepts all outputs that contain “5”, including “The answer is 5” and “2 + 3 is 5”.

However, this variation can sometimes lead to the wrong solution being accepted. Consider the question “What year was Anne Frank born?” Anne Frank was born on June 12, 1929, so the correct response is 1929. If the model outputs “September 12, 1929”, the correct year is included in the output, but the output is factually wrong.

Beyond simple tasks, exact match rarely works. Given the original French sentence “Comment ça va?”, there are multiple possible English translations, such as “How are you?”, “How is everything?”, and “How are you doing?” If the reference data contains only these three translations and a model generates “How is it going?”, the model’s response will be marked as wrong. The longer and more complex the original text, the more possible translations there are. It’s impossible to create an exhaustive set of possible responses for an input. For complex tasks, lexical similarity and semantic similarity work better.

### Lexical similarity

Lexical similarity measures how much two texts overlap. You can do this by first breaking each text into smaller tokens.

In its simplest form, lexical similarity can be measured by counting how many tokens two texts have in common. As an example, consider the reference response  _“My cats scare the mice”_  and two generated responses:

-   “My cats eat the mice”
    
-   “Cats and mice fight all the time”
    

Assume that each token is a word. If you count overlapping of individual words only, response A contains 4 out of 5 words in the reference response (the similarity score is 80%), whereas response B contains only 3 out of 5 (the similarity score is 60%). Response A is, therefore, considered more similar to the reference response.

One way to measure lexical similarity is  _approximate string matching_, known colloquially as  _fuzzy matching_. It measures the similarity between two texts by counting how many edits it’d need to convert from one text to another, a number called  _edit distance_. The usual three edit operations are:

1.  Deletion: “b_r_ad” -> “bad”
    
2.  Insertion: “bad” -> “ba_r_d”
    
3.  Substitution: “b_a_d” -> “b_e_d”
    

Some fuzzy matchers also treat transposition, swapping two letters (e.g., “ma_ts_” -> “ma_st_”), to be an edit. However, some fuzzy matchers treat each transposition as two edit operations: one deletion and one insertion.

For example, “bad” is one edit to “bard” and three edits to “cash”, so “bad” is considered more similar to “bard” than to “cash”.

Another way to measure lexical similarity is  _n-gram similarity_, measured based on the overlapping of sequences of tokens,  _n-grams_, instead of single tokens. A 1-gram (unigram) is a token. A 2-gram (bigram) is a set of two tokens. “My cats scare the mice” consists of four bigrams: “my cats”, “cats scare”, “scare the”, and “the mice”. You measure what percentage of n-grams in reference responses is also in the generated response.[12](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#id922)

Common metrics for lexical similarity are BLEU, ROUGE, METEOR++, TER,  and CIDEr.  They differ in exactly how the overlapping is calculated. Before foundation models, BLEU, ROUGE, and their relatives were common, especially for translation tasks. Since the rise of foundation models, fewer benchmarks use lexical similarity. Examples of benchmarks that use these metrics are  [WMT](https://oreil.ly/92yRh),  [COCO Captions](https://oreil.ly/BO3-0), and  [GEMv2](https://arxiv.org/abs/2206.11249).

A drawback of this method is that it requires curating a comprehensive set of reference responses. A good response can get a low similarity score if the reference set doesn’t contain any response that looks like it. On some benchmark examples,  [Adept](https://oreil.ly/OWD2v)  found that its model Fuyu performed poorly not because the model’s outputs were wrong, but because some correct answers were missing in the reference data.

Not only that, but references can be wrong. For example, the organizers of the WMT 2023 Metrics shared task, which focuses on examining evaluation metrics for machine translation, reported that they found many bad reference translations in their data. Low-quality reference data is one of the reasons that reference-free metrics were strong contenders for reference-based metrics in terms of correlation to human judgment ([Freitag et al., 2023](https://oreil.ly/tmWqk)).

Another drawback of this measurement is that higher lexical similarity scores don’t always mean better responses. For example, on HumanEval, a code generation benchmark, OpenAI found that BLEU scores for incorrect and correct solutions were similar. This indicates that optimizing for BLEU scores isn’t the same as optimizing for functional correctness ([Chen et al., 2021](https://arxiv.org/abs/2107.03374)).

### Semantic similarity

Lexical similarity measures whether two texts look similar, not whether they have the same meaning. Consider the two sentences “What’s up?” and “How are you?” Lexically, they are different—there’s little overlapping in the words and letters they use. However, semantically, they are close. Conversely, similar-looking texts can mean very different things. “Let’s eat, grandma” and “Let’s eat grandma” mean two completely different things.

_Semantic similarity_  aims to compute the similarity in semantics. This first requires transforming a text into a numerical representation, which is called an  _embedding_. For example, the sentence “the cat sits on a mat” might be represented using an embedding that looks like this:  `[0.11, 0.02, 0.54]`. Semantic similarity is, therefore, also called  _embedding similarity_.

[“Introduction to Embedding”](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#ch03a_introduction_to_embedding_1730150757064669)  discusses how embeddings work. For now, let’s assume that you have a way to transform texts into embeddings. The similarity between two embeddings can be computed using metrics such as cosine similarity. Two embeddings that are exactly the same have a similarity score of 1. Two opposite embeddings have a similarity score of –1.

_I’m using text examples, but semantic similarity can be computed for embeddings of any data modality, including images and audio._  Semantic similarity for text is sometimes called semantic textual similarity.

###### Warning

>While I put semantic similarity in the exact evaluation category, it can be considered subjective, as different embedding algorithms can produce different embeddings. However, given two embeddings, the similarity score between them is computed exactly.

Metrics for semantic textual similarity include  [BERTScore](https://arxiv.org/abs/1904.09675)  (embeddings are generated by BERT) and  [MoverScore](https://oreil.ly/v2ENK)  (embeddings are generated by a mixture of  algorithms).

Semantic textual similarity doesn’t require a set of reference responses as comprehensive as lexical similarity does. However, the reliability of semantic similarity depends on the quality of the underlying embedding algorithm. Two texts with the same meaning can still have a low semantic similarity score if their embeddings are bad. Another drawback of this measurement is that the underlying embedding algorithm might require nontrivial compute and time to run.

Before we move on to discuss AI as a judge, let’s go over a quick introduction to embedding. The concept of embedding lies at the heart semantic similarity, and is the backbone of many topics we explore throughout the book, including vector search in  [Chapter 6](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch06.html#ch06_rag_and_agents_1730157386571386)  and data deduplication in  [Chapter 8](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch08.html#ch08_dataset_engineering_1730130932019888).