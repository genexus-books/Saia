---
sidebar_label: 'Search Chat Api'
sidebar_position: 3
---

# Saia SearchChat API

## Introduction

The Saia SearchChat API is a RESTful API that provides search functionality for chat conversations. It allows you to execute searches based on specific profiles and questions, and retrieves search results along with relevant metadata. This documentation provides detailed information on the available endpoints and how to interact with them. If you want to change parameters configuration use the [Search Index Profile](SearchIndexProfile.md) section.

## Endpoints

The Saia SearchChat API provides the following endpoint:

| Method | Path                  |
| ------ | --------------------- |
| POST   | /execute              |

### 1. `/SearchChat` - Execute Search Query

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
  | question  | string | The question to ask             |

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
    "text": "Example reply",
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
curl -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer {YOUR_API_TOKEN}"
  -d '{
  "profile": "example_profile",
  "question": "example_question"
}' https://api.beta.saia.ai/v1/search/execute
```