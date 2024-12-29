### Prompting and Prompt Engineering
Whether using or building AI, you must know how best to communicate with it. This is especially true for generative AI models, which primarily interact with the user through language input. Users can ask the model to perform a specific task by providing a textual description or instruction. In a broad sense, this input from users is a “prompt.” Prompting is how humans can talk to AI. It is a way to tell an AI agent what we want and how we want it using human language.

Prompt engineering is a discipline that creates and optimizes prompts to leverage language models across various applications and research areas. A prompt engineer will translate your idea from your regular conversational language into more precise and optimized instructions for the AI. At its core, prompting presents a specific task or instruction to the language model, which responds based on the information in the prompt. A prompt can be a simple question or a more complex input with additional context, examples, and information to guide the model in producing the desired outputs. The effectiveness of the results largely depends on the precision and relevance of the prompt.

Before going further into prompting, let’s distinguish **prompts** and s**ystem prompts**. Below, we explore this with two different use cases: story generation and product description. In both examples, we highlight the distinction between a system prompt (which directs the model on how to behave) and a user prompt (which provides the specific task). This distinction is unique to the OpenAI API, which employs a system prompt to guide the model’s behavior, while other LLMs typically rely on a single, uniform prompt. Still, it is essential to understand and leverage system prompts for optimal results.

In this first example, the system prompt establishes the model’s role, instructing it to act as a helpful assistant for story writing. The user prompt then sets up the beginning of a story, providing an initial context that introduces a talking-animal world and a brave protagonist, Benjamin, the mouse. The objective is for the model to continue and expand upon the story based on this setup.


```
system_prompt = "You are a helpful assistant whose goal is to help write stories."

prompt = """Continue the following story. Write no more than 50 words.
Once upon a time, in a world where animals could speak, a courageous mouse named Benjamin decided to"""
```

_In this case, the system prompt guides the model to act as a story-writing assistant, while the user prompt provides the specific task of continuing the story within a given word limit._

Next, we apply the same structure to a product description. Here, the system prompt sets the model’s behavior to help with writing product descriptions, and the user prompt provides specific product details, including characteristics like “luxurious,” “handcrafted,” and “limited edition.” The objective is to create a concise product description that highlights these attributes.


```
system_prompt = """You are a helpful assistant whose goal is to help write product descriptions."""

prompt = """Write a captivating product description for a luxurious, handcrafted, limited-edition fountain pen made from rosewood and gold.
Write no more than 50 words."""
```
_Here, the system prompt ensures the model takes on the role of a product description assistant, while the user prompt provides essential product characteristics, resulting in a focused, compelling output._