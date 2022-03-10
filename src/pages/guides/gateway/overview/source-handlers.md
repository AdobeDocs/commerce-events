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

<!-- Link Definitions -->

[GraphQL Mesh]: https://www.graphql-mesh.com/docs/getting-started/introduction
[source handlers]: https://www.graphql-mesh.com/docs/handlers/handlers-introduction
[API]: https://www.graphql-mesh.com/docs/handlers/openapi
[GraphQL]: https://www.graphql-mesh.com/docs/handlers/graphql
[JSON]: https://www.graphql-mesh.com/docs/handlers/json-schema
