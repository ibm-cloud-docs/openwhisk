---

copyright:
  years: 2017
lastupdated: "2017-09-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Using the OpenWhisk REST APIs
{: #openwhisk_rest_api}

After your OpenWhisk environment is enabled, you can use OpenWhisk with your web apps or mobile apps with REST API calls.
{: shortdesc}

For more information about the APIs for actions, activations, packages, rules, and triggers, see the [OpenWhisk API documentation](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


All the capabilities in the system are available through a REST API. Collection and entity endpoints are available for actions, triggers, rules, packages, activations, and namespaces.

Available collection endpoints:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

The `{APIHOST}` is the OpenWhisk API hostname (for example, openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13, and so on).
For the `{namespace}`, the character `_` can be used to specify the user's *default
namespace*.

You can perform a GET request on the collection endpoints to fetch a list of entities in the collection.

The following entity endpoints are available for each type of entity:
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

The namespace and activation endpoints support GET requests. The actions, triggers, rules, and packages endpoints support GET, PUT, and DELETE requests. The endpoints of actions, triggers, and rules also support POST requests, which are used to invoke actions and triggers and enable or disable rules. 

All APIs are protected with HTTP Basic authentication. 
You can use the [wskadmin](../tools/admin/wskadmin) tool to generate a new namespace and authentication.
The Basic authentication credentials are in the `AUTH` property in your `~/.wskprops` file, delimited by a colon. 
You can also retrieve these credentials by using the CLI running `wsk property get --auth`.


In the following example, the [cURL](https://curl.haxx.se) command tool is used to get the list of all packages in the `whisk.system` namespace:

```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

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

In this example, the authentication was passed by using the `-u` flag. You can also pass this value also as part of the URL, such as, `https://$AUTH@{APIHOST}`.

The OpenWhisk API supports request-response calls from web clients. OpenWhisk responds to `OPTIONS` requests with Cross-Origin Resource Sharing headers. Currently, all origins are allowed (that is, Access-Control-Allow-Origin is "`*`") and Access-Control-Allow-Headers yield Authorization and Content-Type.

**Attention:** Because OpenWhisk currently supports only one key per namespace, it is not recommended to use CORS beyond simple experiments. Use [Web Actions](webactions.md) or [API Gateway](apigateway.md) to expose your actions to the public and not use the OpenWhisk authorization key for client applications that require CORS.

## Using the CLI verbose mode
{: #openwhisk_rest_api_cli_v}

The OpenWhisk CLI is an interface to the OpenWhisk REST API.
You can run the CLI in verbose mode with the flag `-v`, which prints all the information about the HTTP request and response.

Display the namespace value for the current user by running the following command:
```
wsk namespace list -v
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
To create or update an action, send an HTTP request with method `PUT` on the actions collection. For example, to create a `nodejs:6` action with the name `hello` by using a single file content, use the following command:
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

To create an action as a web action, you need to add an [annotation](annotations.md) of `web-export=true` for web actions. Since web-actions are publicly accessible, you want to protect pre-defined parameters (that is, treat them as final) using the annotation `final=true`. If you create or update an action that uses the CLI flag `--web true`, the command adds both annotations `web-export=true` and `final=true`.

Run the following curl command to provide the complete list of annotations to set on the action.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}
You can now invoke this action as a public URL with no OpenWhisk authorization. Try to invoke by using the web action public URL, and including an extension like `.json` or `.http`, for example, at the end of the URL.
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}
```json
{
  "payload": "Hello John"
}
```
This example source code does not work with `.http`, see [web actions](webactions.md) documentation on how to modify.

## Sequences
{: #openwhisk_rest_api_sequences}

To create an action sequence, provide the names of the actions that compose the sequence in the desired order. So that the output from the first action is passed as input to the next action.

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

Create a sequence with the actions `/whisk.system/utils/split` and `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

When you specify the names of the actions, they must be full qualified.

## Triggers
{: #openwhisk_rest_api_triggers}

To create a trigger, the minimum information you need is a name for the trigger. You could also include default parameters that get passed to the action through a rule when the trigger gets fired.

Create a trigger with name `events` with a default parameter `type` with value `webhook` set.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Now whenever you have an event that needs to fire this trigger it just takes an HTTP request with a method `POST` by using the OpenWhisk Authorization key.

To fire the trigger `events` with a parameter `temperature`, send the following HTTP request.

```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Triggers with Feed Actions
{: #openwhisk_rest_api_triggers_feed}

Special triggers can be created by using a feed action. The feed action is an action that helps with the configuration of a feed provider that is in charge of firing the trigger whenever an event for the trigger occurs. Learn more about these feed providers in the [feeds.md] documentation.

Some of the available triggers that leverage a feed action are periodic/alarms, Slack, Github, Cloudant/Couchdb, and messageHub/Kafka. You also can create your own feed action and feed provider.

Create a trigger with name `periodic` to be fired at a specified frequency, every 2 hours (for example, 02:00:00, 04:00:00, ...).

Using the CLI, run the following command to create the trigger:
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

Since the `-v` flag is used, two HTTP requests are sent. One is to create a trigger that is called `periodic`, and the other is to invoke the feed action. The feed action, `/whisk.system/alarms/alarm`, is sent the parameters to configure the feed provider to fire the trigger every 2 hours.

To achieve this activity with the REST API, create the trigger first like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

As you can see, the annotation `feed` is stored in the trigger. Later, this annotation can be used to know which feed action to use to delete the trigger.

Now that the trigger is created, lets invoke the feed action
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

Deleting the trigger is similar to creating the trigger. So this time, delete the trigger by using the feed action in order to configure the feed provider to also delete the handler for the trigger.

Invoke the feed action to delete the trigger handler from the feed provider by running the following command:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

Now delete the trigger with an HTTP request by using the `DELETE` method:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Rules
{: #openwhisk_rest_api_rules}

To create a rule that associates a trigger with an action, send an HTTP request with a `PUT` method to provide the trigger and action in the body of the request.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

Rules can be enabled or disabled, and you can change the status of the rule by updating its status property. For example, to disable the rule `t2a` send in the body of the request `status: "inactive"` with a `POST` method.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Packages
{: #openwhisk_rest_api_packages}

To create an action in a package, you have to create a package first. Create a package with the name `iot` and send an HTTP request with a `PUT` method by running the following command:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Activations
{: #openwhisk_rest_api_activations}

To get the list of the last three activations, use an HTTP request with a `GET` method by passing the query parameter `limit=3` like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

To get all the details of an activation that include results and logs, send an HTTP request with a `GET` method by passing the activation identifier as a path parameter like so:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
