---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-11"

keywords: annotations, annotate, package, parameters, actions

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


# Annotations
{: #annotations}

{{site.data.keyword.openwhisk}} actions, triggers, rules, and packages (collectively referred to as entities) can include annotations.
{: shortdesc}

Annotations are attached to entities like parameters. Annotations consist of a `key` that defines a name and `value` that defines the value. Annotations are most commonly used to document actions and packages. Packages in the {{site.data.keyword.openwhisk_short}} catalog carry annotations. These annotations include descriptions of the functionality that is offered by its actions, parameters to use at package binding time, invoke-time parameters, or whether a parameter is a secret. Annotations are invented as needed, for example, to allow for UI integration.

You can document an entity from the CLI by using the `--annotation` or `-a` flag.

## Action annotations
{: #annotations_action}

| Annotation | Description |
| --- | --- |
| `description` | A description of the action. |
| `parameters` | An array that describes actions that are required to execute the action. |
| `sampleInput` | An example that shows the input schema with typical values. |
| `sampleOutput` | An example that shows the output schema, usually for the `sampleInput`. |



The following code is an example set of annotations for an `echo` action, which returns its input arguments unmodified. This action is useful for logging input parameters, for example, as part of a sequence or rule.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Web action annotations
{: #annotations-specific-to-web-actions}

The following web action annotations must be explicitly set to `true` to enable API interactivity.

| Annotation | Description |
| --- | --- | 
| `web-export` | When applied to an action, the action becomes a [web action](/docs/openwhisk?topic=cloud-functions-actions_web). The action becomes accessible to REST calls without authentication so that users can access the actions from a browser. The owner of the web action incurs the cost of running them. In other words, the owner of the action also owns the activations record. |
| `final` | When applied to an action, any action parameters that were previously defined can't be overridden by parameters that are provided during invocation. |
| `raw-http` | When applied to an action that has the `web-export` annotation, the HTTP request query and body parameters are passed to the action as reserved properties. |
| `web-custom-options` | Enables a web action to respond to OPTIONS requests with customized headers. Otherwise, a [default CORS response](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options) applies. |
| `require-whisk-auth` | The web action can be invoked only by requests that provide appropriate authentication credentials. When set to a boolean value, it controls whether the request's Basic Authentication value is authenticated. A value of `true` authenticates the credentials, and a value of `false` invokes the action without any authentication. When set to an integer or a string, this value must match the request's `X-Require-Whisk-Auth` header value. |

## Package annotations
{: #annotations_package}

| Annotation | Description |
| --- | --- |
| `description` | A description of the package. |
| `parameters` | An array that describes parameters that are scoped to the package. |

## Parameter annotations
{: #annotations_parameter}

| Annotation | Description |
| --- | --- |
| `name` | The name of the parameter. |
| `description` | A pithy description of the parameter. |
| `doclink` | A link to further documentation for the parameter (useful for OAuth tokens). |
| `required` | True for required parameters and false for optional ones. |
| `bindTime` | True if the parameter is specified when a package is bound. |
| `type` | The type of the parameter, one of `password`, `array` (but can be used more broadly). |

## Activation annotations
{: #annotations_activation}

You can document activation records with the following annotations:

| Annotation | Description |
| --- | --- |
| `path` | The fully qualified path name of the action that generated the activation. If this activation was the result of an action in a package binding, the path refers to the parent package. |
| `kind` | The kind of action executed, and one of the supported {{site.data.keyword.openwhisk_short}} runtime kinds. |
| `limits` | The time, memory, and log limits that this activation were subject to. |

For sequence-related activations, the system generates the following annotations:

| Annotation | Description |
| --- | --- |
| `topmost` | This annotation is only present for an outermost sequence action. |
| `causedBy` | This annotation is only present for actions that are contained in a sequence. |
| `waitTime` | The time that is spent waiting in the internal {{site.data.keyword.openwhisk_short}} system. This time is roughly the time that is spent between the receipt of the activation request and when the invoker provisioned a container for the action. This value is only present for non-sequence related activations. For sequences, this information can be derived from the `topmost` sequence activation record. |
| `initTime` | The time that is spent initializing the function. If this value is present, the action required initialization and represents a cold start. A warm activation skips initialization, and in this case, the annotation is not generated. |

The following example shows these annotations as they might appear in an activation record:

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}

