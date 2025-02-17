## Challenges of Comparative Evaluation

With pointwise evaluation, the heavy-lifting part of the process is in designing the benchmark and metrics to gather the right signals. Computing scores to rank models is easy. With comparative evaluation, both signal gathering and model ranking are challenging. This section goes over the three common challenges of comparative evaluation.

### Scalability bottlenecks

Comparative evaluation is data-intensive. The number of model pairs to compare grows quadratically with the number of models. In January 2024, LMSYS evaluated 57 models using 244,000 comparisons. Even though this sounds like a lot of comparisons, this averages only 153 comparisons per model pair (57 models correspond to 1,596 model pairs). This is a small number, considering the wide range of tasks we want a foundation model to do.

Fortunately, we don’t always need direct comparisons between two models to determine which one is better. Ranking algorithms typically assume  _transitivity_. If model A ranks higher than B, and B ranks higher than C, then with transitivity, you can infer that A ranks higher than C. This means that if the algorithm is certain that A is better than B and B is better than C, it doesn’t need to compare A against C to know that A is better.

However, it’s unclear if this transitivity assumption holds for AI models.  Many papers that analyze Elo for AI evaluation cite transitivity assumption as a limitation ([Boubdir et al.](https://arxiv.org/abs/2311.17295);  [Balduzzi et al.](https://arxiv.org/abs/1806.02643); and  [Munos et al.](https://arxiv.org/abs/2312.00886)). They argued that human preference is not necessarily transitive. In addition, non-transitivity can happen because different model pairs are evaluated by different evaluators and on different prompts.

There’s also the challenge of evaluating new models. With independent evaluation, only the new model needs to be evaluated. With comparative evaluation, the new model has to be evaluated against existing models, which can change the ranking of existing models.

This also makes it hard to evaluate private models. Imagine you’ve built a model for your company, using internal data. You want to compare this model with public models to decide whether it would be more beneficial to use a public one.  If you want to use comparative evaluation for your model, you’ll likely have to collect your own comparative signals and create your own leaderboard or pay one of those public leaderboards to run private evaluation for you.

The scaling bottleneck can be mitigated with better matching algorithms. So far, we’ve assumed that models are selected randomly for each match, so all model pairs appear in approximately the same number of matches. However, not all model pairs need to be equally compared. Once we’re confident about the outcome of a model pair, we can stop matching them against each other. An efficient matching algorithm should sample matches that reduce the most uncertainty in the overall ranking.

### Lack of standardization and quality control

One way to collect comparative signals is to crowdsource comparisons to the community the way LMSYS Chatbot Arena does. Anyone can go to  [the website](https://oreil.ly/td_MY), enter a prompt, get back two responses from two anonymous models, and vote for the better one. Only after voting is done are the model names revealed.

The benefit of this approach is that it captures a wide range of signals and is relatively difficult to game.[24](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch03.html#id976)  However, the downside is that it’s hard to enforce standardization and quality control.

First, anyone with internet access can use any prompt to evaluate these models, and there’s no standard on what should constitute a better response. It might be a lot to expect volunteers to fact-check the responses, so they might unknowingly prefer responses that sound better but are factually incorrect.

Some people might prefer polite and moderate responses, while others might prefer responses without a filter. This is both good and bad. It’s good because it helps capture human preference in the wild. It’s bad because human preference in the wild might not be appropriate for all use cases. For example, if a user asks a model to tell an inappropriate joke and a model refuses, the user might downvote it. However, as an application developer, you might prefer that the model refuses. Some users might even maliciously pick the toxic responses as the preferred ones, polluting the ranking.

Second, crowdsourcing comparisons require users to evaluate models outside of their working environments. Without real-world grounding, test prompts might not reflect how these models are being used in the real world. People might just use the first prompts that come to mind and are unlikely to use sophisticated prompting techniques.

Among  [33,000 prompts](https://oreil.ly/eI9Vq)  published by LMSYS Chatbot Arena in 2023, 180 of them are “hello” and “hi”, which account for 0.55% of the data, and this doesn’t yet count variations like “hello!”, “hello.”, “hola”, “hey”, and so on. There are many brainteasers. The question “X has 3 sisters, each has a brother. How many brothers does X have?” was asked 44 times.

Simple prompts are easy to respond to, making it hard to differentiate models’ performance. Evaluating models using too many simple prompts can pollute the  ranking.

If a public leaderboard doesn’t support sophisticated context construction, such as augmenting the context with relevant documents retrieved from your internal databases, its ranking won’t reflect how well a model might work for your RAG system. The ability to generate good responses is different from the ability to retrieve the most relevant documents.

One potential way to enforce standardization is to limit users to a set of predetermined prompts. However, this might impact the leaderboard’s ability to capture diverse use cases. LMSYS instead lets users use any prompts but then filter out  [hard prompts](https://x.com/lmarena_ai/status/1792625968865026427)  using their internal model and rank models using only these hard prompts.

Another way is to use only evaluators that we can trust. We can train evaluators on the criteria to compare two responses or train them to use practical prompts and sophisticated prompting techniques. This is the approach that Scale uses with  [their private comparative leaderboard](https://oreil.ly/kIJ9F). The downside of this approach is that it’s expensive and it can severely reduce the number of comparisons we can get.

Another option is to incorporate comparative evaluation into your products and let users evaluate models during their workflows. For example, for the code generation task, you can suggest users two code snippets inside the user’s code editor and let them pick the better one. Many chat applications are already doing this. However, as mentioned previously, the user might not know which code snippet is better, since they’re not the expert.

On top of that, users might not read both options and just randomly click on one. This can introduce a lot of noise to the results. However, the signals from the small percentage of users who vote correctly can sometimes be sufficient to help determine which model is better.

_Some teams prefer AI to human evaluators. AI might not be as good as trained human experts but it might be more reliable than random internet users_.

### From comparative performance to absolute performance

For many applications, we don’t necessarily need the best possible models. We need a model that is good enough. Comparative evaluation tells us which model is better. It doesn’t tell us how good a model is or whether this model is good enough for our use case. Let’s say we obtained the ranking that model B is better than model A. Any of the following scenarios could be valid:

1.  Model B is good, but model A is bad.
    
2.  Both model A and model B are bad.
    
3.  Both model A and model B are good.
    

You need other forms of evaluation to determine which scenario is true.

Imagine that we’re using model A for customer support, and model A can resolve 70% of all the tickets. Consider model B, which wins against A 51% of the time. It’s unclear how this 51% win rate will be converted to the number of requests model B can resolve. Several people have told me that in their experience, a 1% change in the win rate can induce a huge performance boost in some applications but just a minimal boost in other applications.

When deciding to swap out A for B, human preference isn’t everything. We also care about other factors like cost. Not knowing what performance boost to expect makes it hard to do the cost–benefit analysis. If model B costs twice as much as A, comparative evaluation isn’t sufficient to help us determine if the performance boost from B will be worth the added cost.