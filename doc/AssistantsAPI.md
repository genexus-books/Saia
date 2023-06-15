# GBrain Assistants API

This API allows you to interact with GBrain Assistants.

## Endpoints

| Path              | Description             |
| ----------------- | ----------------------- |
| POST /assistant    | Create a new assistant  |
| POST /assistantChat| Send a chat message     |

## POST /assistant

Create a new assistant.

### Description

This endpoint allows you to create a new assistant.

### Request Body

| Name                  | Type      | Description                    |
| --------------------- | --------- | ------------------------------ |
| assistant__postInput  | Object    | The input data for the assistant creation. |

### Response

Sample Response:
```json
{
  "id": "3b29b51e-1266-4460-ae70-44288a8d0282",
  "name": "My Assistant",
  "status": "active"
}
```

Sample CURL Request:
```bash
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistant \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "My Assistant"
  }'
```

## POST /assistantChat

Send a chat message to an assistant.

### Description

This endpoint allows you to send a chat message to an existing GBrain assistant.

### Request Body

| Name                      | Type      | Description                           |
| ------------------------- | --------- | ------------------------------------- |
| assistantChat__postInput  | Object    | The input data for the chat message.   |

### Response

Sample Response:
```json
{
  "message": "Hello!",
  "sender": "user"
}
```

Sample CURL Request:
```bash
curl -X POST https://beta.pia.genexus.dev/GBrain/API/v1/assistantChat \
  -H 'Content-Type: application/json' \
  -d '{
    "message": "Hello!",
    "sender": "user"
  }'
```
