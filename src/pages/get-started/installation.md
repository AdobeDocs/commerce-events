---
title: Install Adobe I/O Events for Adobe Commerce
description: Learn how to install the Commerce modules needed for Adobe I/O Events for Adobe Commerce.
---

# Install and configure Adobe I/O Events for Adobe Commerce

Adobe I/O Events for Adobe Commerce requires Adobe Commerce 2.4.5 or later. Magento Open Source is not supported.

After you have created an [App Builder project](./project-setup.md), you must install the Commerce modules that enable integrations with Adobe I/O Events.

## Install Adobe I/O modules on Commerce

The following steps apply to both Adobe Commerce on cloud infrastructure and on-premises installations. Cloud customers must perform additional steps to configure the `ece-tools` package.

1. If you are running Commerce 2.4.5, use the following command to load the eventing modules:

   ```bash
   composer require magento/commerce-eventing=^1.0 --no-update
   ```

   Commerce 2.4.6 beta and later loads these modules automatically.

1. Update the project dependencies.

  ```bash
  composer update
  ```

1. Run the following command to initialize the `AdobeCommerceEvents` module. This module consists of generated plugins based on a list of subscribed events and helps publish and process events.

   ```bash
   bin/magento events:generate:module
   ```

1. Enable the new modules:

   ```bash
   bin/magento module:enable --all
   ```

1. Upgrade your instance:

   ```bash
   bin/magento setup:upgrade
   ```

1. Compile your instance to generate new classes:

   ```bash
   bin/magento setup:di:compile
   ```

## Cloud configuration

Use the following steps to perform additional configuration for Adobe Commerce on cloud infrastructure:

1. Add the `app/etc/config.php` file to your working repository:

   ```bash
   git add app/etc/config.php
   ```

1. Run the `php vendor/bin/ece-tools -V` command to determine your version of ece-tools. If the version is less than `2002.1.13`, [update to the most recent version](https://devdocs.magento.com/cloud/project/ece-tools-update.html).

1. Enable eventing in the `.magento.env.yaml` file:

   ```yaml
   stage:
      global:
         ENABLE_EVENTING: true
   ```

1. Commit and push updated files to the Cloud environment.