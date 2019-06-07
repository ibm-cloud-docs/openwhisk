---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-07"

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

Use APIs to directly manage {{site.data.keyword.openwhisk}} actions. The API Gateway acts as a proxy to [web actions](/docs/openwhisk?topic=cloud-functions-actions_web) and provides HTTP method routing, client ID and secrets, rate limits, CORS, viewing API usage, viewing response logs, and API sharing policies.
{: shortdesc}

For more information about API management, you can read the [API management documentation](/docs/api-management?topic=api-management-manage_openwhisk_apis).

Creating APIs with API Gateway is not supported for IAM-based namespaces. Use a Cloud Foundry-based namespace instead.
{: tip}

## Creating your first API
{: #api_create}

Before you begin, install the [{{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Save the following code into a JavaScript file named `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Create a web action that is named `hello` using the file that you created. Be sure to add the flag `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
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
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  A new URL is generated exposing the `hello` action by using a GET HTTP method.

4. Send a test HTTP request to the URL by using the following cURL command.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  **Example output**
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

The web action `hello` is invoked, which returns a JSON object that includes the parameter `name` in the query parameter. You can pass parameters to the action with simple query parameters or by using the request body. Web actions can publicly invoke an action without using authentication.

## Using full control over the HTTP response
{: #api_control}

The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. For example, when you use the `--response-type json` flag, the full result of the action is returned in JSON format and the `Content-Type` header is automatically set to `application/json`.

To return different content types in the body, use full control over the HTTP response properties such as `statusCode` and `headers`. You can use the `--response-type http` flag to configure the target URL of the web action with the `http` extension. You can change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [web actions](/docs/openwhisk?topic=cloud-functions-actions_web) documentation.

1. Save the code for the `hello` action that is returning the JSON properties `body`, `statusCode`, and `headers`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Update the action with the modified result.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Update the API response type by using the `--response-type http` flag.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Call the updated API by using the following cURL command.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Example output**
  ```
  {
  "payload": "Hello world Serverless API"
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

