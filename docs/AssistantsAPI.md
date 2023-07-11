# Saia Assistant Api

This is the documentation for the Saia Assistant API.

## Endpoints

| Path | Description |
|---|---|
| POST /text/begin | Begins a text conversation with the Saia Assistant. |
| POST /text | Sends a text prompt to the Saia Assistant. |
| POST /chat | Sends a chat request to the Saia Assistant. |
| POST /text2img | Generates an image based on the given text. |
| GET /request/{requestId}/status | Retrieves the status of a request. |
| POST /request/{requestId}/cancel | Cancels a request. |

## POST /text/begin

Begins a text conversation with the Saia Assistant.

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
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/text/begin \
  -H "Content-Type: application/json" \
  -d '{
    "assistant": "string",
    "prompt": "string",
    "revision": 0,
    "revisionName": "string"
  }'
```

## POST /text

Sends a text prompt to the Saia Assistant.

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
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/text \
  -H "Content-Type: application/json" \
  -d '{
    "assistant": "string",
    "prompt": "string",
    "revision": 0,
    "revisionName": "string"
  }'
```

## POST /chat

Sends a chat request to the Saia Assistant.

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
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/chat \
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
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/text2img \
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
curl -X GET https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/request/{requestId}/status
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
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1.0/assistant/request/{requestId}/cancel
```
