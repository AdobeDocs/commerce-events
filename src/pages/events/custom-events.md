---
title: Adding custom event
description: Add custom events using declarative configuration
---

# Adding custom event

In some cases, the client may need only events that meet certain conditions.

For example, the client wants only to receive events when the stock of some products is low, currently, it's only
possible to achieve this by sending all events on product or inventory updates and filtering them on the client side.
Such an approach leads to sending a large amount of not necessary data and adding additional logic to filter it.

You can subscribe either to available Commerce events or declare your own events as an extension to original event. In a
case, if no subscription to original events was defined - only custom event will be emitted.

## Defining custom event

To create a custom event, it has to be defined in `io_events.xml` configuration.

In the following example, we define a new event
called `plugin.magento.catalog.model.resource_model.product.save_low_stock_event` when original
event `plugin.magento.catalog.model.resource_model.product.save` is triggered.

The required criteria for the event is when `qty` fields of original event is `less` than `20`

```xml

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="...">
    <event name="plugin.magento.catalog.model.resource_model.product.save_low_stock_event"
           original="plugin.magento.catalog.model.resource_model.product.save">
        <fields>
            <field name="id"/>
            <field name="sku"/>
            <field name="qty"/>
        </fields>
        <rules>
            <AndRule>
                <field>qty</field>
                <operator>lessThan</operator>
                <value>20</value>
            </AndRule>
        </rules>
    </event>
</config>
```

Next configuration is required for custom event:

| Field    | Description                                               |
|----------|-----------------------------------------------------------|
| name     | A unique name of event                                    |
| original | An original event that has to be triggered                |
| fields   | A set of fields from original event                       |
| rules    | A set of rules that has to be met to trigger custom event |

## Rules

Rules can be defined as set or logical operators.

| Rule    | Description      |
|---------|------------------|
| AndRule | Logical AND rule |
| OrRule  | Logical OR rule  |

A combination of rules can be used together.

```xml

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="...">
    <event name="plugin.magento.catalog.model.resource_model.product.save_low_stock_event"
           from="plugin.magento.catalog.model.resource_model.product.save"
    >
        <fields>
            <field name="id"/>
            <field name="sku"/>
            <field name="qty"/>
        </fields>
        <rules>
            <AndRule>
                <field>qty</field>
                <operator>lessThan</operator>
                <value>20</value>
            </AndRule>
            <AndRule>
                <field>price</field>
                <operator>greaterThan</operator>
                <value>100</value>
            </AndRule>
            <AndRule>
                <field>category_id</field>
                <operator>in</operator>
                <value>3,4,5</value>
            </AndRule>
            <AndRule>
                <field>name</field>
                <operator>Regex</operator>
                <value>/^TV .*/i</value>
            </AndRule>
        </rules>
    </event>
</config>
```

## Operators

Operators are represented as a comparison statements between desired and original values

| Operator    | Description                                                                                                     |
|-------------|-----------------------------------------------------------------------------------------------------------------|
| greaterThan | Greater than original value                                                                                     |
| lessThan    | Less than original value                                                                                        |
| equal       | Equal to original value                                                                                         | 
| regex       | Value must be compatible with [regular expression match](https://www.php.net/manual/en/function.preg-match.php) |
| in          | Value presents in a comma-separated list of value                                                               |

## Known limitations

* If the event is registered with custom rules the plugin will be generated for the original event if it's a plugin-type
  event. For observer-type events, no additional generation is required.
* Before storing event data in the database for batch sending will be added additional checking if the rules were met (
  in case the event is registered with rules). If rules weren't met event won't be stored in the database and won't be
  sent to the Eventing Service.
