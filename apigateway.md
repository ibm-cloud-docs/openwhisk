---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-12"

keywords: serverless, rest api, gateway, web actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Creating serverless REST APIs
{: #apigateway}

You can use APIs to directly manage {{site.data.keyword.openwhisk}} [web actions](/docs/openwhisk?topic=cloud-functions-actions_web). 
{: shortdesc}

Creating APIs with API Gateway is not supported for IAM-based namespaces. Use a Cloud Foundry-based namespace instead.
{: important}

## Why use REST APIs with {{site.data.keyword.openwhisk_short}}?

You can use the API Gateway as a proxy to your web actions. API Gateway provides HTTP method routing, client ID and secrets, rate limits, CORS, viewing API usage, viewing response logs, and API sharing policies.

For more information about API management, you can read the [API management documentation](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Creating your first API
{: #api_create}

Before you begin, install the [{{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Save the following code into a JavaScript file named `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Create a web action that is named `hello` by using the file that you created. Be sure to add the flag `--web true`. Replace `<filepath>` with the file path of your `hello.js` file.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Example output**
  ```
  ok: created action hello
  ```
  {: screen}

3. Create an API with base path `/hello`, path `/world`, method `get`, and response type `json`.
  ```
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
  ```
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



## Using full control over the HTTP response
{: #api_control}

The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. For example, when you use the `--response-type json` flag, the full result of the action is returned in JSON format and the `Content-Type` header is automatically set to `application/json`.

To return different content types in the body, use full control over the HTTP response properties such as `statusCode` and `headers`. You can use the `--response-type http` flag to configure the target URL of the web action with the `http` extension. You can change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [web actions](/docs/openwhisk?topic=cloud-functions-actions_web) documentation.

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
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Output**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Update the API response type by using the `--response-type http` flag.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Output**
  ```
  ok: created API /hello/world GET for action /_/hello https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Call the updated API by using the following cURL command.
  ```
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

## Modifying the configuration
{: #api_modify_config}

After you create your configuration, you can use the [APIs tab](https://cloud.ibm.com/openwhisk/apimanagement) in the {{site.data.keyword.openwhisk_short}} dashboard to modify the configuration in the following ways.

* [Create a {{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) that wraps a set of {{site.data.keyword.openwhisk_short}} actions.
* [Secure your API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) by applying API security and rate-limiting policies.
* [Manage traffic](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) by viewing API usage statistics and checking out response logs.
* [Socialize and share](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) your API with developers both within and outside {{site.data.keyword.cloud_notm}}.

Once you are done updating the configuration, you can download the definition file in JSON format, and then reimport it by using the CLI. Downloading and importing the configuration is useful, for example, for an unattended deployment in a continuous integration and deployment (CICD) pipeline. You can also upload and reimport the API definition file by using the UI.

