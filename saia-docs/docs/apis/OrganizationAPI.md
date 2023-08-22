---
sidebar_label: 'Administration API'
sidebar_position: 4
---

# SAIA Organization API

This API allows you to interact with SAIA organization data.

## Introduction

The SAIA Organization API provides endpoints to retrieve organization data, such as projects and requests. It allows you to fetch project details and export request data.

### Generic Variables
Notice the following properties needed when using the API.

| Variable | Description |
| ------ | ---------------------- |
| `$BASE_URL` | The base URL for your SAIA installation, for example `https://api.beta.saia.ai` or the value provided to you. |
| `$SAIA_APITOKEN` | An API token generated for each organization |


### Error Messages

It there is an error during the execution, all APIs return a list of errors and a status code `400*`:

```json
{
  "errors": [
    {
      "id": "integer",
      "description": "string"
    },
    ...
  ]
}
```

## Endpoints

Below is a summary of the available endpoints for this API:

| Method | Path                   | Description                      |
| ------ | ---------------------- | -------------------------------- |
| GET    | /assistants            | Get the list of assistants       |
| GET    | /projects              | Get the list of projects         |
| GET    | /project/{id}          | Get project details              |
| POST   | /project          | Create a project                 |
| PUT    | /project/{id}          | Update a project                 |
| DELETE | /project/{id}          | Delete a project                 |
| GET    | /project/{id}/tokens   | Get the list of Tokens for the project |
| GET    | /request/export        | Export request data              |

## GET /assistants

Get a list of assistants.

### Parameters

| Name   | Type   | Description |
|--------|--------|-------------|
| detail | string | Defines the level of detail required, options are `summary` (default) or `full` (optional). |

### Response

Using the default `summary` option will only show the first level. The `full` option will display revision detail and assistant composition:

```json
{
  "assistants": [
    {
      "assistantId": "string",
      "assistantName": "string",
      "intents": [ /* full option */
        {
          "assistantIntentDefaultRevision": "number",
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
                ...
              ],
              "modelId": "string",
              "modelName": "string",
              "prompt": "string",
              "providerName": "string",
              "revisionDescription": "string",
              "revisionId": "string",
              "revisionName": "string",
              "timestamp": "timestamp"
            },
            ...
          ]
        }
      ]
    },
    ...
  ],
  "projectId": "string",
  "projectName": "string"
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/organization/assistants" \
  -H "Authorization: Bearer $SAIA_APITOKEN" \
  -H "Accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/organization/assistants?detail=full
```

Keep an eye on the returned `assistantId` element which is needed for other related APIs.

## GET /projects

Get a list of projects.

### Parameters

| Name   | Type   | Description |
|--------|--------|-------------|
| detail | string | Defines the level of detail required, options are `summary` (default) or `full` (optional). |

By default active projects will be listed, use the `full` detail option to list all projects.

### Response

```json
{
  "projects": [
    {
      "projectActive": "boolean",
      "projectDescription": "string",
      "projectId": "string",
      "projectName": "string",
      "projectStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
    },
    ...
  ]
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/organization/projects" \
  -H "Authorization: Bearer $SAIA_APITOKEN" \
  -H "Accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/organization/projects?detail=full
```

Keep an eye on the returned `projectId` item value which is needed for other related APIs.

## GET /project/{id}

Get project `{id}` details.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID (required) |

### Response

```json
{
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "searchProfiles": [
    {
      "name": "string",
      "description": "string"
    },
      ...
  ]
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/organization/project/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

##

## POST /project

Creates a new project.

### Request Body

```json
{
  "Name": "string",
  "Description": "string"
}
```

### Response

```json
{
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "searchProfiles": [
    {
      "name": "string",
      "description": "string"
    },
    ...
  ],
  "tokens": [
    {
      "description": "string",
      "id": "string",
      "name": "string",
      "status": "string", /* Active, Blocked */
      "timestamp": "timestamp"
    },
    ...
  ]
}
```

Notice the `tokens` element (default API Tokens) are only returned at Project creation time. You can retrieve them using the [GET Tokens](#get-projectidtokens) endpoint.

When the creation is no successful, status code `400*` will be detailed with a collection of errors:

```json
{
  "errors": [
    {
      "id": "integer",
      "description": "string"
    },
    ...
  ]
}
```

### CURL Example

```shell
curl -X POST "$BASE_URL/v1/organization/project" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "name": "my Project",
      "description": "My awesome project"
 }'
```

## PUT /project/{id}

Update a project.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID ProjectId (required)  |

### Request Body

```json
{
  "name": "string", /* Required */
  "description": "string",
  "active": "boolean"
}
```

### Response

```json
{
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 1:Deleted, 2:Hidden */
  "searchProfiles": [
    {
      "name": "string",
      "description": "string"
    },
    ...
  ]
}
```

When the creation is no successful, status code `400*` will be detailed with a collection of errors.

### CURL Example

```shell
curl -X PUT "$BASE_URL/v1/organization/project/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json" \
 -d '{
  "name":"Sample Project",
  "description":"sample project description updated",
  "active":true
}'
```

## DELETE /project/{id}

Delete a project.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID ProjectId (required)  |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` error and a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/organization/project/{id}" \
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## GET /project/{id}/tokens

Get the list of API tokens for the `{id}` project.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | Project GUID (required) |

### Response

```json
{
  "tokens": [
    {
      "description": "string",
      "id": "string",
      "name": "string",
      "status": "string", /* Active, Blocked */
      "timestamp": "timestamp"
    },
    ...
  ]
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/organization/project/{id}/tokens"
 -H "Authorization: Bearer $SAIA_APITOKEN" \
 -H "accept: application/json"
```

## GET /request/export

Export request data.

### Parameters

| Name        | Type    | Description          |
| ----------- | ------- | -------------------- |
| assistantName | string | Assistant name (optional) |
| status       | string | Status (optional)    |
| skip         | integer | Number of entries to skip |
| count        | integer | Number of entries to retrieve |

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
curl -X GET "https://api.beta.saia.ai/v1/organization/request/export?Assistantname=example&Status=completed" \
  -H "Authorization: Bearer $SAIA_APITOKEN" \
  -H "Accept: application/json"
```
