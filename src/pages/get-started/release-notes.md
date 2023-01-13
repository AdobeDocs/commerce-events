---
title: Adobe I/O Events for Adobe Commerce Release Notes
details: This page lists new features and known issues for each release of Adobe I/O Events for Adobe Commerce.
---

# Release notes

These release notes describe the latest version of Adobe I/O Events for Adobe Commerce.

## 1.0.0

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
