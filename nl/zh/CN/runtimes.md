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

# 运行时

## 操作运行时
操作可以使用多种编程语言（例如，Javascript、Python 等）进行编码和执行。以下各部分中说明了可用的运行时环境。

以下链接返回的 JSON 响应显示了每个区域中 IBM Cloud Functions 的可用运行时。

响应的 `runtimes` 部分包含可用运行时集。

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

`image` 部分包含 [DockerHub](https://hub.docker.com/) 上运行时映像的名称以及使用的标记。 

以下示例指向映像 `ibmfunct/action-nodejs-v10` 和 `openwhisk/nodejs6action`。标记可以是版本号（如 `1.9.0`），也可以是简短形式的 git 落实散列（如 `b99d71e`）。

示例 image 字段。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- 操作运行时会定期更新。这些更新包括对运行时内的包的安全修订和次版本更新。次版本更新可能会导致向后兼容性中断。运行时更新可能会影响操作。不会自动迁移到相同类型的更新运行时。
- 不推荐使用的运行时上的操作无法成功完成，直到将相应运行时更新为支持的运行时为止。对失败的操作进行故障诊断时，要确定运行时是否为不推荐使用的运行时，请在查询响应中检查是否有 `deprecated=true`。要更新运行时，请参阅[更改操作运行时](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)。

## JavaScript 运行时环境
{: #openwhisk_ref_javascript_environments}

JavaScript 操作可以使用 Node.js V8 或 V10 执行。 

Node.js V6 是缺省版本，但自 2018 年 12 月 6 日开始已不推荐使用。要继续使用 JavaScript 操作，请更新为 Node.js V8 或 V10。
{: deprecated}

### 带有 IBM SDK 的 Node.js V10 环境
{: #openwhisk_ref_javascript_environments_10}
如果在创建或更新操作时使用值 `nodejs:10` 显式指定 `--kind` 标志，那么将使用 Node.js V10 环境。

#### 从 `nodejs:8` 迁移到 `nodejs:10`
- `ibm_db` npm 包在 `nodejs:10` 中不可用。`ibm_db` 包不支持 Node.js 10。您可以在 [ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) 中跟踪此问题的进度。
- `cloudant` npm 包在 `nodejs:10` 中不可用，不推荐使用该包，在导入 nodejs 模块时，需要使用官方 npm 包 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) V3.0.0（即 `require('@cloudant/cloudant')`），此外 [V3.x 仅返回 Promise](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)。
- `cradle` npm 包在 `nodejs:10` 中不可用。
- `log4js` npm 包在 `nodejs:10` 中不可用。您可以在 [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) 上跟踪此问题。
- `watson-developer-cloud` npm 包在 `nodejs:10` 中不可用。您可以在 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780) 中跟踪此问题的新版本的进度。

有关 Nodejs V10 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md) 中找到。

### 带有 IBM SDK 的 Node.js V8 环境
{: #openwhisk_ref_javascript_environments_8}
如果在创建或更新操作时使用值 `nodejs:8` 显式指定 `--kind` 标志，那么将使用 Node.js V8 环境。

Node.js V8 现在处于维护方式，要到 2019 年 12 月才可用。请参阅 [Node.js 发行版计划](https://github.com/nodejs/Release)。
{: deprecated}
 
有关 Node.js V8 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md) 中找到。

### Node.js V6 环境（不推荐）
{: #openwhisk_ref_javascript_environments_6}
Node.js V6 是缺省版本，但已不推荐使用。要继续使用 JavaScript 操作，请更新为 Node.js V8 或 V10。
{: deprecated}

有关 Nodejs V6 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md) 中找到。

## Python 运行时环境
{: #openwhisk_ref_python_environments}

OpenWhisk 支持使用两种不同的运行时版本来运行 Python 操作。

### Python 3.7 操作（基于 Debian Stretch）
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 操作使用 Python 3.7.x 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind python:3.7`。


除了 Python 3.7 标准库外，该运行时还包含 IBM Cloud 服务的 SDK 包，可供 Python 操作使用。

有关 Python 3.7 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md) 中找到。

### Python 3.6 操作（基于 Debian Jessie）
{: #openwhisk_ref_python_environments_3.6}

Python 3 操作使用 Python 3.6.x 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind python:3.6`。


除了 Python 3.6 标准库外，该运行时还包含 IBM Cloud 服务的 SDK 包，可供 Python 操作使用。

有关 Python 3.6 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md) 中找到。

### Python 2 操作

Python 2 操作使用 Python 2.7.15 执行，除非在创建或更新操作时指定 `--kind` 标志。要显式选择此运行时，请使用 `--kind python:2`。

使用 virtualenv 创建 Python 操作时，请使用 Docker 映像 `openwhisk/python2action`。
除了 Python 2.7 标准库外，以下包也可供 Python 2 操作使用。

有关 Python 2 运行时环境的详细信息可在 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md) 中找到。

