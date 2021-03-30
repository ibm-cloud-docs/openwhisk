---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-30"

keywords: services, serverless, functions

subcollection: openwhisk

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Binding {{site.data.keyword.cloud_notm}} services to {{site.data.keyword.openwhisk_short}} entities
{: #services}

You can incorporate functionality from {{site.data.keyword.cloud_notm}} services in your {{site.data.keyword.openwhisk_short}} app.
{: shortdesc}

**How do I add {{site.data.keyword.cloud_notm}} services to my app?**

1. You can hardcode REST API calls into your app. This option might be the quickest way to communicate with an {{site.data.keyword.cloud_notm}} service.
2. You can use a preinstalled or installable package to incorporate functionality. You can use the actions and feeds that are stored in the packages within your app code. This option might slim down your code a bit, which might be useful if your app is close to the system limits.

**How do I set up parameters that must be accessed by my app?**

These parameters might include values that make your app reusable with different data or they might include values that are required by the service, such as credentials.

1. You can hardcode parameters into your app. This option might not be the most secure way of storing confidential information such as credentials.
2. You can bind the parameters to your app by binding them to an action or package.

Parameters for each package are available in the package descriptions, which you can find in the Integrating serverless apps section of the documentation.
{: tip}

## Binding a service to an action or package
{: #services_bind}

Bind any {{site.data.keyword.cloud_notm}} service to any action. When a service is bound, a new parameter is created on your existing action that contains the service instance credentials.
{: shortdesc}

You can bind only one instance of a service at a time and you cannot bind multiple instances of the same service to an action or package. You can bind a service to an individual action or you can bind a service to a package and then all actions that are contained in that package are bound to that service.
{: note}

Before you begin, [create an action](/docs/openwhisk?topic=openwhisk-actions) and [define credentials](/docs/account?topic=account-externalapp#externalapp) for the service that you want to bind to the action.

1. Get the list of available service keys of services that you want to bind to an action or package.

   ```
   ibmcloud resource service-keys
   ```
   {: pre}

   **Example output**

   ```
   Name                             State    Created At
   Service credentials-1            active   Tue Sep 17 15:50:43 UTC 2019
   cloudant-my1-creds               active   Tue Sep 17 14:30:08 UTC 2019
   ```
   {: screen}

   To find more details, use `--output json`.

   If you know the service name or ID, you can use the `--instance-name NAME` option to get only the subset of keys for a specific service instance. For example:

   ```
   ibmcloud resource service-keys --instance-name My-Cloudant
   ```
   {: pre}

2. Get details of a selected service key.

   ```
   ibmcloud resource service-key <service_key_name>
   ```
   {: pre}

   **Example output**

   ```
   Name:          cloudant-my1-creds
   ID:            crn:v1:bluemix:public:cloudantnosqldb:eu-de:a/123456789012345678901234567890ab:f6f38f72-95af-4699-842c-4cac2a1ea19e:resource-key:5dab35d4-fe20-433e-9cb9-7ae4d93887d5
   Created At:    Wed Nov 28 16:30:08 UTC 2018
   State:         active
   Credentials:
                 apikey:  ABC123ABC123abc123abc123ABC123ABC123abc123
                 ...
   ```
   {: screen}

3. Bind the service to an action or package. The [`ibmcloud fn service bind`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_service_bind) command makes your {{site.data.keyword.cloud_notm}} service credentials available to your {{site.data.keyword.openwhisk_short}} code at run time.

    <table>
    <thead>
        <tr>
        <th>Parameter</th>
        <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>The service name that you're binding.</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>The name of the action or package that you want to bind the service to.</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>(Optional) Specify a service instance name. If you don't specify a service instance name, the first instance for the service is selected.</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>(Optional) Specify the credentials name. If you don't specify the credentials name, the first credentials for the service instance are selected.</td>
        </tr>
    </tbody>
    </table>

   **Example syntax**

   ```
   ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
   ```
   {: pre}

   For example, to bind an {{site.data.keyword.cloudant}} service instance to an action called `hello`, run the following command.

   ```
   ibmcloud fn service bind cloudantnosqldb hello --instance My-Cloudant --keyname cloudant-my1-creds
   ```
   {: pre}

   If your action is not found, try adding the package name to the command: `demo\hello`. Note that if there's only one instance of service type `cloudantnosqldb` and one service key, the `--instance` and `--keyname` options can be omitted.

   If you receive the message `Unable to refresh user access token: CloudFoundry API endpoint is not set`, try running `ibmcloud target --cf` with no arguments and then running the command again.
   {: tip}

   **Example output**

   ```
   Credentials 'cloudant-my1-creds' from 'cloudantnosqldb' service instance 'My-Cloudant' bound to 'hello'.
   ```
   {: screen}

4. Verify that the credentials are successfully bound. The action that the service is bound to doesn't support any custom flags, but does support the `debug` and `verbose` flags.

   ```
   ibmcloud fn action get hello parameters
   ```
   {: pre}

   **Example output**

   ```
   "key": "__bx_creds",
       "value": {
           "cloudantnosqldb": {
               "apikey": "ABC123ABC123abc123abc123ABC123ABC123abc123",
               "credentials": "cloudant-my1-creds",
               ...
           }
       }
   ```
   {: screen}

   In this example, the credentials for the Cloudant service, along with any other credentials for other service types belong to a parameter named `__bx_creds`. The action code can access the necessary information by getting the `__bx_creds` parameter.

   For example, in `nodejs`, you can use the following code to get the apikey for the Cloudant service in the previous sample.

   ```
   function main(params) {
       const apikey = params.__bx_creds.cloudantnosqldb.apikey; 
       // ...
       return { message: 'Hello done.' };
   }
   ```
   {: screen}

For more information about passing parameters to an action or package, see [Binding parameters to actions](/docs/openwhisk?topic=openwhisk-actions#actions_params).

## Binding Cloud Foundry based services
{: #services_bind_cf}

Cloud Foundry based services do not expose credentials through a service key resource, but instead use Cloud Foundry based service keys.
{: shortdesc}

1. Find the name of the service and the service instance that you want to bind to an action or package. In the example output, `composer` is the service and `Composer-qp` is the service instance name.

   ```
   ibmcloud service list
   ```
   {: pre}

   **Example output**

   ```
   name              service        plan   bound apps   last operation
   Composer-qp       composer       free                create succeeded
   Composer-uc       composer       free                create succeeded
   Discovery-37      discovery      lite                create succeeded
   ```
   {: screen}

2. Get the name of the credentials that are defined for a service instance.

   ```
   ibmcloud service keys SERVICE_NAME
   ```
   {: pre}

3. Bind the service to an action. The [`ibmcloud fn service bind`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_service_bind) command makes your {{site.data.keyword.cloud_notm}} service credentials available to your {{site.data.keyword.openwhisk_short}} code at run time.

   **Example syntax**

   ```
   ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
   ```
   {: pre}

   For example, to bind an IBM Watson Composer service to an action named `hello`, run the following command.

   ```
   ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
   ```
   {: pre}

   **Example output**

   ```
   Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello`.
   ```
   {: screen}

4. Verify that the credentials are successfully bound. The action that the service is bound to doesn't support any custom flags, but does support the `debug` and `verbose` flags.

   ```
   ibmcloud fn action get hello parameters
   ```
   {: pre}

   **Example output**

   ```
   ok: got action Hello World
   {
     "parameters": [
         {
             "key": "var1",
             "value": "val1"
         },
         {
             "key": "dog",
             "value": "cat"
         },
         {
             "key": "__bx_creds",
             "value": {
                 "composer": {
                     "password": "[Service password]",
                     "url": "[Service url]",
                     "username": "[Service username]",
                     "instance": "Composer-qp",
                     "credentials": "Credentials-1"
                 },
             }
         }
       ],
   }
   ```
   {: screen}

   In this example, the credentials for the `composer` service, along with any other credentials for other service types belong to a parameter called `__bx_creds`. The action code can access the necessary information by getting the `__bx_creds` parameter.

For more information about passing parameters to an action or package, see [Binding parameters to actions](/docs/openwhisk?topic=openwhisk-actions#actions_params).

## Unbinding services from actions
{: #services_unbind}

Unbinding a service from an action or package removes existing service bindings.
{: shortdesc}

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
