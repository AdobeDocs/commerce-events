---
title: Headers
description: Specifies the means, format, and restrictions for sending operation headers through the mesh in Adobe Graph.
---

# Headers

To specify request headers for your mesh, you can add them inside the `JSON` file that describes your mesh or you can add them when querying.

## Configure headers in your mesh file

To add headers directly to a source handler in your mesh file, for example `mesh.json`, add the `operationHeaders` object with key value pairs for your headers. In the following example, the `LiveSearch` source has a header named `Magento-Website-Code` with a value of `base`.

<InlineAlert variant="info" slots="text"/>

Header variables are not supported in the mesh file. 

```json
{
  "meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://<host>/graphql",
            "operationHeaders": {
              "Store": "<store_view_code>"
            }
          }
        }
      },
      {
        "name": "LiveSearch",
        "handler": {
          "graphql": {
            "endpoint": "https://<host>/search/graphql"
            "operationHeaders": {
              "Magento-Environment-Id": "<environment_id>"
              "Magento-Website-Code": "base"
              "Magento-Store-Code": "main_website_store"
              "Magento-Store-View-Code": "default"
              "X-Api-Key": "search_gql"
            }
          }
        }
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```

## Add or update headers at runtime

When you use GraphiQL or another tool to interact with your mesh, you can add headers at runtime that are passed through the mesh to specified handler by using the following format:

```json
{"GGW-SH-<SourceName>-<HeaderName>": "my-header-value"}
```

Using this example, the components of the header name are:

-  `GGW-SH` is a required string that indicates to the GraphQL Gateway Server that what follows is a source header.
-  `SourceName` is the name of your previously created source or handler. The source names in the example in the previous section are `Commerce` and `LiveSearch`.
-  `HeaderName` is the name of the header you are adding or modifying.
-  `my-header-value` is the value you are adding or modifying for the specified header.

For example, if you want to send a new value (`new-store-code`) for a store code (`Magento-Store-Code`) to a source handler (`LiveSearch`) in your mesh, use the following:

```json
{"GGW-SH-LiveSearch-magento-store-code":"new-store-code"}
```

### Add a header to all sources

If you want to send a header to all sources in your mesh, you can replace the source handler name with `*`. For example:

```json
{"GGW-SH-*-auth-token":"new-auth-token"}
```

This can be useful for authorization, authentication, and tracking headers that could be the same across multiple sources. If you want to apply a header to all sources except one, specify that source separately. For example:

```json
{"GGW-SH-*-auth-token":"new-auth-token"}
{"GGW-SH-differentSource-auth-token":"different-auth-token"}
```
