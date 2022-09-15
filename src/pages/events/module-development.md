---
title: Commerce module development
description: Learn what you need to do to enable your modules for Adobe I/O Events.
---

# Commerce module development

This topic describes how to enable your custom modules for Adobe I/O Events. You can also manually register observer events using the [`events:subscribe` command](./commands.md#subscribe-to-a-commerce-event).

## Register events

You can programmatically register events using the following methods:

*  Create an `io_events.xml` file in your module
*  Declare them in the system `config.php` file

### io_events.xml

Create the `<module-root>/app/etc/io_events.xml` file and define a list of events that should always be emitted. Events listed in this file cannot be disabled with the [`events:unsubscribe` command](./commands.md#unsubscribe-from-a-commerce-event).

The following example registers multiple events.

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
   <event name="observer.customer_login" />
   <event name="observer.customer_logout" />
   <event name="plugin.magento.catalog.api.category_repository.save" />
   <event name="plugin.magento.catalog.api.category_repository.save" />
   <event name="plugin.magento.catalog.api.category_repository.delete" />
   <event name="plugin.magento.catalog.api.product_repository.save" />
   <event name="plugin.magento.catalog.api.product_repository.delete" />
   <event name="plugin.magento.customer.api.customer_repository.save" />
   <event name="plugin.magento.customer.api.customer_repository.delete" />
   <event name="plugin.magento.sales.api.order_repository.save" />
   <event name="plugin.magento.sales.api.order_management.place" />
</config>
```

Run the [events:generate:module command](./commands.md#generate-a-commerce-module-based-on-a-list-of-subscribed-events) to generate the required plugins.

### config.php

You can also create an `io_events` section in the Commerce [`app/etc/config.php file`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html). Events registered using this mechanism can be disabled from the command line.

For example:

```php
'io_events' => [
    'observer.customer_login' => 1,
    'observer.customer_logout' => 1,
    'plugin.magento.catalog.api.category_repository.save' => 1,
    'plugin.magento.catalog.api.category_repository.delete' => 1,
    'plugin.magento.catalog.api.product_repository.save' => 1,
    'plugin.magento.catalog.api.product_repository.delete' => 1,
    'plugin.magento.customer.api.customer_repository.save' => 1,
    'plugin.magento.customer.api.customer_repository.delete' => 1,
    'plugin.magento.sales.api.order_management.save' => 1,
    'plugin.magento.sales.api.order_management.place' => 1
]
```

Run the [events:generate:module command](./commands.md#generate-a-commerce-module-based-on-a-list-of-subscribed-events) to generate the required plugins.
