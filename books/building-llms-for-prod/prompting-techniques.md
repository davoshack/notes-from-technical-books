## Prompting Techniques
### Zero-Shot Prompting
**Zero-shot prompting** is when a model is asked to produce a specific output without examples demonstrating the task, just the instructions. Many tasks are well within large language models’ capabilities, allowing them to provide excellent outcomes even without examples or in-depth guides. Here’s an example of zero-shot prompting, where the LLM was asked to write a short poem about the summer:
```
prompt_system = """ You are a helpful assistant whose goal is to write short poems."""

prompt = """Write a short poem about summer."""
```
> In this case, the model could generate the poem in any style. However, if you want the poem in a specific style, the prompt must include a clear description or example.

### In-Context Learning and Few-Shot Prompting
In-context learning is a technique where the model leverages examples or demonstrations provided directly within the prompt to perform a task without any additional training. Few-shot prompting is a specific type of in-context learning where the model is given a small set of examples—typically two to five—that help it generalize and adapt to the task at hand. This approach allows the model to handle more complex or nuanced tasks by learning from just a few relevant instances within the prompt.

Unlike zero-shot prompting, where the model generates responses for tasks it has never seen before, few-shot prompting enhances performance by offering in-context examples as guides. The few-shot prompt typically consists of a few input-output pairs that illustrate how the task should be performed, enabling the model to mimic the patterns it observes and apply this knowledge to new, similar tasks.

Let’s test this technique with an example.
```
prompt_system = """ You are a helpful assistant whose goal is to write short poems."""

prompt = """Write a short poem about summer, based on the given examples."""

Example = {"nature": """Birdsong fills the air,\nMountains high and valleys \ deep,\nNature's music sweet.""",
"winter": """Snow blankets the ground,\nSilence is the only \ sound,\nWinter's beauty found."""}
```
In the following example, we instruct the LLM to identify the emotion linked to a specific color. This is possible by providing a set of examples illustrating color-emotion associations.
```
prompt = """ Here are some examples of colors and the emotions associated with them:
Color: Red, Emotion: Passion
Color: Blue, Emotion: Serenity
Color: Green, Emotion: Tranquility
"""
```
```
prompt = """Now, given a new color, identify the emotion associated with it: {"Color": "purple", "Emotion":"""
```
> This prompt provides clear instructions and several examples to help the model understand the task.

While **few-shot learning** is effective, it encounters challenges, mainly when tasks are complex. More advanced strategies, like chain-of-thought prompting, become increasingly valuable in such cases. This technique breaks down complex problems into simpler phases, offering examples for each stage and enhancing the model’s logical reasoning capacity.

### Role Prompting
Role prompting involves instructing the LLM to assume a specific role or identity for task execution, such as functioning as a copywriter. This instruction can influence the model’s response by providing context or perspective for the task. When working with role prompts, the iterative process involves defining the role in the prompt, such as specifying, “As a copywriter, create engaging catchphrases for AWS services.” Next, the prompt is utilized to generate a response from an LLM. The response is then evaluated, and the prompt is refined as needed for improved outcomes.

While system prompts used in the above examples provide a foundational way to set the tone, behavior, or goals for an AI model, role prompting offers more granular control by allowing for adjustments within that context. For instance, within a system prompt that sets the AI as a helpful assistant, role prompting could have the AI act as a technical expert, a customer service representative, or a creative writer, depending on the specific needs of the conversation.

Let’s test this with a comparative example where we generate engaging catchphrases for AWS with and without a role prompt:

Without any role or system prompts used:
```
prompt = "Create engaging catchphrases for AWS services."
```
Now, with a system prompt:
```
system_prompt = "You are an expert copywriter specializing in cloud computing and AWS services. Your responses should demonstrate a deep understanding of AWS’s products, focus on innovation, scalability, and reliability, and create compelling, creative catchphrases that resonate with a tech-savvy audience. Always aim to highlight AWS’s strengths and competitive advantages."
```
```
prompt = "As a copywriter, create engaging and innovative catchphrases for AWS services."
```
> As demonstrated, incorporating a role prompt significantly enhances the relevance and creativity of responses, especially in specialized areas like AWS services. This approach allows users to leverage the full potential of LLMs, producing more precise, impactful outcomes compared to general prompts.

### Chain Prompting
Chain Prompting involves linking a series of prompts sequentially, where the output from one prompt serves as the input for the next. When implementing chain prompting, start by identifying and extracting relevant information from the generated response. This extracted information is then used to craft the subsequent prompt, ensuring it builds logically on the previous answer. The process continues until the intended result is achieved.

Let’s test this technique with an example.
```
prompt 1 = "What is the name of the famous scientist who developed the theory of general relativity?"

prompt 2 = "Provide a brief description of that scientist's theory of general relativity."
```

### Chain-of-Thought Prompting
Chain-of-Thought Prompting (CoT) is a method designed to prompt large language models to articulate their thought processes, enhancing the accuracy of the results. This technique involves presenting examples that showcase the reasoning process, guiding the LLM to explain its logic while responding to prompts. CoT has proven beneficial for arithmetic, common-sense reasoning, and symbolic thinking tasks.

CoT offers several advantages. Firstly, it simplifies complex tasks by enabling the LLM to break down challenging problems into more manageable steps. This feature is valuable for tasks requiring calculations, logical analysis, or multi-step reasoning. Secondly, CoT can guide the model through a series of related prompts, fostering more coherent and contextually appropriate outputs. This can result in more precise and practical responses, especially in tasks requiring a thorough understanding of the problem or subject matter.

However, there are limitations to CoT that should be considered. One limitation is that it is effective primarily with models with around 100 billion parameters or more (as of August 2024). Smaller models often produce nonsensical thought processes, reducing accuracy compared to traditional prompting methods. Another limitation is that CoT’s effectiveness varies across different types of tasks. While it shows significant benefits for tasks involving arithmetic, common sense, and symbolic reasoning, its impact on other tasks might be less meaningful.

Here’s a comparative example demonstrating how CoT prompting can optimize results and help generate accurate responses with a text model like ChatGPT.

In a standard prompt, when the model is given an example such as, “Roger had 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? The answer is 11,” it is then asked to answer the following question: “The cafeteria had 23 apples. If they used 20 to make lunch and bought 6 more, how many apples do they have?” The model incorrectly responds with “27.”

However, using the Chain-of-Thought (CoT) technique, if you prompt the model with: “Roger had 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? Answer: Roger started with 5 balls. Two cans of 3 tennis balls each add 6 more balls, so 5 + 6 = 11. The answer is 11,” the model then correctly responds to the next question: “The cafeteria had 23 apples originally. They used 20 to make lunch, leaving 3 apples. After buying 6 more, they now have 3 + 6 = 9 apples. The answer is 9.”