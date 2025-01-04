# LangChain Introduction

LangChain is an open-source framework designed to simplify the development, productionization, and deployment of applications powered by large language models (LLMs). It ensures easy extensibility with third-party integrations and partner packages, allowing developers to adapt the framework to their specific needs. Additionally, it supports the creation of cognitive architectures through chains, agents, and advanced retrieval strategies.

LangChain offers a range of powerful features, such as abstractions and the LangChain Expression Language (LCEL), which allows users to easily compose and manage chains (sequences of modular components that process inputs and outputs to perform complex tasks). LangChain also introduced LangGraph to build more robust and stateful multi-actor applications. Furthermore, LangServe enables the deployment of LangChain chains as REST APIs, making the process of bringing applications into production much smoother. The broader LangChain ecosystem also includes LangSmith, a developer platform for debugging, testing, evaluating, and monitoring LLM applications.

One significant application of LangChain is in the area of retrieval-augmented generation (RAG). By integrating external knowledge sources, RAG systems can provide LLMs with relevant, factual information during the generation process. This ensures that the generated output is more accurate, reliable, and contextually appropriate.

LangChain provides useful abstractions for building RAG systems. With LangChain’s retrieval components, developers can easily integrate external data sources, such as documents or databases, into their LLM-powered applications. This allows the models to access and utilize relevant information during the generation process, enabling more accurate output.

To effectively utilize LangChain in building sophisticated LLM applications, it’s important to understand its key concepts and components, which range from basic functionalities to advanced features. These components work together to manage the flow of data, structure outputs, and optimize the performance of LLM-driven applications. Below, we break down LangChain’s key features into groups, beginning with basic functionalities and then gradually introducing more advanced components essential for setups like RAG. (While some of the following components were explained earlier, they are repeated here to help you understand their role as a part of a complete system.)

At the core of any interaction with language models are the basic elements that define how the model processes input and generates output. These foundational components ensure that LLMs can effectively interpret user prompts and return structured, meaningful responses.

-   **Prompts:**  LangChain provides tooling to create and work with prompt templates. These are predefined recipes that shape how queries are presented to the language model.
-   **Output Parsers:**  Output parsers are essential for converting the raw outputs from an LLM into structured formats, making the responses easier to work with or further process.

Once the basic input-output functionality is in place, the next step is to efficiently manage and handle documents, which often serve as the primary data source in many LLM applications. These components focus on loading, segmenting, and organizing documents for better interaction with the model.

-   **Document Loaders:**  Documents, along with their metadata, are loaded from a configured source via the Document Loader. This provides the raw material for language model interactions.
-   **Text Splitters:**  Since many documents can be too large for processing in a single query, text splitters divide them into manageable chunks. They can also combine and filter documents as needed to fit into LLM-friendly sizes.

With documents loaded and structured, there is a need for enhanced search and retrieval capabilities. These components allow for intelligent querying and retrieval of relevant information from large datasets or document collections, ensuring that the language model has access to pertinent data.

-   **Retrievers:**  Retrievers take a string query as input and return a list of relevant documents. LangChain integrates with advanced retrieval systems to make querying and searching highly efficient.
-   **Indexes:**  Indexes organize and store data in a way that enables quick and efficient search operations, allowing for more scalable and optimized retrieval processes.

For applications that require powerful search capabilities across large and unstructured datasets, embeddings come into play. These components convert textual data into high-dimensional vectors, enabling the use of vector searches and further improving the retrieval process.

-   **Embedding Models:**  LangChain’s Embeddings class interfaces with various text embedding models, such as OpenAI and Hugging Face, providing a uniform way to generate embeddings from text.
-   **Vector Databases:**  Vector databases are used to store (in what we usually call vector stores), index, and search embedded data. They are essential for performing fast and efficient vector searches, a common approach for managing unstructured data in applications like retrieval-augmented generation.

Once the system is set up to retrieve and embed data, more advanced features come into play. These include agents and chains, which allow for more complex workflows and decision-making within applications, enabling multi-step processes and tool integration.

-   **Agents:**  Agents are responsible for making decisions within a LangChain application. They define the plan of action, determining which components or processes should be used to achieve a goal.
-   **Chains:**  Chains integrate various components into a sequence, allowing for streamlined workflows. This might include connecting LLMs with prompt templates, memory, and output parsers, all within a single user-friendly interface.
-   **Tools:**  Tools are specialized functions that assist the language model in task completion. They can range from simple data-fetching methods, such as Google searches, to complex processes like database queries or running additional chains.

