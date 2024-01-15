---
sidebar_label: 'Retrieval'
sidebar_position: 4
---

# Retrieval

The subsection that allows you to specify how to obtain the augmented information sent to the context.

![image](https://github.com/genexus-books/Saia/blob/70e1a94dd086f9d9f0c376a648ff61c7a7dcb7bb/saia-docs/assets/images/RAGAssistantsSection5.png?raw=true)

## Retriever Type

It indicates the type of retriever used to obtain information; the default value is VectorStore.

The values it can take are: 

* ### HyDE

Specific retrieval method called [Hypothetical Document Embeddings](https://arxiv.org/abs/2212.10496) (HyDE). This method uses embedding 
techniques to take queries, generate hypothetical answers, embed the generated document, and use it as the final example. 

By default, the HyDE class comes with some default queries, but you can also create custom queries that must have a single input variable 
{question}.

* ### Contextual Compression

It tries to improve the answers returned from vector store document similarity searches by better taking into account the context from the 
query.

It wraps another retriever, and uses a Document Compressor as an intermediate step after the initial similarity search that removes 
information irrelevant to the initial query from the retrieved documents. It aims to reduce the amount of distraction a subsequent chain has 
to deal with when parsing the retrieved documents and making its final judgments.

* ### VectorStore

This is the default value, which directly uses the defined VectorStore without any further pre-processing.

* ### Self Query

It first queries itself to retrieve filter information based on the natural language query. It parses the response and processes it as a 
JSON structure; then it executes a second query to the LLM with the query and filters applied based on the first one. As a prerequisite, each 
document (and possibly chunks) needs to be ingested with associated filters applied.

* ### Multi Query

It automates the process of prompt tuning by using an LLM to generate multiple queries from different perspectives for a given user input 
query. For each query, it retrieves a set of relevant documents and takes the unique union across all queries to get a larger set of 
potentially relevant documents. By generating multiple perspectives on the same question, the MultiQueryRetriever might be able to overcome 
some of the limitations of the distance-based retrieval and get a richer set of results.

* ### Score Threshold

It uses what is called Recursive Similarity Search. With it, you can do a similarity search without having to rely solely on the 
“Document Count” value. The system will return all similar question matches based on the minimum score threshold configured 
(check below the associated parameter).

* ### Graph 

It enables the use of a graph-based information representation approach for retrieval.

## Retriever prompt

It specifies the query that is sent to the retriever to search for information. This query could be a question or a specific request.

When the following values are configured, this parameter is not taken into account:

* VectorStore
* scoreThreshold
* selfQuery

Clicking on the Set Default Prompt button automatically sets the Retriever Prompt to default values according to the option selected in 
"Retriever Type".

For example, if the value set in Retriever Type is graph, clicking on the Set Default Prompt button will populate Retriever prompt with the 
following:

```
Task: Generate Cypher statement to query a graph database.
Instructions:
Use only the provided relationship types and properties in the schema.
Do not use any other relationship types or properties that are not provided.

Schema:
{schema}

Note: Do not include any explanations or apologies in your responses.
Do not respond to any questions that might ask anything else than for you to construct a Cypher statement.
Do not include any text except the generated Cypher statement.

Examples: Here are a few examples of generated Cypher statements for particular questions:

# How many people played in Top Gun?
MATCH (m:Movie {{title:"Top Gun"}})<-[:ACTED_IN]-()
RETURN count(*) AS numberOfActors

The question is:
{question}
```
Below are the default values of "Retriever prompt" according to the value set in Retriever Type.

#### Hypothetical Document Embeddings
```
{
"prompt": "Please write a minimal passage to answer the question only\nQuestion: {question}\nPassage:"
}
```
The specific query for HyDE requesting a minimum passage to answer the question.

#### Contextual Compression
```
{
  "prompt": "Given the following question and context, extract any part of the context *AS IS* that is relevant to answer the question. If none of the context is relevant, return empty.\n> Question: {question}\n> Context:\n>>>\n{context}\n>>>\nExtracted relevant parts:"
}
```
The query for Contextual Compression that looks for relevant parts of the context that answer the question.

#### Multi Query
```
You are an AI language model assistant. Your task is
to generate {queryCount} different versions of the given user
question to retrieve relevant documents from a vector database.
By generating multiple perspectives on the user question,
your goal is to help the user overcome some of the limitations
of distance-based similarity search.

Provide these alternative questions separated by newlines between XML tags. For example:
<questions>
Question 1
Question 2
Question 3
</questions>

Original question: {question}
```
Multi Query generates 5 additional queries from different perspectives. Each generated query is used to retrieve a set of relevant 
documents from the configured vectorStore, and then the single join of all document sets is performed to obtain a larger set of potentially 
relevant documents. The queryCount parameter can be modified in the Profile Metadata section.

## Score Threshold

It defines the minimal valid value to consider the information as valid when retrieved from the VectorStore; otherwise, it is discarded. 
If there are no valid Documents, no interaction takes place with the LLM. The default value is 0.0.

## Profile Metadata
It may contain additional metadata related to the retriever's profile. 
The default value is an empty object {}.

In certain cases, it is convenient to be able to fine-tune each access to the LLM to obtain the desired result.

In particular, when historical information is used, an extra call is made to the LLM to summarize the user query and history into a new 
query that considers the history. By default, the same LLM Settings are used; however, this call can be configured in a specific way, and 
for this it is necessary to configure the desired values in the chat.search.llm parameter. For example, a valid configuration is:
```
{"chat":{"search":{"llm":{"provider":"openai","modelName":"gpt-3.5-turbo","maxTokens":1002,"cache":true}}}}
```
In the same way, depending on the configured retriever and prompt, another call to the LLM is made again. To specifically configure the LLM, it is necessary to use the chat.retriever.llm property. Note that, in this case, the queryCount value is also set to 3, which is specific to multiQuery.
```
{"chat":{"retriever":{"llm":{"provider":"openai","modelName":"gpt-3.5-turbo-16k","maxTokens":1003,"cache":true},"queryCount":3}}}
```
You can have it use both configurations. The resulting metadata is the union of the previous ones:
```
{"chat":{"search":{"llm":{"provider":"openai","modelName":"gpt-3.5-turbo","maxTokens":1002,"cache":true}},"retriever":{"llm":{"provider":"openai","modelName":"gpt-3.5-turbo-16k","maxTokens":1003,"cache":true},"queryCount":3}}}
```
Note that the values to be configured in the llm property are the same as those described in the LLM Settings section.

## Endpoint
URL address pointing to the specific server or service where the models or retrieval methods are hosted; the value is optional.





