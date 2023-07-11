# Saia SearchChat API

## Introduction

The Saia SearchChat API is a RESTful API that provides search functionality for chat conversations. It allows you to execute searches based on specific profiles and questions, and retrieves search results along with relevant metadata. This documentation provides detailed information on the available endpoints and how to interact with them. If you want to change parameters configuration use the [Search Index Profile](SearchIndexProfile.md) section.

## Endpoints

The Saia SearchChat API provides the following endpoint:

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
    "profile": "example_profile",
    "question": "example_question"
  }
  ```

  | Parameter | Type   | Description                     |
  | --------- | ------ | ------------------------------- |
  | profile   | string | The profile to search           |
  | question  | string | The question to search          |

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
    "text": "Example text",
    "log": "Example log",
    "requestId": "12345678-1234-1234-1234-123456789abc",
    "error": {
      "code": 500,
      "message": "Internal Server Error"
    },
    "success": true,
    "timestamp": "2023-06-15T18:09:40Z",
    "status": "success",
    "result": {
      "success": true,
      "messages": [
        "Search query executed successfully"
      ]
    }
  }
  ```

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
curl -X POST -H "Content-Type: application/json" -d '{
  "profile": "example_profile",
  "question": "example_question"
}' https://beta.pia.genexus.dev/GBrain/API/v1.0/search/execute
```
