---
title: Adobe I/O Events for Adobe Commerce Release Notes
details: This page lists new features and known issues for each release of Adobe I/O Events for Adobe Commerce.
---

# Release notes

These release notes describe the latest version of Adobe I/O Events for Adobe Commerce.

## Version 1.1.0

### Release date

April 20, 2023

### Enchancements

*  Added support for delivering events using message queues. Previously, all events were delivered by cron, which could delay delivery by up to a minute. Use the `bin/magento events:subscribe --priority` command to register the event as requiring expedited delivery. See [Configure Adobe Commerce](configure-commerce.md/#check-cron-and-message-queue-configuration) for information about configuring the message queues.

*  Added the ability to filter on fields in an array of nested objects. For example, you can retrieve the `sku` and `qty` fields in an `items[]` array as shown:

```text
items[].sku
items[].qty
```

Previously, you could not specify individual fields within an array. All fields within the array would be returned. [Commerce module development](module-development.md/#array-of-nested-objects) demonstrates this feature in full context.

### Bug fixes

- Fixed conversion of an nested array of objects. Previously they were omitted in the final payload.
- Fixed displayed event information for multiple events that are based on `DataObjects`.
- Fixed performance degradation that occurred when an event was being saved.
* Made other minor bug fixes.

## Version 1.0.1

### Release date

February 8, 2023

### Enhancements

*  Added the `storeId`, `websiteId`, and `storeGroupId` to the event payload metadata whenever it is available.

*  Changed the template engine that is used in the generation of the module with plugins for eventing.

### Bug fixes

*  Fixed multiple static and integration test failures.

## Version 1.0.0

### Release date

January 17, 2023

### Compatibility

Adobe Commerce for Cloud

*  2.4.5+, 2.4.6 beta
*  `ece-tools` 2002.1.13+

Adobe Commerce (on-premises)

*  2.4.5+
*  2.4.6 beta

### Known issues

*  Registering a plugin-type event rule causes the system to generate a plugin for the parent rule. No additional generation is required for observer-type events.

*  Conditional events that evaluate as false are not stored in the database or sent to the eventing service.
