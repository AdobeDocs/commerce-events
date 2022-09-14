---
title: Commerce module development
description: 
---

# Commerce module development

This topic 

## Register events

You can register events in two ways:

### io_events.xml

Add `app/etc/io_events.xml` file to your module with a list of events that should be always emitted (can be disabled during runtime). For example:

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
    <event name="observer.store_group_save_after" />
    <event name="plugin.magento.sales.api.invoice_item_repository.save" />
    <event name="plugin.magento.configurable_product.api.option_repository.save" />
    <event name="plugin.magento.gift_wrapping.model.resource_model.wrapping.save" />
</config>
```

### config.php

Or add a list of events to the io_events section in app/etc/config.php file, for example:

```php
'io_events' => [
    'plugin.magento.catalog.api.product_link_repository.save' => 1,
    'plugin.magento.catalog.api.product_repository.save' => 1,
    'plugin.magento.theme.model.resource_model.theme.file.save' => 0,
    'observer.catalog_product_save_after' => 1
]
```