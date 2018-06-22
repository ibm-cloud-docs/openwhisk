---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotations
{: #openwhisk_annotations}

{{site.data.keyword.openwhisk}} actions, triggers, rules, and packages (collectively referred to as assets) can be decorated with `annotations`. Annotations are attached to assets just like parameters with a `key` that defines a name and `value` that defines the value. It is convenient to set them from the command line interface (CLI) by using the `--annotation` flag or `-a` for short.
{: shortdesc}

Rationale: Annotations were added to {{site.data.keyword.openWhisk_short}} to allow for experimentation without changing the underlying asset schema. Until the writing of this document, there was deliberate effort to not define what `annotations` are permitted. However, as annotations are more heavily used to impart semantic changes, it's important to start documenting them.

The most prevalent use of annotations to date is to document actions and packages. Many of the packages in the {{site.data.keyword.openwhisk_short}} catalog carry annotations, such as: a description of the functionality that is offered by its actions, parameters to use at package binding time, invoke-time parameters, or if a parameter is a "secret" (for example, password) or not. Annotations are invented as needed, for example, to allow for UI integration.

Here is a sample set of annotations for an `echo` action, which returns its input arguments unmodified (for example, `function main(args) { return args }`). This action is useful for logging input parameters, for example, as part of a sequence or rule.
```
ibmcloud wsk action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

Annotations that describe **packages** include:

- `description`: A pithy description of the package.
- `parameters`: An array that describes parameters that are scoped to the package.

Annotations that describe **actions** include:

- `description`: A pithy description of the action.
- `parameters`: An array that describes actions that are required to execute the action.
- `sampleInput`: An example that shows the input schema with typical values.
- `sampleOutput`: An example that shows the output schema, usually for the `sampleInput`.

Annotations that describe **parameters** include:

- `name`: The name of the parameter.
- `description`: A pithy description of the parameter.
- `doclink`: A link to further documentation for the parameter (useful for OAuth tokens).
- `required`: True for required parameters and false for optional ones.
- `bindTime`: True if the parameter is specified when a package is bound.
- `type`: The type of the parameter, one of `password`, `array` (but can be used more broadly).

The annotations are _not_ checked. So while it is conceivable to use the annotations to infer if a composition of two actions into a sequence is legal, for example, the system does not yet do that.

## Annotations specific to web actions
{: #annotations-specific-to-web-actions}

Recently, the core API was extended with new features. To enable packages and actions to participate in these features, new annotations are introduced that are semantically meaningful. These annotations must be explicitly set to `true` to have effect. Changing the value from `true` to `false` excludes the attached asset from the new API. The annotations have no meaning otherwise in the system. See the following annotations:

- `web-export`: Applies only to an action. If present, it makes its corresponding action accessible to REST calls _without_ authentication. These are called [_web actions_](openwhisk_webactions.html) because they allow one to use OpenWhisk actions from a browser for example. It is important to note that the _owner_ of the web action incurs the cost of running them in the system. In other words, the _owner_ of the action also owns the activations record.
- `final`: Applies only to an action. It makes all of the action parameters that are already defined immutable. A parameter of an action that carries the annotation cannot be overridden by invoke-time parameters once the parameters value is defined through its enclosing package or the action definition.
- `raw-http`: Applies only to an action in the presence of a `web-export` annotation. If present, the HTTP request query and body parameters are passed to the action as reserved properties.
- `web-custom-options`: When set, this annotation enables a web action to respond to OPTIONS requests with customized headers, otherwise a [default CORS response](openwhisk_webactions.html#options-requests) applies.
- `require-whisk-auth`: This annotation protects the web action so that it is only invoked by requests that provide appropriate authentication credentials. When set to a boolean value, it controls whether or not the request's Basic Authentication value (i.e. Whisk auth key) will be authenticated. A value of `true` authenticates the credentials, and a value of `false` invokes the action without any authentication. When set to a number or a string, this value must match the request's `X-Require-Whisk-Auth` header value. In both cases, it is important to note that the _owner_ of the web action incurs the cost of running them in the system (i.e., the _owner_ of the action also owns the activations record).

## Annotations specific to activations

The system can decorate activation records with the following annotations:

- `path`: the fully qualified path name of the action that generated the activation. Note that if this activation was the result of an action in a package binding, the path refers to the parent package.
- `kind`: the kind of action executed, and one of the support OpenWhisk runtime kinds.
- `limits`: the time, memory and log limits that this activation were subject to.

For sequence related activations, the system generates the following annotations:

- `topmost`: this is only present for an outermost sequence action.
- `causedBy`: this is only present for actions that are contained in a sequence.

Lastly, and in order to provide performance transparency, activations also record:

- `waitTime`: the time spent waiting in the internal OpenWhisk system. This is roughly the time spent between the controller receiving the activation request and when the invoker provisioned a container for the action. This value is currently only present for non-sequence related activations. For sequences, this information can be derived from the `topmost` sequence activation record.
- `initTime`: the time spent initializing the function. If this value is present, the action required initialization and represents a cold start. A warm activation skips initialization, and in this case, the annotation is not generated.

An example of these annotations as they would appear in an activation record is shown below.

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
