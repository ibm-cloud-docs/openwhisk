---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Creating web actions
{: #openwhisk_webactions}

Web actions are OpenWhisk actions which are annotated to quickly enable developers to build web-based applications. These annotated actions allow developers to program backend logic that your web application can access anonymously, without requiring an OpenWhisk authentication key. It is up to the action developer to implement their own desired authentication and authorization (that is, OAuth flow).
{: shortdesc}

Web action activations are associated with the user that created the action. This action defers the cost of an action activation from the caller to the owner of the action.

Look at the following JavaScript action `hello.js`:
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

You can create a _web action_ **hello**  in the package `demo` for the Namespace `guest` by using the CLI's `--web` flag with a value of `true` or `yes`:
```
ibmcloud wsk package create demo
```
{: pre}

```
ibmcloud wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

Using the `--web` flag with a value of `true` or `yes` allows an action to be accessible through a REST interface without the need for credentials. To configure a web action with credentials see the [Securing web actions](./openwhisk_webactions.html#securing-web-actions) section. A web action can be invoked by using a URL that is structured as follows:
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

The package name is **default** if the action is not in a named package.

An example is `guest/demo/hello`. The web action API path can be used with `curl` or `wget` without an API key. It can even be entered directly in your browser.

Try opening [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane) in your web browser. Or try invoking the action by using `curl`:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

In the following example, a web action performs an HTTP redirect:
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

In the following example, a web action sets a single cookie:
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    },
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

In the following example, a web action sets multiple cookies:
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

The following example returns an `image/png`:
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
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

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [response size limit](./openwhisk_reference.html) for actions since a response that exceeds the predefined system limits fail. Large objects are not sent inline through OpenWhisk, but instead deferred to an object store, for example.

## Handling HTTP requests with actions
{: #openwhisk_webactions_http}

An OpenWhisk action that is not a web action requires both authentication and must respond with a JSON object. In contrast, web actions can be invoked without authentication, and can be used to implement HTTP handlers that respond with _headers_, _statusCode_, and _body_ content of different types. The web action must return a JSON object. However, the OpenWhisk system (namely the `controller`), treats a web action differently if its result includes one or more of the following top-level JSON properties:

- `headers`: A JSON object where the keys are header-names and the values are string, number, or boolean values for those headers (default is no headers). To send multiple values for a single header, the header's value is a JSON array of values.
- `statusCode`: A valid HTTP status code (default is 200 OK).
- `body`: A string that is either plain text or a base64 encoded string (for binary data).

The controller is to pass along the action-specified headers, if any, to the HTTP client that terminates the request/response. Similarly, the controller responds with the status code when present. Lastly, the body is passed along as the body of the response. Unless a `Content-Type` header is declared in the action result’s `headers`, the body is passed along as is if it’s a string (or results in an error otherwise). When the `Content-Type` is defined, the controller determines whether the response is binary data or plain text and decode the string by using a base64 decoder as needed. If the body fails to decode correctly, an error is returned to the caller.

_Note_: A JSON object or array is treated as binary data, and must be base64 encoded.

## HTTP Context
{: #http-context}

All web actions, when invoked, receive HTTP request details as parameters to the action input argument.

See the following HTTP parameters:

- `__ow_method` (type: string). The HTTP method of the request.
- `__ow_headers` (type: map string to string): The request headers.
- `__ow_path` (type: string): The unmatched path of the request (matching stops once the action extension is consumed).
- `__ow_user` (type: string): The Namespace that identifies the OpenWhisk authenticated subject
- `__ow_body` (type: string): The request body entity, as a base64 encoded string when content is binary, or plain string otherwise
- `__ow_query` (type: string): The query parameters from the request as an unparsed string

A request cannot override any of the named `__ow_` parameters. Doing so, results in a failed request with status equal to 400 Bad Request.

The `__ow_user` is only present when the web action is [annotated to require authentication](./openwhisk_annotations.html#annotations-specific-to-web-actions) and allows a web action to implement its own authorization policy. The `__ow_query` is available only when a web action elects to handle the ["raw" HTTP request](#raw-http-handling). It is a string that contains the query parameters that are parsed from the URI (separated by `&`). The `__ow_body` property is present in either "raw" HTTP requests, or when the HTTP request entity is not a JSON object or form data. Otherwise, web actions receive query and body parameters as first class properties in the action argument. Body parameters take precedence over query parameters, which in turn take precedence over action and package parameters.

## HTTPS Endpoint support

Supported SSL protocols: TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3 ([draft version 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Unsupported SSL protocols: SSLv2, SSLv3

## Extra features
{: #extra-features}

Web actions provide extra features that include:

- `Content extensions`: The request must specify its desired content type as `.json`, `.html`, `.http`, `.svg`, or `.text`. The type is specified by adding an extension to the action name in the URI so that an action `/guest/demo/hello` is referenced as `/guest/demo/hello.http`, for example, to receive an HTTP response back. For convenience, the `.http` extension is assumed when no extension is detected.
- `Projecting fields from the result`: The path that follows the action name is used to project out one or more levels of the response.
`/guest/demo/hello.html/body`. This feature allows an action that returns a dictionary `{body: "..." }` to project the `body` property, and directly return its string value instead. The projected path follows an absolute path model (as in XPath).
- `Query and body parameters as input`: The action receives query parameters as well as parameters in the request body. The precedence order for merging parameters is: package parameters, action parameters, query parameter, and body parameters. Each of these parameters can override any previous values if overlap occurs. As an example, `/guest/demo/hello.http?name=Jane` can pass the argument `{name: "Jane"}` to the action.
- `Form data`: In addition to the standard `application/json`, web actions can receive URL encoded from data `application/x-www-form-urlencoded data` as input.
- `Activation using multiple HTTP verbs`: A web action can be invoked through any of these HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`, as well as `HEAD` and `OPTIONS`.
- `Non JSON body and raw HTTP entity handling`: A web action can accept an HTTP request body other than a JSON object, and can elect to always receive such values as opaque values (plain text when not binary, or base64 encoded string otherwise).

The example below briefly sketches how you might use these features in a web action. Consider an action `/guest/demo/hello` with the following body:
```javascript
function main(params) {
    return { response: params };
}
```

When this action is invoked as a web action, you can alter the response of the web action by projecting different paths from the result.

For example, to return the entire object, and see what arguments the action receives:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}

