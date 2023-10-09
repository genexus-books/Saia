---
sidebar_label: 'Search Index Profiles'
sidebar_position: 16
---

# Search Index Profiles

## Introduction

All the interaction with the Search and Chat component is configured through a Search Index Profile. A default is created during initialization and later it can be modified or new ones created to change the assistant behavior.

It can be separated in the following sections:

 * `Prompt`: configuration on how the system will be prompted.
 * `LLM and Embeddings`: specific configuration on what LLM will be used, and associated parameters.
 * `History, Document Count, Scores`: Number of conversations and documents to query and associated scores.
 * `Retrievers`: How the context will be augmented.

## Prompt

The `Profile Prompt` directs on how to use the LLM, the default value is:

```
You are a helpful AI assistant. Use the following pieces of context to answer the question at the end.
If you don't know the answer, just say you don't know. DO NOT try to make up an answer.
If the question is not related to the context, politely respond that you are tuned to only answer questions that are related to the context.

{context}

Question: {question}
Helpful answer in markdown:
```

Keep the `{context}` and `{question}` variables set; because they will be substituted with the associated information before the interaction.

The `Profile Template` applies when the `History Count` property is higher than 0; it will compact the previous interactions not to loose valuable context data.

The default value is:

```
Given the following conversation and a follow up question, rephrase the follow up question to be a standalone question.

Chat History:
{chat_history}
Follow Up Input: {question}
Standalone question:
```

## LLM and Embeddings

The LLM configuration parameters can be changed using the following pseudo-json structure:

```json
{
  "provider": "string",
  "apiKey": "string",
  "modelName": "string",
  "temperature": "decimal",
  "maxTokens": "integer",
  "topP": "decimal",
  "n": "integer",
  "stream": "boolean",
  "verbose": "boolean",
  "cache": "boolean",
  "frequencyPenalty": "decimal",
  "presencePenalty": "decimal"
}
```

Currently the text/chat [OpenAI models](https://platform.openai.com/docs/models/) can be used with the following [parameters](https://platform.openai.com/docs/api-reference/chat).

The provider element can be configured with the following:

 * `openai`
 * `azureopenai`

The embeddings section must match the `Provider` used, for `OpenAI` the following configuration applies:

```
{
  provider: 'openai',
  modelName: 'text-embedding-ada-002'
}
```

[More Information](https://platform.openai.com/docs/api-reference/embeddings)

### Azure OpenAI 

When using the `azureopenai` provider the following properties are mandatory for the LLM configuration: `apiKey` and `endpoint`. The `endpoint` is configured in the associated [Search Profile](#history-document-count-scores).

```json
// LLM Settings
{
  "provider": "azureopenai",
  "apiKey": "somevalue"
}
// Endpoint Sample
https://{name}.openai.azure.com/openai/deployments/{deployment}/chat/completions?api-version={version}
```

## History, Document Count, Scores

The `History Count` enables the usage of chat mode keeping in-context the latest interactions. It is used in conjunction with the `Profile Template`.

The `Document Count` defines how many documents are retrieved from the vectorestore to augment the context.

The `Score Threshold` (defaults to 0.0) defines the minimal valid value to consider the information as valid when retrieved from the vectorstore, otherwise is discarded. In case there are no valid `Documents`, no interaction is done with the LLM. The following return value is expected:

```json
{"result":{"messages":["Score threshold not met"],"success":false},"text":""}
```

## Retrievers

It specifies how is obtained the augmented information sent to the context. Previously you need to [upload documents](Documents.md).

### VectorStore

This is the default value, directly uses the defined `VectorStore` without any further pre-processing.

### Hypothetical Document Embeddings (HyDE)

Implements Hypothetical Document Embeddings as detailed [here](https://arxiv.org/abs/2212.10496). It is an embedding technique that takes queries, generates a hypothetical answer, and then embeds that generated document and uses that as the final example. the default prompt is:

```
Please write a minimal passage to answer the question only 
Question: {question}
Passage:
```

### Contextual Compression

Tries to improve the answers returned from vector store document similarity searches by better taking into account the context from the query.

It wraps another retriever, and uses a Document Compressor as an intermediate step after the initial similarity search that removes information irrelevant to the initial query from the retrieved documents. It aims to reduce the amount of distraction a subsequent chain has to deal with when parsing the retrieved documents and making its final judgments.

The initial prompt is as follows:

```
Given the following question and context, extract any part of the context *AS IS* that is relevant to answer the question. If none of the context is relevant return empty.

Remember, *DO NOT* edit the extracted parts of the context.

> Question: {question}
> Context:
>>>
{context}
>>>
Extracted relevant parts:
```

### Self Query

If first query itself to retrieve filter information based on the natural language query. It parses the response and processes it as a JSON structure; then it executes a second query to the LLM with the query and filters applied based on the first one. As a pre-requisite; each document (and possibly chunks) needs to be ingested with associated filters applied.

The prompt cannot be overridden, it uses the following one:

```
Your goal is to structure the user's query to match the request schema provided below.

<< Structured Request Schema >>
When responding use a markdown code snippet with a JSON object formatted in the following schema:

{{
    \"query\": string \\ text string to compare to document contents
    \"filter\": string \\ logical condition statement for filtering document
}}

The query string should contain only text that is expected to match the contents of documents. Any conditions in the filter should not be mentioned in the query as well.

A logical condition statement is composed of one or more comparison and logical operation statements.

A comparison statement takes the form: `comp(attr, val)`:
- `comp` (eq |  | gt | gte | lt | lte): comparator
- `attr` (string):  name of attribute to apply the comparison to
- `val` (string): is the comparison value

A logical operation statement takes the form `op(statement1, statement2, ...)`:
- `op` (and | or): logical operator
- `statement1`, `statement2`, ... (comparison statements or logical operation statements): one or more statements to apply the operation to
Make sure that you only use the comparators and logical operators listed above and no others.
Make sure that filters only refer to attributes that exist in the data source.
Make sure that filters take into account the descriptions of attributes and only make comparisons that are feasible given the type of data being stored.
Make sure that filters are only used as needed. If there are no filters that should be applied return \"NO_FILTER\" for the filter value.
```

### Advanced

The `Metadata` field is specifically used with `Self Query` to describe the available filter values. For example if the data is ingested with specific tags such as `year`, `quarter`; a possible metadata definition could be:

```
[{
  "name": "year",
  "description": "The year of the document",
  "type": "integer"
 },
 {
  "name": "quarter",
  "description": "The quarter of the document",
  "type": "integer"
 }
]
```

### Multi Query

It automates the process of prompt tuning by using an LLM to generate multiple queries from different perspectives for a given user input query.
For each query, it retrieves a set of relevant documents and takes the unique union across all queries to get a larger set of potentially relevant documents.
By generating multiple perspectives on the same question, the MultiQueryRetriever might be able to overcome some of the limitations of the distance-based retrieval and get a richer set of results.

### Score Threshold

It uses what is called `Recursive Similarity Search`. With it, you can do a similarity search without having to rely solely on the `k` value. The system will return all similar question matches based on the minimum `score threshold` configured.
