---
sidebar_label: 'RAG Assistants API'
sidebar_position: 3
---

# GeneXus Enterprise AI RAG Assistants API

This API allows you to define different RAG Assistants.

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

Check the parameters explanation [here](../RAG/RAGAssistantsSection.md).

> The following endpoints require a GeneXus Enterprise AI API token related to **project** scope.

## Endpoints

Below is a summary of the available endpoints for this API:

| Method | Path                   | Description                      |
| ------ | ---------------------- | -------------------------------- |
| GET    | /v1/search/profiles                 | Get all Project profiles |
| GET    | /v1/search/profile/{name}           | Get a specific profile   |
| POST   | /v1/search/profile                  | Create a new profile     |
| PUT    | /v1/search/profile/{name}           | Update a profile         |
| DELETE | /v1/search/profile/{name}           | Delete a profile         |
| GET    | /v1/search/profile/{name}/documents | Get documents for a profile |
| GET    | /v1/search/profile/{name}/document/{id} | Retrieve Document information |
| POST   | /v1/search/profile/{name}/document  | Uploads a Document |
| DELETE | /v1/search/profile/{name}/document/{id} | Delete a Document |
| POST   | /v1/search/execute                  | Execute a search query   |

## GET /profiles

Retrieve all the RAG Assistants for a Project.

### Response

```json
{
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "searchProfiles": [
    {
      "description": "string",
      "name": "string"
    },
    ...
  ]
}
```

The `description` parameter is required for the [chat](ChatWithDocumentsAPI.md) option.

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profiles" \
  -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
  -H "Accept: application/json"
```

## GET /v1/search/profile/{name}

Get RAG Assistants `{name}` details.

### Response

```json
{
  "name": "string",
  "description": "string",
  "indexOptions": {
    "chunks": {
      "chunkOverlap": "integer",
      "chunkSize": "integer"
    }
  },
  "searchOptions": {
    "historyCount": "integer",
    "llm": {
      "cache": "boolean",
      "frequencyPenalty": "decimal",
      "maxTokens": "integer",
      "modelName": "string",
      "n": "integer",
      "presencePenalty": "decimal",
      "provider": "string",
      "stream": "boolean",
      "temperature": "decimal",
      "topP": "decimal",
      "verbose": "boolean"
    },
    "retriever": {
      "type": "string" /* vectorStore, selfQuery, hyde, contextualCompression */
    },
    "search": {
      "k": "integer",
      "prompt": "string",
      "returnSourceDocuments": "boolean",
      "scoreThreshold": "decimal",
      "template": "string"
    }
  },
  "status": "integer" /* 1:Enabled, 2:Disabled */
}
```

The `type` parameter is explained [here](../RAG/Retrieval.md).

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/profile

Creates a RAG Assistant.

### Request Body

```json
{
  "name": "string", /* Required */
  "description": "string",
  "searchOptions": {
    "historyCount": "integer",
    "llm": {
      "cache": "boolean",
      "frequencyPenalty": "decimal",
      "maxTokens": "integer",
      "modelName": "string",
      "n": "integer",
      "presencePenalty": "decimal",
      "provider": "string",
      "stream": "boolean",
      "temperature": "decimal",
      "topP": "decimal",
      "verbose": "boolean"
    },
    "search": {
      "k": "integer",
      "prompt": "string",
      "returnSourceDocuments": "boolean",
      "scoreThreshold": "decimal",
      "template": "string"
    },
    "retriever": {
      "type": "string", /* vectorStore, selfQuery, hyde, contextualCompression */
      "prompt": "string" /* not needed when using vectorStore */
    }
  },
  "indexOptions": {
    "chunks": {
      "chunkOverlap": "integer",
      "chunkSize": "integer"
    }
  }
}
```

### Response

Equivalent to the [Get Response](#get-v1searchprofilename).

### CURL Example

```shell
curl -X POST "$BASE_URL/v1/search/profile" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "name": "my Search Profile",
      "description": "My awesome profile",
      "searchOptions": {
        "historyCount": 2,
        "llm": {
          "temperature": 0.1,
          "maxTokens": 1500,
          "modelName": "gpt-3.5-turbo-16k"
        },
        "search": {
          "k": 5
        }
      }
    }'
