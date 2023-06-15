# Pia Beta API Documentation

Version: 20230607171556

## Base URL

https://beta.pia.genexus.dev/GBrain/API/v1

## Paths

### /assistant

#### POST

**Summary:** assistant__post

**Operation ID:** GBrain.Api.GBrainApi.assistant__post

**Request Body:**

- Required: No
- Content Type: application/json
- Schema: [assistant__postInput](#components/schemas/assistant__postInput)

**Responses:**

- 200: Success
  - Content Type: application/json
  - Schema: [Pia.API.v1.GenericQueryResponse](#components/schemas/Pia.API.v1.GenericQueryResponse)

- 404: Not found

## Sample CURL Snippets

### /assistant POST

```shell
curl -X POST \
  https://beta.pia.genexus.dev/GBrain/API/v1/assistant \
  -H 'Content-Type: application/json' \
  -d '{
    "exampleProperty": "exampleValue"
}'
```

Please note that the above CURL snippet is just an example and you may need to adjust the actual request data according to your specific use case.
