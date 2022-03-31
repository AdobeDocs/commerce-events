---
title: Command Reference
description: A description of the CLI commands available for Adobe Graph.
---

# Command Reference

The Adobe Graph CLI allows you to manage and modify tenants. This page covers commands exclusive to Adobe Graph. For authorization and other Adobe I/O Extensible CLI commands, refer to the [Adobe IO CLI command list]. For installation instructions, refer to [Getting Started].

All commands on this page support the `--help` argument, which provides information about the command.

-  [aio commerce-gateway:tenant:create](aio_commerce-gateway:tenant:create)
-  [aio commerce-gateway:tenant:update](#aio_commerce-gateway:tenant:update)
-  [aio commerce-gateway:tenant:get](#aio_commerce-gateway:tenant:get)

## aio commerce-gateway:tenant:create

Creates a new tenant based on the settings in the specified JSON `[FILE]` in your working directory. The `tenantId` key value in your `JSON` file determines the name for your tenant. For more information, see [Creating a tenant].

### Usage

```bash
aio commerce-gateway:tenant:create [FILE]
```

## aio commerce-gateway:tenant:update

Updates an existing tenant based on the settings in the specified JSON `[FILE]`. Specify the `[TENANTID]` for the tenant you want to update and the `JSON` file that contains your updated settings. For more information, see [Updating a tenant].

<InlineAlert variant="info" slots="text"/>

You cannot modify the `tenantId` when updating a tenant.

### Usage

```bash
aio commerce-gateway:tenant:update [TENANTID] [FILE]
```

## aio commerce-gateway:tenant:get

Retrieves the current `JSON` mesh file for the specified tenant. Specify the `[TENANTID]` for the tenant you want to view.

### Usage

```bash
aio commerce-gateway:tenant:get [TENANTID]
```

<!-- Link Definitions -->
[Getting Started]:../gateway/getting-started
[Adobe IO CLI command list]:https://github.com/adobe/aio-cli#commands
[Creating a tenant]:../gateway/create-tenant
[Updating a tenant]:../gateway/create-tenant#update_an_existing_tenant
