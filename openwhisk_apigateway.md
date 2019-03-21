---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: serverless, rest api, gateway, web actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creating serverless REST APIs
{: #openwhisk_apigateway}

Use APIs to directly manage {{site.data.keyword.openwhisk}} actions. The API Gateway acts as a proxy to [web actions](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions) and provides HTTP method routing, client ID and secrets, rate limits, CORS, viewing API usage, viewing response logs, and API sharing policies.
{: shortdesc}

For more information about API management, you can read the [API management documentation](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis).



## Creating your first API
{: #create_cli_api}

Before you begin, install the [{{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

1. Save the following code into a JavaScript file named `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Create a web action named `hello` using the file you created. **Note:** Be sure to add the flag `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Example output:
  ```
  ok: created action hello
  ```
  {: screen}

3. Create an API with base path `/hello`, path `/world`, method `get`, and response type `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Example output:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  A new URL is generated exposing the `hello` action by using a GET HTTP method.

4. Send a test HTTP request to the URL using the cURL command.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Example output:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

The web action `hello` is invoked, which returns a JSON object that includes the parameter **name** in the query parameter. You can pass parameters to the action with simple query parameters or by using the request body. Web actions can publicly invoke an action without using authentication.

## Using full control over the HTTP response
{: #full_control}

The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. For example, when you use the `--response-type json` flag, the full result of the action is returned in JSON format and the **Content-Type** header is automatically set to `application/json`.

To return different content types in the body, use full control over the HTTP response properties such as **statusCode** and **headers**. You can use the `--response-type http` flag to configure the target URL of the web action with the `http` extension. You can change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [web actions](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions) documentation.

1. Change the code for the `hello.js` action returning the JSON properties `body`, `statusCode`, and `headers`.
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

  Example output:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

## Exposing multiple web actions
{: #multiple_web_actions}

You can expose multiple web actions to implement your app backend. For example, to expose a set of actions for a book club, you can use a series of actions to implement your backend for the book club:

| Action | HTTP method | Description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Get book details  |
| postBooks   | POST | Add a book |
| putBooks    | PUT | Update book details |
| deleteBooks | DELETE | Delete a book |

In this example, the API is defined with a path parameter. When you use path parameters, the API must be defined with a response type of `http`. The path value, starting with the base path and including the actual path parameter values, is available in the `__ow_path` field of the action's JSON parameter. For more details about HTTP context fields, see the [web actions HTTP Context](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#http-context) documentation.

To try out this book club web actions example:

1. Create an API for the book club named `Book Club`, with `/club` as its HTTP URL base path, `books` as its resource, and `{isbn}` as a path parameter that is used to identify a specific book by using its International Standard Book Number (ISBN).
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  The first action that is exposed with the base path `/club` is labeled with the name `Book Club`. Any other actions that are exposed under `/club` are now associated with `Book Club`.

2. List all of the `Book Club` actions that are exposed.
  ```
  ibmcloud fn api list /club -f
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

3. Add a book titled `JavaScript: The Good Parts` by using an HTTP POST.
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

4. Get a list of books by using an HTTP GET call to the `getBooks` action.
  ```
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

5. Delete a specific book by using an HTTP DELETE call to the `deleteBooks` action. In this example, the `deleteBooks` action's `__ow_path` field value is `/club/books/978-0596517748`, where `978-0596517748` is the path's `{isbn}` actual value.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Exporting and importing the configuration
{: #export_import_config}

To export or import a configuration, you can continue using the book club example.

1. Export the `Book Club` API into a file named `club-swagger.json`. This file can be used as a base to re-create the APIs by using a file as input.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Test the swagger file by first deleting all exposed URLs under a common base path.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Example output:
  ```
  ok: deleted API /club
  ```
  {: screen}

  You can delete all of the exposed URLs by using either the base path `/club` or API name label `"Book Club"`.
  {: tip}

3. Restore the `Book Club` API by using the `club-swagger.json` file.
  ```
  ibmcloud fn api create --config-file club-swagger.json
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

4. Verify that the `Book Club` API is re-created.
  ```
  ibmcloud fn api list /club
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

## Modifying the configuration
{: #modify_config}

After you have created your configuration, you can use the [**APIs tab**](https://cloud.ibm.com/openwhisk/apimanagement) in the {{site.data.keyword.openwhisk_short}} dashboard to modify the configuration in the following ways.

* [Create a {{site.data.keyword.openwhisk_short}} API](https://cloud.ibm.com/openwhisk/apimanagement) that wraps a set of {{site.data.keyword.openwhisk_short}} actions.
* [Secure your API](https://cloud.ibm.com/openwhisk/apimanagement by applying API security and rate limiting policies.
* [Manage traffic](https://cloud.ibm.com/openwhisk/apimanagement) by viewing API usage statistics and checking out response logs.
* [Socialize and share](https://cloud.ibm.com/openwhisk/apimanagement) your API with developers both within and outside {{site.data.keyword.Bluemix_notm}}.

Once you are done updating the configuration, you can download the definition file in JSON format, and then re-import it by using the CLI. Downloading and importing the configuration is useful, for example, for an unattended deployment in a continuous integration and deployment (CICD) pipeline. You also have the option to upload and re-import the API definition file using the UI.
