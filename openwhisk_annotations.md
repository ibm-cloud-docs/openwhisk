---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotations on OpenWhisk assets

OpenWhisk actions, triggers, rules, and packages (collectively referred to as assets) can be decorated with `annotations`. Annotations are attached to assets just like parameters with a `key` that defines a name and `value` that defines the value. It is convenient to set them from the command line interface (CLI) via `--annotation` or `-a` for short.
{: shortdesc}

Rationale: Annotations were added to OpenWhisk to allow for experimentation without changing the underlying asset schema. Until the writing of this document, there was deliberate effort to not define what `annotations` are permitted. However, as annotations are more heavily used to impart semantic changes, it's important to start documenting them.

The most prevalent use of annotations to date is to document actions and packages. Many of the packages in the OpenWhisk catalog carry annotations, such as: a description of the functionality that is offered by its actions, parameters to use at package binding time, invoke-time parameters, or if a parameter is a "secret" (for example, password) or not. Annotations are invented as needed, for example, to allow for UI integration.

Here is a sample set of annotations for an `echo` action, which returns its input arguments unmodified (for example, `function main(args) { return args }`). This action is useful for logging input parameters, for example, as part of a sequence or rule.

```
wsk action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

Annotations that describe packages include:

- `description`: A pithy description of the package.
- `parameters`: An array that describes parameters that are scoped to the package.

Annotations that describe actions include:

- `description`: A pithy description of the action.
- `parameters`: An array that describes actions that are required to execute the action.
- `sampleInput`: An example that shows the input schema with typical values.
- `sampleOutput`: An example that shows the output schema, usually for the `sampleInput`.

Annotations that describe parameters include:

- `name`: The name of the parameter.
- `description`: A pithy description of the parameter.
- `doclink`: A link to further documentation for the parameter (useful for OAuth tokens).
- `required`: True for required parameters and false for optional ones.
- `bindTime`: True if the parameter is specified when a package is bound.
- `type`: The type of the parameter, one of `password`, `array` (but can be used more broadly).

The annotations are _not_ checked. So while it is conceivable to use the annotations to infer if a composition of two actions into a sequence is legal, for example, the system does not yet do that.

## Annotations specific to web actions
{: #openwhisk_annotations_webactions}

Recently, the core API was extended with new features. To enable packages and actions to participate in these features, new annotations are introduced that are semantically meaningful. These annotations must be explicitly set to `true` to have effect. Changing the value from `true` to `false` excludes the attached asset from the new API. The annotations have no meaning otherwise in the system. See the following annotations:

- `final`: Applies only to an action. It makes all of the action parameters that are already defined immutable. A parameter of an action that carries the annotation cannot be overridden by invoke-time parameters once the parameters value is defined through its enclosing package or the action definition.
- `web-export`: Applies only to an action. If present, it makes its corresponding action accessible to REST calls _without_ authentication. These are called [_web actions_](openwhisk_webactions.html) because they allow one to use OpenWhisk actions from a browser for example. It is important to note that the _owner_ of the web action incurs the cost of running them in the system. In other words, the _owner_ of the action also owns the activations record.
- `require-whisk-auth`: Applies onto to an action. If an action carries the `web-export` annotation, and this annotation is also `true`, the route is only accessible to an authenticated subject. It is important to note that the _owner_ of the web action incurs the cost of running them in the system. In other words, the _owner_ of the action also owns the activations record.
- `raw-http`: Applies only to an action in the presence of a `web-export` annotation. If present, the HTTP request query and body parameters are passed to the action as reserved properties.

