# Understanding Fine-Tuning

While pre-training gives large language models (LLMs) a general understanding of language, it falls short for instruction-following tasks. LLMs are trained to predict the next token in its training data (often web pages) and this doesn’t necessarily make it immediately capable of answering questions or following instructions. For example, if a user types in a regular Google search or a pre-trained LLM, like “In what country is Montreal?”, rather than generating an answer the LLM might instead generate lists of similar questions with similar meanings. Such a model would struggle to give good answers to requests like summarizing a web page or generating SQL queries. Fine-tuning is a method to address these limitations and specialize a model at understanding the format of user requests and the types of tasks it needs to perform.

Fine-tuning resumes training a pre-trained model to increase the performance of a specific task using task-specific data like question-answer pairs for general systems like ChatGPT or Claude. This allows the model to adjust its internal parameters and representations to better suit the task, thus improving its ability to tackle domain-specific issues.

**Instruction fine-tuning**  is a strategy popularized by OpenAI in 2022 with their  [InstructGPT models](https://arxiv.org/pdf/2203.02155.pdf). It gives LLMs the capacity to follow written human instructions and thus increases the level of control over the model’s outputs. The goal is to train an LLM to interpret prompts as instructions rather than just input for general text completion/generation.

However, standard fine-tuning for LLMs can be resource-heavy and expensive. It requires modifying all parameters in the pre-trained models, often in billions. Therefore, using more efficient and cost-effective fine-tuning techniques, such as Low-Rank Adaptation, is essential.

Several techniques are available to improve the performance of LLMs:

-   **Standard Fine-Tuning:**  It adjusts all the parameters in LLM to increase performance to a specific task. Although effective, it demands extensive computational resources, making it less valuable.
-   **Low-Rank Adaptation (LoRA):**  It modifies only a small subset of parameters by applying low-rank approximations on the lower layers of LLMs. This is a more efficient approach, significantly reducing the number of parameters that need training. LoRA reduces GPU memory requirements and lowers training costs.
-   **Supervised Fine-Tuning (SFT):**  It trains a base model on a new dataset under supervision. This new dataset typically includes demonstration data, prompts, and corresponding responses. The model learns from this data and generates responses that align with the expected outputs. SFT can be used for instruction fine-tuning.
-   **Reinforcement Learning from Human Feedback (RLHF):**  It iteratively trains models to align with human feedback. This approach can be more effective than SFT as it facilitates continuous improvement based on human input. Similar methodologies include Direct Preference Optimization (DPO) and Reinforcement Learning from AI Feedback (RLAIF).
