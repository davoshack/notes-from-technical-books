## Querying in LlamaIndex

The following sections dive into how to implement advanced RAG techniques using LlamaIndex.

The querying process in LlamaIndex involves several key elements:

-   **Retrievers**: These classes fetch a collection of nodes from an index in response to a query. They are responsible for sourcing the relevant data from the indexes.
-   **Query Engine**: This core class processes a query and delivers a response object. The Query Engine compiles the final output using both the retrievers and response synthesizer modules.
-   **Query Transform**: This class is used to refine a raw query string through various transformations aimed at improving the retrieval process. It works together with a Retriever and a Query Engine.

Integrating these components leads to the creation of an efficient retrieval engine. The next sections show how to improve search results by adopting advanced techniques like query construction, expansion, and transformations.

### Query Construction

[Query construction](https://blog.langchain.dev/query-construction/)  in RAG is the process of converting user queries into a format compatible with various data sources. This involves converting questions into vector formats for unstructured data, enabling comparison with vector representations of source documents to identify the most relevant chunks. It is also applicable to structured data, such as databases, where queries are formulated in languages like SQL for effective data retrieval.

The core idea is to leverage the inherent structure of the data to address user queries. For instance, a query like “movies about aliens in the year 1980” combines a semantic element like “aliens” (better retrieved through vector storage) with a structured element like “year == 1980”. The process includes translating a natural language query into the specific query language of a database, whether it’s SQL for relational databases or Cypher for graph databases.

The implementation of query construction varies based on the use case. One approach involves  **MetadataFilter**  classes for vector stores, incorporating metadata filtering and an auto-retriever that converts natural language into unstructured queries. This requires defining the source, interpreting the user prompt, extracting conditions, and forming a request. Another approach is  **text-to-SQL**  for relational databases, where converting natural language into SQL requests faces challenges such as hallucinations (e.g., using non-existent tables or fields). This is managed by providing the LLM with an accurate database schema and using few-shot examples to guide the query generation.

Query Construction enhances the quality of answers produced by RAG by inferring logical filter conditions directly from user questions. The retrieved texts are refined before being passed to the LLM for the final answer synthesis.

>💡Query Construction is a process that translates natural language queries into structured or unstructured database queries, enhancing the accuracy of data retrieval.

### Query Expansion

Query expansion enhances the original query by adding related terms or synonyms. This technique is beneficial when the initial query is too specific or uses specialized terminology. By incorporating broader or more commonly used terms relevant to the subject, query expansion broadens the search’s scope. For example, with an initial query like “climate change effects,” query expansion might include adding synonymous or related phrases such as “global warming impact,” “environmental consequences,” or “temperature rise implications.” One method is to use the synonym_expand_policy function from the KnowledgeGraphRAGRetriever class.

### Query Transformation

Query transformations involve adjusting the original query to enhance its effectiveness in retrieving relevant information. These modifications can encompass alterations in the query’s structure, the incorporation of synonyms, or the addition of context.

For example, consider the user query, “_What were Microsoft’s revenues in 2021?_” To optimize this query for better performance in search engines and vector databases, it could be restructured to something more concise like  _“Microsoft revenues 2021”._  Query transformations involve changing the structure of a query to increase its performance.