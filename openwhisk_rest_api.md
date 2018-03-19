---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# REST APIs
{: #openwhisk_rest_api}

After your OpenWhisk environment is enabled, you can use OpenWhisk with your web apps or mobile apps with REST API calls.
{: shortdesc}

For more information about the APIs for Actions, Activations, Packages, Rules, and Triggers, see the [OpenWhisk API documentation](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


All the capabilities in the system are available through a REST API. Collection and entity endpoints are available for Actions, Triggers, Rules, Packages, Activations, and Namespaces.

Available collection endpoints:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

The `{APIHOST}` is the OpenWhisk API hostname (for example, openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13, and so on). For the `{namespace}`, the character `_` can be used to specify the user's *default
namespace*.

You can perform a GET request on the collection endpoints to fetch a list of entities in the collection.

The following entity endpoints are available for each type of entity:
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

The Namespace and Activation endpoints support GET requests. The Actions, Triggers, Rules, and Packages endpoints support GET, PUT, and DELETE requests. The endpoints of Actions, Triggers, and Rules also support POST requests, which are used to invoke Actions and Triggers and enable or disable Rules. 

All APIs are protected with HTTP Basic authentication. You can use the [wskadmin ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) tool to generate a new Namespace and authentication. The Basic authentication credentials are in the `AUTH` property in your `~/.wskprops` file, delimited by a colon. You can also retrieve these credentials by using the CLI running `bx wsk property get --auth`.

In the following example, the [cURL](https://curl.haxx.se) command tool is used to get the list of all Packages in the `whisk.system` Namespace:
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

Output:
```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```
{: codeblock}

In this example, the authentication was passed by using the `-u` flag. You can also pass this value also as part of the URL, such as, `https://$AUTH@{APIHOST}`.

The OpenWhisk API supports request-response calls from web clients. OpenWhisk responds to `OPTIONS` requests with Cross-Origin Resource Sharing headers. Currently, all origins are allowed (that is, Access-Control-Allow-Origin is "`*`") and Access-Control-Allow-Headers yield Authorization and Content-Type.

**Attention:** Because OpenWhisk currently supports only one key per Namespace, it is not recommended to use CORS beyond simple experiments. Use [Web Actions](./openwhisk_webactions.html) or [API Gateway](./openwhisk_apigateway.html) to expose your Actions to the public and not use the OpenWhisk authorization key for client applications that require CORS.

## Using the CLI verbose mode
{: #openwhisk_rest_api_cli_v}

The OpenWhisk CLI is an interface to the OpenWhisk REST API. You can run the CLI in verbose mode with the flag `-v`, which prints all the information about the HTTP request and response.

Display the Namespace value for the current user by running the following command:
```
bx wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

The printed information provides the properties of the HTTP request, and performs an HTTP method `GET` on the URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` by using a User-Agent header `OpenWhisk-CLI/1.0 (<CLI-Build-version>)`, and a Basic Authorization header `Basic XXXYYYY`. 
Notice that the authorization value is your base64-encoded OpenWhisk authorization string.
The response is of content type `application/json`.

## Actions
{: #openwhisk_rest_api_actions}

To create or update an Action, send an HTTP request with method `PUT` on the Actions collection. For example, to create a `nodejs:6` Action with the name `hello` by using a single file content, use the following command:
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

To perform a blocking invocation on an action, send an HTTP request with a method `POST` and body that contains the input parameter `name`, use the following command:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

You get the following response:
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
To get the `response.result`, run the command again with the query parameter `result=true` like in the following example:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
You get the following response:
```json
{
  "payload": "hello John"
}
```

## Annotations and Web Actions
{: #openwhisk_rest_api_webactions}

To create an Action as a web Action, you need to add an [annotation](./openwhisk_annotations.html) of `web-export=true` for web Actions. Since web Actions are publicly accessible, you want to protect pre-defined parameters (that is, treat them as final) using the annotation `final=true`. If you create or update an Action that uses the CLI flag `--web true`, the command adds both annotations `web-export=true` and `final=true`.

Run the following curl command to provide the complete list of annotations to set on the Action.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

You can now invoke this Action as a public URL with no OpenWhisk authorization. Try to invoke by using the web Action public URL, and including an extension like `.json` or `.http`, for example, at the end of the URL.
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

Output:
```json
{
  "payload": "Hello John"
}
```
{: codeblock}

This example source code does not work with `.http`, see [Web Actions](./openwhisk_webactions.html) documentation on how to modify.

## Sequences
{: #openwhisk_rest_api_sequences}

To create an Action sequence, provide the names of the Actions that compose the sequence in the desired order. So that the output from the first Action is passed as input to the next Action.

```
bx wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
```

Create a sequence with the Actions `/whisk.system/utils/split` and `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

When you specify the names of the Actions, they must be full qualified.

## Triggers
{: #openwhisk_rest_api_triggers}

To create a Trigger, the minimum information you need is a name for the Trigger. You could also include default parameters that get passed to the Action through a Rule when the Trigger gets fired.

Create a Trigger with name `events` with a default parameter `type` with value `webhook` set.
```bash
curl -u $AUTH https://openwhisk.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Now whenever you have an event that needs to fire this Trigger it just takes an HTTP request with a method `POST` by using the OpenWhisk Authorization key.

To fire the Trigger `events` with a parameter `temperature`, send the following HTTP request.
```bash
curl -u $AUTH https://openwhisk.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Triggers with Feed Actions
{: #openwhisk_rest_api_triggers_feed}

Special Triggers can be created by using a Feed Action. The Feed Action is an Action that helps with the configuration of a Feed provider that is in charge of firing the Trigger whenever an event for the Trigger occurs. Learn more about these feed providers in the [feeds.md] documentation.

Some of the available Triggers that leverage a Feed Action are periodic/alarms, Slack, Github, Cloudant/Couchdb, and messageHub/Kafka. You also can create your own Feed Action and Feed provider.

Create a Trigger with name `periodic` to be fired at a specified frequency, every 2 hours (for example, 02:00:00, 04:00:00, ...).

Using the CLI, run the following command to create the Trigger:
```bash
bx wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

Since the `-v` flag is used, two HTTP requests are sent. One is to create a Trigger that is called `periodic`, and the other is to invoke the Feed Action. The Feed Action, `/whisk.system/alarms/alarm`, is sent the parameters to configure the Feed provider to fire the Trigger every 2 hours.

To achieve this activity with the REST API, create the trigger first like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

As you can see, the annotation `Feed` is stored in the Trigger. Later, this annotation can be used to know which Feed Action to use to delete the Trigger.

Now that the Trigger is created, lets invoke the Feed Action
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

Deleting the Trigger is similar to creating the Trigger. So this time, delete the Trigger by using the Feed Action in order to configure the Feed provider to also delete the handler for the Trigger.

Invoke the Feed Action to delete the Trigger handler from the feed provider by running the following command:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

Now delete the Trigger with an HTTP request by using the `DELETE` method:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Rules
{: #openwhisk_rest_api_rules}

To create a rule that associates a Trigger with an Action, send an HTTP request with a `PUT` method to provide the Trigger and Action in the body of the request.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

Rules can be enabled or disabled, and you can change the status of the Rule by updating its status property. For example, to disable the rule `t2a`, send in the body of the request `status: "inactive"` with a `POST` method.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Packages
{: #openwhisk_rest_api_packages}

To create an Action in a Package, you have to create a Package first. Create a Package with the name `iot` and send an HTTP request with a `PUT` method by running the following command:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Activations
{: #openwhisk_rest_api_activations}

To get the list of the last three Activations, use an HTTP request with a `GET` method by passing the query parameter `limit=3` like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

To get all the details of an Activation that include results and logs, send an HTTP request with a `GET` method by passing the Activation identifier as a path parameter like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
