---
title: Headers
description: Specifies the means, format, and restrictions for sending operation headers through the mesh in Adobe Graph.
---
# Headers

To specify request headers for your mesh, you can add them inside the `JSON` file that describes your mesh or you can add them when querying.

## Add headers to your mesh file

To add headers directly to a source handler in your mesh file, for example `mesh.json`, add the `operationHeaders` object with key value pairs for your headers. In the following example, the header `my-mesh-header` has a value of `my-mesh-header-value`.

```json
{
    "meshConfig": {
      "sources": [
        {
          "name": "headersData",
          "handler": {
            "JsonSchema": {
              "baseUrl": "<your_Magento_url>",
              "operationHeaders": {
                "my-mesh-header": "my-mesh-header-value"
              },
              "operations": [
                {
                  "type": "Query",
                  "field": "data",
                  "path": "/cart",
                  "method": "GET",
                  "responseSchema": "./your-response-schema.json"
                }
              ]
            }
          }
        }
      ]
    },
    "tenantId": "<your_tenant_Id>"
  }
```

## Add or update headers in a request

When you use GraphiQL, Postman, or other tools to interact with your mesh, you can add headers at runtime that are passed through the mesh to specified handler by using the following format:

```json
{"GGW-SH-<SourceName>-<HeaderName>": "my-header-value"}
```

Using this example, the components of the header name are:

-  `GGW-SH` indicates to the GraphQL Gateway Server that what follows is a source header. It does not need to be modified.
-  `SourceName` is the name of your previously created source or handler. The source name in the example in the previous section is `headersData`.
-  `HeaderName` is the name of the header you are adding or modifying.
-  `my-header-value` is the value you are adding or modifying for the specified header.

<InlineAlert variant="info" slots="text"/>

 Since we use hyphens to separate header strings, hyphens are disallowed in source handler names.

For example, if you want to send a new value (`new-store-code`) for a store code (`magento-store-code`) to a source handler (`MonolithAPI`) in your mesh, use the following:

```json
{"GGW-SH-MonolithAPI-magento-store-code":"new-store-code"}
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
