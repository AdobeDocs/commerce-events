---
title: Set up an eventing project
details: Create a project in the Adobe Developer Console, generate API credentials, and download the workspace configuration.
---

# Set up an eventing project

Adobe I/O Events for Adobe Commerce allows you to send and monitor custom Adobe Commerce user-driven events. Follow the instructions on this page to create and configure a project for Adobe I/O Events.

## Requirements

To get started with Adobe I/O Events, you will need:

*  An Adobe Developer account. (Getting started with Adobe Developer Console
)[https://developer.adobe.com/developer-console/docs/guides/getting-started/] describes how to enroll in the Adobe developer program.

*  An Adobe Commerce on cloud infrastructure or on-premises instance.

## Set up a project

[Projects Overview](https://developer.adobe.com/developer-console/docs/guides/projects/) describes the different types of projects and how to manage them. Here, we'll create a templated project. 

1. In the Adobe Developer Console, select the desired organization from the dropdown menu in the top-right corner.

1. Select **Create project from template**.

   ![Create a project](../_images/create-project.png)

1. On the **Set up templated project** page, specify a project title and app name. Then click **Save**.

   ![Add an api](../_images/set-up-templated-project.png)

1. In the Stage workspace, click the **Add service** pop-up menu and select **API**.

1. On the **Add an API** page, filter by Adobe Services, select **I/O Events**, and click **Next**.

   ![Select IO events](../_images/adobe-io-events.png)

1. Select the **Generate a key pair** option and click **Generate keypair**. The `config.zip` file downloads automatically.

   ![generate a key pair](../_images/generate-key-pair.png)
   
   **Note**: If you want to manually [create a public key certificate], you can select the **Upload your public key** option.

1. Click **Save configured API**.

1. Unzip the downloaded `config.zip` file. The extracted `config` directory should contain a `certificate_pub.crt` and a `private.key` file.


## Download the workspace configuration

To download a `.json` file containing your workspace configuration:

1. Open the corresponding project on the [Adobe Developer Console].

1. Select the **Project Overview** and click the **Download** button.

   ![download the workspace config](../_images/download-workspace-config.png)

   The `<Workspace-name>.json` file downloads automatically.

## Set up App Builder locally (optional)


1. Log in to Adobe IO:

   ```bash
   aio login
   ```
   
   Your web browser displays the log in page. Enter your Adobe ID credentials.
   
1. Return to your terminal and navigate to where you want to initialize your project. Enter the following command:

   ```bash
   aio app init
   ```

   The terminal prompts you to select the path to your workspace.

1. Select the organization that your project was created under.

1. Select the project that you created.

1. Select the extension point to implement. The DX Experience Cloud SPA option initializes a project with a default UI and creates a default Adobe I/O Runtime Action named "generic".

   To run the project locally, run:

   ```bash
   aio app run
   ```
   
   To deploy the project, run:
   
   ```bash
   aio app deploy
   ```

## Begin configuring events on Commerce

You must configure Commerce to communicate with your project. You will need the files that you downloaded from the the Adobe Developer Console.

1. In the Commerce Admin, navigate to **Stores** > Settings > **Configuration** > **Adobe Services** > **Adobe I/O Events** > **General configuration**.

1. Copy and paste the contents of the `private.key` file into the **Service Account Private Key** field. Use the following command to copy the contents.

   ```bash
   cat config/private.key | pbcopy
   ```

1. Copy the contents of the `<workspace-name>.json` file into the **Adobe I/O Workspace Configuration** field.

1. Enter a unique identifier in the **Adobe Commerce Instance ID** field. This value can be any string, but it must be unique.

1. Click **Save Config** but do not leave the page.

## Create an event provider and finish Commerce configuration

You cannot create an event provider until after you have configured and saved the private key, workspace file, and instance ID values.


1. Create a `<Commerce-root-directory>/app/etc/event-types.json` file and add the following contents:

   ```json
   {
    "provider": {
        "label": "Adobe Commerce Events",
        "description": "Provides out-of-process extensibility for Adobe Commerce"
        }
    }

1. Run the following command to create an event provider:

   ```bash
   bin/magento events:create-event-provider
   ```

   The commane displays a message similar to the the following:

   ```terminal
   No event provider found, a new event provider will be created
   A new event provider has been created with ID 63a1f8fe-e911-45a4-9d3f
   ```

1. Copy the ID returned in the command output into the **Adobe I/O Event Provider ID** field in the Admin. 

1. Click **Save Config**.

## Test
If you already have an event provider, add the ID to the "Adobe I/O Event Provider ID" field

<!-- Link Definitions -->
[Adobe Developer Console]: 
[create a new project]: https://developer.adobe.com/developer-console/docs/guides/projects/#create-a-new-project
[create a public key certificate]: https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/JWTCertificate/
