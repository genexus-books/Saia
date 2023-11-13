---
sidebar_label: 'Chat with documents'
sidebar_position: 15
---

# Chat with documents

## Introduction

To enable the possibility of chatting or searching for information stored in documents, you first need to create search indexes. These indexes essentially allow you to group content within a context or area of knowledge. For example, you could have an index for legal matters, another for support, helpdesk, etc.

Once these indexes are created, you need to perform the content indexing process.

This will allow you to start conducting searches and answering questions through a conversational interface with the indexed content.

## Document Upload

### Metadata

When uploading documents, you can add extra metadata to be added to the chunks of documents. The format is `key/value` configuration for each value. For example:

```
type: 'legal'
year: 2003
```
The supported files are `txt`, `pdf`, `docx`, `epub`, `json`, `jsonl`, `csv`; any other extension will be treated as a text file.

Later on, during the [retrieval process](./apis/ChatWithDocumentsAPI.md#1-execute---execute-search-query), it is possible to filter against specific metadata.

## Indexes

It refers to ways to structure documents (unstructured data) so that LLMs can best interact with them. The most common way that indexes are used is in a `retrieval` step before interacting with a LLM.

Index configuration can be checked on the `Search Documents` console section. There is a list of `Search Profiles` and each one with the possibility to use a specific index. You can also check and modify this information with the associated [API](./SearchIndexProfile.md).

## Chunks

LLMs are often limited by the amount of text that you can pass to them. Therefore, it is necessary to split them up into smaller chunks. The default chunking configuration is as follows:

 * `chunkSize`: The maximum number of characters in each chunk. The default value is 1000 characters.
 * `chunkOverlap`: The number of overlapping characters between adjacent chunks. The default value is 200 characters.

```
{
    "chunks":{
        "chunkSize":1000,
        "chunkOverlap":100
    },
    "extra_chunks":[]
}
```

You can change the default for your index and also create several index strategies to test which is the best chunking strategy for you. Use the `extra_chunks` element to further add chunks. The following configuration adds two extra configuration.

```
{
    "chunks":{
        "chunkSize":1000,
        "chunkOverlap":100
    },
    "extra_chunks":[
        {
            "chunkSize":500,
            "chunkOverlap":80
        },
        {
            "chunkSize":3000,
            "chunkOverlap":100
        }
    ]
}
```

In this case, each document will be chunked three times and ingested separately. Later on, when querying you can change the default namespace appending the extra `chunkSize` value. 
