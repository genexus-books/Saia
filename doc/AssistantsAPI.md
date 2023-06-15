# GBrain Assistants API

This API allows you to interact with GBrain Assistants. GBrain Assistants are intelligent virtual assistants that can perform various tasks based on your input.

## What is a REST API?

A REST API (Representational State Transfer) is an architectural style for designing networked applications. It is based on a set of principles that allow clients to interact with resources on a server through standard HTTP methods such as GET, POST, PUT, and DELETE. REST APIs are widely used in web development and are particularly suited for building scalable and flexible systems.

## Endpoints

### /assistant

This endpoint allows you to interact with a GBrain Assistant by sending a POST request. The assistant will perform an action based on the provided input.

#### Parameters

| Name    | Type          | Description                               |
|---------|---------------|-------------------------------------------|
| Request | application/json | The JSON object containing the request data. |

#### Response Example

```json
{
  "status": "success",
  "message": "Action performed successfully",
  "data": {
    "result": "Some result"
  }
}
```

#### CURL Example

```bash
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistant -H "Content-Type: application/json" -d '{"request": "Some request"}'
```

### /assistantChat

This endpoint allows you to have a chat conversation with a GBrain Assistant by sending a POST request. The assistant will respond to your messages accordingly.

#### Parameters

| Name    | Type          | Description                               |
|---------|---------------|-------------------------------------------|
| Request | application/json | The JSON object containing the chat messages. |

#### Response Example

```json
{
  "status": "success",
  "message": "Chat conversation completed",
  "data": {
    "conversation": [
      {
        "message": "Hello",
        "response": "Hi, how can I assist you?"
      },
      {
        "message": "Can you tell me the weather tomorrow?",
        "response": "Sure, the weather tomorrow will be sunny with a high of 25°C."
      }
    ]
  }
}
```

#### CURL Example

```bash
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistantChat -H "Content-Type: application/json" -d '{"message": "Hello"}'
```
