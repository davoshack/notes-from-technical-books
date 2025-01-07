# LlamaIndex Introduction

• Find the  [Notebook](https://colab.research.google.com/github/towardsai/ragbook-notebooks/blob/main/notebooks/Chapter%2005%20-%20LlamaIndex_Introduction.ipynb)  for this section at  [towardsai.net/book](http://towardsai.net/book).

LlamaIndex, like other LLM tooling frameworks, allows for the easy creation of LLM-powered apps with useful and straightforward abstractions. LlamaIndex makes it simple to build RAG-based applications by combining extracting relevant information from large databases with the text generation capabilities of LLMs. This section provides an overview of LlamaIndex and some essential concepts. RAG systems will be covered in more depth in Chapters 8 and 9.

## Data Connectors

The performance of RAG-based applications is notably improved when they access a vector store compiling information from multiple sources. However, handling data in various formats presents particular challenges. Data connectors, known as Readers, play a crucial role in addressing this. They parse and convert data into a more manageable format, which includes text and basic metadata, and simplify the data ingestion process. They automate data collection from different sources, including APIs, PDFs, and SQL databases, and effectively format this data.

The open-source project  [LlamaHub](https://llamahub.ai/)  hosts various data connectors to incorporate multiple data formats into the LLM.

You can check out some of the loaders on the  [LlamaHub](https://llamahub.ai/)  GitHub repository, including the  [Wikipedia](https://llamahub.ai/l/readers/llama-index-readers-wikipedia?from=)  integration used in the example.