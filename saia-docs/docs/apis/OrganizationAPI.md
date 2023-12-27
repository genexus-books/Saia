---
sidebar_label: 'Organization API'
sidebar_position: 2
---

# GeneXus Enterprise AI Organization API

This API provides endpoints to retrieve organization data, such as projects and requests. It allows you to fetch project details and export request data.

Check the [generic variables](./APIReference.md#generic-variables) needed to use the API.

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
> This endpoint requires a GeneXus Enterprise AI API token related to **project** scope.

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
  -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
  -H "Accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/organization/assistants?detail=full
```

Keep an eye on the returned `assistantId` element which is needed for other related APIs.

## GET /projects

Get a list of projects.
> This endpoint requires a GeneXus Enterprise AI API token related to **organization** scope.

### Parameters

| Name   | Type   | Description |
|--------|--------|-------------|
| detail | string | Defines the level of detail required, options are `summary` (default) or `full` (optional). |
| name | string | Searches by project name (equals) (optional) |

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
      "projectStatus": "integer", /* 0:Active, 2:Hidden */
    },
    ...
  ]
}
```

### CURL Example

```shell
curl -X GET "$BASE_URL/v1/organization/projects" \
  -H "Authorization: Bearer $SAIA_ORGANIZATION_APITOKEN" \
  -H "Accept: application/json"
# using the full detail option change the URL to
$BASE_URL/v1/organization/projects?detail=full
# using the name option filter change the URL to
$BASE_URL/v1/organization/projects?name=projectName
```

Keep an eye on the returned `projectId` item value which is needed for other related APIs.

## GET /project/{id}

Get project `{id}` details.
> This endpoint can be used whether the GeneXus Enterprise AI API token is related to the organization scope and project scope as well.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID (required) |

### Response

```json
{
  "organizationId": "string",
  "organizationName": "string",
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 2:Hidden */
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

## POST /project

Creates a new project.
> This endpoint requires a GeneXus Enterprise AI API token related to **organization** scope.

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
  "projectStatus": "integer", /* 0:Active, 2:Hidden */
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

When the creation is not successful, StatusCode `400*` will be detailed with a collection of errors:

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
 -H "Authorization: Bearer $SAIA_ORGANIZATION_APITOKEN" \
 -H "accept: application/json" \
 -d '{
      "name": "my Project",
      "description": "My awesome project"
 }'
```

## PUT /project/{id}

Update a project.
> This endpoint requires a GeneXus Enterprise AI API token related to **organization** scope.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID ProjectId (required)  |

### Request Body

```json
{
  "name": "string", /* Required */
  "description": "string"
}
```

### Response

```json
{
  "projectActive": "boolean",
  "projectDescription": "string",
  "projectId": "string",
  "projectName": "string",
  "projectStatus": "integer", /* 0:Active, 2:Hidden */
  "searchProfiles": [
    {
      "name": "string",
      "description": "string"
    },
    ...
  ]
}
```

When the creation is not successful, StatusCode `400*` will be detailed with a collection of errors.

### CURL Example

```shell
curl -X PUT "$BASE_URL/v1/organization/project/{id}" \
 -H "Authorization: Bearer $SAIA_ORGANIZATION_APITOKEN" \
 -H "accept: application/json" \
 -d '{
  "name":"Sample Project",
  "description":"sample project description updated"
}'
```

## DELETE /project/{id}

Delete a project.
> This endpoint requires a GeneXus Enterprise AI API token related to **organization** scope.

### Parameters

| Name | Type   | Description |
|------|--------|-------------|
| id   | string | GUID ProjectId (required)  |

### Response

StatusCode `200` is detailed when successfully deleted, otherwise `400*` is displayed with a collection of errors.

### CURL Example

```shell
curl -X DELETE "$BASE_URL/v1/organization/project/{id}" \
 -H "Authorization: Bearer $SAIA_ORGANIZATION_APITOKEN" \
 -H "accept: application/json"
```

## GET /project/{id}/tokens

Get the list of API tokens for the `{id}` project.
> This endpoint requires a GeneXus Enterprise AI API token related to **organization** scope.

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
 -H "Authorization: Bearer $SAIA_ORGANIZATION_APITOKEN" \
 -H "accept: application/json"
```

## GET /request/export

Export request data.
> This endpoint requires a GeneXus Enterprise AI API token related to **project** scope.

### Parameters

| Name        | Type    | Description          |
| ----------- | ------- | -------------------- |
| assistantName | string | Assistant name (optional) |
| status       | string | Status (optional)    |
| skip         | integer | Number of entries to skip |
| count        | integer | Number of entries to retrieve |

### Response

```json
{
  "items": [
    {
      "assistant": "string",
      "intent": "string",
      "timestamp": "string",
      "prompt": "string",
      "output": "string",
      "inputText": "string",
      "status": "string"
    },
    ...
  ]
}
```

### CURL Example

```shell
curl -X GET "https://api.saia.ai/v1/organization/request/export?assistantname=example&status=completed" \
  -H "Authorization: Bearer $SAIA_PROJECT_APITOKEN" \
  -H "Accept: application/json"
```
