---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-14"

keywords: deploying actions, manifest, manifest file, functions, openwhisk, API

subcollection: openwhisk

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Deploying entities with a manifest file
{: #deploy}

You can use {{site.data.keyword.openwhisk}} to describe and deploy all of your namespace entities by using a manifest file that is written in YAML. You can use this file to deploy all your Functions [Packages](/docs/openwhisk?topic=openwhisk-pkg_ov), [Actions](/docs/openwhisk?topic=openwhisk-actions), [Triggers](/docs/openwhisk?topic=openwhisk-triggers), and [Rules](/docs/openwhisk?topic=openwhisk-rules) with a single command.

The manifest file describes the set of entities you would like to deploy and undeploy as a group. The manifest file contents must adhere to the [OpenWhisk deployment YAML specification](https://github.com/apache/openwhisk-wskdeploy/tree/master/specification#package-specification){: external}. Once defined, you can use your manifest file to deploy or redeploy a group of Functions entities into the same or different Functions namespace. You can use the Functions plug-in commands `ibmcloud fn deploy` and `ibmcloud fn undeploy` to deploy and undeploy the Functions entities that are defined in your manifest file.

## Creating the Hello World API example
{: #deploy_helloworld_example}

This example takes some simple Node.js code `helloworld.js`, creates a web action `hello_world` inside a package `hello_world_package` and defines a REST API for this action.
{: shortdesc}

1. Create a `helloworld.js` file with the following code.

    ```javascript
    function main() {
       return {body: 'Hello world'};
    }
    ```
    {: codeblock}

    The deployment manifest file defines the following variables.
    * The package name.
    * The action name.
    * The action annotation that indicates it is to be a web action.
    * The action code file name.
    * The API with a base path of `/hello`.
    * The endpoint path of `/world`.

2. Create the `hello_world_manifest.yml` file.

    ```yaml
    packages:
      hello_world_package:
        version: 1.0
        license: Apache-2.0
        actions:
          hello_world:
            function: helloworld.js
            web-export: true
        apis:
          hello-world:
            hello:
              world:
                hello_world:
                  method: GET
                  response: http
    ```
    {: codeblock}

3. Use the **`deploy`** command to deploy the package, action, and API.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. You can list the actions, packages, and APIs to confirm that the three expected entities were created successfully.

    1. List the actions by using the following command.

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. List the packages by using the following command.

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. List the APIs by using the following command.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. Invoke the API.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

Optional: You can undeploy the same entities by using the `undeploy` command.

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## More OpenWhisk deployment examples
{: more_deploy_examples}

The Functions deployment is based on the OpenWhisk deployment project, which has [multiple deployment manifest examples](https://github.com/apache/openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples){: external} that can be used within Functions.  You can use the `ibmcloud fn deploy` command instead of `wskdeploy`.

## Deployment manifest specification
{: manifest_specification}

Functions deployment manifests must adhere to the OpenWhisk deployment manifest specification. Refer to the [OpenWhisk deployment manifest specification documentation](https://github.com/apache/openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification){: external} for details.
