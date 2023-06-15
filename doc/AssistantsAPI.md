# GBrain Assistants API

Version: 20230607171556

## Introduction
This API provides endpoints to interact with GBrain Assistants.

## Table of Contents
- [Assistant](#assistant)
- [Assistant Chat](#assistant-chat)

## Assistant
- Endpoint: `POST /assistant`
- Description: This endpoint allows you to send a request to the GBrain assistant.
- Request Body:

  | Parameter    | Type   | Description   |
  | ------------ | ------ | ------------- |
  | assistant    | string |               |
  | prompt       | string |               |
  | revision     | integer|               |
  | revisionName | string |               |

- Response:

  ```json
  {
    "response": "Assistant response"
  }
  ```

- Sample CURL:
  ```shell
  curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistant -H "Content-Type: application/json" -d '{
    "assistant": "Assistant name",
    "prompt": "Prompt text",
    "revision": 1,
    "revisionName": "Revision name"
  }'
  ```

## Assistant Chat
- Endpoint: `POST /assistantChat`
- Description: This endpoint allows you to have a conversation with the GBrain assistant.
- Request Body:

  | Parameter    | Type   | Description         |
  | ------------ | ------ | ------------------- |
  | assistant    | string |                     |
  | messages     | array  | Chat Request Data   |
  | revision     | integer|                     |
  | revisionName | string |                     |

- Response:

  ```json
  {
    "response": "Assistant response"
  }
  ```

- Sample CURL:
  ```shell
  curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistantChat -H "Content-Type: application/json" -d '{
    "assistant": "Assistant name",
    "messages": [
      {
        "message": "Hello",
        "role": "User"
      },
      {
        "message": "Hi!",
        "role": "Assistant"
      }
    ],
    "revision": 1,
    "revisionName": "Revision name"
  }'
  ```