## Swift 操作
{: #swift-actions}

Swift 3.1.1 和 4.1 运行时已不推荐使用，到 2019 年 2 月 28 日即不再可用。请使用 kind `swift:4.2` 和新的编译过程来启动任何新的操作，或将任何现有操作迁移到 Swift 4.2 运行时。
{: tip}

### Swift 3
Swift 3 操作使用 Swift 3.1.1 `--kind swift:3.1.1` 执行。请始终指定 kind `swift:3.1.1`，因为先前的 Swift 版本不受支持。


必须迁移所有 Swift 操作才可使用 kind `swift:3.1.1`。作为最佳实践，请在创建或更新操作时始终提供该特定 kind。
{: tip}

在使用单个 Swift 源文件时，Swift 3.1.1 操作可以使用以下包：
- KituraNet V1.7.6：https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON V15.0.1：https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK V0.16.0：https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 操作可以使用 Swift 4.1 或 4.2（分别使用 `--kind swift:4.1` 或 `--kind swift:4.2` 指定）执行。缺省 `--kind swift:default` 为 Swift 4.2。

Swift 4.x 操作运行时未嵌入任何包，请遵循[打包的 Swift 操作](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable)的指示信息以使用 Package.swift 来包含依赖项。

在使用单个 Swift 源文件时，Swift 4.1 操作可以使用以下包：
- Watson Developer Cloud SDK V0.38.1：https://github.com/watson-developer-cloud/swift-sdk

在使用单个 Swift 源文件时，Swift 4.2 操作可以使用以下包：
- Watson Developer Cloud SDK V1.2.0：https://github.com/watson-developer-cloud/swift-sdk

### 将 Swift 3.1.1 迁移到 Swift 4.1

#### 使用单个源操作文件的 SwiftyJSON
如果您具有未编译的 `swift:3.1.1` 操作，并且使用的是 **SwftyJSON** 包，那么您需要预编译操作，并指定要用于 `swift:4.2` kind 操作的 SwiftyJSON 版本。请注意，从 Swift 4.1 开始，改进了对 JSON 数据的管理。

## PHP 操作
{: #openwhisk_ref_php}

自 2019 年 1 月 11 日开始，不推荐使用 PHP 7.1 和 7.2。要继续使用 PHP 操作，请更新为 PHP 7.3。
{: deprecated}

PHP 操作使用 PHP 7.3.0 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind php:7.3`。这是使用扩展名为 `.php` 的文件创建操作时的缺省行为。

不推荐使用 PHP 7.1 和 7.2 运行时。请将所有操作迁移到 PHP 7.3，以缩短等待时间，加快端到端运行速度。

除了标准扩展名外，还可使用以下 PHP 扩展名：

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

## Docker 操作
{: #openwhisk_ref_docker}

Docker 操作在 Docker 容器中运行用户提供的二进制文件。该二进制文件在基于 [python:3.6-alpine](https://hub.docker.com/r/library/python) 的 Docker 映像中运行，所以该二进制文件必须与此分发版兼容。

通过 Docker 框架，可以方便地构建兼容 OpenWhisk 的 Docker 映像。可以使用 `ibmcloud fn sdk install docker` CLI 插件命令来安装该框架。

主二进制程序必须位于容器内的 `/action/exec` 中。可执行文件通过可以反序列化为 `JSON` 对象的单个命令行自变量字符串来接收输入自变量。该文件必须使用 `stdout` 以单行序列化 `JSON` 字符串形式返回结果。

您可以通过修改 `dockerSkeleton` 中包含的 `Dockerfile` 来包含任何编译步骤或依赖关系。
