---
title: Adobe Commerce eventing commands 
description: Learn about the commands needed to set up eventing on an Adobe Commerce instance.
---

# Eventing commands

Adobe I/O Events allow you to receive notifications of real-time events taking place in Adobe services.

## Create an event provider

The `events:create-event-provider` command creates an event provider ID in Adobe IO Events and returns this ID. Add the generated ID as the value of the **Stores** > Configuration > **Adobe Services** > **Adobe IO Events** > **Adobe I/O Event Provider ID** field in the Commerce Admin.

<InlineAlert variant="info" slots="text"/>
Ensure the Commerce root directory contains a valid `app/etc/event-types.json` file before running this command.

<!--Add a cross reference to the discussion about this file when it becomes available. -->

### Usage

`bin/magento events:create-event-provider`

### Example

```bash
bin/magento events:create-event-provider
```

### Response

```terminal
No event provider found, a new event provider will be created

A new event provider has been created with ID <ID>.
```

## Subscribe to a Commerce event

The `events:subscribe` command subscribes the current provider to the specified event. You must define the event code using the following pattern:

```text
<type>.<event_name>
```

where:

-  `type` specifies the origin of the event. Specify `observer` if the event is emitted by a Commerce observer, or specify `plugin` if the event is emitted by a plugin.
-  `event_name` identifies the event to subscribe. For example: `catalog_product_save_after`.

### Usage

`bin/magento events:subscribe <event_code>`

### Arguments

`<event_code>` Required. Specifies the event to subscribe to. The value must in the format `<type.event_name>`.

### Example

```bash
bin/magento events:subscribe observer.catalog_product_save_after
```

### Response

```terminal
The subscription catalog_product_save_after was successfully created
```

## Unsubscribe from a Commerce event

The `events:unsubscribe` command causes the current provider to unsubscribe from the specified event. Use the `bin/magento events:list` command to retrieve list of subscribed events.

<InlineAlert variant="info" slots="text"/>

Do not include `com.adobe.commerce.` in the event code. The code must begin with `observer` or `plugin`.

### Usage

`bin/magento events:unsubscribe <event_code>`

### Arguments

`<event_code>` Required. Specifies the event to unsubscribe from.

### Example

```bash
bin/magento events:unsubscribe observer.catalog_product_save_after
```

### Response

```terminal
Successfully unsubscribed from the `observer.catalog_product_save_after` event
```

## List subscribed Commerce events

The `events:list` command returns a list of subscribed events.

### Usage

`bin/magento events:list`

### Example

```bash
bin/magento events:list
```

### Response

```terminal
com.adobe.commerce.observer.catalog_product_save_after
com.adobe.commerce.plugin.magento.sales.api.order_repository.delete
com.adobe.commerce.plugin.magento.sales.api.order_repository.save
```
