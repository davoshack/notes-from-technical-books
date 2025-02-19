## Querying in LlamaIndex

The following sections dive into how to implement advanced RAG techniques using LlamaIndex.

The querying process in LlamaIndex involves several key elements:

-   **Retrievers**: These classes fetch a collection of nodes from an index in response to a query. They are responsible for sourcing the relevant data from the indexes.
-   **Query Engine**: This core class processes a query and delivers a response object. The Query Engine compiles the final output using both the retrievers and response synthesizer modules.
-   **Query Transform**: This class is used to refine a raw query string through various transformations aimed at improving the retrieval process. It works together with a Retriever and a Query Engine.

Integrating these components leads to the creation of an efficient retrieval engine. The next sections show how to improve search results by adopting advanced techniques like query construction, expansion, and transformations.