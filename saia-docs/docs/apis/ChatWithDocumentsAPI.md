---
sidebar_label: 'Chat with Documents API'
sidebar_position: 4
---

# SAIA Chat with Documents API

This API enables searches or queries on the indexed content. 

This documentation provides detailed information on the available endpoints and how to interact with them. 

If you want to manage settings associated with your search profile (ex. k, model, historyCount), use the [Search Index Profile](../SearchIndexProfile.md) section.

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

> The following endpoints require a Saia API token related to **project** scope.

## Endpoints

The SAIA Chat with documents API provides the following endpoints:

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
        {"key": "string","value": "string"},
        ...
    ]
  }
  ```

  | Parameter | Type   | Description                     |
  | --------- | ------ | ------------------------------- |
  | id | string | Identifier for the conversation |
  | profile   | string | The profile to search           |
  | question  | string | The question to ask             |
  | [variables](../Prompt.md#design) | collection | A list of key/value properties (optional)|

For conversations with history, use the `id` optional element to refer to a particular conversation. These conversations will respect the `History Count` parameter from your [Search Profile](../SearchIndexProfile.md#history-document-count-scores). If no `id` value is set, no history will be considered and your query will be treated as a one-off. 

The `variables` parameter is used to fill in an associated prompt with values. 

#### Response

- Status Code: 200
- Content-Type: application/json

  ```json
  {
    "documents": [
      {
        "pageContent": "Example page content",
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

You can use the `requestId` element to review the Request detail in the console.

Example of request using Search Profile that does not exist or is disabled:
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
curl -X POST
  -H "Content-Type: application/json"
  -H "Authorization: $SAIA_PROJECT_APITOKEN"
  -d '{
  "profile": "Default",
  "question": "Explain to me what is SAIA?"
}' $BASE_URL/v1/search/execute
```
