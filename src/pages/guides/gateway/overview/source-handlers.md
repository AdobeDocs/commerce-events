---
title: Adding source handlers
---

# Adding source handlers

Although [GraphQL Mesh] supports many types of [source handlers], at launch Adobe Graph only supports the following:

-  [APIs](#apis)
-  [GraphQL endpoints](#graphql_endpoints)
-  [JSON schemas](#json_schemas)

<InlineAlert variant="info" slots="text"/>

We will add support for additional handlers in future releases.

## APIs

The [API] handler allows you to connect OpenAPIs and Swagger schemas using a `.json` or `.yaml` file.

```bash
{
  "meshConfig": {
    "sources": [
      {
        "name": "Magento REST",
        "handler": {
          "openapi": {
            "source": "your_Magento_API"
          }
        },
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```
### API Handler Configuration

 Adobe Graph currently supports the following configuration options for API handlers.

-  **source** - Your API source or endpoint
-  **sourceFormat** - The format of the source file (`json` | `yaml`)
-  **schemaHeaders** - JSON Headers to fetch your schema
-  **operationHeaders** - JSON headers for your API calls
-  **baseUrl** - Your base URL
-  **qs** - JSON object for query search parameters
-  **includeHttpDetails** - A boolean flag to include HTTP Response details

<InlineAlert variant="info" slots="text"/>

For more information, see GraphQL Mesh's [Config API Reference]. Note that Adobe Graph uses an older version of GraphQL Mesh, so settings in external documentation may not be accurate for these purposes. Additionally, only the options specified above are currently supported.

## GraphQL endpoints

The [GraphQL] handler allows you to connect GraphQL schemas using a `.json` or `.yaml` file.

```bash
{
  "meshConfig": {
    "sources": [
      {
        "name": "PWA",
        "handler": {
          "graphql": {
            "endpoint": "your_Venia_url"
          }
        },
      {
        "name": "AEM",
        "handler": {
          "graphql": {
            "endpoint": "<your_AEM_url>"
          }
        }
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```
### GraphQL Handler Configuration

 Adobe Graph currently supports the following configuration options for GraphQL handlers.

-  **endpoint** - The URL or path of your GraphQL endpoint
-  **schemaHeaders** - JSON Headers to fetch your schema
-  **operationHeaders** - JSON headers for your calls
-  **useGETForQueries** - A boolean option to use HTTP GET
-  **method** - HTTP method used (`GET` | `POST`)

<InlineAlert variant="info" slots="text"/>

For more information, see GraphQL Mesh's [GraphQL Config API Reference]. Note that Adobe Graph uses an older version of GraphQL Mesh, so settings in external documentation may not be accurate for these purposes. Additionally, only the options specified above are currently supported.

## JSON schemas

The [JSON] handler allows you to load rest services using a `.yaml` file.

```bash
"sources":
  - "name": "<your_json_schema>"
    "handler":
      "jsonSchema":
        "baseUrl": "<your_schema>/<endpoint>"
        "operations":
          - "type": "Query"
            "field": "carts"
            "path": "/carts"
            "method": "GET"
            "responseSchema": ".<your_schema>/carts.json"
```
### JSON Schema Handler Configuration

 Adobe Graph currently supports the following configuration options for JSON Schema handlers.

-  **baseurl** - The URL or path of your JSON Schema
-  **schemaHeaders** - JSON Headers to fetch your schema
-  **operationHeaders** - JSON headers for your calls
-  **operations** - An array that contains:
   -  field
   -  description
   -  type (`Query` | `Mutation` | `Subscription`)
   -  requestSchema
   -  requestSample
   -  requestTypeName
   -  requestBaseBody - This body will merge with the request body sent with the HTTP request
   -  responseSchema
   -  responseSample
   -  responseTypeName
   -  argTypeMap
   -  pubsubTopic
-  **ignoreErrorResponses** - A Boolean option to ignore errors

<InlineAlert variant="info" slots="text"/>

For more information, see GraphQL Mesh's [JSON Schema Config API Reference]. Note that Adobe Graph uses an older version of GraphQL Mesh, so settings in external documentation may not be accurate for these purposes. Additionally, only the options specified above are currently supported.

<!-- Link Definitions -->

[GraphQL Mesh]: https://www.graphql-mesh.com/docs/getting-started/introduction
[source handlers]: https://www.graphql-mesh.com/docs/handlers/handlers-introduction
[API]: https://www.graphql-mesh.com/docs/handlers/openapi
[GraphQL]: https://www.graphql-mesh.com/docs/handlers/graphql
[JSON]: https://www.graphql-mesh.com/docs/handlers/json-schema
[Config API Reference]: https://www.graphql-mesh.com/docs/handlers/openapi#config-api-reference
[GraphQL Config API Reference]: https://www.graphql-mesh.com/docs/handlers/graphql#config-api-reference
[JSON Schema Config API Reference]: https://www.graphql-mesh.com/docs/handlers/json-schema#config-api-reference