In any LLM-driven application, memory and monitoring are very important for both optimizing performance and improving user experience. These features help maintain context across interactions and allow for better debugging, logging, and analysis.

-   **Memory:**  This component stores past interactions with the language model, providing ongoing context for future interactions. It is particularly useful in maintaining coherent conversations over multiple exchanges.
-   **Callbacks:**  LangChain offers a callback system, allowing developers to monitor and log interactions at different stages of the application. This is invaluable for debugging, tracking performance, and enabling features like streaming results.

By organizing these components in a structured way, LangChain allows for a flexible, scalable, and powerful approach to building applications with language models. Starting with basic input-output handling and progressing to advanced retrieval, search, and decision-making capabilities, the framework provides everything necessary to build highly functional LLM-powered systems.

Throughout the book, we will cover each component and use it for building RAG-based applications. The next topic introduces LangChain agents, giving a quick glimpse into what is possible with them.

LangChain agents are decision-making components that dynamically choose actions or tools based on user inputs and task requirements. They allow the language model to interact with external systems, run multi-step workflows, and access resources like databases or APIs, enabling more complex and adaptive behavior in applications. Agents are key to automating processes, executing plans, and handling tasks beyond simple prompt-response interactions.

These agents complete tasks using chains, prompts, memory, and tools. They can perform diverse tasks, including executing steps in a predetermined sequence, interfacing with external systems such as Gmail or SQL databases, and more. In Chapter 10, we will discuss building agents in more depth.

Understanding the different types of agents available is crucial for tailoring applications to specific needs. LangChain offers a variety of agent types, each with specialized functions:

-   [Zero-shot ReAct](https://python.langchain.com/docs/modules/agents/agent_types/react)**:**  This agent utilizes the ReAct framework (Yao et al., 2022), which synergizes reasoning and acting in language models. ReAct enables the model to interleave reasoning traces (e.g., tracking action plans, handling exceptions) with task-specific actions (e.g., querying external sources like knowledge bases). This approach improves interpretability and reduces issues like hallucination by allowing the model to act and reflect simultaneously. In this “zero-shot” variant, the agent relies solely on the descriptions of tools to decide their usage without needing prior examples. Chapter 10 dives deeper into the concept of agents, including how to construct such systems using frameworks like ReAct.
-   [Structured Input ReAct](https://python.langchain.com/docs/modules/agents/agent_types/structured_chat)**:**  This agent manages tools that necessitate multiple inputs.
-   [OpenAI Functions Agent](https://python.langchain.com/docs/modules/agents/agent_types/openai_functions_agent): This agent is specifically developed for function calls for fine-tuned models and is compatible with advanced models such as gpt-3.5-turbo and gpt-4-turbo.
-   [Self-Ask with Search Agent](https://python.langchain.com/docs/modules/agents/agent_types/self_ask_with_search)**:**  This agent sources factual responses to questions, specializing in the “Intermediate Answer” tool. It is similar to the methodology in the original  [self-ask with search](https://ofir.io/self-ask.pdf)  research (Press et al., 2022).
-   [ReAct Document Store Agent](https://python.langchain.com/docs/modules/agents/agent_types/react): This agent combines the “Search” and “Lookup” tools to provide a continuous thought process.
-   [Plan-and-Execute Agents](https://blog.langchain.dev/plan-and-execute-agents/): This type formulates a plan consisting of multiple actions, which are then carried out sequentially. These agents are particularly effective for complex or long-running tasks, maintaining a steady focus on long-term goals. However, one trade-off of using these agents is the potential for increased latency.

Agents essentially determine the logic behind selecting an action and deciding whether to use multiple tools, a single tool or none, based on the task at hand. To further augment the functionality of agents, LangChain provides a suite of tools that integrate with external systems, enhancing their capabilities.

Some examples of these tools include  [the Python tool](https://python.langchain.com/docs/integrations/toolkits/python)  to generate and execute Python codes to answer a question,  [the JSON tool](https://python.langchain.com/docs/integrations/toolkits/json)  to interact with a JSON file that doesn’t fit in the LLM context window, and  [the CSV tool](https://python.langchain.com/docs/integrations/toolkits/csv)  to interact with CSV files.

[Custom tools](https://python.langchain.com/docs/modules/agents/tools/custom_tools)  enhance agents’ versatility, allowing them to be tailored for specific tasks and interactions. These tools offer task-specific functionality and flexibility for behaviors aligned with unique use cases.

The degree of customization is dependent on the development of advanced interactions. In such cases, tools can be coordinated to execute complex behaviors. Examples include generating questions, conducting web searches for answers, and compiling summaries of the information.