Example output:
```
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

To run with a query parameter, see the following example command:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

Example output:
```
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

You can also run with form data:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

Example output:
```
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

Run the following command for a JSON object:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

Example output:
```
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

Run the following command to project the name (as text):
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

Example output:
```
Jane
```
{: screen}

For convenience, query parameters, form data, and JSON object body entities are all treated as dictionaries, and their values are directly accessible as action input properties. This behavior is not the case for web actions, which opt to handle HTTP request entities more directly, or when the web action receives an entity that is not a JSON object.

See the following example that uses a "text" content-type, as was shown previously.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

Example output:
```
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

## Content extensions
{: #openwhisk_webactions_extensions}

A content extension is usually necessary to invoke a web action. The absence of an extension assumes `.http` as the default. The `.json` and `.http` extensions do not require a projection path, whereas the `.html`, `.svg`, and `.text` extensions do. For convenience, the default path is assumed to match the extension name. To invoke a web action and receive an `.html` response, the action must respond with a JSON object that contains a top-level property called `html` (or the response must be in the explicit path). In other words, `/guest/demo/hello.html` is equivalent to projecting the `html` property explicitly, as in `/guest/demo/hello.html/html`. The fully qualified name of the action must include its package name, which is `default` if the action is not in a named package.

## Protected parameters
{: #openwhisk_webactions_protected}

Action parameters are protected and treated as immutable. Parameters are automatically finalized to enable web actions.
```
ibmcloud wsk action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

The result of these changes is that the `name` is bound to `Jane` and cannot be overridden by query or body parameters because of the final annotation. This design secures the action against query or body parameters that try to change this value whether by accident or intentionally.

