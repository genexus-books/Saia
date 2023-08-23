---
sidebar_position: 2
sidebar_label: 'Assistant API'
---

# SAIA Assistant API

This is the documentation for the SAIA Assistant API and related operations.

## Endpoints

| Method | Path                           | Description                  |
| ------ | ------------------------------ | ---------------------------- |
| GET    | /v1/assistant/{id}             | Get assistant data |
| POST   | /v1/assistant                  | Create a new assistant |
| PUT    | /v1/assistant/{id}             | Update an assistant |
| DELETE | /v1/assistant/{id}             | Delete an assistant |
| POST   | /v1/assistant/text/begin       | Begins a text conversation with the SAIA Assistant |
| POST   | /v1/assistant/text             | Sends a text prompt to the SAIA Assistant |
| POST   | /v1/assistant/chat             | Sends a chat request to the SAIA Assistant |
| POST   | /v1/assistant/text2img         | Generates an image based on the given text |
| GET    | /v1/assistant/request/{id}/status | Retrieves the status of a request |
| POST   | /v1/assistant/request/{id}/cancel | Cancels a request |


## GET /v1/assistant/{id}

Get assistant data.

### Parameters

| Name   | Type   | Description |
| ------ | ------ | ----------- |
| id     | string | The assistant ID. |
| detail | string | Defines the level of detail required, options are `summary` (default) or `full` (optional). |

Using the default `summary` option will show detail up to the current revision. The `full` option will detail all revision information and composition.

### Response

```json
{
  "assistantId": "string",
  "assistantName": "string",
  "assistantStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "intents": [ /* One element when summary, everything when using full detail*/
      {
          "assistantIntentDefaultRevision": "string",
          "assistantIntentDescription": "string",
          "assistantIntentId": "string",
          "assistantIntentName": "string",
          "revisions": [
              {
                  "metadata": [
                      {
                          "key": "string",
                          "type": "string",
                          "value": "string"
                      },
                      {
                          "key": "string",
                          "type": "string",
                          "value": "string"
                      }
                  ],
                  "modelName": "string",
                  "prompt": "string",
                  "providerName": "string",
                  "revisionDescription": "string",
                  "revisionId": "string",
                  "revisionName": "string",
                  "timestamp": "timestamp"
              }
          ]
      }
  ],
  "projectId": "string",
  "projectName": "string"
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/assistant/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/assistant/{id}?detail=full
```

## POST /v1/assistant

Create a new assistant.

#### Request Body

```json
{
  "type": "string", /* text, chat */
  "name": "string", /* Required */
  "description": "string",
  "prompt": "string", /* Required */
  "llmSettings": {
    "providerName": "string",
    "modelName": "string",
    "temperature": "decimal",
    "maxTokens": "integer",
  }
}
```

#### Response

```json
{
  "assistantId": "string",
  "assistantName": "string",
  "assistantStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "intents": [
    {
      "assistantIntentDefaultRevision": "integer",
      "assistantIntentDescription": "string",
      "assistantIntentId": "string",
      "assistantIntentName": "string",
      "revisions": [
        {
          "revisionId": "integer",
          "revisionName": "string",
          "revisionDescription": "string",
          "providerName": "string",
          "modelName": "string",
          "prompt": "string",
          "timestamp": "timestamp",
          "metadata": [
            {
              "key": "string",
              "value": "string",
              "type": "string"
            },
            ...
          ]
        }
      ]
    }
  ],
  "projectId": "string",
  "projectName": "string",
}
```

Keep an eye on the returned `assistantId` element which is needed for other related APIs.

### CURL Example

```shell
curl -X POST "$BASE_URL/v1/assistant" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "type": "chat",
      "name": "TestAssistant",
      "prompt": "translate the following text to Esperanto"
 }'
```

### PUT /v1/assistant/{id}

Updates an existing assistant. The assistant `name` and `type` properties cannot be changed.

#### Parameters

| Name   | Type   | Description |
| ------ | ------ | ----------- |
| id     | string | The ID of the assistant. |

#### Request Body

```json
{
  "action": "string", /* save (default), saveNewRevision, savePublishNewRevision */
  "revisionId": "integer", /* Required when action = save */
  "prompt": "string", /* Required */
  "llmSettings": {
    "providerName": "string",
    "modelName": "string",
    "temperature": "decimal",
    "maxTokens": "integer",
  }
}
```

The `action` parameter by default (`save`option value) will update the current assistant `revisionId`. When using `saveNewRevision`, it will create a new revision based on the provided data but will not be active. Use the `savePublishNewRevision` option to create a new revision and set it as active.

#### Response

The current updated revision will be returned.

