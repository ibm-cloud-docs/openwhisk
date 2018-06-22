---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creating serverless REST APIs
{: #openwhisk_apigateway}

{{site.data.keyword.openwhisk}} actions can benefit from being managed by APIs directly by introducing the API Gateway, which acts as a proxy to [web actions](./openwhisk_webactions.html), and provides them with additional features. The extra features include: HTTP method routing, client id/secrets, rate limits, CORS, view API usage, view response logs, and API sharing policies. For more information about API management, you can read the [API management documentation](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Create APIs from OpenWhisk web actions by using your Browser
{: #create_api_browser}

You can use the [**APIs tab**](https://console.bluemix.net/openwhisk/apimanagement) in the [{{site.data.keyword.openwhisk_short}} Dashboard](https://console.bluemix.net/openwhisk/) to perform the following tasks:

* [Create a Cloud Functions API](https://console.bluemix.net/openwhisk/apimanagement) - Create an API that wraps a set of OpenWhisk actions.
* [Secure your API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Apply API security and rate limiting policies to protect your API.
* [Manage Traffic](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - View API usage statistics and check out response logs.
* [Socialize & Share](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - Share your API with developers both within and outside {{site.data.keyword.Bluemix_notm}}.

## Create APIs from OpenWhisk web actions by using the CLI plug-in
{: #create_api_cli}

The following section steps you through API management tasks using the {{site.data.keyword.openwhisk_short}} CLI plug-in. To create and manage APIs through the CLI, you must first install the [{{site.data.keyword.openwhisk_short}} CLI plug-in](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) for {{site.data.keyword.Bluemix_notm}}.

For convenience, the steps are broken down into smaller subtopics which you can quickly jump to by using the following list of API tasks:

* [Create first API](openwhisk_apigateway.html#create_cli_api)
* [Full control over the HTTP response](openwhisk_apigateway.html#full_control)
* [Exposing multiple web actions](openwhisk_apigateway.html#multiple_web_actions)
* [Export the configuration](openwhisk_apigateway.html#export_config)
* [Import the configuration](openwhisk_apigateway.html#import_config)
* [Modify the configuration](openwhisk_apigateway.html#modify_config)

### Create your first API by using the CLI
{: #create_cli_api}

1. Create a JavaScript file named **hello.js** which contains the following content:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Create a web action named **hello** using the `hello.js` file created in step one. **Note:** Be sure to add the flag `--web true`.
  ```
  ibmcloud wsk action create hello hello.js --web true
  ```
  {: pre}

  Example output:
  ```
  ok: created action hello
  ```
  {: screen}

3. Create an API with base path `/hello`, path `/world`, and method `get`, with response type `json`:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type json
  ```
  {: pre}

  Example output:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  A new URL is generated exposing the `hello` action by using a __GET__ HTTP method.

4. Finally, send an HTTP request to the URL using the **curl** command:
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Example output:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

The web action **hello** is invoked, which returns back a JSON object that includes the parameter **name** sent through the query parameter. You can pass parameters to the action with simple query parameters, or by using the request body. Web actions can invoke an action in a public way without using the OpenWhisk authorization API key.

### Full control over the HTTP response
{: #full_control}

The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. Using `--response-type json` returns the full result of the action in JSON format, and automatically sets the Content-Type header to `application/json`.

You want to have full control over the HTTP response properties like `statusCode` and `headers`, so you can return different content types in the `body`. The flag `--response-type http` makes this possible by configuring the target URL of the web action with the `http` extension.

You can choose to change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [web actions](./openwhisk_webactions.html) documentation.

1. Change the code for the `hello.js` returning the JSON properties `body`, `statusCode`, and `headers`:
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

2. Update the action with the modified result:
  ```
  ibmcloud wsk action update hello hello.js --web true
  ```
  {: pre}

3. Update the API response type by using the `--response-type http` flag:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Call the updated API by using the following **curl** command:
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Example output:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

Now, you are in full control of your APIs, and can control the content. Like returning HTML, or set the status code for things like Not Found (404), Unauthorized (401), or even Internal Error (500).

### Exposing multiple web actions
{: #multiple_web_actions}

For example, if you want to expose a set of actions for a book club, you can use a series of actions to implement your backend for the book club:

| Action | HTTP method | Description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Get book details  |
| postBooks   | POST | Add a book |
| putBooks    | PUT | Update book details |
| deleteBooks | DELETE | Delete a book |

In this example, the API is defined with a **path parameter**. When using path parameters, the API must be defined with a response type of `http`. The path value, starting with the base path and including the actual path parameter value(s), is available in the `__ow_path` field of the action's JSON parameter. Refer to the [web actions HTTP Context](./openwhisk_webactions.html#http-context) documentation for more details, including info about more HTTP context fields that are available to web actions invoked with an `http` response type.

1. Create an API for the book club, named **Book Club**, with `/club` as its HTTP URL base path, `books` as its resource, and `{isbn}` as a path parameter that is used to identify a specific book by using it's International Standard Book Number (ISBN).
  ```bash
  ibmcloud wsk api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud wsk api create /club /books get getBooks                       --response-type http
  ibmcloud wsk api create /club /books post postBooks                     --response-type http
  ibmcloud wsk api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud wsk api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Notice that the first action that is exposed with the base path `/club` gets the API label with the name **Book Club**. Any other actions that are exposed under `/club` are now associated with **Book Club**.

2. List all of the **Book Club**  actions that are exposed by using the following command:
  ```
  ibmcloud wsk api list /club -f
  ```
  {: pre}

  Example output:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. For fun, you can add a book titled, **JavaScript: The Good Parts**, with an HTTP __POST__:
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Example output:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Get a list of books by using the action **getBooks** with HTTP __GET__:
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Example output:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. You can delete a specific book by using the action **deleteBooks** with HTTP __DELETE__. In this example, the **deleteBooks** action's `__ow_path` field value is `/club/books/978-0596517748`, where `978-0596517748` is the path's `{isbn}` actual value.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Export the configuration
{: #export_config}

1. Export the API named **Book Club** into a file that can be used as a base to re-create the APIs by using a file as input.
  ```
  ibmcloud wsk api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Test the swagger file by first deleting all exposed URLs under a common base path by using the following command:
  ```
  ibmcloud wsk api delete /club
  ```
  {: pre}

  Example output:
  ```
  ok: deleted API /club
  ```
  {: screen}

  You can delete all of the exposed URLs by using either the base path `/club` or API name label **"Book Club"**:
  {: tip}

### Import the configuration
{: #import_config}

1. Now, restore the API named **Book Club** by using the file name `club-swagger.json`:
  ```
  ibmcloud wsk api create --config-file club-swagger.json
  ```
  {: pre}

  Example output:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

2. Verify that the **Book Club** API is re-created:
  ```
  ibmcloud wsk api list /club
  ```
  {: pre}

  Example output:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

### Modify the configuration by using the UI
{: #modify_config}

You can edit the configuration in the {{site.data.keyword.openwhisk_short}} Dashboard, click the [APIs tab](https://console.ng.bluemix.net/openwhisk/apimanagement) to set up security, rate limits, and other features. Once you are done updating the configuration, you can download the definition file in JSON format, and then re-import it by using the CLI. This can be useful, for example, for an unattended deployment in a continuous integration and deployment (CICD) pipeline. You also have the option to upload and re-import the API definition file using the UI.
