# GBrain API Documentation

## Introduction

This documentation provides information on how to use the GBrain API. The GBrain API allows developers to interact with the GBrain assistant.

## Base URL

All API requests must be made to the following base URL:

```
https://beta.pia.genexus.dev/GBrain/API/v1
```

## Endpoints

### POST /assistant

This endpoint is used to interact with the GBrain assistant.

**Parameters**

| Name       | Type             | Description               |
|------------|------------------|---------------------------|
| requestBody | JSON             | The request body containing the assistant data. |

**Request Body**

The request body should be a JSON object following the schema "assistant__postInput". 

**Response**

Sample Response:
```json
{
  "data": {
    "message": "Hello, how can I assist you?"
  },
  "success": true
}
```

**CURL Example**

```shell
curl -X POST "https://beta.pia.genexus.dev/GBrain/API/v1/assistant" -H "Content-Type: application/json" -d '{
  "input": "Hi"
}'
```

### POST /assistantChat

This endpoint is used to chat with the GBrain assistant.

**Parameters**

| Name       | Type             | Description               |
|------------|------------------|---------------------------|
| requestBody | JSON             | The request body containing the chat data. |

**Request Body**

The request body should be a JSON object following the schema "assistantChat__postInput". 

**Response**

Sample Response:
```json
{
  "data": {
    "message": "Sure, I can help you with that."
  },
  "success": true
}
```

**CURL Example**

```shell
curl -X POST "https://beta.pia.genexus.dev/GBrain/API/v1/assistantChat" -H "Content-Type: application/json" -d '{
  "chat": "Can you tell me about the weather?"
}'
```

Please note that the above documentation assumes that the referenced schemas "assistant__postInput", "assistantChat__postInput", and "Pia.API.v1.GenericQueryResponse" are defined in the API specification file.
