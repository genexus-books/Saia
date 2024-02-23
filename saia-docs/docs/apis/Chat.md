---
sidebar_label: 'Chat API'
sidebar_position: 4
---

# Chat API

This API is specifically designed to centralize the usage of any `Assistant` in a single entry point.

The general endpoint is:

| Method | Path |
| --- | ---- |
| POST   | /chat |

## Request

- Method: POST
- Path: $BASE_URL/chat
- Headers:
  - Content-Type: application/json
  - Authorization: Bearer $SAIA_PROJECT_APITOKEN
- Request Body

The payload will vary depending on the selected `Assistant`. The general pattern is:

```javascript
{
    "model": "string",
    "messages": [ ... ],
    "stream": boolean
    // other parameters
}
```

The `model` needs to address the assistant `type` and `name`. Then, depending on the `type` the parameters will vary. The format is as follows:

```json
    "model": "saia:<assistant_type>:<assistant_name>",
```

| Type | Description |
| --- | --- |
| `assistant` | Identifies a [standard assistant](./AssistantsAPI.md#genexus-enterprise-ai-assistant-api) |
| `search` | Identifies a [RAG Assistant](../RAG/RAGAssistantsSection.md) |

The `messages` element defines the desired messages to be added. The minimal value needs to be the following, where the `content` details the user input.

```json
{
    "role": "string", /* user, system and may support others depending on the selected model */
    "content": "string"
}
```

You can add additional parameters; these are possible body samples.

### Assistant Sample

```json
{
    "model": "saia:assistant:translate-to-spanish", /* Using a Standard Assistant named 'translate-to-spanish' */
    "messages": [
        {
            "role": "user",
            "content": "Hi, welcome to GeneXus Enterprise AI!!"
        }
    ],
    "stream": true,
    "temperature": 0,
    "max_tokens": 500
}
```

The expected result is to `stream` the translated content depending on the Prompt defined.

### RAG Sample

```json
{
    "model": "saia:search:Default", /* Using the Default RAG Assistant */
    "messages": [
        {
            "role": "user",
            "content": "Summarize the features of GeneXus Enterprise AI"
        }
    ],
    "stream": true,
    "temperature": 0,
    "max_tokens": 500
}
```

The expected result is to query the Default RAG Assistant and stream a reply once the sources were obtained.

### CURL samples

```shell
# RAG Assistant
curl -X POST "$BASE_URL/chat" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "Content-Type: application/json" \
 --data '{
    "model": "saia:search:Default",
    "messages": [
        {
            "role": "user",
            "content": "Summarize the features of GeneXus Enterprise AI"
        }
    ],
    "stream": true,
    "temperature": 0,
    "max_tokens": 500
}'
# Assistant
curl -X POST "$BASE_URL/chat" \
 -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
 -H "Content-Type: application/json" \
 --data '{
    "model": "saia:assistant:translate-to-spanish",
    "messages": [
        {
            "role": "user",
            "content": "Hi, welcome to GeneXus Enterprise AI!!"
        }
    ],
    "stream": true
}'
```
