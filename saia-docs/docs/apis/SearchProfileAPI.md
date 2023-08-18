---
sidebar_label: 'SearchProfile API'
sidebar_position: 6
---

# SAIA SearchProfile API

This API allows you to define different search profiles.

## Introduction

The SAIA SearchProfile API provides endpoints to manage Search Profiles, meaning the associated elements needed to create and handle Search and Chat interactions.

Check the [variables definition](./OrganizationAPI.md#generic-variables) to correctly use `$BASE_URL` and `$SAIA_APITOKEN`.

Check parameters explanation [here](./SearchIndexProfile.md).

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

Retrieve all the search profiles for a Project.

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

The `description` parameter is mandatory to be used with the [chat](./SearchAndChatAPI.md#saia-searchchat-api) option.

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profiles" \
  -H "Authorization: Bearer $SAIA_APITOKEN" \
  -H "Accept: application/json"
```

## GET /v1/search/profile/{name}

Get Search Profile `{name}` details.

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
  }
}
```

The `type` parameter is explained [here](./SearchIndexProfile.md#retrievers).

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/profile

Creates a Search Profile.

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
curl -X POST "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
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

Update a Search Profile.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile Name (required) |

### Request Body

Equivalent to the [Post Request](#post-v1searchprofile), but the following elements are discarded:

 * `name` element
 * `indexOptions` section

### Response

Equivalent to the [Get Response](#get-v1searchprofilename).

### CURL Example

```shell
curl -X PUT "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "description": "Updated Search Profile",
      "searchOptions": {
        "historyCount": 4,
        "llm": {
          "temperature": 0.5,
          "maxTokens": 1000
        },
        "search": {
          "k": 2,
          "prompt": "You are an Assistant, only reply using the following context:\n{context}\n Question is: {question}\n",
          "scoreThreshold": 0.85
        }
      }
}'
```

## DELETE /v1/search/profile/{name}

Delete a Search Profile.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile Name (required) |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` error and a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/search/profile/{name}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## GET /profile/{name}/documents

List the documents for a Profile.

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
  ]
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/search/profile/{name}/documents" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
# Use the optional skip and count parameters
$BASE_URL/v1/search/profile/{name}/documents?skip={skip}&count={count}
```

## GET /v1/search/profile/{name}/document/{id}

Using the `{name}` Search Profile, it gets detail about the `{id}` document.

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
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/profile/{name}/document

Uploads a Document to the associated `{name}` Search Profile. Notice that the file extension must be a [supported one](./Documents.md).

### Request Body

The supported options are `binary` or `multipart/form-data` including a `File` type.

#### Binary

Useful for its simplicity, encode the binary data directly in the request body. Set the request with the associated `Content-Type` header to indicate the type of data being sent (e.g., `application/pdf`, `text/plain`).

It is mandatory to set a `filename` header value with the document name and extension, to be assigned when uploading the data, for example:

```
filename: SampleFile.pdf
```

#### form-data

This format allows you to include both binary data and other form fields in a single request. Each part of the data (binary file, text fields, etc.) is separated by a boundary and sent as separate parts. It is expected to be used for a large file.

### Response

Equivalent to the [Get Response](#get-v1searchprofilenamedocumentid). Notice that once the document is uploaded the `indexStatus` will be `Unknown` as it is queued to be ingested. Use the [Get Response](#get-v1searchprofilenamedocumentid) method to check the document status, the expected result is `Success`.

### CURL Example

```shell
# binary
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
--header 'filename: SampleFile.pdf' \
--header 'Content-Type: application/pdf' \
--data '@/C:/temp/SampleFile.pdf'
# multi-part
curl -X POST "$BASE_URL/v1/search/profile/{name}/document" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "Content-Type: application/pdf" \
 --form 'file=@"/C:/temp/SampleFile.pdf"'
```

### Restriction

It is not possible to bind [Metadata](./Documents.md#metadata) to the file.

## DELETE /v1/search/profile/{name}/document/{id}

Delete a Document.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| name | string | Search Profile name (required) |
| id | string | Document Id (required) |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` error and a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/search/profile/{name}/document/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## POST /v1/search/execute

Execute a search query, more detail [here](./SearchAndChatAPI.md#saia-searchchat-api).
