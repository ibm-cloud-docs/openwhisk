---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Runtimes

## Action runtimes
Actions can be coded and executed in several programming languages (e.g. Javascript, Python, etc.). The available runtime environments are shown in the following sections.

The following links return a JSON response that shows the available runtimes for IBM Cloud Functions in each region.

The `runtimes` section of the response contains the set of available runtimes.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

The `image` section contains the name of the runtime image on [DockerHub](https://hub.docker.com/) and the tag that is used. 

The following examples point to the images `ibmfunctions/action-nodejs-v10` and `openwhisk/nodejs6action`.
The tags can be version numbers like `1.9.0` or the short form of a git commit hash, like `b99d71e`.

Example image fields.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- Action runtimes are updated on a regular basis. These updates include security fixes and minor version updates to the packages inside the runtimes. Minor version updates might introduce backward compatibility breaks. Runtime updates might impact your actions. There is no automatic migration to a newer runtime of the same type.
- Actions that on deprecated runtimes cannot be completed successfully until the runtime is updated to a supported one. When troubleshooting a failing action, to identify whether a runtime is deprecated, check for `deprecated=true` in the query response. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)

## JavaScript runtime environments
{: #openwhisk_ref_javascript_environments}

JavaScript actions can be executed in Node.js version 8 or 10. 

Node.js version 6 is the default version, but is deprecated as of 6 December 2018. To continue using a JavaScript action, update to Node.js version 8 or 10.
{: deprecated}

### Node.js version 10 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_10}
The Node.js version 10 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:10` when creating or updating an action.

#### Migrating from `nodejs:8` to `nodejs:10`
- The `ibm_db` npm package is not available in `nodejs:10`. The `ibm_db` package doesn't support Node.js 10. You can track progress in this issue [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- The `cloudant` npm package is not available in `nodejs:10`, the package is deprecated, you need to use the official npm package [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 when importing the nodejs module (i.e `require('@cloudant/cloudant')`) also [v3.x only returns Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- The `cradle` npm package is not available in `nodejs:10`.
- The `log4js` npm package is not available in `nodejs:10`. You can track the issue at [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- The `watson-developer-cloud` npm package is not available in `nodejs:10`. You can track progress on the new version in this  issue [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Detailed information about the nodejs version 10 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

### Node.js version 8 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
The Node.js version 8 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:8` when creating or updating an action.

Node.js version 8 is in maintenance mode and is available until December 2019. See the [Node.js release schedule](https://github.com/nodejs/Release).
{: deprecated}
 
Detailed information about the Node.js version 8 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).

### Node.js version 6 environment (deprecated)
{: #openwhisk_ref_javascript_environments_6}
Node.js version 6 is the default version, but is deprecated. To continue using a JavaScript action, update to Node.js version 8 or 10.
{: deprecated}

Detailed information about the nodejs version 6 runtime environment can be found in the [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md).

## Python runtime environments
{: #openwhisk_ref_python_environments}

OpenWhisk supports running Python actions by using two different runtime versions.

### Python 3.7 actions (Debian Stretch based)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 actions are executed with Python 3.7.x. To use this runtime, specify the `wsk` CLI parameter `--kind python:3.7` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries.

Detailed information about the Python 3.7 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Python 3.6 actions (Debian Jessie based)
{: #openwhisk_ref_python_environments_3.6}

Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the `wsk` CLI parameter `--kind python:3.6` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries.

Detailed information about the Python 3.6 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Python 2 actions

Python 2 actions are executed with Python 2.7.15 unless you specify the `--kind` flag when you create or update an action. To explicitly select this runtime, use `--kind python:2`.

When creating python actions using virtualenv, use the docker image `openwhisk/python2action`.
The following packages are available for use by Python 2 actions, in addition to the Python 2.7 standard library.

Detailed information about the Python 2 runtime environment can be found in the [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).

## Swift actions
{: #swift-actions}

Swift 3.1.1 and 4.1 runtimes are deprecated and are available until 28 February 2019.
Start any new actions or migrate any existing actions to Swift 4.2 runtime using the kind `swift:4.2` and new compile process.
{: tip}

### Swift 3
Swift 3 actions are executed with Swift 3.1.1 `--kind swift:3.1.1`. Always specify kind `swift:3.1.1` as previous versions of Swift are unsupported.

You must migrate all Swift actions to use kind `swift:3.1.1`. As a best practice, always provide the specific kind when you create or update actions.
{: tip}

Swift 3.1.1 actions can use the following packages when using a single Swift source file:
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 actions can be executed using Swift 4.1 or 4.2 using `--kind swift:4.1` or `--kind swift:4.2` respectively.
The default `--kind swift:default` is Swift 4.2.

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged swift actions](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable) to include dependencies using a Package.swift.

Swift 4.1 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Swift 4.2 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Migrating Swift 3.1.1 to Swift 4.1

#### SwiftyJSON using a single source action file
If you have a `swift:3.1.1` action that is not compiled, and uses the **SwiftyJSON** package, you need to pre-compile your action, and specify the version of SwiftyJSON you want to use for `swift:4.2` kind action. Take into account that starting with Swift 4.1, there are improvements to managing JSON data.

## PHP actions
{: #openwhisk_ref_php}

PHP 7.1 and 7.2 are deprecated as of 11 January 2019. To continue using a PHP action, update to PHP 7.3.
{: deprecated}

PHP actions are executed with PHP 7.3.0. To use this runtime, specify the `wsk` CLI parameter `--kind php:7.3` when you create or update an action. This behavior is the default when you create an action with a file that has a `.php` extension.

The PHP 7.1 and 7.2 runtime are deprecated. Migrate all actions to PHP 7.3 for better latency and faster runs end to end.

The following PHP extensions are available in addition to the standard ones:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Docker actions
{: #openwhisk_ref_docker}

Docker actions run a user-supplied binary in a Docker container. The binary runs in a Docker image based on [python:3.6-alpine](https://hub.docker.com/r/library/python), so the binary must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `ibmcloud fn sdk install docker` CLI plug-in command.

The main binary program must be located in `/action/exec` inside the container. The executable receives the input arguments from a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result by using `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.
