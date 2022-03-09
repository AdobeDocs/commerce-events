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

[Rename] transforms allow you to rename a GraphQL field, type, or field argument. In the example below, we rename an API field from `integrationCustomerTokenServiceV1CreateCustomerAccessTokenPost` to `CreateCustomerToken`.

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

[Prefix] transforms allow you to append prefixes to GraphQL types and root operations. In the example below, the [AEM] schema will be prefixed with "AEM_" in order to distinguish it from [PWA].

```bash
{
  "meshConfig": {
    "sources": [
      {
        "name": "PWA",
        "handler": {
          "graphql": {
            "endpoint": "<your_Venia_url>"
          }
        }
      },
       {
        "name": "AEM",
        "handler": {
          "graphql": {
            "endpoint": "<your_AEM_url>"
          }
        },
        "transforms": [
          {
            "prefix": {
              "includeRootOperations": true,
              "value": "AEM_"
            }
          }
        ]
      }
    ]
  },
  "tenantId": "<your_tenant_id"
}
```

## Filter

[Filter] transforms allow you to specify which fields are returned in your response. In the example below, the [PWA] handler will only return the Category and Customer Order information in its responses.

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
            "endpoint": "<your_AEM_url>"
          }
        }
      }
    ]
  },
  "tenantId": "<your_tenant_id>"
}
```

## Replace

[Replace] transforms allow you to replace one the configuration properties of one field with another. In the example below, the `parent` field is being replaced by the `child` field.

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
                    "field": "child"
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

[Type Merge] transforms allow you to combine multiple sources by merging a type from each source. For example, you could combine responses from two different APIs on a single field,provided you [rename] the fields you want to stitch to the same name. For more information, see this [GraphQL Mesh Example].

<!-- I couldn't really come up with an example here, so linking out made more sense to me. -->

## Naming Convention

[Naming Convention] transforms allow you to apply casing and other conventions to your response. In the example below, `category` fields are converted to uppercase, while `urlResolver` fields are converted to lowercase.


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
        "transforms": [
          {
            "namingConvention": {
              "category": uppercase
              "urlResolver": lowercase
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
[Filter]: https://www.graphql-mesh.com/docs/transforms/filter-schema
[Replace]: https://www.graphql-mesh.com/docs/transforms/replace-field
[Type Merge]: https://www.graphql-mesh.com/docs/transforms/type-merging
[Naming Convention]: https://www.graphql-mesh.com/docs/transforms/naming-convention
[Federation]: https://www.graphql-mesh.com/docs/transforms/federation
[Encaspsulate]: https://www.graphql-mesh.com/docs/transforms/encapsulate
[GraphQL Mesh Example]: https://www.graphql-mesh.com/docs/recipes/multiple-apis#merging-types-from-different-sources-using-type-merging