```json
{
  "assistantId": "string",
  "assistantName": "string",
  "assistantStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "intents": [
    {
      "assistantIntentDefaultRevision": "integer",
      "assistantIntentDescription": "string",
      "assistantIntentId": "string",
      "assistantIntentName": "string",
      "revisions": [
        {
          "revisionId": "integer",
          "revisionName": "string",
          "revisionDescription": "string",
          "providerName": "string",
          "modelName": "string",
          "prompt": "string",
          "timestamp": "timestamp",
          "metadata": [
            {
              "key": "string",
              "value": "string",
              "type": "string"
            },
            ...
          ]
        }
      ]
    }
  ],
  "projectId": "string",
  "projectName": "string",
}
```

### CURL Example

```shell
curl -X PUT "$BASE_URL/v1/assistant/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "action": "savePublishNewRevision",
      "prompt": "translate the following text to Latin",
      "llmSettings":{
          "modelName":"gpt-3.5-turbo",
          "temperature":0.0
      }
 }'
```

## DELETE /assistant/{id}

Delete an assistant.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | Assistant ID (required) |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` error and a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/assistant/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## POST /text/begin

Begins a text conversation with the SAIA Assistant.

### Parameters

| Name | Type | Description |
|---|---|---|
| assistant | string | The name of the assistant. |
| prompt | string | The text prompt for the assistant. |
| revision | integer | The revision number. |
| revisionName | string | The name of the revision. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "success": true,
  "providerName": "string",
  "providerResponse": "string",
  "progress": 0,
  "timestamp": "string",
  "status": "string",
  "text": "string"
}
```

### CURL Example

```shell
curl -X POST $BASE_URL/v1/assistant/text/begin \
  -H "Content-Type: application/json" \
  -d '{
    "assistant": "string",
    "prompt": "string",
    "revision": 0,
    "revisionName": "string"
  }'
```

## POST /text

Sends a text prompt to the SAIA Assistant.

### Parameters

| Name | Type | Description |
|---|---|---|
| assistant | string | The name of the assistant. |
| prompt | string | The text prompt for the assistant. |
| revision | integer | The revision number. |
| revisionName | string | The name of the revision. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "success": true,
  "providerName": "string",
  "providerResponse": "string",
  "progress": 0,
  "timestamp": "string",
  "status": "string",
  "text": "string"
}
```

### CURL Example

```shell
curl -X POST $BASE_URL/v1/assistant/text \
  -H "Content-Type: application/json" \
  -d '{
    "assistant": "AssistantName",
    "prompt": "Your Input here",
    "revision": 1
  }'
```

## POST /chat

Sends a chat request to the SAIA Assistant.

### Parameters

| Name | Type | Description |
|---|---|---|
| assistant | string | The name of the assistant. |
| messages | array | The chat request data. |
| revision | integer | The revision number. |
| revisionName | string | The name of the revision. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "success": true,
  "providerName": "string",
  "providerResponse": "string",
  "progress": 0,
  "timestamp": "string",
  "status": "string",
  "text": "string"
}
```

### CURL Example

```shell
curl -X POST $BASE_URL/v1/assistant/chat \
  -H "Content-Type: application/json" \
  -d '{
    "assistant": "string",
    "messages": [
      {
        "role": "string",
        "content": "string"
      }
    ],
    "revision": 0,
    "revisionName": "string"
  }'
```

## POST /text2img

Generates an image based on the given text.

### Parameters

| Name | Type | Description |
|---|---|---|
| prompt | string | The text to generate the image from. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "providerName": "string",
  "providerResponse": "string",
  "progress": 0,
  "timestamp": "string",
  "status": "string",
  "hotlink": true,
  "output": [
    {
      "image_url": "string"
    }
  ]
}
```

### CURL Example

```shell
curl -X POST $BASE_URL/v1/assistant/text2img \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "string"
  }'
```

## GET /request/{requestId}/status

Retrieves the status of a request.

### Parameters

| Name | Type | Description |
|---|---|---|
| requestId | string (uuid) | The ID of the request. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "providerName": "string",
  "providerResponse": "string",
  "progress": 0,
  "timestamp": "string",
  "status": "string"
}
```

### CURL Example

```shell
curl -X GET $BASE_URL/v1/assistant/request/{requestId}/status
```

## POST /request/{requestId}/cancel

Cancels a request.

### Parameters

| Name | Type | Description |
|---|---|---|
| requestId | string (uuid) | The ID of the request. |

### Response

```json
{
  "requestId": "string",
  "error": {
    "code": 0,
    "message": "string"
  },
  "providerResponse": "string",
  "timestamp": "string",
  "status": "string"
}
```

### CURL Example

```shell
curl -X POST $BASE_URL/v1/assistant/request/{requestId}/cancel
```