## Securing web actions
{: #securing-web-actions}

By default, a web action can be invoked by anyone having the web action's invocation URL. Use the `require-whisk-auth` [web action annotation](./openwhisk_annotations.html#annotations-specific-to-web-actions) to secure the web action. When the `require-whisk-auth` annotation is set to `true`, the action will authenticate the invocation request's Basic Authorization credentials against the action owner's whisk auth key. When set to a number or a case-sensitive string, the action's invocation request must include a `X-Require-Whisk-Auth` header having this same value. Secured web actions will return the message `Not Authorized` when credential validation fails.

Alternatively, use the `--web-secure` flag to automatically set the `require-whisk-auth` annotation.  When set to `true`, a random number is generated as the `require-whisk-auth` annotation value. When set to `false`, the `require-whisk-auth` annotation is removed.  When set to any other value, that value is used as the `require-whisk-auth` annotation value.

Example using **--web-secure**:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

Example using **require-whisk-auth**:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

Example using **X-Require-Whisk-Auth**:
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

It is important to note that the owner of the web action owns all of the activations records, and incurs the cost of running the action in the system regardless of how the action was invoked.

## Disabling web actions

To disable a web action from being invoked via web API (`https://openwhisk.bluemix.net/api/v1/web/`), pass a value of `false` or `no` to the `--web` flag to update an action with the CLI.
```
ibmcloud wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## Raw HTTP handling

A web action can elect to interpret and process an incoming HTTP body directly, without the promotion of a JSON object to first class properties available to the action input (for example, `args.name` versus parsing `args.__ow_query`). This process is done through a `raw-http` [annotation](./openwhisk_annotations.html). Using the same example that was shown earlier, but now as a "raw" HTTP web action that receives `name`, both as a query parameter, and as JSON value in the HTTP request body:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

Example output:
```
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

OpenWhisk uses the [Akka Http](http://doc.akka.io/docs/akka-http/current/scala/http/) framework to [determine](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html) which content types are binary and which are plain text.

### Enabling raw HTTP handling

Raw HTTP web actions are enabled through the `--web` flag by using a value of `raw`.
```
ibmcloud wsk action create /guest/demo/hello hello.js --web raw
```
{: pre}

### Disabling raw HTTP handling

Disabling raw HTTP can be accomplished by passing a value of `false` or `no` to the `--web` flag.
```
ibmcloud wsk update create /guest/demo/hello hello.js --web false
```
{: pre}

### Decoding binary body content from Base64

When raw HTTP content is processed, the `__ow_body` content is encoded in Base64 when the request `Content-Type` is binary. The following functions demonstrate how to decode the body content in Node, Python, and Swift. Simply save a method to a file, create a raw HTTP web action that utilizes the saved artifact, and then invoke the web action.

#### Node

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

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

As an example, save the Node function as `decode.js` and execute the following commands:
```
ibmcloud wsk action create decode decode.js --web raw
```
{: pre}

Example output:
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

Example output:
```
{
  "body": "Decoded body"
}
```
{: screen}

## Options Requests
{: #options-requests}

By default, an OPTIONS request made to a web action results in CORS headers that are automatically added to the response headers. These headers allow all origins and the options, get, delete, post, put, head, and patch HTTP verbs.

See the following headers:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Alternatively, OPTIONS requests can be handled manually by a web action. To enable this option, add a
`web-custom-options` annotation with a value of `true` to a web action. When this feature is enabled, CORS headers are not automatically added to the request response. Instead, it is the developer's responsibility to append their desired headers programmatically.

See the following example to create custom responses to OPTIONS requests:
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

Save the function to `custom-options.js` and execute the following commands:
```
ibmcloud wsk action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

Example output:
```
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```
{: screen}

## Error Handling
{: #openwhisk_webactions_errors}

A {{site.data.keyword.openwhisk_short}} action fails in two different possible failure modes. The first is known as an _application error_, and is analogous to a caught exception: the action returns a JSON object that contains a top level `error` property. The second is a _developer error_, which occurs when the action fails catastrophically, and does not produce a response (similar to an uncaught exception). For web actions, the controller handles application errors as follows:

- Any specified path projection is ignored and the controller projects the `error` property instead.
- The controller applies the content handling that is implied by the action extension to the value of the `error` property.

Developers must know how web actions might be used, and generate appropriate error responses. For example, a web action that is used with the `.http` extension returns an HTTP response like `{error: { statusCode: 400 }`. Failing to do so is a mismatch between the implied `Content-Type` from the extension and the action `Content-Type` in the error response. Special consideration must be given to web actions that are sequences so that components that make up a sequence can generate adequate errors when necessary.
