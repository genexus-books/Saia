---
sidebar_label: 'Prompts'
sidebar_position: 14
---

# Prompt Management

## Introduction

Mastering `Prompt Engineering` is a crucial skill needed for shaping precise and contextually accurate AI responses, ensuring optimal performance and user satisfaction.

Incorporating `{variables}` within prompts allows for dynamic content substitution at runtime, enabling the system to respond contextually to user inputs and enhancing the overall conversational experience.

As use cases it is very common to add `context`, `user`, `metadata` and other information that should be added to the prompt in specific places and will vary, and it to be composable from the caller. From a Prompt Perspective is to inject data with the desired values at certain locations.

## Design

Use the `{variableName}` pattern as placeholder for variable substitution in any place a Prompt is used, alphanumeric characters including underscore and hyphens are supported.

From the API caller, the developer will not only need to define the prompt strategy on where to place the variables definition but to fill-in with the associated data at runtime.

The Assistants API supports the usage of an optional `variables` collection definition using the `key`(string)/`value`(string) pattern for each element. This allows for dynamic substitution of variable definitions within a Prompt.

When sending JSON values in the `value` element, it's important to use the correct notation, which involves using an extra set of delimiters. For JSON objects, use curly brackets, and for JSON arrays, use square brackets. Notice there is a slight difference when using [Assistant](./apis/AssistantsAPI.md#post-chat) vs [Search Profiles](./apis/ChatWithDocumentsAPI.md#saia-chat-with-documents-api), where the later needs to use double curly brackets.

Below are examples of valid values:

### String case

```json
{"key": "type", "value": "AI assistant"}
```

### Array case

```json
# Assistant sample
{"key": "properties", "value": "[{\"name\": \"something\", \"value\": \"something else\"}]"}
# Search Profile sample
{"key": "properties", "value": "[{{\"name\": \"something\", \"value\": \"something else\"}}]"} 
```

### Object case

```json
# Assistant samplecase
{"key": "item", "value": "{\"name\": \"something\"}"}
# Search Profile sample
{"key": "item", "value": "{{\"name\": \"something\"}}"}
```

The supported endpoints are:

* [/v1/assistant/chat](./apis/AssistantsAPI.md#post-chat)
* [/v1/search/execute](./apis/ChatWithDocumentsAPI.md#saia-chat-with-documents-api)

The following variable names are reserved: `{inputText}`, `{context}`, `{chat_history}`, `{question}`.

## Use case

The following prompt definition could be used from any Assistant:

```
You are a {type}. Use the following pieces of context to answer the question at the end.
{disclaimer}
{hardDisclaimer}

{context}

Question: {question}
{responseHint}
```

Notice the usage of the `{type}`, `{disclaimer}`, `{hardDisclaimer}`, `{responseHint}` variables which will be substituted at runtime.

 * `{type}`: helpful AI assistant
 * `{disclaimer}`: If you don't know the answer, just say you don't know. DO NOT try to make up an answer.
 * `{hardDisclaimer}`: If the question is not related to the context, politely respond that you are tuned to only answer questions that are related to the context.
 * `{responseHint}`: Helpful answer in markdown:

Using the associated APIs, the optional `variables` element could be filled as follows:

```json
{
  ...
  "variables": [
      {"key": "type","value": "helpful AI assistant"},
      {"key": "disclaimer","value": "If you don't know the answer, just say you don't know. DO NOT try to make up an answer."},
      {"key": "hardDisclaimer","value": "If the question is not related to the context, politely respond that you are tuned to only answer questions that are related to the context."},
      {"key": "responseHint","value": "Helpful answer in markdown:"}
  ]
}
```

The resulting prompt after the variables substitution process, the `{context}` and `{question}` variables are left unchanged:

```
You are a helpful AI assistant. Use the following pieces of context to answer the question at the end.
If you don't know the answer, just say you don't know. DO NOT try to make up an answer.
If the question is not related to the context, politely respond that you are tuned to only answer questions that are related to the context.

{context}

Question: {question}
Helpful answer in markdown:
```

and the Assistant execution process will continue as usual.

## Considerations

The parameter substitution is applied to all the involved prompts; when using the Search&Chat assistant it depends on the [retriever](./Retrieval.md) used.

Parameter substitution is `Case Insensitive`.

No validation is done on the server-side if some variables are not substituted.

Use the [Console Requests](./Backoffice.md#requests) section to review the resulting prompt.
