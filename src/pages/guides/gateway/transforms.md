---
title: Transforms
---

# Tranforms

While [handlers] let you bring outside sources into Adobe Graph, [transforms] allow you to modify the schema in order to get a specific format for your response.

Although [GraphQL Mesh] supports several different [transforms], at launch Adobe Graph will only support:

-  [Rename](#rename)
-  [Prefix](#prefix)
-  [Filter](#filter)
-  [Replace](#replace)
-  [Type Merge](#type-merge)
-  [Naming Convention](#naming-convention)

Additionally, these transforms are available but are not fully supported at this time:

-  [Encapsulate]
-  [Federation]

## Rename

[Rename] transforms allow you to rename a GraphQL field, type, or field argument. Renaming allows you to avoid conflicting names, simplify complicated names, and make queries look more like mutations. In the example below, we rename a long API field name from `integrationCustomerTokenServiceV1CreateCustomerAccessTokenPost` to the shorter `CreateCustomerToken`.

`rename` elements can contain arrays of individual renaming operations, defined in separate `renames` objects. Each of these objects must define the `from` and `to` values.

<InlineAlert variant="info" slots="text"/>

You can use [RegEx flags] to enable the use of regular expressions when renaming using this transform. For example, you could use the key value pair `field: api(.*)` in the `from` object to rename any field of the corresponding type that begins with "api".

```json
{
  "meshConfig": {
    "sources": [
      {
        "name": "Magento REST",
        "handler": {
          "openapi": {
            "source": "https://www.example.com/rest/all/schema?services=all"
          }
        },
          "transforms": [
            {
              "rename": {
              "renames": [
                {
                  "from": {
                    "type": "Mutation",
                    "field": "integrationCustomerTokenServiceV1CreateCustomerAccessTokenPost"
                  },
                  "to": {
                    "type": "Mutation",
                    "field": "CreateCustomerToken"
                  }
                }
              ]
            }
          }
        ]
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```

## Prefix

[Prefix] transforms allow you to append prefixes to existing types and root operations. `prefix` is similar to `rename` in that it allows you to modify names to avoid conflicts, simplify complicated names, and change the appearance of your query. In contrast with `rename`, `prefix` is simpler and only allows you to append a prefix to the existing name. In the example below, the [PWA] schema will be prefixed with "Venia_" in order to distinguish it from [AEM].

```json
{
  "meshConfig": {
    "sources": [
      {
        "name": "AEM",
        "handler": {
          "graphql": {
            "endpoint": "example1.com"
          }
        }
      },
       {
        "name": "PWA",
        "handler": {
          "graphql": {
            "endpoint": "example2.com"
          }
        },
        "transforms": [
          {
            "prefix": {
              "includeRootOperations": true,
              "value": "Venia_"
            }
          }
        ]
      }
    ]
  },
  "tenantId": "<your_tenant_id"
}
```

## Filter Schema

The [Filter Schema] transform allows you to include or exclude fields from your configuration file. This means that queries run with this configuration will use the filtered schema. In the example below, the queries that reference the [PWA] handler will have the Category and Customer Order fields filtered out of their schema.

<!-- I'm not certain if the paragraph above is accurate. The documentation here is a little sparse: https://www.graphql-mesh.com/docs/transforms/filter-schema -->

```json
{
  "meshConfig": {
    "sources": [
      {
        "name": "PWA",
        "handler": {
          "graphql": {
            "endpoint": "example1.com"
          }
        },
        "transforms": [
          {
            "filterSchema": {
              "filters": [
                "Query.!category",
                "Query.!customerOrders"
              ]
            }
          }
        ]
      },
      {
        "name": "AEM",
        "handler": {
          "graphql": {
            "endpoint": "example2.com"
          }
        }
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```

## Replace

[Replace] transforms allow you to replace the configuration properties of one field with another, which allows you to hoist field values from a subfield to its parent. Use this transform to clean up redundant looking queries or replace field types. In the example below, the `parent` field is being replaced by the `child` field.

```json
{
  "meshConfig": {
    "sources": [
      {
        "name": "PWA",
        "handler": {
          "graphql": {
            "endpoint": "example1.com"
          }
        },
          "transforms": [
            {
              "replace-field": {
              "replacements": [
                {
                  "from": {
                    "type": "Query",
                    "field": "parent"
                  },
                  "to": {
                    "type": "<your_API_Response>",
                    "field": "child",
                  "scope": "hoistvalue"
                  }
                }
              ]
            }
          }
        ]
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```

## Type Merge

[Type Merge] transforms allow you to combine multiple sources by merging a type from each source. For example, you could combine responses from two different APIs on a single field, provided you [rename] the fields you want to stitch to the same name. For more information, see this [GraphQL Mesh Example].

<!-- I couldn't really come up with an example here, so linking out made more sense to me. -->

## Naming Convention

[Naming Convention] transforms allow you to apply casing and other conventions to your response. In the example below, `category` fields are converted to uppercase, while `urlResolver` fields are converted to lowercase.


```json
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
        "transforms": [
          {
            "namingConvention": {
              "category": "upperCase",
              "urlResolver": "camelCase"
            }
          }
        ]
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

<!-- Link Definitions -->
[AEM]: https://experienceleague.adobe.com/docs/experience-manager-cloud-service.html
[PWA]: https://developer.adobe.com/commerce/pwa-studio/
[Rename]: https://www.graphql-mesh.com/docs/transforms/rename
[GraphQL Mesh]: https://www.graphql-mesh.com/docs/getting-started/introduction
[handlers]: ../gateway/source-handlers.md
[transforms]: https://www.graphql-mesh.com/docs/transforms/transforms-introduction
[Prefix]: https://www.graphql-mesh.com/docs/transforms/prefix
[Rename]: https://www.graphql-mesh.com/docs/transforms/rename
[Filter Schema]: https://www.graphql-mesh.com/docs/transforms/filter-schema
[Replace]: https://www.graphql-mesh.com/docs/transforms/replace-field
[Type Merge]: https://www.graphql-mesh.com/docs/transforms/type-merging
[Naming Convention]: https://www.graphql-mesh.com/docs/transforms/naming-convention
[Federation]: https://www.graphql-mesh.com/docs/transforms/federation
[Encaspsulate]: https://www.graphql-mesh.com/docs/transforms/encapsulate
[GraphQL Mesh Example]: https://www.graphql-mesh.com/docs/recipes/multiple-apis#merging-types-from-different-sources-using-type-merging
[RegEx flags]: https://www.graphql-mesh.com/docs/transforms/rename#config-api-reference