### Model development

_Model development_ is the layer most commonly associated with traditional ML engineering. It has three main responsibilities: modeling and training, dataset engineering, and inference optimization. Evaluation is also required, but because most people will come across it first in the application development layer, I’ll discuss evaluation in the next section.

#### Modeling and training

_Modeling and training_ refers to the process of coming up with a model architecture, training it, and finetuning it. Examples of tools in this category are Google’s TensorFlow, Hugging Face’s Transformers, and Meta’s PyTorch.

Developing ML models requires specialized ML knowledge. It requires knowing different types of ML algorithms (such as clustering, logistic regression, decision trees, and collaborative filtering) and neural network architectures (such as feedforward, recurrent, convolutional, and transformer). It also requires understanding how a model learns, including concepts such as gradient descent, loss function, regularization, etc.

With the availability of foundation models, ML knowledge is no longer a must-have for building AI applications. I’ve met many wonderful and successful AI application builders who aren’t at all interested in learning about gradient descent. However, ML knowledge is still extremely valuable, as it expands the set of tools that you can use and helps troubleshooting when a model doesn’t work as expected.

**On the Differences Among Training, Pre-Training,  Finetuning, and Post-Training**

Training always involves changing model weights, but not all changes to model weights constitute training. For example, quantization, the process of reducing the precision of model weights, technically changes the model’s weight values but isn’t considered training.

The term training can often be used in place of pre-training, finetuning, and post-training, which refer to different training phases:

Pre-training

_Pre-training refers to_ training a model from scratch—the model weights are randomly initialized. For LLMs, pre-training often involves training a model for text completion. Out of all training steps, pre-training is often the most resource-intensive by a long shot. For the InstructGPT model, pre-training takes up to  [98% of the overall compute and data resources](https://oreil.ly/G3LUh). Pre-training also takes a long time to do. A small mistake during pre-training can incur a significant financial loss and set back the project significantly. Due to the resource-intensive nature of pre-training, this has become an art that only a few practice. Those with expertise in pre-training large models, however, are heavily sought after.[24](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch01.html#id642)

Finetuning

Finetuning means continuing to train a previously trained model—the model weights are obtained from the previous training process. Because the model already has certain knowledge from pre-training, finetuning typically requires fewer resources (e.g., data and compute) than pre-training.

Post-training

Many people use  _post-training_  to refer to the process of training a model after the pre-training phase. Conceptually, post-training and finetuning are the same and can be used interchangeably. However, sometimes, people might use them differently to signify the different goals. It’s usually post-training when it’s done by model developers. For example, OpenAI might post-train a model to make it better at following instructions before releasing it. It’s finetuning when it’s done by application developers. For example, you might finetune an OpenAI model (which might have been post-trained itself) to adapt it to your needs.

Pre-training and post-training make up a spectrum.[25](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch01.html#id645)  Their processes and toolings are very similar. Their differences are explored further in Chapters  [2](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch02.html#ch02_understanding_foundation_models_1730147895571359)  and  [7](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch07.html#ch07).

Some people use the term training to refer to prompt engineering, which isn’t correct. I read a [_Business Insider_ article](https://oreil.ly/0VqmX) where the author said she trained ChatGPT to mimic her younger self. She did so by feeding her childhood journal entries into ChatGPT. Colloquially, the author’s usage of the word _training_ is correct, as she’s teaching the model to do something. But technically, if you teach a model what to do via the context input into the model, you’re doing prompt engineering. Similarly, I’ve seen people using the term _finetuning_ when what they do is prompt engineering.

#### Dataset engineering

_Dataset engineering_ refers to curating, generating, and annotating the data needed for training and adapting AI models.

In traditional ML engineering, most use cases are close-ended—a model’s output can only be among predefined values. For example, spam classification with only two possible outputs, “spam” and “not spam”, is close-ended. Foundation models, however, are open-ended. Annotating open-ended queries is much harder than annotating close-ended queries—it’s easier to determine whether an email is spam than to write an essay. So data annotation is a much bigger challenge for AI engineering.

Another difference is that traditional ML engineering works more with tabular data, whereas foundation models work with unstructured data. In AI engineering, data manipulation is more about deduplication, tokenization, context retrieval, and quality control, including removing sensitive information and toxic data. Dataset engineering is the focus of [Chapter 8](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch08.html#ch08_dataset_engineering_1730130932019888).

Many people argue that because models are now commodities, data will be the main differentiator, making dataset engineering more important than ever. How much data you need depends on the adapter technique you use. Training a model from scratch generally requires more data than finetuning, which, in turn, requires more data than prompt engineering.

Regardless of how much data you need, expertise in data is useful when examining a model, as its training data gives important clues about that model’s strengths and weaknesses.

#### Inference optimization

_Inference optimization_ means making models faster and cheaper. Inference optimization has always been important for ML engineering. Users never say no to faster models, and companies can always benefit from cheaper inference. However, as foundation models scale up to incur even higher inference cost and latency, inference optimization has become even more important.

One challenge with foundation models is that they are often  _autoregressive_—tokens are generated sequentially. If it takes 10 ms for a model to generate a token, it’ll take a second to generate an output of 100 tokens, and even more for longer outputs. As users are getting notoriously impatient, getting AI applications’ latency down to the  [100 ms latency](https://oreil.ly/gGXZ-)  expected for a typical internet application is a huge challenge.  Inference optimization has become an active subfield in both industry and academia.