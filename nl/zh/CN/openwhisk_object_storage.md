---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# （试验性）Object Storage 事件源
{: #cloud_object_storage}

`/whisk.system/cos-experimental` 包可能不稳定，可能会经常发生改变而与早期版本不兼容，还可能被临时通知停用。建议不要在生产环境中使用此包。此试验性包目前仅在美国南部区域可用。
{: important}

在此示例中，您将了解如何执行以下操作： 
* [侦听更改](#listening_to_cos_bucket_changes)，即对 {{site.data.keyword.cos_full}} 实例的更改。
* [创建触发器](#creating_a_trigger_cos)，用于响应这些更改。
* [创建操作](#creating_action_to_process_object)，用于检索和处理更改。
* [创建规则](#associating_action_with_change_trigger)，用于将操作关联到更改触发器。
<br>

**样本用例：**通过 `/whisk.system/cos-experimental` 包，可以侦听对存储在 {{site.data.keyword.cos_full_notm}} 存储区中的 GPS 街道数据的更改。然后，在发生更改时，可以触发自动重新生成 GPS 地图，以便用户可以使用其 GPS 应用程序访问最新的街道数据。

## 关于 IBM Cloud Object Storage
{: #cloud_object_storage_info}

**开始之前：**要了解有关 {{site.data.keyword.cos_full_notm}} 的信息，请参阅[关于 Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage)。有关设置 {{site.data.keyword.cos_full_notm}} 实例的更多信息，请参阅[供应 {{site.data.keyword.cos_short}} 实例](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage)。

## 侦听对 IBM Cloud Object Storage 存储区的更改
{: #listening_to_cos_bucket_changes}

可以使用 {{site.data.keyword.openwhisk}} 来侦听对 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) 存储区的更改，并使用操作处理存储区中的一个或多个对象。 

`/whisk.system/cos-experimental` 包支持您配置 {{site.data.keyword.cos_full_notm}} 实例中的事件，并包含以下订阅源：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/cos-experimental`|包|apikey、auth_endpoint、bucket、endpoint、interval|包含 `changes` 订阅源操作的包。|
|`/whisk.system/cos-experimental/changes`|订阅源|apikey、auth_endpoint、bucket、endpoint、interval|对 {{site.data.keyword.cos_full_notm}} 存储区进行更改时触发触发器事件。|
{: shortdesc}

可以使用 `changes` 订阅源来配置 {{site.data.keyword.cos_full_notm}} 事件源服务，以在每次对 {{site.data.keyword.cos_full_notm}} 实例中的存储区进行更改时都触发触发器。

此示例中使用的参数：

**apikey**：_（必需，除非绑定到包）_。`apikey` 参数是 {{site.data.keyword.cos_full_notm}} 实例的 IAM API 密钥。通常，此值会绑定到包。但是，如果使用 `changes` 订阅源操作时指定了 `apikey` 值，那么对于凭证，将使用指定的值，而不使用绑定凭证的 API 密钥。

**auth_endpoint**：_（可选）_。`auth_endpoint` 参数是 {{site.data.keyword.cos_full_notm}} 用于通过 `apikey` 生成令牌的授权端点。缺省端点为 {{site.data.keyword.Bluemix}} 端点。

**bucket**：_（必需）_。`bucket` 参数是 {{site.data.keyword.cos_full_notm}} 存储区的名称。

**endpoint**：_（必需）_。`endpoint` 参数是用于连接到 {{site.data.keyword.cos_full_notm}} 实例的 {{site.data.keyword.cos_full_notm}} 端点。可以在 [{{site.data.keyword.cos_full_notm}} 文档](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints)中找到您的端点。

**interval**：_（可选）_。`interval` 参数是存储区轮询时间间隔，以整数分钟为单位。 `interval` 值必须至少为 1 分钟，并且缺省情况下设置为 1 分钟。

## 创建触发器，用于响应 changes 订阅源
{: #creating_a_trigger_cos}

创建触发器时，可以通过将 {{site.data.keyword.cos_full_notm}} 凭证直接绑定到 `cos-experimental` 包来避免将这些凭证传递到 `changes` 订阅源操作。
 {: shortdesc}
 
 1. 首先，创建可以修改以包含凭证的包绑定。以下命令将在名称空间中创建包绑定 `myCosPkg`。
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. 将 {{site.data.keyword.cos_short}} 凭证绑定到包。
 将 {{site.data.keyword.cos_short}} 凭证绑定到包会将 `apikey` 值绑定到包，以便在调用 `changes` 订阅源操作时不需要指定 `apikey` 值。 
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 使用创建的包绑定中的 `changes` 订阅源来创建名为 `myCosTrigger` 的触发器。请使用您的存储区名称和 {{site.data.keyword.cos_short}} 端点参数值。
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
示例输出：
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. 开始对激活进行轮询，以清楚地了解发生的情况。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. 创建用于观察 changes 订阅源的操作。例如，名为 `showCosChange` 的操作，此操作包含以下 JavaScript 代码：
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
 6. 创建规则，用于将 `showCosChange` 操作连接到 `myCosTrigger` 触发器：
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. 在 {{site.data.keyword.cos_short}} 仪表板中，修改现有存储区对象或创建存储区对象。要了解如何向存储区添加对象，请参阅[向存储区添加一些对象](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects)。
 
 8. 对于每个存储区对象更改，请观察 `myCosTrigger` 触发器和 `showCosChange` 操作的新激活。这些激活将在配置的存储区轮询时间间隔内显示。

如果观察不到新的激活，请验证 `apikey`、`endpoint` 和 `bucket` 参数值是否正确。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### 触发器事件的数据结构
{: #data_structure_trigger_event}

生成的事件的内容具有以下参数：

  - `file`：文件或对象元数据。
  - `status`：检测到的更改。此值为 `added`、`modified` 或 `deleted`。
  - `bucket`：{{site.data.keyword.cos_short}} 存储区的名称。
  - `endpoint`：用于连接到 {{site.data.keyword.cos_short}} 实例的 {{site.data.keyword.cos_short}} 端点。
  - `key`：更改的存储区对象的标识。此值与 `file.Key` 相同，但会显示在触发器事件 JSON 的顶部。

存储区更改触发器事件的示例 JSON 表示：
```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## 创建用于处理已更改对象的操作
{: #creating_action_to_process_object}

可以创建用于检索和处理对象的单个操作。或者，可以创建一个序列，其中使用一个操作来检索对象，另一个操作来处理对象。

### 创建用于检索和处理对象的操作
{: #creating_action_to_retrieve_object}

此样本操作代码会检索并处理存储区更改通知文档。在手动操作调用期间，可以直接将 `apikey` 和 `serviceInstanceId` 参数传递到操作，但当触发器调用此操作时，这些值必须从 {{site.data.keyword.cos_short}} 中获取，Object Storage 必须使用 `ibmcloud fn service bind` 命令绑定到此操作。

示例代码：

```javascript
const COS = require('ibm-cos-sdk')

function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

  const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

  return new Promise(function(resolve, reject) {
       client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
      if (err != null) {
        console.log(err)
        reject({ err: err })
      } else {
console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
    })
  });
}
```
{: codeblock}

由于此操作使用 `ibm-cos-sdk` npm 包，因此该操作必须打包为 [Node.js 模块](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action)或[单个捆绑软件](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action)。

将此操作打包成 .zip 文件后，创建用于通过 {{site.data.keyword.cos_short}} 检索和处理对象的操作：

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

针对此操作，[绑定](#cos_binding_credentials_to_action) {{site.data.keyword.cos_short}} 凭证。然后，[创建规则](#associating_action_with_change_trigger)，用于在触发器触发时调用此操作。

### 创建用于检索和处理对象的操作序列

无需在操作中包含对象检索代码，而是可以使用 {{site.data.keyword.cos_short}} 包（必须[手动安装](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation)）中的 `object-read` 操作。操作代码仅需要处理从 `object-read` 返回的结果。

仅处理存储区对象的操作的示例代码：
```javascript
  function main(data) {
    if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. 创建用于仅处理 {{site.data.keyword.cos_short}} 中对象的操作：
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. 将 {{site.data.keyword.cos_short}} 凭证绑定到 `cos-experimental` 包绑定。
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. `object-read` 操作可与 `myCosProcessObjectAction` 一起编写以创建操作序列。
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

除了 `object-read` 操作外，还可以使用可安装的 {{site.data.keyword.cos_short}} 包中包含的其他操作。

针对此操作，[绑定](#cos_binding_credentials_to_action) {{site.data.keyword.cos_short}} 凭证。然后，[创建规则](#associating_action_with_change_trigger)，用于在触发器触发时调用此操作。

 ## 将凭证绑定到操作
 {: #cos_binding_credentials_to_action}
 
 通过以下命令将 {{site.data.keyword.cos_short}} 凭证绑定到操作，可以避免在调用期间传递敏感凭证：
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## 创建规则以将操作与更改触发器相关联
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

可以使用[规则](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use)中的操作或操作序列来访存和处理与 {{site.data.keyword.cos_short}} 更改事件关联的对象。

创建规则，用于在发生新的 {{site.data.keyword.cos_short}} 触发器事件时激活 `MyCosAction` 操作。
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



