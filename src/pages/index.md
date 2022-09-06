---
title: Commerce Extensibility
description: 
---

<Hero slots="image, heading, text"/>

![Commerce Extensibility](_images/home-bg.jpeg)

# Adobe Commerce Extensibility

Learn how to create extensions for Adobe Commerce and Magento Open Source using the Adobe infrastructure.

<Resources slots="heading, links"/>

#### Resources

* [PHP Developer Guide](https://developer.adobe.com/commerce/php/development/)

## Overview

This documentation provides resources for developing extensions for Adobe Commerce and Magento Open Source using the Adobe infrastructure.

## Discover

Use these sections to learn more.

<DiscoverBlock slots="heading, link, text"/>

### Sections

[]()
<!-- [Calculated Metrics API]() 

[Quickstart Guide](guides/)

Get started with the Adobe Analytics APIs.

<DiscoverBlock slots="heading, link, text"/>

### Guides

     
Returns information on the user's company that is necessary for making other Adobe Analytics API calls.

<DiscoverBlock slots="link, text"/>

[Segments API](guides/segments_api/) 

Provides configuration guidance and best practices for the /segments endpoint.

<DiscoverBlock slots="link, text"/>

[Reporting Guide API](guides/reporting_api/)

Provides configuration guidance and best practices for the /reports endpoint.

<DiscoverBlock slots="link, text"/>

[Migrating from 1.4 to 2.0](guides/migrating/)

For help migrating from the 1.4 versions of the Analytics API to the newer and more capable /reports API.   

<DiscoverBlock width="100%" slots="heading, link, text"/>

### API References

[Try the API](api/) 

Try the Analytics API with Swagger UI. Explore, make calls, with full endpoint descriptions. -->

## Contributing

We encourage you to participate in our open documentation initiative, if you have suggestions, corrections, additions
or deletions for this documentation, check out the source from [this github repo](https://github.com/adobe/gatsby-theme-spectrum-example), and submit a pull
request with your contribution.

## API Requests & Rate Limits

The timeout for API requests through adobe.io is currently *60 seconds*.

The default rate limit for an Adobe Analytics Company is *120 requests per minute*. (The limit is enforced as *12 requests every 6 seconds*).
When rate limiting is being enforced you will get `429` HTTP response codes with the following response body: `{"error_code":"429050","message":"Too many requests"}`
