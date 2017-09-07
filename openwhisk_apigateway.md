---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# API Gateway
{: #openwhisk_apigateway}

OpenWhisk actions can benefit from being managed by API management.

The API Gateway acts as a proxy to [Web Actions](webactions.md), and provides them with additional features. The extra features include: HTTP method routing, client id/secrets, rate limits, CORS, view API usage, view response logs, and API sharing policies.
For more information about API management, you can read the [API management documentation](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).

## Create APIs from OpenWhisk web actions by using your Browser.

With API Gateway, you can expose an OpenWhisk action as an API. After you define the API, you can apply security and rate limit policies, view API usage and response logs, and define API sharing policies.
In the OpenWhisk Dashboard, click the [APIs tab](https://console.ng.bluemix.net/openwhisk/apimanagement).


## Create APIs from OpenWhisk web actions by using the CLI

### OpenWhisk CLI configuration

Configure the OpenWhisk CLI with the API host.

Two Bluemix regions are available that require their own unique API host and Authorization key.

* US South
  * API host: `openwhisk.ng.bluemix.net`

* United Kingdom
  * API host: `openwhisk.eu-gb.bluemix.net`

Run the following command to set the API host for your desired Bluemix region:

US South:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

United Kingdom:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

**Note:** If you ever need to switch regions, you must reconfigure the CLI with both the API host, and authorization key because the authorization key is specific per region.

Artifacts such as actions, rules, and packages are region-specific. So if you use the same artifact in multiple regions you must deploy it to each desired region.

To be able to use the `wsk api` command, the CLI configuration file `~/.wskprops` needs to contain the Bluemix Access Token.

To get the access token, use the following CLI command:
```
wsk bluemix login
```
{: pre}

For more information about this command run:
```
wsk bluemix login -h
```
{: pre}

**Note:** If the `wsk bluemix login` command fails with the error `BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso`, login with the Bluemix CLI using `bluemix login`, then issue `wsk bluemix login --sso`.

### Create your first API by using the CLI

1. Create a JavaScript file with the following content. For this example, the file name is 'hello.js'.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Create a web action from the following JavaScript function. For this example, the action is called 'hello'. Make sure to add the flag `--web true`.
  
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}
  ```
  ok: created action hello
  ```
  
3. Create an API with base path `/hello`, path `/world`, and method `get`, with response type `json`:
  
  ```
  wsk api create /hello /world get hello --response-type json
  ```
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  A new URL is generated exposing the `hello` action via a __GET__ HTTP method.
  
4. Finally, send an HTTP request to the URL.
  
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```
  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  The web action `hello` is invoked, which returns back a JSON object that includes the parameter `name` sent via query parameter. You can pass parameters to the action via simple query parameters, or via the request body. Web actions can invoke an action in a public way without the OpenWhisk authorization API key.
  
### Full control over the HTTP response
  
  The `--response-type` flag controls the target URL of the web action to be proxied by the API Gateway. Using `--response-type json` returns the full result of the action in JSON format, and automatically sets the Content-Type header to `application/json`. 
  
  Once you get started, you want to have full control over the HTTP response properties like `statusCode`, `headers`, and return different content types in the `body`. The flag `--response-type http` makes this possible by configuring the target URL of the web action with the `http` extension.

  You can choose to change the code of the action to comply with the return of web actions with the `http` extension, or include the action in a sequence to pass its result to a new action. The new action can then transform the result to be properly formatted for an HTTP response. You can read more about response types and web actions extensions in the [Web Actions](webactions.md) documentation.

  Change the code for the `hello.js` returning the JSON properties `body`, `statusCode`, and `headers`
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Notice that the body needs to be return encoded in `base64` and not a string.
  
  Update the action with the modified result: 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Update the API with `--response-type http`: 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Call the updated API: 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Now, you are in full control of your APIs, and can control the content. Like returning HTML, or set the status code for things like Not Found (404), Unauthorized (401), or even Internal Error (500).

### Exposing multiple web actions

For example, if you want to expose a set of actions for a book club, you can use a series of actions to implement your backend for the book club:

| action | HTTP method | description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | get book details  |
| postBooks   | POST | adds a book |
| putBooks    | PUT | updates book details |
| deleteBooks | DELETE | deletes a book |

Create an API for the book club, named `Book Club`, with `/club` as its HTTP URL base path, and `books` as its resource.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Notice that the first action that is exposed with the base path `/club` gets the API label with name `Book Club`. Any other actions that are exposed under `/club` are associated with `Book Club`.

List all the actions that are exposed by using the following command:
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

For fun, you can add a book, `JavaScript: The Good Parts`, with an HTTP __POST__:
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Get a list of by using the action `getBooks` via HTTP __GET__
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Export the configuration
Export the API named `Book Club` into a file that can be used as a base to re-create the APIs by using a file as input. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Test the swagger file by first deleting all exposed URLs under a common base path.
You can delete all of the exposed by using either the base path `/club` or API name label `"Book Club"`:
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Change the configuration

You can edit the configuration in the OpenWhisk Dashboard, click the [APIs tab](https://console.ng.bluemix.net/openwhisk/apimanagement) to set up security, rate limits, and other features.

### Import the configuration

Now, restore the API named `Book Club` by using the file `club-swagger.json`
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Verify that the API is re-created:
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
