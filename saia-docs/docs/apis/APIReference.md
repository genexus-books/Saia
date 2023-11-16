---
sidebar_label: 'API Overview'
sidebar_position: 1
---

# GeneXus Enterprise AI API Reference

Table of contents
=================

* [Introduction ](#introduction)
* [Generic Variables](#generic-variables)
* [Authentication ](#authentication)
* [Errors ](#errors)
* [Versioning ](#versioning)
* [Proxy AI API](../ProxyAPI.md)
* [Organization API](OrganizationAPI.md)
* [Assistants API](AssistantsAPI.md)
* [Search Profile API](SearchProfileAPI.md)
* [Chat with Documents API](ChatWithDocumentsAPI.md)


# Introduction
GeneXus Enterprise AI provides various APIs that, on one hand, allow integration with Large Language Model (LLM) systems and perform actions that modify the platform's metadata. On the other hand, a different set of APIs enables interaction with defined assistants.

### Generic Variables
Notice the following properties needed when using the API.

| Variable | Description |
| ------ | ---------------------- |
| `$BASE_URL` | The base URL for your GeneXus Enterprise AI installation, for example, `https://api.saia.ai` or the value provided to you. |
| `$SAIA_APITOKEN` | An API token generated for each project |


## Authentication
In order to use the API, you need to authenticate each request using API Tokens. These tokens are managed in GeneXus Enterprise AI [backoffice](Backoffice.md) and uniquely identify the sender of the request.

To authenticate your requests, you need to provide your token via HTTP Basic Auth. This means that your token is encoded in the username field, and the password field should be left empty. Once authenticated, you will be able to access all endpoints within your API scope.

For security purposes, it is strongly recommended that you do not share your API tokens with anyone and revoke them immediately if they are compromised.

# Errors
REST API employs the widely accepted practice of using HTTP response codes to convey the status of an API request. The codes in the 2xx range indicate that the request was successful and the server has returned the expected data. On the other hand, the codes in the 4xx range signify that the request failed because of a client-side error, such as missing or invalid parameters, unauthorized access, or any other fault in the request. The codes in the 5xx range suggest that there's an error on the server-side, and the request couldn't be completed due to a server malfunction or connectivity issues. Such errors, fortunately, are infrequent in the service.

By following these HTTP response codes, users can easily understand whether their API requests have succeeded or failed, and the probable causes of failure if there are any.


If there is an error during the execution, all APIs return a list of errors and a status code `400*`:

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

## Versioning
The API versioning strategy is designed to minimize disruptions to your application when backwards-incompatible changes are introduced.

Whenever changes are made to the API, a new version is released. This approach allows you to continue using the previous version of the API until you are ready to upgrade to the latest version.

It is strongly recommended that you always specify the version number when making API requests to ensure the correct behavior of your application. You can find the latest version number in the documentation or by contacting the support team.

By using versioning, you ensure that your application remains stable and functional, while still providing access to the latest API features and functionality.
