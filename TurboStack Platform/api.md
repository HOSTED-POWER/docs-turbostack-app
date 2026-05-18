---
label: TurboStack API
order: 80
icon: terminal
tags: [api, rest, swagger, openapi, token, authentication]
meta:
  description: "TurboStack API reference for REST endpoints, authentication, Personal Access Tokens, Swagger, OpenAPI, clients, hosts, groups, credentials, monitoring, and deploy calls."
---
# API

The TurboStack API gives you direct access to the main platform resources over a straightforward REST API. It is designed for integrations, internal tooling, and deployment workflows that need to read platform data or update configuration in a controlled way.

This reference covers authentication, Personal Access Tokens, request conventions, and the currently published endpoints for three core resource types:

- `Clients`
- `Hosts`
- `Groups`

Each endpoint section includes the documented options, a short placeholder for functional explanation, a reusable cURL call, and a separate example request and response.

!!! info
Interactive Swagger UI: [my.turbostack.app/swagger](https://my.turbostack.app/swagger)

Raw OpenAPI document: [my.turbostack.app/api-docs/api-docs.json](https://my.turbostack.app/api-docs/api-docs.json)
!!!

## Base URL

All examples on this page use the production API base URL:

```text
https://my.turbostack.app
```

## Creating a Personal Access Token

Before using the API, create a personal access token in the platform.

1. Log in to the [TurboStack Platform](https://my.turbostack.app/).
2. Click your profile icon in the top-right corner.
3. Open **Profile settings**.
4. Go to **Personal Access Token (API)**.
5. Click **Create Token**.

When creating a token, you must define:

- A clear token name
- An expiration date

The expiration date can also be set to `never`, but this is not recommended for security reasons. A limited lifetime combined with regular rotation is the safer approach.

## Authentication

All documented endpoints require a bearer token in the `Authorization` header.

```http
Authorization: Bearer YOUR_API_TOKEN
Accept: application/json
Content-Type: application/json
```

In the OpenAPI definition this is described as an API key in the `Authorization` header, using the format `Bearer {your-token}`.

## Request conventions

Use the following conventions consistently in scripts and integrations:

- Base URL: `https://my.turbostack.app`
- API prefix: `/api/v1`
- Authentication header: `Authorization: Bearer YOUR_API_TOKEN`
- Accept header: `Accept: application/json`
- Content type for write operations: `Content-Type: application/json`

In the current published API, write operations use `POST`, including update actions.

## Common workflow

Most API workflows follow the same pattern:

1. Create a `Personal Access Token`
2. Call a list endpoint such as `GET /api/v1/hosts`
3. Fetch the resource you want to modify
4. Submit the updated payload with `POST`
5. Trigger a deploy when the host configuration was changed
6. Check deploy status

For host configuration changes, the usual sequence is:

1. `GET /api/v1/hosts/{id}`
2. `POST /api/v1/hosts/{id}`
3. `POST /api/v1/hosts/{id}/deploy`
4. `GET /api/v1/hosts/{id}/deploy`

## Pagination

List endpoints support the following optional query parameters:

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `size` | integer | `15` | Number of items returned per page |
| `page` | integer | `1` | Page number to return |

## Response model overview

The API returns lightweight JSON objects. The fields below are the recurring properties documented in the published OpenAPI specification.

### Client object

| Field | Type | Description |
| --- | --- | --- |
| `id` | number | Unique client ID |
| `firstname` | string | Client first name |
| `lastname` | string | Client last name |
| `email` | string | Client email address |
| `created_at` | string | Creation timestamp |
| `updated_at` | string | Last update timestamp |

### Host object

| Field | Type | Description |
| --- | --- | --- |
| `id` | number | Unique host ID |
| `client_id` | number | Owning client ID |
| `active` | number | Host state indicator |
| `name` | string | Host name |
| `json` | string | Stored host configuration payload |
| `created_at` | string | Creation timestamp |
| `updated_at` | string | Last update timestamp |
| `monitoring` | object | Live server status information |

### Host monitoring object

| Field | Type | Description |
| --- | --- | --- |
| `load` | object | Load information from monitoring |
| `disk` | object | Disk information from monitoring |
| `memory` | object | Memory information from monitoring |

### Group object

| Field | Type | Description |
| --- | --- | --- |
| `id` | number | Unique group ID |
| `client_id` | number | Owning client ID when available |
| `name` | string | Group name |
| `json` | string | Stored group configuration payload |
| `created_at` | string | Creation timestamp |
| `updated_at` | string | Last update timestamp |

## Clients

The client endpoints are mainly used to discover customer records and the hosts or groups that belong to them.

### `GET /api/v1/clients`

Returns a paginated list of clients.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `size` | query | No | Number of items per page |
| `page` | query | No | Page number |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/clients?size={SIZE}&page={PAGE}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `GET /api/v1/clients/{id}`

Returns a single client by ID.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the client |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/clients/{CLIENT_ID}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `GET /api/v1/clients/{id}/hosts`

Returns the hosts linked to a specific client.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the client |
| `size` | query | No | Number of items per page |
| `page` | query | No | Page number |

**Response notes**

- Host items may include a `monitoring` object with live `load`, `disk`, and `memory` data.

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/clients/{CLIENT_ID}/hosts?size={SIZE}&page={PAGE}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `GET /api/v1/clients/{id}/groups`

Returns the groups linked to a specific client.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the client |
| `size` | query | No | Number of items per page |
| `page` | query | No | Page number |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/clients/{CLIENT_ID}/groups?size={SIZE}&page={PAGE}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


## Hosts

The host endpoints form the operational core of the API. They allow you to inspect hosts, store host configuration, retrieve credentials, and start or monitor deployments.

### `GET /api/v1/hosts`

Returns a paginated list of hosts.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `size` | query | No | Number of items per page |
| `page` | query | No | Page number |

**Response notes**

- Host items may include a `monitoring` object with live `load`, `disk`, and `memory` data.

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/hosts?size={SIZE}&page={PAGE}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `GET /api/v1/hosts/{id}`

Returns one specific host, including its stored configuration payload.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the host |

**Response notes**

- The host object may include a `monitoring` object with live `load`, `disk`, and `memory` data.

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/hosts/{HOST_ID}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `POST /api/v1/hosts/{id}`

Saves or updates the JSON configuration of a host. Use this endpoint to persist configuration changes before you start a deployment.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the host |
| `json` | body | Yes | Host configuration object to store |

!!! warning
The OpenAPI document only defines this payload as a generic `json` object. In practice, this should contain the same configuration structure that TurboStack stores for the host in the platform.
!!!

**Request body shape**

```json
{
  "json": {
    "...": "host configuration object"
  }
}
```

**cURL call**

```bash
curl --request POST \
  --url 'https://my.turbostack.app/api/v1/hosts/{HOST_ID}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{
    "json": {
      "YOUR_CONFIGURATION_KEY": "YOUR_VALUE"
    }
  }'
```


### `GET /api/v1/hosts/{id}/deploy`

Returns the current deployment status for a host.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the host |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/hosts/{HOST_ID}/deploy' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `POST /api/v1/hosts/{id}/deploy`

Starts a deployment for a host after its configuration has been saved.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the host |
| `type` | body | Yes | Deployment type to execute |

Supported deploy types:

- `deploy`
- `fullDeploy`
- `fullResetDeploy`

**Request body shape**

```json
{
  "type": "deploy | fullDeploy | fullResetDeploy"
}
```

**cURL call**

```bash
curl --request POST \
  --url 'https://my.turbostack.app/api/v1/hosts/{HOST_ID}/deploy' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{
    "type": "deploy | fullDeploy | fullResetDeploy"
  }'
```


### `GET /api/v1/hosts/{id}/credentials`

Returns host credentials together with related configuration data.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the host |

**Documented responses**

| Status | Meaning |
| --- | --- |
| `200` | Host credentials and configuration |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Host not found |
| `500` | Internal server error |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/hosts/{HOST_ID}/credentials' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


## Groups

The group endpoints make it possible to inspect and update shared group configuration stored in the platform.

### `GET /api/v1/groups`

Returns a paginated list of groups.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `size` | query | No | Number of items per page |
| `page` | query | No | Page number |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/groups?size={SIZE}&page={PAGE}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `GET /api/v1/groups/{id}`

Returns a single group by ID.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the group |

**cURL call**

```bash
curl --request GET \
  --url 'https://my.turbostack.app/api/v1/groups/{GROUP_ID}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json'
```


### `POST /api/v1/groups/{id}`

Saves or updates a specific group.

**What this call does**



**Options**

| Parameter | In | Required | Description |
| --- | --- | --- | --- |
| `id` | path | Yes | ID of the group |
| `json` | body | Yes | Group configuration object to store |

The API returns the saved group ID on success.

**Request body shape**

```json
{
  "json": {
    "...": "group configuration object"
  }
}
```

**Documented responses**

| Status | Meaning |
| --- | --- |
| `200` | Group ID after saving |
| `403` | Forbidden |
| `404` | Group not found |

**cURL call**

```bash
curl --request POST \
  --url 'https://my.turbostack.app/api/v1/groups/{GROUP_ID}' \
  --header "Authorization: Bearer $TSTOKEN" \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{
    "json": {
      "YOUR_GROUP_CONFIGURATION_KEY": "YOUR_VALUE"
    }
  }'
```
