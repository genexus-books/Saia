---
sidebar_label: 'Document Indexing'
sidebar_position: 5
---

# Document Management

## Introduction

Before using the Search and Chat option, you need to upload documents to be ingested in a vectorstore database. Internally this process called `Ingestion` will create an index over the data.

The supported files are `txt`, `pdf`, `docx`, `epub`, `json`, `jsonl`, `csv`; any other extension will be treated as a text file.

## Indexes

It refers to ways to structure documents (unstructured data) so that LLMs can best interact with them. The most common way that indexes are used is in a `retrieval` step before interacting with a LLM.

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

In this case, each document will be chunked three times and ingested separately. Later on, when querying you can change the default namespace appending the extra `chunkSize` value. You can use the `Playground` to change this configuration and evaluate the best fit.

## Document Upload

### Metadata

When uploading documents, you can add extra metadata to be added to the chunks of documents. The format is `key/value` configuration for each value. For example:

```
type: 'legal'
year: 2003
```

Later on, during the retrieval process, it is possible to filter against specific metadata.