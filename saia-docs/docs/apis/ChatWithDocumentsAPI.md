---
sidebar_label: 'Chat with Documents API'
sidebar_position: 4
---

# GeneXus Enterprise AI Chat with Documents API

This API enables searches or queries on the indexed content. 

This documentation provides detailed information on the available endpoints and how to interact with them. 

If you want to manage settings associated with your RAG Assistants (ex. k, model, historyCount), use the [RAG Assistants Section](../RAG/RAGAssistantsSection.md).

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

> The following endpoints require a GeneXus Enterprise AI API token related to **project** scope.

## Endpoints

The GeneXus Enterprise AI Chat with documents API provides the following endpoints:

| Method | Path                  |
| ------ | --------------------- |
| POST   | /execute              |

### 1. `/execute` - Execute Search Query

Executes a search query based on a specific profile and question.

#### Request

- Method: POST
- Path: /execute
- Headers:
  - Content-Type: application/json
- Request Body:

  ```json
  {
    "id": "string", /* optional */
    "profile": "string",
    "question": "string",
    "variables": [
        {"key": "string", "value": "string"},
        ...
    ],
    "filters": [
        {
          "key": "string",
          "operator": "string", /* Optional */
          "value": "string or number"
        },
        ...
    ]
  }
  ```

| Parameter | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| id | string | Identifier for the conversation |
| profile   | string | The profile to search |
| question  | string | The question to ask |
| [variables](../Prompt.md#design) | collection | A list of key/value properties (optional) |
| filters | collection | list of filters to apply (optional) |

For conversations with history, use the `id` optional element to refer to a particular conversation. These conversations will respect the `History Count` parameter from your [RAG Assistants](../RAG/RAGAssistantsSection.md#History-Message-Count). If no `id` value is set, no history will be considered and your query will be treated as a one-off. 

The `variables` parameter is used to fill in an associated prompt with values. 

The `filters` parameter is used as a logical condition statement for filtering documents (using the `AND` operator). Each filter item has the following format:

```json
{
  "key": string,
  "operator": string, /* optional defaults to $eq */
  "value": string,
}
```

Possible values for the `operator`:

| Parameter | Description |
| --------- | ----------- |
| `$eq` | Equal (default) |
| `$ne` | Not Equal |
| `$gt` | Greater than |
| `$gte` | Greater than or equal|
| `$lt` | Less than |
| `$lte` | Less than or equal|

These are predefined filters you can use.

| Filter | Description |
| --------- | ----------- |
| `id` | Document GUID returned during [insertion](./SearchProfileAPI.md#post-v1searchprofilenamedocument) |
| `name` | Original document name |
| `extension` | Original document extension |
| `source` | Document source, in general, an Url |

To use specific ones remember to ingest documents with the correct metadata. A valid filters section is:

```json
  ...
  "filters": [
      {"key": "extension", "operator": "$ne", "value": "pdf"},
      {"key": "name", "operator": "$eq", "value": "sample"},
      {"key": "year", "operator": "$gte", "value": 2000} /* year added during ingestion */
  ]
  ...
```

#### Response

- Status Code: 200
- Content-Type: application/json

  ```json
  {
    "documents": [
      {
        "pageContent": "Example page content",
        "score": 0.9,
        "metadata": {
          "source": "Example source",
          "description": "Example description",
          "id": "12345",
          "name": "Example document",
          "extension": "doc",
          "locationLinesFrom": 1,
          "locationLinesTo": 10,
          "locationLinesPageNumber": 1
        }
      }
    ],
    "id": "someId",
    "requestId": "someId",
    "text": "Example reply",
    "result": {
      "success": true,
      "messages": [
        "Search query executed successfully"
      ]
    }
  }
  ```

The returned `score` element (when available) measures the semantic similarity between the `question` and the associated `pageContent`; a value between 0 and 1 where 1 is closest.

You can use the `requestId` element to review the Request detail in the console.

Example of a request using a Search Profile that does not exist or is disabled:
- Status Code: 200
- Content-Type: application/json

  ```json
  {
    "error": {
        "code": 1101,
        "message": "Search Index Profile Name not found"
    },
    "status": "failed",
    "success": false,
    "text": ""
}
  ```

#### Sample CURL Request

```bash
# Simple case
curl -X POST
  -H "Content-Type: application/json"
  -H "Authorization: $SAIA_PROJECT_APITOKEN"
  -d '{
  "profile": "Default",
  "question": "Explain to me what is SAIA?"
}' $BASE_URL/v1/search/execute
# Using variables and filters
curl -X POST
  -H "Content-Type: application/json"
  -H "Authorization: $SAIA_PROJECT_APITOKEN"
  -d '{
  "profile": "Default",
  "question": "Again, explain to me what is SAIA?",
  "variables": [
    {"key": "type","value": "Doc"}
  ],
  "filters": [
    {"key": "extension", "operator": "$ne", "value": "pdf"},
    {"key": "name", "operator": "$eq", "value": "sample"},
    {"key": "year", "operator": "$gte", "value": 2000}
  ]
}' $BASE_URL/v1/search/execute    
```