```

## PUT /v1/search/profile/{name}

Update a RAG Assistants.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile Name (required) |

### Request Body

Equivalent to the [Post Request](#post-v1searchprofile), but the following elements are discarded:

 * `name` element
 * `indexOptions` section

In addition, `status` element can be specified taking values 1:Enabled, 2:Disabled.

### Response

Equivalent to the [Get Response](#get-v1searchprofilename).

### CURL Example

```shell
curl -X PUT "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "description": "Updated Search Profile",
      "status": 1,
      "searchOptions": {
        "historyCount": 4,
        "llm": {
          "temperature": 0.5,
          "maxTokens": 1000
        },
        "search": {
          "k": 2,
          "prompt": "You are an Assistant, only reply using the following context:\n{context}\n Question is: {question}\n",
          "scoreThreshold": 0.2
        }
      }
}'
```

## DELETE /v1/search/profile/{name}

Delete a RAG Assistants.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile Name (required) |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise StatusCode `400*` with a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## GET /profile/{name}/documents

List the documents for a RAG Assistant.

### Parameters

| Name  | Type    | Description       |
|-------|---------|-------------------|
| name  | string  | Profile name      |
| skip  | integer | Number of documents to skip |
| count | integer | Number of documents to return (defaults to 10) |

### Response

```json
{
{
  "documents": [
    {
      "extension": "string",
      "id": "string",
      "name": "string",
      "timestamp": "timestamp",
      "url": "string",
      "indexStatus": "string", /* Unknown, Starting, Failed, Pending, Success */
      "indexDetail": "string"
    },
    ...
  ],
  "count": "integer" /* Total number of documents */
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}/documents" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
# Use the optional skip and count parameters
$BASE_URL/v1/search/profile/{name}/documents?skip={skip}&count={count}
```

## GET /v1/search/profile/{name}/document/{id}

Using the `{name}` RAG Assistants, it gets details about the `{id}` document.

### Parameters

| Name   | Type   | Description |
|--------|--------|-------------|
| name | string | Search Profile name (required) |
| id | string | Document Id (required) |

### Response

```json
{
  "extension": "string",
  "id": "string",
  "indexStatus": "string", /* Unknown, Starting, Failed, Pending, Success */
  "indexDetail": "string",
  "keyName": "string",
  "metadata": [
    {
      "key": "string",
      "value": "string"
    },
    ...
  ],
  "name": "string",
  "timestamp": "timestamp",
  "url": "string"
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}/document/{id}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/profile/{name}/document

```

## GET /v1/search/profile/{name}/document/{id}

Using the `{name}` RAG Assistants, it gets details about the `{id}` document.

### Parameters

| Name | Type | Description |
|--------|--------|-------------|
| name | string | Search Profile name (required) |
| id | string | Document Id (required) |

### Response

```json
{
  "extension": "string",
  "id": "string",
  "indexStatus": "string", /* Unknown, Starting, Failed, Pending, Success */
  "indexDetail": "string",
  "keyName": "string",
  "metadata": [
    {
      "key": "string",
      "value": "string"
    },
    ...
  ],
  "name": "string",
  "timestamp": "timestamp",
  "url": "string"
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}/document/{id}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/profile/{name}/document

Uploads a Document to the associated `{name}` RAG Assistants. Notice that the file extension must be a 
[supported one](../RAG/HowtoRAGAssistants.md).

### Request Body

The supported options are `binary` or `multipart/form-data` including a `File` type.

#### Binary

Useful for its simplicity, encodes the binary data directly in the request body. Set the request with the associated `Content-Type` header to indicate the type of data being sent (e.g., `application/pdf`, `text/plain`).

It is mandatory to set a `filename` header value with the document name and extension. For example:

```
filename: SampleFile.pdf
```

Notice that this option does not enable to upload metadata.

#### form-data

This format allows you to include both binary data and other form fields in a single request. Each part of the data (binary file, text fields, etc.) is separated by a boundary and sent as separate parts. It is expected to be used for large files.

If you want to attach metadata to the file to be processed during ingestion, add a `metadata` form-data variable with the desired value; remember that the expected format is a `key/value` JSON list. For example the following is a valid metadata for a Document:


#### form-data


This format allows you to include both binary data and other form fields in a single request. Each part of the data (binary file, text fields, etc.) is separated by a boundary and sent as separate parts. It is expected to be used for large files.


If you want to attach metadata to the file to be processed during ingestion, add a `metadata` form-data variable with the desired value; remember that the expected format is a `key/value` JSON list. For example the following is a valid metadata for a Document:nt to the associated `{name}` RAG Assistants. Notice that the file extension must be a [supported one](../RAG/HowtoRAGAssistants.md).

### Request Bod

The supported options are `binary` or `multipart/form-data` including a `File` type.

#### Binary

Useful for its simplicity, encodes the binary data directly in the request body. Set the request with the associated `Content-Type` header to indicate the type of data being sent (e.g., `application/pdf`, `text/plain`).

It is mandatory to set a `filename` header value with the document name and extension. For example:

```
filename: SampleFile.pdf
```

Notice that this option does not enable to upload metadata.

#### form-data

This format allows you to include both binary data and other form fields in a single request. Each part of the data (binary file, text fields, etc.) is separated by a boundary and sent as separate parts. It is expected to be used for large files.

If you want to attach metadata to the file to be processed during ingestion, add a `metadata` form-data variable with the desired value; remember that the expected format is a `key/value` JSON list. For example the following is a valid metadata for a Document:

```json
{
  "type": "test",
  "domain": "Knowledge",
  "year": 2023,
  "quarter": "q3"
}
```

The metadata can be uploaded as `Text` or directly from a `File`.

### Response

Equivalent to the [Get Response](#get-v1searchprofilenamedocumentid). Notice that, once the document is uploaded, the `indexStatus` will be `Unknown` as it is queued to be ingested. Use the [Get Response](#get-v1searchprofilenamedocumentid) API to check the document status, the expected result is `Success`.

Possible return errors:

 * [2027](./ErrorCodes.md#2027)
 * [2028](./ErrorCodes.md#2028)

### CURL Example

To upload a `SampleFile.pdf` file, you can follow these steps:

```shell
# binary
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
--header 'filename: SampleFile.pdf' \
--header 'Content-Type: application/pdf' \
--data '@/C:/temp/SampleFile.pdf'
# multi-part
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "Content-Type: application/pdf" \
 --form 'file=@"/C:/temp/SampleFile.pdf"'
# multi-part with metadata as text
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "Content-Type: application/pdf" \
 --form 'metadata="{\"type\":\"test\",\"domain\":\"Knowledge\",\"year\":2023,\"quarter\":\"q3\"}"' \
 --form 'file=@"/C:/temp/SampleFile.pdf"'
# multi-part with metadata as a File
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "Content-Type: application/pdf" \
 -F "Content-Type: application/pdf" \
 --form 'metadata=@"/C:/temp/upload_file_metadata.json" \
 --form 'file=@"/C:/temp/SampleFile.pdf"'
```

## DELETE /v1/search/profile/{name}/document/{id}

Delete a Document.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile name (required) |
| id | string | Document Id (required) |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` is displayed with a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/search/profile/{name}/document/{id}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/execute

Execute a search query, for more detail [here](./ChatWithDocumentsAPI.md).
