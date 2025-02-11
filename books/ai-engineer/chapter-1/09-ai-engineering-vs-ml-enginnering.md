## AI Engineering Versus ML Engineering

While the unchanging principles of deploying AI applications are reassuring, it’s also important to understand how things have changed. This is helpful for teams that want to adapt their existing platforms for new AI use cases and developers who are interested in which skills to learn to stay competitive in a new market.

At a high level, building applications using foundation models today differs from traditional ML engineering in three major ways:

1.  Without foundation models, you have to train your own models for your applications. With AI engineering, you use a model someone else has trained for you. This means that AI engineering focuses less on modeling and training, and more on model adaptation.
    
2.  AI engineering works with models that are bigger, consume more compute resources, and incur higher latency than traditional ML engineering. This means that there’s more pressure for efficient training and inference optimization. A corollary of compute-intensive models is that many companies now need more GPUs and work with bigger compute clusters than they previously did, which means there’s more need for engineers who know how to work with GPUs and big clusters.[23](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch01.html#id640)
    
3.  AI engineering works with models that can produce open-ended outputs. Open-ended outputs give models the flexibility to be used for more tasks, but they are also harder to evaluate. This makes evaluation a much bigger problem in AI engineering.
    
In short, AI engineering differs from ML engineering in that it’s less about model development and more about adapting and evaluating models. I’ve mentioned model adaptation several times in this chapter, so before we move on, I want to make sure that we’re on the same page about what model adaptation means. In general, model adaptation techniques can be divided into two categories, depending on whether they require updating model weights.

_Prompt-based techniques, which include prompt engineering, adapt a model without updating the model weights._  You adapt a model by giving it instructions and context instead of changing the model itself. Prompt engineering is easier to get started and requires less data. Many successful applications have been built with just prompt engineering. Its ease of use allows you to experiment with more models, which increases your chance of finding a model that is unexpectedly good for your applications. However, prompt engineering might not be enough for complex tasks or applications with strict performance requirements.

_Finetuning, on the other hand, requires updating model weights._ You adapt a model by making changes to the model itself. In general, finetuning techniques are more complicated and require more data, but they can improve your model’s quality, latency, and cost significantly. Many things aren’t possible without changing model weights, such as adapting the model to a new task it wasn’t exposed to during training.