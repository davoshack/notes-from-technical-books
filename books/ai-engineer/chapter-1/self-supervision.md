## Self-supervision

Language modeling is just one of many ML algorithms. There are also models for object detection, topic modeling, recommender systems, weather forecasting, stock price prediction, etc.  What’s special about language models that made them the center of the scaling approach that caused the ChatGPT moment?

The answer is that language models can be trained using _self-supervision_, while many other models require _supervision_. Supervision refers to the process of training ML algorithms using labeled data, which can be expensive and slow to obtain. Self-supervision helps overcome this data labeling bottleneck to create larger datasets for models to learn from, effectively allowing models to scale up.
Here’s how.

With supervision, you label examples to show the behaviors you want the model to learn, and then train the model on these examples. Once trained, the model can be applied to new data. For example, to train a fraud detection model, you use examples of transactions, each labeled with “fraud” or “not fraud”. Once the model learns from these examples, you can use this model to predict whether a transaction is fraudulent.

A drawback of supervision is that data labeling is expensive and time-consuming.

*Self-supervision helps overcome the data labeling bottleneck. In self-supervision, instead of requiring explicit labels, the model can infer labels from the input data. Language modeling is self-supervised because each input sequence provides both the labels (tokens to be predicted) and the contexts the model can use to predict these labels.*
###### Note

> Self-supervision differs from unsupervision. In self-supervised learning, labels are inferred from the input data. In unsupervised learning, you don’t need labels at all.
Self-supervised learning means that language models can learn from text sequences without requiring any labeling. Because text sequences are everywhere—in books, blog posts, articles, and Reddit comments—it’s possible to construct a massive amount of training data, allowing language models to scale up to become LLMs.

LLM, however, is hardly a scientific term. How large does a language model have to be to be considered _large_? What is large today might be considered tiny tomorrow. A model’s size is typically measured by its number of parameters. A _parameter_ is a variable within an ML model that is updated through the training process.[7](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch01.html#id547) In general, though this is not always true, the more parameters a model has, the greater its capacity to learn desired behaviors.

When OpenAI’s first generative pre-trained transformer (GPT) model came out in June 2018, it had 117 million parameters, and that was considered large. In February 2019, when OpenAI introduced GPT-2 with 1.5 billion parameters, 117 million was downgraded to be considered small.