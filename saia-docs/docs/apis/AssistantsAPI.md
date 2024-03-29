---
sidebar_position: 5
sidebar_label: 'Assistant API'
---

# GeneXus Enterprise AI Assistant API

This API enables the creation of new assistants, the modification of their definitions, and the retrieval of information about them. Furthermore, it allows the execution of prompts associated with previously defined assistants.

> The following endpoints require a GeneXus Enterprise AI API token related to **project** scope.

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

## Endpoints

| Method | Path                           | Description                  |
| ------ | ------------------------------ | ---------------------------- |
| GET    | /v1/assistant/{id}             | Gets assistant data |
| POST   | /v1/assistant                  | Creates a new assistant |
| PUT    | /v1/assistant/{id}             | Updates an assistant |
| DELETE | /v1/assistant/{id}             | Deletes an assistant |
| POST   | /v1/assistant/text/begin       | Begins a text conversation with the GeneXus Enterprise AI Assistant |
| POST   | /v1/assistant/text             | Sends a text prompt to the GeneXus Enterprise AI Assistant |
| POST   | /v1/assistant/chat             | Sends a chat request to the GeneXus Enterprise AI Assistant |
| GET    | /v1/assistant/request/{id}/status | Retrieves the status of a request |
| POST   | /v1/assistant/request/{id}/cancel | Cancels a request |


## GET /v1/assistant/{id}

Gets assistant data.

### Parameters

| Name   | Type   | Description |
| ------ | ------ | ----------- |
| id     | string | The assistant ID. |
| detail | string | Defines the level of detail required, options are `summary` (default) or `full`. |

If you use the default `summary` option, the active revision details will be shown. The `full` option will display information about all revisions.

### Response

```json
{
  "assistantId": "string",
  "assistantName": "string",
  "assistantStatus": "integer", /* 1:Enabled, 2:Disabled */
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
                  "timestamp": "timestamp",
                  "variables": [ /* Optional */
                    {
                      "key": "string"
                    },
                    ...
                  ]
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
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/assistant/{id}?detail=full
```

## POST /v1/assistant

Creates a new assistant.

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
  "assistantStatus": "integer", /* 1:Enabled, 2:Disabled */
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
          ],
          "variables": [ /* Optional */
            {
              "key": "string"
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

The returned `assistantId` is needed for other related APIs.

### CURL Example

```shell
curl -X POST "$BASE_URL/v1/assistant" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "type": "chat",
      "name": "TestAssistant",
      "prompt": "translate the following text to Esperanto"
 }'
```

## PUT /v1/assistant/{id}

Updates an existing assistant. The assistant `type` property cannot be changed.

### Parameters

| Name   | Type   | Description |
| ------ | ------ | ----------- |
| id     | string | The ID of the assistant. |

#### Request Body

```json
{
  "name": "string", /* Optional */
  "description": "string", /* Optional */
  "status": "integer", /* Optional      1:Enabled, 2:Disabled */
  "action": "string", /* save, saveNewRevision (default), savePublishNewRevision */
  "revisionId": "integer", /* Required if user needs update an existant revision when action = save */
  "prompt": "string", /* Required if revisionId is specified or in case of actions saveNewRevision and savePublishNewRevision*/
  "llmSettings": {
    "providerName": "string",
    "modelName": "string",
    "temperature": "decimal",
    "maxTokens": "integer",
  }
}
```
The default value of the `action` parameter, `saveNewRevision`, will create a new revision but will not set it as active. The `save` option will update the active revision. The `savePublishNewRevision` option will create a new revision and set it as active. 

If only an update of `name`, `description`, or `status` is needed, **one of them must be provided at least**, without any changes in the revision, it can be specified as:
```json
{
  "name": "string",
  "description": "string",
  "status": "integer", /* 1:Enabled, 2:Disabled */
  "action": "string" /* use "save" action */
}
```

#### Response

The current revision will be returned.

```json
{
  "assistantId": "string",
  "assistantName": "string",
  "assistantStatus": "integer", /* 1:Enabled, 2:Disabled */
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
          ],
          "variables": [ /* Optional */
            {
              "key": "string"
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
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
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

Deletes an assistant.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | Assistant ID (required) |

### Response

StatusCode `200` indicates a successful deletion, otherwise StatusCode `400*` with a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/assistant/{id}" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "accept: application/json"
```

## POST /text/begin

Begins a text conversation with the GeneXus Enterprise AI Assistant.

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

Sends a text prompt to the GeneXus Enterprise AI Assistant.

### Parameters

| Name | Type | Description |
|---|---|---|
| assistant | string | The name of the assistant. |
| prompt | string | The text prompt for the assistant. |
| revision | integer | The revision number. |
| revisionName | string | The name of the revision. |
| [variables](../Prompt.md#design) | collection | A list of key/value properties (optional)|

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

Sends a chat request to the GeneXus Enterprise AI Assistant.

### Parameters

| Name | Type | Description |
|---|---|---|
| assistant | string | The name of the assistant. |
| messages | array | The chat request data. |
| revision | integer | The revision number. |
| revisionName | string | The name of the revision. |
| [variables](../Prompt.md#design) | collection | A list of key/value properties (optional)|

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
    "variables": [
        {"key": "string", "value": "string"},
        {"key": "string", "value": "string"}
    ],
    "revision": 0,
    "revisionName": "string"
  }'
```

Notice the [variables](../Prompt.md#design) section is optional and depends on your Prompt configuration.

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
