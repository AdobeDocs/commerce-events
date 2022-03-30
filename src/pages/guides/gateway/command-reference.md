---
title: Command Reference
description: A description of the CLI commands available for Adobe Graph.
---

# Command Reference

The Adobe Graph CLI allows you to manage and modify tenants. Select the desired command below to view more information:

-  [aio commerce-gateway:tenant:create](aio_commerce-gateway:tenant:create)
-  [aio commerce-gateway:tenant:update](#aio_commerce-gateway:tenant:update)
-  [aio commerce-gateway:tenant:get](#aio_commerce-gateway:tenant:get)
-  [aio config:set](#aio_config:set)

## Installation

For installation instructions, refer to [Getting Started].

## AIO Commands

This page covers commands exclusive to Adobe Graph. For authorization and other Adobe I/O Extensible CLI commands, refer to the [Adobe IO CLI command list].

## aio commerce-gateway:tenant:create

Creates a new tenant based on the settings in the specified `JSON` file in your working directory. The `tenantId` key value in your `JSON` file determines the name for your tenant. For more information, see [Creating a tenant].

### Usage

```bash
aio commerce-gateway:tenant:create mesh.json
```

### Options

```txt
--help      Information about command usage
```

## aio commerce-gateway:tenant:update

Updates an existing tenant based on the settings in the specified `JSON` file. Specify the `tenantId` for the tenant you want to update and the `JSON` file that contains your updated settings. For more information, see [Updating a tenant].

<InlineAlert variant="info" slots="text"/>

Updating the tenant does not allow you to rename the tenantId.

### Usage

```bash
aio commerce-gateway:tenant:update tenantId mesh.json
```

### Options

```txt
--help      Information about command usage
```

## aio commerce-gateway:tenant:get

Retrieves the current `JSON` mesh file for the specified tenant. Specify the `tenantId` for the tenant you want to view.

### Usage

```bash
aio commerce-gateway:tenant:get tenantId
```

### Options

```txt
--help      Information about command usage
```

## aio config:set

The config file is a `JSON` file that specifies your `baseUrl` and `apikey`. This command sets or updates the configuration file.

### Usage

```bash
aio config:set aio-cli-plugin-commerce-admin config.json
```

### Options

```txt
--help      Information about command usage
```

<!-- Link Definitions -->
[Getting Started]:../guides/gateway/getting-started
[Adobe IO CLI command list]:https://github.com/adobe/aio-cli#commands
[Creating a tenant]:../guides/gateway/create-tenant
[Updating a tenant]:../guides/gateway/create-tenant#update_an_existing_tenant
