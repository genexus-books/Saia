---
sidebar_label: 'Chat with Documents API'
sidebar_position: 4
---

# SAIA Chat with Documents API

This API enables searches or queries on the indexed content. 

This documentation provides detailed information on the available endpoints and how to interact with them. 

If you want to change parameters configuration use the [Search Index Profile](../SearchIndexProfile.md) section.

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

## Endpoints

The SAIA Chat with documents API provides the following endpoint:

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
  | variables | collection | A list of key/value properties (optional)|

To keep track of the conversation use a unique value for the `id` optional element. Notice that the last n items (`History Count` parameter from the [Search Profile](../SearchIndexProfile.md#history-document-count-scores)) will be considered to help answer the query. When no value is set; the chat will not consider the latest questions and answers.

The `variables` optional elements details a list of variables to be substituted on the associated prompts during execution.

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

- Status Code: 404
- Content-Type: application/json

  ```json
  {
    "error": {
      "code": 404,
      "message": "Not found"
    },
    "success": false
  }
  ```

#### Sample CURL Request

```bash
curl -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer {YOUR_API_TOKEN}"
  -d '{
  "profile": "Default",
  "question": "Explain to me what is SAIA?"
}' $BASE_URL/v1/search/execute
```
