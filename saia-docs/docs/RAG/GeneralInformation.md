---
sidebar_label: 'Configuration - General Information'
sidebar_position: 4
---
# Configuration - General Information

![image](https://github.com/genexus-books/Saia/blob/25b9adecc75e4453e71780bc32e6ff7f37151de1/saia-docs/assets/images/RAGAssistantsSection3.png?raw=true)

## Name
Identifying name of the assistant that can be customized according to your preferences.

## Description
Detailed description of the purpose and capabilities of your assistant. This is a place to provide information about how the assistant 
enhances the end user experience.

## Status
Indicates whether the assistant is enabled or disabled for use. 

## Embeddings Settings
Specific parameters related to embeddings and related model characteristics:

### Provider Name
 
Determines the language model service provider used by your RAG assistant. 

This is a mandatory parameter that can take any of the following values:

* azureopenai: To use Azure OpenAI.
* cohere: To use Cohere.
* fake: A fictitious value that could be useful for testing or simulation purposes.
* googlevertexai: To use Google Vertex AI.
* huggingface: To use Hugging Face.
* openai: To use OpenAI.

### Model Name

Specific name of the model being used. For example, if the Provider parameter has the "openai" value, modelName takes the value 
'text-embedding-ada-002'.

### apiKey

API authentication key provided to access the language model service.  For example, if the Provider parameter is "azureopenai", you must 
specify "apiKey" with the authentication key.

### Type

It allows configuring the information processing approach. 

The default value is similarity, which uses a standard similarity approach. It is effective for general information search and retrieval tasks.

### Endpoint

The URL pointing to the language model service.

### Dimensions 

It allows configuring the index dimensionality in the vectorStore. Each model has a specific dimension, and the default value is 1536, 
associated with the OpenAI provider.
