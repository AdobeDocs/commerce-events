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

For each event you register, you must define which fields to transmit to your App Builder application. The payload of an event can be massive. In addition, some events include sensitive or PCI compliance data by default. The following payload is returned for the `com.adobe.commerce.observer.catalog_product_save_after` event:

```json
{
  position: 'camel:3f25472c-5f70-4f36-84be-2248d2e2d421.camel:9cd9055e-f56d-4fd3-8017-9db5ac1c2b3b.e501d499-cfc8-450d-bc42-8637bbf12601.0.1663803959.o3fdxogbstwwnxbxq2r-',
  event: {
    data: {
      key: '04ac68a4-27bd-4339-a6b1-fddb3c968a2b',
      value: [Object],
      source: 'demo.demo',
      pipelineMessageId: '1663803954935:545088:VA7_A1:128:0'
    },
    id: 'c23e6bec-f1ee-4eef-97ec-cf22f04bc5b1',
    source: 'urn:uuid:9dd02f4d-c439-481a-9ccd-ac43af7ae821',
    specversion: '1.0',
    type: 'com.adobe.commerce.observer.catalog_product_save_after',
    datacontenttype: 'application/json',
    time: '2022-09-21T23:45:56.702Z',
    event_id: 'e501d499-cfc8-450d-bc42-8637bbf12601',
    recipient_client_id: '76e82e864661495b89d774b0e49adfb9'
  }
}
{
  key: '04ac68a4-27bd-4339-a6b1-fddb3c968a2b',
  value: {
    _edit_mode: true,
    store_id: '0',
    entity_id: '3',
    attribute_set_id: '4',
    type_id: 'simple',
    sku: 'test2',
    has_options: false,
    required_options: false,
    created_at: '2022-07-28 14:13:40',
    updated_at: '2022-09-06 20:37:25',
    row_id: '3',
    created_in: '1',
    updated_in: '2147483647',
    name: 'test2',
    meta_title: 'test2',
    meta_description: 'test2 ',
    page_layout: 'product-full-width',
    options_container: 'container2',
    url_key: 'test2',
    msrp_display_actual_price_type: '0',
    gift_message_available: '2',
    gift_wrapping_available: '2',
    is_returnable: '2',
    status: '1',
    visibility: '4',
    tax_class_id: '2',
    price: '123.000000',
    meta_keyword: 'test2',
    options: [],
    media_gallery: { images: [], values: [] },
    tier_price: [],
    tier_price_changed: 0,
    quantity_and_stock_status: {
        is_in_stock: '1', 
        qty: '333'
    },
    category_ids: [ '4' ],
    is_salable: 1,
    stock_data: {
      item_id: '3',
      product_id: '3',
      stock_id: '1',
      qty: '333',
      min_qty: '0',
      use_config_min_qty: '1',
      is_qty_decimal: '0',
      backorders: '0',
      use_config_backorders: '1',
      min_sale_qty: '1',
      use_config_min_sale_qty: '1',
      max_sale_qty: '10000',
      use_config_max_sale_qty: '1',
      is_in_stock: '1',
      notify_stock_qty: '1',
      use_config_notify_stock_qty: '1',
      manage_stock: '1',
      use_config_manage_stock: '1',
      stock_status_changed_auto: '0',
      use_config_qty_increments: '1',
      qty_increments: '1',
      use_config_enable_qty_inc: '1',
      enable_qty_increments: '0',
      is_decimal_divided: 0,
      website_id: '0',
      deferred_stock_update: '0',
      use_config_deferred_stock_update: '1',
      type_id: 'simple',
      min_qty_allowed_in_shopping_cart: [Array]
    },
    use_config_gift_message_available: '1',
    use_config_gift_wrapping_available: '1',
    current_product_id: '3',
    affect_product_custom_options: '1',
    current_store_id: '0',
    product_has_weight: '1',
    is_new: '0',
    website_ids: { '1': '1' },
    url_key_create_redirect: 'test2',
    ignore_links_flag: false,
    can_save_custom_options: true,
    save_rewrites_history: true,
    can_save_bundle_selections: false,
    is_custom_option_changed: true,
    parent_id: 0,
    special_from_date_is_formated: true,
    custom_design_from_is_formated: true,
    news_from_date_is_formated: true,
    news_to_date_is_formated: true,
    is_changed_categories: false,
    is_changed_websites: false,
    force_reindex_eav_required: true
  },
  source: 'demo.demo',
  pipelineMessageId: '1663803954935:545088:VA7_A1:128:0'
}
```

