---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-06"

keywords: serverless, rest api, gateway, web actions, functions, API, HTTP

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

# Creating serverless REST APIs
{: #apigateway}

You can use APIs to directly manage {{site.data.keyword.openwhisk}} [web actions](/docs/openwhisk?topic=openwhisk-actions_web). 
{: shortdesc}



**Why use REST APIs with {{site.data.keyword.openwhisk_short}}?**

You can use the API Gateway as a proxy to your web actions. API Gateway provides HTTP method routing, client ID and secrets, rate limits, CORS, viewing API usage, viewing response logs, and API sharing policies.

For more information about API Management, you can read the [API Management documentation](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Creating your first API
{: #api_create}

You can create APIs by using the CLI or from the console.
{: shortdesc}

**Before you begin**

You must have `SpaceDeveloper` permissions in your Cloud Foundry space to create REST APIs. Space permissions can be seen by running `ibmcloud account space-roles <org>`.

By default, anyone can invoke a web action by using the invocation URL, even if you secure the calling API. You can secure your web action by using the `require-whisk-auth web action` annotation. For more information, see [Securing your API web action](#api_secure).
{: note}

### Creating your first API by using the CLI
{: #api_create_cli}

Before you begin, install the [{{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=openwhisk-cli_install).

1. Save the following code as a JavaScript file named `hello.js`.

    ```javascript
    function main({name:name='Serverless API'}) {
        return {payload: `Hello, ${name}!`};
    }
    ```
    {: codeblock}

2. Create a web action that is named `hello` by using the file that you created. Be sure to add the flag `--web true` and the `--web-secure <secret>`. Replace `<filepath>` with the file path of your `hello.js` file and `<secret>` with a secret value of your own choosing for your action. For more information about using the `--web-secure` flag, see [Securing your API web action](#api_secure).

    ```bash
    ibmcloud fn action create hello <filepath>/hello.js --web true --web-secure <secret>
    ```
    {: pre}

    **Example output**

    ```
    ok: created action hello
    ```
    {: screen}

3. Create an API with base path `/hello`, path `/world`, method `get`, and response type `json`.

    ```bash
    ibmcloud fn api create /hello /world get hello --response-type json
    ```
    {: pre}

    **Example output**

    A new URL is generated exposing the `hello` action by using a `GET` HTTP method.

    ```
    ok: created API /hello/world GET for action /_/hello
    https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
    ```
    {: screen}


4. Send a test HTTP request to the URL by using the following cURL command.

    ```bash
    curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
    ```
    {: pre}

    **Example output**

    The web action `hello` is invoked and returns a JSON object that includes the parameter `name` in the query parameter. You can pass parameters to the action with simple query parameters or by using the request body. Web actions can publicly invoke an action without using authentication.

    ```
    {
    "payload": "Hello, Jane!"
    }
    ```
    {: screen}

### Creating an API from the console
{: #api_create_ui}

1. Go to the [API](https://cloud.ibm.com/functions/apimanagement){: external}  page in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Create API**.

3. Complete the fields in the **API Information** section.

4. Click **Create operation**. Use the Create operations section to define parameters for your API.  To create an API that is similar to the API in the CLI example, use base path `/hello`, path `/world`, method `get`, and response type `json`.

5. Click **Create operation**.

6. Complete any remaining information for your API. You can also edit this information after your API is created.

7. Click **Create**.

You can find details about your API by selecting it from the {{site.data.keyword.openwhisk_short}} APIs page.

## Using full control over the HTTP response
{: #api_control}

The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. For example, when you use the `--response-type json` flag, the full result of the action is returned in JSON format and the `Content-Type` header is automatically set to `application/json`.

To return different content types in the body, use full control over the HTTP response properties such as `statusCode` and `headers`. You can use the `--response-type http` flag to configure the target URL of the web action with the `http` extension. You can change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [web actions](/docs/openwhisk?topic=openwhisk-actions_web) documentation.

1. Save the following code as `hello.js`.

    ```javascript
    function main({name:name='Serverless API'}) {
        return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
        };
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version of your `hello.js` code.

    ```bash
    ibmcloud fn action update hello <filepath>/hello.js --web true --web-secure <secret>
    ```
    {: pre}

    **Example output**

    ```
    ok: updated action hello
    ```
    {: screen}

3. Update the API response type by using the `--response-type http` flag.

    ```bash
    ibmcloud fn api create /hello /world get hello --response-type http
    ```
    {: pre}

    **Example output**

    ```
    ok: created API /hello/world GET for action /_/hello https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
    ```
    {: screen}

4. Call the updated API by using the following cURL command.

    ```bash
    curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
    ```
    {: pre}

    **Example output**

    ```
    {
    "payload": "Hello, Serverless API!"
    }
    ```
    {: screen}

## Securing your API web action
{: #api_secure}

By default, anyone can invoke a web action by using the invocation URL, even if you secure the calling API. You can secure your web action by using the `require-whisk-auth web action` annotation.
{: shortdesc}

This example uses the same names and code as the example in [Creating your first API by using the CLI](#api_create_cli). If you created those entities, delete the action and the API by running `ibmcloud fn action delete ACTION_NAME` and `ibmcloud fn api delete API_NAME`. You must create the web secure action before you can create the API. If you want to update an existing action that is called by an API, you must delete the API, update the action, and then re-create the API. 

1. Save the following code into a JavaScript file named `hello.js`.

    ```javascript
    function main({name:name='Serverless API'}) {
        return {payload: `Hello, ${name}!`};
    }
    ```
    {: codeblock}

2. Create a web action that is named `hello` by using the file that you created. Add the `--web true` and `--web-secure <secret>` flags. Replace `<filepath>` with the file path of your `hello.js` file and `<secret>` with a secret value of your own choosing for your action.

    ```bash
    ibmcloud fn action create hello <filepath>/hello.js --web true --web-secure <secret>
    ```
    {: pre}

    For example, `ibmcloud fn action create hello myjavascriptfiles/hello.js --web true --web-secure ajcqdres`.

    **Example output**

    ```
    ok: created action hello
    ```
    {: screen}

    If you want to verify that your action is secure, follow the steps in [Securing web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure).

3. Create an API with base path `/hello`, path `/world`, method `get`, and response type `json`.

    ```bash
    ibmcloud fn api create /hello /world get hello --response-type json
    ```
    {: pre}

    **Example output**

    A new URL is generated, exposing the `hello` action by using a `GET` HTTP method.

    ```
    ok: created API /hello/world GET for action /_/hello
    https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
    ```
    {: screen}


4. Send a test HTTP request to the URL by using the following cURL command.

    ```bash
    curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
    ```
    {: pre}

    **Example output**

    The web action `hello` is invoked and returns a JSON object that includes the parameter `name` in the query parameter.

    ```
    {
    "payload": "Hello, Jane!"
    }
    ```
    {: screen}

## Modifying the API configuration
{: #api_modify_config}

After you create your configuration, you can use the [APIs tab](https://cloud.ibm.com/functions/apimanagement){: external} in the {{site.data.keyword.openwhisk_short}} dashboard to modify the configuration in the following ways.

* [Create a {{site.data.keyword.openwhisk_short}} API](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) that wraps a set of {{site.data.keyword.openwhisk_short}} actions.
* [Secure your API](/docs/api-management?topic=api-management-manage_apis#settings_api_manage_apis) by applying API security and rate-limiting policies.
* [Manage traffic](/docs/api-management?topic=api-management-manage_apis#settings_api_manage_apis) by viewing API usage statistics and checking out response logs.
* [Socialize and share](/docs/api-management?topic=api-management-manage_apis#share_api_manage_apis) your API with developers both within and outside {{site.data.keyword.cloud_notm}}.

After you are finished updating the configuration, you can download the definition file in JSON format, and then import it again by using the CLI. Downloading and importing the configuration is useful, for example, for an unattended deployment in a continuous integration and deployment (CICD) pipeline. You can also upload and import the API definition file by using the console.


