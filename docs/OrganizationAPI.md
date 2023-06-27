# GBrain Organization API

This API allows you to interact with GBrain organization data.

## Introduction

The GBrain Organization API provides endpoints to retrieve organization data, such as projects and requests. It allows you to fetch project details and export request data.

## Endpoints

Below is a summary of the available endpoints for this API:

| Method | Path                   | Description                      |
| ------ | ---------------------- | -------------------------------- |
| GET    | /project/get           | Get projects                     |
| GET    | /request/export        | Export request data              |

## GET /project/get

Get projects.

### Parameters

| Name        | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| Assistantname | string | Assistant name (optional)       |
| Status       | string | Status (optional)               |
| Skip         | integer | Number of entries to skip       |
| Count        | integer | Number of entries to retrieve   |

### Response

```json
{
  "ApiTokenName": "string",
  "OrganizationId": "string",
  "OrganzationName": "string",
  "Projects": [
    {
      "ProjectId": "string",
      "ProjectName": "string",
      "SearchIndexProfile": [
        {
          "Id": "string",
          "Name": "string"
        }
      ]
    }
  ]
}
```

### CURL Example

```shell
curl -X GET "https://beta.pia.genexus.dev/GBrain/API/v1.0/organization/project/get" \
  -H "Authorization: Bearer $GBRAIN_APITOKEN" \
  -H "Accept: application/json"
```

## GET /request/export

Export request data.

### Parameters

| Name        | Type    | Description          |
| ----------- | ------- | -------------------- |
| Assistantname | string | Assistant name (optional) |
| Status       | string | Status (optional)    |
| Skip         | integer | Number of entries to skip |
| Count        | integer | Number of entries to retrieve |

### Response

```json
[
  {
    "item": {
      "assistant": "string",
      "intent": "string",
      "timestamp": "string",
      "prompt": "string",
      "output": "string",
      "inputText": "string",
      "status": "string"
    }
  }
]
```

### CURL Example

```shell
curl -X GET "https://beta.pia.genexus.dev/GBrain/API/v1.0/organization/request/export?Assistantname=example&Status=completed" \
  -H "Authorization: Bearer $GBRAIN_APITOKEN" \
  -H "Accept: application/json"
```