You define an array of fields to transmit in your configuration file. Specify any field within an event's `value` object to ensure it is transmitted to an application. If the field is part of a secondary object, such `stock_data` in the above example, use the format `<object-name>.field`. For example: `stock_data.product_id`.

After you've registered at least one event, run the [events:generate:module command](./commands.md#generate-a-commerce-module-based-on-a-list-of-subscribed-events) to generate the required plugins.

### io_events.xml

Create the `<module-root>/etc/io_events.xml` file and define a list of events that should always be transmitted. Events listed in this file cannot be disabled with the [`events:unsubscribe` command](./commands.md#unsubscribe-from-a-commerce-event).

The following example registers multiple events. The `<fields>` element defines the contents of each transmitted event.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
    <event name="commerce.observer.catalog_product_save_after">
        <fields>
            <field name="entity_id" />
            <field name="sku" />
            <field name="is_new" />
        </fields>
    </event>
    <event name="plugin.magento.gift_wrapping.model.resource_model.wrapping.save">
        <fields>
            <field name="id" />
            <field name="name" />
            <field name="status" />
        </fields>
    </event>
    <event name="plugin.magento.sales.api.invoice_item_repository.save">         
        <fields>
            <field name="id" />
            <field name="name" />
            <field name="status" />
        </fields>
    </event>
    <event name="observer.store_group_save_after">
        <fields>
            <field name="id" />
            <field name="name" />
        </fields>
    </event>
</config>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">

</config>
```

The contents of an `commerce.observer.catalog_product_save_after` event are similar to the following:

```json
{
  position: 'camel:3f25472c-5f70-4f36-84be-2248d2e2d421.camel:9cd9055e-f56d-4fd3-8017-9db5ac1c2b3b.e501d499-cfc8-450d-bc42-8637bbf12601.0.1663803959.o3fdxogbstwwnxbxq2r-',
  ....
}
{
  key: '04ac68a4-27bd-4339-a6b1-fddb3c968a2b',
  value: {
    entity_id: '3',
    sku: 'test2',
    is_new: '0'
  },
  source: 'demo.demo',
  pipelineMessageId: '1663803954935:545088:VA7_A1:128:0'
}
```

### config.php

You can also create an `io_events` section in the Commerce [`app/etc/config.php file`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html). Events registered using this mechanism can be disabled from the command line.

For example:

```php
'io_events' => [
    'commerce.observer.catalog_product_save_after' => [
        'fields' => [
            'entity_id',
            'sku',
            'is_new',
        ],
        'enabled' => 1
    ],
    'plugin.magento.catalog.api.product_link_repository.save' => [
        'fields' => [
            'id',
            'name',
        ],
        'enabled' => 1
    ],
     'plugin.magento.catalog.api.product_link_repository.delete' => [
        'fields' => [
            'id',
        ],
        'enabled' => 1
    ],
]
```

The payload for the `commerce.observer.catalog_product_save_after` event is the same as shown in [`io_events.xml`](#io_events.xml).

## Configuration merging from different modules

If multiple Commerce modules register the same event, the configuration manager merges the two configurations. The transmitted event includes all the fields defined in the modules.

In the following example, the `MODULE1/etc/io_events.xml` file registers the `commerce.observer.catalog_product_save_after` event with the `entity_id` and `sku` fields.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
    <event name="commerce.observer.catalog_product_save_after">
        <fields>
            <field name="entity_id" />
            <field name="sku" />
        </fields>
    </event>
</config>
```

The `MODULE2/etc/io_events.xml` file registers the same event with the `entity_id` field and two others.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
    <event name="commerce.observer.catalog_product_save_after">
        <fields>
            <field name="entity_id" />
            <field name="quantity_and_stock_status.qty" />
            <field name="stock_data.min_qty" />
        </fields>
    </event>
</config>

The resulting configuration:

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module-commerce-events-client/etc/io_events.xsd">
    <event name="commerce.observer.catalog_product_save_after">
        <fields>             
            <field name="entity_id" />
            <field name="sku" />
            <field name="quantity_and_stock_status.qty" />
            <field name="stock_data.min_qty" />
        </fields>
    </event>
</config>
```