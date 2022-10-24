---
title: Adobe Commerce event management commands 
description: Provides details about the commands needed to set up Adobe I/O Events for Adobe Commerce.
---

# Event management commands

Adobe I/O Events allow you to receive notifications of real-time events taking place in Adobe services.

Adobe Commerce provides the following commands to configure and process events:

*  Enable integration between Commerce and Adobe I/O events
   *  [events:create-event-provider](#create-an-event-provider)  

*  Manage events subscriptions
   *  [events:subscribe](#subscribe-to-a-commerce-event)
   *  [events:metadata:populate](#create-event-metadata-in-adobe-io)
   *  [events:unsubscribe](#unsubscribe-from-a-commerce-event)
   *  [events:list](#list-subscribed-commerce-events)

*  Manage registrations
   *  [events:registration:create](#create-a-registration)
   *  [events:registration:delete](#delete-a-registration)
   *  [events:registration:get-all](#get-registration-details)

*  Generate a Commerce module
   *  [events:generate:module](#generate-a-commerce-module-based-on-a-list-of-subscribed-events)

## Create an event provider

The `events:create-event-provider` command creates an event provider ID in Adobe IO Events and displays this ID. Add the generated ID as the value of the **Stores** > Configuration > **Adobe Services** > **Adobe IO Events** > **Adobe I/O Event Provider ID** field in the Commerce Admin.

The `--label` and `--description` arguments are optional. However, if you do not specify them, then you must create a system `app/etc/event-types.json` file and define the values in that file before running the `events:create-event-provider` command. We recommend specifying the `--label` and `--description` arguments when you run the command.

If you decide to omit the arguments, the `event-types.json` file must have the following format:

```json
{
 "provider": {
     "label": "My Adobe Commerce Events",
     "description": "Provides out-of-process extensibility for Adobe Commerce"
     }
 }
```

### Usage

`bin/magento events:create-event-provider --label "<unique_provider_label>" --description "<provider description>"`

### Arguments

`--label` A name that distinguishes your event provider from others in the project. The name can contain English alphanumeric characters and underscores (_) only. The first character must be a letter.

`--description` A string that describes your event provider.

### Example

```bash
bin/magento events:create-event-provider --label "my_new_event_provider" --description "Event provider in the Stage workspace"
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

*  `type` specifies the origin of the event. Specify `observer` if the event is emitted by a Commerce observer, or specify `plugin` if the event is emitted by a plugin.
*  `event_name` identifies the event to subscribe. For example: `catalog_product_save_after`.

<InlineAlert variant="info" slots="text"/>

You can subscribe to any available observer event. You cannot subscribe to a plugin event unless it was registered in the `app/etc/config.php` file and subsequently unsubscribed with the [`events:unsubscribe` command](#unsubscribe-from-a-commerce-event). [Register events](./module-development.md#register-events describes the format of these files.)

Adobe Commerce does not send all event fields to your external application by default. Instead, you must use the `--field` option to specify which fields to transmit. To send all event fields, you must specify a separate `--field` option for each field. This practice keeps data transmission to a minimum and helps prevent accidentally sending sensitive information. If the Commerce event contains objects, use dotted notation to specify fields within an object. For example, For example, if your event contains a `stock_data` object, and you want to send its `product_id` and `qty` fields, you would specify the `--fields stock_data.product_id` and `--fields stock_data.qty` command options. [Commerce module development](./module-development.md) provides a detailed example of using files to register events.

### Usage

`bin/magento events:subscribe <event_code> --fields=<name1> --fields=<name2>`

### Arguments

`<event_code>` Required. Specifies the event to subscribe to. The value must in the format `<type.event_name>`.

### Options

`--fields=<field_name>` Required. An event field to transmit to the Adobe App Builder application. You can specify this option multiple times. Each instance can contain only one field name.

### Example

```bash
bin/magento events:subscribe observer.catalog_product_save_after --fields=sku --fields=stock_data.qty 
```

### Response

```terminal
The subscription com.adobe.commerce.observer.catalog_product_save_after was successfully created
```

## Unsubscribe from a Commerce event

The `events:unsubscribe` command causes the current provider to unsubscribe from the specified event. You cannot unsubscribe from events defined in a module's `etc/io_events.xml` file. However, you can unsubscribe events that were registered in the `app/etc/config.php` file or from the [`events:subscribe` command](#subscribe-to-a-commerce-event).

Use the [`events:list` command](#list-subscribed-commerce-events) to retrieve a list of subscribed events.

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
observer.catalog_product_save_after
observer.customer_login
```

## Create a registration

The `events:registration:create` command registers the merchant to Adobe Identity Management Services. You must configure the **Stores** > Configuration > **Adobe Services** > **Adobe I/O Events** > **Commerce Events** > **Merchant ID** and **Environment ID** fields before running this command.

### Usage

`bin/magento events:registration:create`

### Example

```bash
bin/magento events:registration:create
```

### Response

```terminal
Registration created
```

## Delete a registration

The `events:registration:delete` command deletes the specified registrant from the IMS organization.

### Usage

`bin/magento events:registration:delete <registration-id>`

### Arguments

`<registration-id>` Required. The ID assigned to the registration. Use the `events:registration:get-all` command to retrieve the ID.

### Example

```bash
bin/magento events:registration:delete e037f0de-3489-49c7-9366-df86491072b4
```

### Response

```terminal
Registration was deleted
```

## Get registration details

The `events:registration:get-all` command returns details about a registrant. The response includes the registration ID, merchant ID, environment ID, IMS organization ID, and instance ID.

### Usage

`bin/magento events:registration:get-all`

### Example

```bash
bin/magento events:registration:get-all
```

### Response

```terminal
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| e037f0de-3489-49c7-9366-df86491072b4 | {"id":"e037f0de-3489-49c7-9366-df86491072b4","merchant_id":"extension-docs","environment_id":"extension-docs","ims_org_id":"12345678901234567890ABCD@AdobeOrg","instance_id":"extensibility-docs2","event_provider_metadata":"3rd_party_custom_events"} |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

## Create event metadata in Adobe I/O

The `events:metadata:populate` command creates event metadata based on XML and application configurations.

### Usage

`events:metadata:populate`

### Example

```bash
bin/magento events:metadata:populate
```

### Response

```terminal
The events metadata was successfully created:
```

## Generate a Commerce module based on a list of subscribed events

The `events:generate:module` command generates a module with plugins based on your configuration and places it into the Commerce `app/code/Magento/AdobeCommerceEvents` directory.

### Usage

`bin/magento events:generate:module`

### Example

```bash
bin/magento events:generate:module
```

### Response

```terminal
Module was generated in the app/code/Magento directory
```
