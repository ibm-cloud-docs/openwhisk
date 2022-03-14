---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-14"

keywords: web actions, serverless, functions, actions, requests, HTTP, error

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Creating web actions
{: #actions_web}

When you create an {{site.data.keyword.openwhisk}} web action, the result is a URL that can be used to trigger the action from any web app.
{: shortdesc}

The {{site.data.keyword.openwhisk}} web actions API endpoint changed. To align with other customer services, a new `functions.appdomain.cloud` API endpoint is available for web actions. The API endpoint `functions.cloud.ibm.com` is still active, but now returns response data as content type `text/plain` instead of `text/html`. Other content types are not changing. Migrate your web actions to use the new API endpoint. The previous endpoints are deprecated and will be deactivated at some point.
{: important}

## Why use web actions instead of standard actions?
{: #why_actions_web}

Run web actions anonymously
:    Web action activations are associated with the user that created the action, rather than the caller of the action. Usually, for API calls to apps such as GitHub, you would include a username and token with the API call for either a specific user or a functional ID. When you use a web action, those kinds of credentials are not required. A web action is accessible through a REST interface without the need for credentials.

:    Though you are not required to use credentials with web actions, you can implement your own authentication and authorization, or OAuth flow. To configure a web action with credentials, see [Securing web actions](#actions_web_secure).

Use any type of HTTP request
:    By default, actions accept only `POST` requests, but web actions can be invoked through any of these HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`, as well as `HEAD` and `OPTIONS`.

Trigger a web action from anywhere
:    When you create an {{site.data.keyword.openwhisk}} web action, you generate a URL to invoke that action from any web-based app. Actions that are not web actions require authentication and must respond with a JSON object. To get the URL of a web action, you can run the [**`action get`**](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_get) command and include the `--url` flag. For example, `ibmcloud fn action get <action_name> --url`.

:    A web action API path can be used with cURL, `wget`, or even be entered directly in your browser. A web action can be invoked by using a URL that is structured as follows, `https://<apihost>/api/v1/web/<namespace_ID>/<packageName>/<actionName>.<ext>`.

Create fewer {{site.data.keyword.openwhisk_short}} entities
:    Because you can invoke a web action from anywhere, you are not required to create other {{site.data.keyword.openwhisk_short}} entities like triggers or rules.

Allows non-2xx return codes
:    {{site.data.keyword.openwhisk_short}} web actions can return non-2xx codes, allowing your code to be more flexible and allow for retries.

## How do web actions work?
{: #how_actions_web_work}

Web actions can be invoked without authentication and can be used to implement HTTP handlers that respond with `headers`, `statusCode`, and `body` content of different types.

Web actions must return a JSON object. However, the controller treats a web action differently if its result includes one or more of the properties as top-level [JSON properties](#web_action_properties).
{: shortdesc}

## Packaging code for web actions
{: #packaging_actions_web}

{{site.data.keyword.openwhisk_short}} actions can be web-enabled by including the `--web true` flag in the [**`action create`** command](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create).
{: shortdesc}

You can create a web action from multiple app files by packaging them as a .zip archive. You can also create a web action by using Docker images.

* To learn how to package your code for actions, see [Preparing apps for actions](/docs/openwhisk?topic=openwhisk-prep).
* To see more examples of how you can create actions, see [Creating actions](/docs/openwhisk?topic=openwhisk-actions).

## Available features of web actions
{: #actions_web_extra}

Web actions support the following features:
{: shortdesc}


### Content extensions
{: #extra_features}

You can specify a content type for your web action response by adding an extension to the action name in the URI. The following content types are supported: `.json`, `.http`, `.html`, `.svg`, or `.text`. For example, an action `<namespace_ID>/demo/hello` is referenced as `<namespace_ID>/demo/hello.json` to receive a `.json` response.  \n For content type `.json`, the full return object is returned, similar to `{resOne: "some text", resTwo: 42}`.  \n For `.html`, `.svg`, or `.text`, the content of the property that is called `body` is returned, similar to `.http`. If you want to use one response object for multiple content types, you can add the corresponding properties into the return object and remove the `body` property. The return object is then similar to `{html: "<p>The html response</p>", text: "text response"}`.  \n If no content type is specified, the `.http` extension is assumed, and the content of the property that is called `body` is returned, for example, `{body: "the return message"}`.

### Query and body parameters as input
{: #query_test}

The action receives query parameters as well as parameters in the request body. The precedence order for merging parameters is: package parameters, action parameters, query parameter, and body parameters. Each of these parameters can override any previous values if overlap occurs. As an example, `/demo/hello.http?name=Jane` can pass the argument `{name: "Jane"}` to the action.

### Form data
{: #form_data}

In addition to the standard `application/json`, web actions can receive URL encoded form data `application/x-www-form-urlencoded data` as input.

### Activations that use multiple HTTP verbs
{: #actions_web_options}

A web action can be invoked through any of these HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`, as well as `HEAD` and `OPTIONS`.

### Non-JSON body and raw HTTP entity handling
{: #actions_web_raw_enable}

A web action can accept an HTTP request body other than a JSON object, and can elect to always receive such values as opaque values (plain text when not binary file, or base64 encoded string otherwise).


## Creating a web action
{: #actions_web_example}

To create a web action:
{: shortdesc}

1. Save the following JavaScript code as `hello.js`.

    ```javascript
    function main({name}) {
    var msg = 'You did not tell me who you are.';
    if (name) {
        msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
    }
    ```
    {: codeblock}

2. Create the `demo` package. The package name is `default` unless explicitly specified.

    ```bash
    ibmcloud fn package create demo
    ```
    {: pre}

3. Create the `hello` action. In this example, the `packageName/actionName` are `demo/hello`. Replace the `<filepath>` variable with the file path of your `hello.js` file and set the `--web` flag to `true`. 

    ```bash
    ibmcloud fn action create demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

4. Invoke or test the `hello` web action without any parameters. Replace the `<apihost>` and `<namespace_ID>` variables. Example `<apihost>`: `https://us-south.functions.appdomain.cloud`.

    For IAM-enabled namespaces, replace the `<namespace_ID>` variable with the namespace ID. To get the ID, run `ibmcloud fn namespace get <namespace_name> --properties`. The namespace name is not valid.
    {: note}

    a. You can test the web action by either: 

    * Opening a URL by using the following structure `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello` in your browser.
    
    * Testing the action by using a cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    * Testing the action by using a `wget` command.  

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    b. The action code returns the following dictionary.

    ```bash
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ```
    {: screen}

    You can also test the action by returning just the `body` property by using the following command:
    {: #projecting_fields}

    ```bash
    curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.html/body
    ```
    {: pre}

    Example output

    Since the `<name>` parameter was not specified, the following message is returned.

    ```html
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. Now try defining the `<name>` parameter. Test the action with a `<name>` parameter by either:

    * Opening `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello?name=Jane` in your browser.

    * Testing the action by using a cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello?name=Jane
        ```
        {: pre}

    * Testing the action by using a `wget` command.

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello?name=Jane
        ```
        {: pre}

    Example output

    ```html
    <html><body><h3>Hello, Jane!</h3></body></html>
    ```
    {: screen}

Next steps, try adding the URL for your `hello` web action to your web app and test it there.

### Web action JSON properties
{: #web_action_properties}

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. If your `Content-Type` is not `application/json`, you must specify a `Content-Type` in the `headers` of your action code.
{: shortdesc}

If the [result size limit](/docs/openwhisk?topic=openwhisk-limits) for actions is reached, the response fails. If you know that your action result is larger than 5 MB, then set up an [object store](/docs/openwhisk?topic=openwhisk-pkg_obstorage).

| JSON property | Description |
| --- | --- |
| `headers`| A JSON object in which the keys are header names and the values are string, number, or boolean values. To send multiple values for a single header, the header's value is a JSON array of the multiple values. No headers are set by default. |
| `statusCode` | A valid HTTP status code. If body content is present, the default is `200 OK`. If no body content is present, the default is `204 No Content`. |
| `body` | A string that is either plain text, a JSON object or array, or a base64 encoded string for binary data. The body is considered empty if it is `null`, the empty string `""`, or undefined. The default is an empty body. |
{: caption="JSON web action properties." caption-side="top"}

The [controller](/docs/openwhisk?topic=openwhisk-about#about_controller) passes any action-specified headers, status code, or body to the HTTP client that terminates the request or response. If the `Content-Type` header is not declared in the action result's `headers`, the body is interpreted as `application/json` for non-string values and `text/html` otherwise. If the `Content-Type` header is defined, the controller determines whether the response is binary data or plain text and decodes the string by using a base64 decoder as needed. If the body isn't decoded correctly, an error is returned to the client.

The owner of the web action owns all the activations records, and incurs the cost of running the action in the system regardless of how the action was invoked.
{: note}

#### Protected parameters
{: #actions_web_protect_parameters}

Action parameters are protected and can be changed only by updating your action. Parameters are automatically finalized to enable web actions.
{: shortdesc}

```bash
ibmcloud fn action create /<namespace_ID>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

The result of these changes is that the `name` is bound to `Jane` and cannot be overridden by query or body parameters because of the final annotation. This design secures the action against query or body parameters that try to change this value whether by accident or intentionally.

### Performing an HTTP redirect by using a web action
{: #http_redirect}

You might use this feature in a web application to redirect a user to the new version of your site.
{: shortdesc}

Before you begin 

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

This example web action redirects your browser to the [{{site.data.keyword.openwhisk_short}} dashboard](https://cloud.ibm.com/functions/){: external}. To create a web action that performs an HTTP redirect:

1. Save the code as `hello.js`.

    ```javascript
    function main() {
    return {
        headers: { location: 'https://cloud.ibm.com/openwhisk/' },
        statusCode: 302
    }
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version for your `hello.js` code. Replace `<filepath>` with the file path of your `hello.js` file.

    ```bash
    ibmcloud fn action update demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

3. Test the `hello` web action. Replace the `<apihost>` and `<namespace_ID>` variables. You can test the web action by using one of the following methods.

    * Opening the URL `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello` in your browser.

    * Running the following cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    * Running the following `wget` command,

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

### Setting cookies by using a web action
{: #multiple_cookie}

You might use this feature in a web application to store a JSON Web Token as a session cookie after a successful login.
{: shortdesc}

Before you begin

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

1. Save the code as `hello.js`.

    ```javascript
    function main() {
    return {
        headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
          'SessionID=asdfgh123456; Path = /'
        ],
        'Content-Type': 'text/html'
        },
        statusCode: 200,
        body: '<html><body><h3>hello</h3></body></html>' }
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version for your `hello.js` code. Replace `<filepath>` with the file path of your `hello.js` file.

    ```bash
    ibmcloud fn action update demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

3. Clear your browser's cookies before you test the action.

4. Test the `hello` web action by opening the URL in your browser. Replace the `<apihost>` and `<namespace_ID>` variables and open `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello`. Example `<apihost>`: `https://us-south.functions.appdomain.cloud`.

The cookies `UserID=Jane` and `SessionID=asdfgh123456` are set in your browser's developer tools.

### Returning an image by using a web action
{: #return_image}

Images that are returned through web actions are required to be base64 encoded strings. You might use this feature in a web application to return the image of a country flag based on user locale.
{: shortdesc}

Before you begin

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

To create a web action that returns an `image/png`:

1. Save the code as `hello.js`. Replace `<base64_encoded_string>` with a base64 encoded string from an image file.

    ```javascript
    function main() {
        let png = '<base64_encoded_string>';
        return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version for your `hello.js` code. Replace `<filepath>` with the file path of your `hello.js` file.

    ```bash
    ibmcloud fn action update demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

3. Test the action in your browser or by using a cURL command. Replace the `<apihost>` and `<namespace_ID>` variables. You can test the web action by either:

    * Opening the URL `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello` in your browser. 
    * Running the following cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    * Running the following `wget` command.

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

### Returning JSON by using a web action
{: #return_json}

You might use this feature in a web application to return a JSON object of user IP information.
{: shortdesc}

Before you begin

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

To create a web action that returns `application/json`:

1. Save the code as `hello.js`.

    ```javascript
    function main(params) {
        return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
        };
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version for your `hello.js` code. Replace `<filepath>` with the file path of your `hello.js` file.

    ```bash
    ibmcloud fn action update demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

3. Test the action in your browser or by using a cURL command. Replace the `<apihost>` and `<namespace_ID>` variables. You can test the web action by using one of the following methods.

    * Opening the URL `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello` in your browser. 
    * Running the following cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    * Running the following `wget` command.

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello
        ```
        {: pre}

    Example output

    ```bash
    {
        "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
        },
        "__ow_method": "get",
        "__ow_path": ""
    }
    ```
    {: screen}

### HTTP context
{: #actions_web_context}

All web actions, when invoked, receive HTTP request details as input parameters to the action argument.
{: shortdesc}

| HTTP parameter | Type | Description |
| --- | --- | --- |
| `__ow_method` | String | The HTTP method of the request. |
| `__ow_headers` | Map string to string | The request headers. |
| `__ow_path` | String | The unmatched path of the request (matching stops once the action extension is consumed). |
| `__ow_user` | String | The namespace_ID that identifies the {{site.data.keyword.openwhisk_short}}-authenticated subject. |
| `__ow_body` | String | The request body entity, as a base64 encoded string when content is a binary file, or plain string otherwise. |
| `__ow_query` | String | The query parameters from the request as an unparsed string. |
{: caption="HTTP parameter descriptions." caption-side="top"}

A request cannot override any of the named `__ow_` parameters. Doing so, results in a failed request with status equal to 400 Bad Request.

The `__ow_user` is only present when the web action is [annotated to require authentication](/docs/openwhisk?topic=openwhisk-annotations#annotations-specific-to-web-actions) and allows a web action to implement its own authorization policy. The `__ow_query` is available only when a web action elects to handle the ["raw" HTTP request](#actions_web_raw_enable). The `__ow_query` is a string that contains the query parameters that are parsed from the URI (separated by `&`). The `__ow_body` property is present in either raw HTTP requests, or when the HTTP request entity is not a JSON object or form data. Otherwise, web actions receive query and body parameters as first class properties in the action argument. Body parameters take precedence over query parameters, which in turn take precedence over action and package parameters.

### HTTPS Endpoint support
{: #actions_web_endpoint}

Supported SSL protocols: TLS 1.2, TLS 1.3 ([draft version 18](https://datatracker.ietf.org/doc/html/draft-ietf-tls-tls13-18){: external})

### Altering the response content of web action
{: #extra_features}

You can alter the response content of a web action to return different content types by using [Content extensions](#actions_web_extra). 
{: shortdesc}

Before you begin

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

To alter the response of a web action:

1. Save the following code as `hello.js`.

    ```javascript
    function main(params) {
        return { response: params };
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version of your `hello.js` code. Replace `<filepath>` with the file path of your `hello.js` file.

    ```bash
    ibmcloud fn action update demo/hello <filepath>/hello.js --web true
    ```
    {: pre}

3. Test the action in your browser or by using a cURL command. Replace the `<apihost>` and `<namespace_ID>` variables.

    a. Return JSON by either:

        * Opening `https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json` in your web browser.
        
        * Running the following cURL command.

            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json
            ```
            {: pre}
            
        * Running the following `wget` command.

            ```bash
            wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json
            ```
            {: pre}

    Example output

    ```bash
    {
    "response": {
        "__ow_method": "get",
        "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
        },
        "__ow_path": ""
    }
    }
    ```
    {: screen}

    b. Test the action by using a query parameter. You can test the action by either:{: #query_test}

        * Running the following cURL command.
        
            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane
            ```
            {: pre}

        * Running the following `wget` command.

            ```bash
            wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane
            ```
            {: pre}

    Example output

    ```bash
    {
    "response": {
        "name": "Jane",
        "__ow_method": "get",
        "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
        },
        "__ow_path": ""
    }
    }
    ```
    {: screen}

    c. You can also test the web action by using form data. You can test the web action by using one of the following methods.{: #form_data}
        * Running the following cURL command.

            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json -d "name":"Jane"
            ```
            {: pre}
    
        * Running the following `wget` command.

            ```bash
            wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json -d "name":"Jane"
            ```
            {: pre}
   
    Example output

    ```bash
    {
    "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "content-length": "10",
        "content-type": "application/x-www-form-urlencoded",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
    }
    }
    ```
    {: screen}

    d. You can specify a JSON object by running the following command. You can test the web action by using one of the following methods.
    
        * Running the following cURL command.

            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
            ```
            {: pre}

        * Running the following `wget` command.

            ```bash
            wget https://<apihost>/api/v1/web/{namespace_ID/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
            ```
            {: pre}

    Example output

    ```bash
    {
    "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "content-length": "15",
        "content-type": "application/json",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
    }
    }
    ```
    {: screen}

    e. You can also return the `name` value as text by either:
      
        * Running the following cURL command.

            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.text/response/name?name=Jane
            ```
            {: pre}

        * Running the following `wget` command.

            ```bash
            wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.text/response/name?name=Jane
            ```
            {: pre}

    Example output

    ```bash
    Jane
    ```
    {: screen}

    In standard actions, query parameters, form data, and JSON object body entities are all treated as dictionaries, and their values are directly accessible as action input properties. This behavior is not the case for web actions, which handle HTTP request entities, or when the web action receives an entity that is not a JSON object.
    {: note}

    f. You can set the `Content-Type` by using one of the following methods.

        * Running the following cURL command.  

            ```bash
            curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
            ```
            {: pre}

        * Running the following `wget` command.

            ```sh
            wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
            ```
            {: pre}
  
    Example output

    ```bash
    {
        "response": {
        "__ow_method": "post",
        "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "content-length": "4",
        "content-type": "text/plain",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
        }
    }
    ```
    {: screen}

## Securing web actions
{: #actions_web_secure}

By default, anyone can invoke a web action by using the invocation URL. You can secure your web action by using the `require-whisk-auth` [web action annotation](/docs/openwhisk?topic=openwhisk-annotations#annotations-specific-to-web-actions).
{: shortdesc}

Before you begin

Create the `demo` package and `hello` web action by completing the steps in [Creating a web action](#actions_web_example).

You can set the `require-whisk-auth` annotation by either:

- Setting the `require-whisk-auth` annotation to `true`. When the `require-whisk-auth` annotation is set to `true`, the web action authenticates the invocation request's Basic Authorization credentials against the web action owner's whisk auth key. When set to a number or a case-sensitive string, the web action's invocation request must include the `X-Require-Whisk-Auth` header set to this same number or case-sensitive string. Secured web actions return the message `Not Authorized` when credential validation fails.

- Allowing the `require-whisk-auth` annotation to be set automatically by using the `--web-secure` flag. When the `--web-secure` flag is set to `true`, a random number is generated as the `require-whisk-auth` annotation value. When set to `false`, the `require-whisk-auth` annotation is removed. When set to any other value, that value is used as the `require-whisk-auth` annotation value.

### Creating a secure web action
{: #actions_web_secure_create}

Start by creating a web action with the `--web secure` set to `true`.
{: shortdesc}

1. Save the following JavaScript code as `hello.js`.

    ```javascript
    function main({name}) {
    var msg = 'You did not tell me who you are.';
    if (name) {
        msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
    }
    ```
    {: codeblock}

2. Update your `hello` web action with the new version of your `hello.js` code and set the `--web secure` option to `true`.

    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
    ```
    {: pre}

3. Get the `hello` web action to view the randomly generated `require-whisk-auth` value.

    ```bash
    ibmcloud fn action get demo/hello
    ```
    {: pre}

    Example output

    The `require-whisk-auth` value was set to `7819991076995522`.
    ```bash
    {
        "namespace": "<namespace_ID>/demo",
        "name": "hello",
        "version": "0.0.34",
        "exec": {
          "kind": "nodejs:10",
          "binary": false
        },
        "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
        ],
        "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
        },
        "publish": false
    }
    ```
    {: screen}

### Testing a secure web action with randomly generated value of `X-Require-Whisk-Auth`
{: #actions_web_secure_test_random}

Test the `hello` web action and provide the randomly generated `X-Require-Whisk-Auth` value. Replace the `<apihost>` and `<namespace_ID>` values. Replace the `<my-secret>` value with the randomly generated number that you found in step 3 of [Creating a secure web action](#actions_web_secure_create). 
{: shortdesc}

You can test the web action by using one of the following methods.

* Testing the web action by using a cURL command.

    ```bash
    curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
    ```
    {: pre}

* Testing the web action by using a `wget` command.

    ```bash
    wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
    ```
    {: pre}

Example output

```bash
{
"body": "<html><body><h3>Hello, Jane!</h3></body></html>"
}
```
{: screen}

### Testing a secure web action with custom value of `X-Require-Whisk-Auth`
{: #actions_web_secure_test_custom}

To secure your web action with a custom value, update your `hello` web action with your own `require-whisk-auth` value. Then, try testing your web action by specifying the `X-Require-Whisk-Auth` value during invocation.
{: shortdesc}

1. Set a `require-whisk-auth` value where `<my-secret>` is your case-sensitive authentication token.

    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --annotation require-whisk-auth <mysecret>
    ```
    {: pre}

2. Test the web action and include your `<my-secret>` value. You can test the web action by using one of the following methods.

    * Testing the web action by using a cURL command.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
        ```
        {: pre}

    * Testing the action by using a `wget` command.

        ```bash
        wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
        ```
        {: pre}

### Testing to see if your web action is secure
{: #actions_web_secure_test}

Test the `hello` web action without setting the `X-Require-Whisk-Auth` parameter to verify that authentication is required. Using the actions that you created in the previous sections, this test results in an error. 
{: shortdesc}

You can get the URL of your web action by running `ibmcloud fn action get hello --url`. You can use the URL to test the web action by using one of the following methods.

* Testing the web action by using a cURL command.

    ```bash
    curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane
    ```
    {: pre}

* Testing the web action by using a `wget` command.

    ```bash
    wget https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane
    ```
    {: pre}

Example output

```bash
{
"code": "4c4423556547f6ac764ee192d4ed27a6",
"error": "Authentication is possible but has failed or not yet been provided."
}
```
{: screen}

The invocation fails because the `X-Require-Whisk-Auth` value was not provided.

## Raw HTTP handling
{: #actions_web_raw}

A web action can elect to interpret and process an incoming HTTP body directly, without the promotion of a JSON object to first class properties available to the web action input (for example, `args.name` versus parsing `args.__ow_query`). This process is done through a `raw-http` [annotation](/docs/openwhisk?topic=openwhisk-annotations). Using the same example that was shown earlier, but now as a "raw" HTTP web action that receives `name`, both as a query parameter, and as JSON value in the HTTP request body.

```bash
curl https://<apihost>/api/v1/web/<namespace_ID>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


Example output

```bash
{
    "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
        "accept": "*/*",
        "connection": "close",
        "content-length": "15",
        "content-type": "application/json",
        "host": "172.17.0.1",
        "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
    }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} uses the [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} framework to [determine which content types are binary files and which are plain text](https://doc.akka.io/api/akka-http/10.0/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### Enabling raw HTTP handling
{: #actions_web_raw_enable}

You can create a raw HTTP web action by setting the `--web` to `raw`.

```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Decoding binary body content from Base64
{: #actions_web_decode}

When raw HTTP content is processed, the `__ow_body` content is encoded in Base64 when the request `Content-Type` is the binary type. The following functions demonstrate how to decode the body content in Node, Python, and Swift.
{: shortdesc}

1. Save the sample code in your preferred language to a file called `decode.<ext>`. Replace `<ext>` with the file extension of the sample code of your preferred language.

    **Node**

    ```javascript
    function main(args) {
        decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
        return {body: decoded}
    }
    ```
    {: codeblock}

    **Python**

    ```python
    def main(args):
        try:
          decoded = args['__ow_body'].decode('base64').strip()
          return {"body": decoded}
        except:
          return {"body": "Could not decode body from Base64."}
    ```
    {: codeblock}

    **Swift**

    ```swift
    extension String {
        func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
        }
    }

    func main(args: [String:Any]) -> [String:Any] {
        if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
          }
        }

    return ["body": "Could not decode body from Base64."]
    }
    ```
    {: codeblock}

2. Create a raw HTTP web action with the sample code by running the following command. In this example, the Node function is saved as `decode.js`. Replace the file path with the file path of your `decode` file and update the file extension to match the extension of the sample code you used.

    ```bash
    ibmcloud fn action create decode <filepath>/decode.js --web raw
    ```
    {: pre}

    **Example output**

    ```bash
    ok: created action decode
    ```
    {: screen}

3. Test the `decode` action by running the following cURL command. 

    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body"         https://<apihost>/api/v1/web/<namespace_ID>/default/decode.json
    ```
    {: pre}

    **Example output**

    ```bash
    {
        "body": "Decoded body"
    }
    ```
    {: screen}

## Options requests
{: #actions_web_options}

By default, an `OPTIONS` request that is made to a web action results in CORS headers that are automatically added to the response headers. These headers allow all origins and the `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD`, and `PATCH` HTTP verbs.
{: shortdesc}

See the following headers,

```bash
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```
{: screen}

Alternatively, `OPTIONS` requests can be handled manually by a web action. To enable this option, add a `web-custom-options` annotation with a value of `true` to a web action. When this feature is enabled, CORS headers are not automatically added to the request response. Instead, you must append your headers programmatically.

To create custom responses to `OPTIONS` requests:

1. Save the following code to a file `custom-options.js`.

    ```js
    function main(params) {
    if (params.__ow_method == "options") {
        return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
          'Access-Control-Allow-Origin': 'example.com'
        },
        statusCode: 200
        }
    }
    }
    ```
    {: codeblock}

2. Create the web action. Set the `--web-custom-options` to `true`.

    ```bash
    ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
    ```
    {: pre}

3. Test the action by using the following cURL command.

    ```bash
    curl https://<apihost>/api/v1/web/<namespace_ID>/default/custom-option.http -kvX OPTIONS
    ```
    {: pre}

    **Example output**

    ```bash
    < HTTP/1.1 200 OK
    < Server: nginx/1.11.13
    < Content-Length: 0
    < Connection: keep-alive
    < Access-Control-Allow-Methods: OPTIONS, GET
    < Access-Control-Allow-Origin: example.com
    ```
    {: screen}

## Error handling
{: #actions_web_errors}

A {{site.data.keyword.openwhisk_short}} action fails in two different possible failure modes. The first is known as an _application error_, and is analogous to a caught exception: the action returns a JSON object that contains a top level `error` property. The second is a _developer error_, which occurs when the action fails and does not produce a response (similar to an uncaught exception). 
{: shortdesc}

For web actions, the controller handles application errors as follows:

- Any specified path projection is ignored and the controller projects the `error` property instead.
- The controller applies the content handling that is implied by the action extension to the value of the `error` property.

Developers must know how web actions might be used and then generate appropriate error responses. For example, a web action that is used with the `.http` extension returns an HTTP response such as `{error: { statusCode: 400 }`. Failing to do so is a mismatch between the implied `Content-Type` from the extension and the action `Content-Type` in the error response. Special consideration must be given to web actions that are sequences so that components that make up a sequence can generate adequate errors when necessary.



## Disabling web actions
{: #actions_web_disable}

You can disable web actions from both the CLI and the console.
{: shortdesc}

To disable a web action from the CLI,

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}

To disable a web action from the console,

1. Go to the [Action page](https://cloud.ibm.com/functions/actions){: external}.
2. Click the web action that you want to disable.
3. On the Endpoints tab, clear the selection for **Enable as Web Action**.
4. Save your changes.

## Web action limits
{: #actions_web_limits}

For more information about request and response limits for web actions, see [System details and limits](/docs/openwhisk?topic=openwhisk-limits#web_action_limits).


