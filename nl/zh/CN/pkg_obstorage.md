---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

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

# Object Storage
{: #pkg_obstorage}

您可以通过与 {{site.data.keyword.cos_full}} 实例集成来扩展 {{site.data.keyword.openwhisk}} 应用程序的功能。

**开始之前：**要了解有关 {{site.data.keyword.cos_full_notm}} 的信息，请参阅[关于 Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api)。有关设置 {{site.data.keyword.cos_full_notm}} 实例的更多信息，请参阅[供应 {{site.data.keyword.cos_full_notm}} 实例](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision)。

## 包
{: #obstorage_packages}
|包|可用性|描述|
| --- | --- | --- |
|[{{site.data.keyword.cos_full_notm}} 包](#pkg_obstorage)|可安装|在 {{site.data.keyword.cos_full_notm}} 实例中执行读取、写入和删除操作。|
|[（试验性）{{site.data.keyword.cos_full_notm}} 事件源](#pkg_obstorage_ev)|预安装（仅限美国南部）|侦听对 {{site.data.keyword.cos_full_notm}} 实例的更改。|

## 创建 IBM Cloud Object Storage 服务实例
{: #pkg_obstorage_service}

使用任一包之前，必须请求 {{site.data.keyword.cos_full_notm}} 的实例，并至少创建一个存储区。

1. [创建 {{site.data.keyword.cos_full_notm}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision)。

2. 为 {{site.data.keyword.cos_full_notm}} 服务实例[创建一组 HMAC 服务凭证 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。在**添加内联配置参数（可选）**字段中，添加 `{"HMAC":true}`。

3. [至少创建一个存储区 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets)。

## 使用 {{site.data.keyword.cos_full_notm}} 包读取和写入存储区
{: #pkg_obstorage_install}

具有 {{site.data.keyword.cos_full_notm}} 服务实例后，可以使用 {{site.data.keyword.openwhisk}} CLI 或 UI 将 {{site.data.keyword.cos_full_notm}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #pkg_obstorage_cli}

开始之前：

[安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.cos_full_notm}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.cos_full_notm}} 包存储库。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 导航至 `runtimes/nodejs` 或 `runtimes/python` 目录。{{site.data.keyword.cos_full_notm}} 包中的操作会部署在您选择的运行时中。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 部署包。可以重复先前的步骤以在其他运行时中重新部署包。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 验证 `cloud-object-storage` 包是否已添加到包列表中。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    输出：
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. 将所创建的 {{site.data.keyword.cos_full_notm}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    示例输出：
      ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. 验证该包是否已配置为使用 {{site.data.keyword.cos_full_notm}} 服务实例凭证。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    示例输出：
      ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #pkg_obstorage_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将 {{site.data.keyword.cos_full_notm}} 包安装到其中的名称空间。 

3. 单击**安装包**。

4. 单击 **IBM Cloud Object Storage** 包组，然后单击 **IBM Cloud Object Storage** 包。

5. 在**可用运行时**部分中，从下拉列表中选择 `Node.JS` 或 `Python`。然后，单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **cloud-object-storage** 的新包。

7. 要使用 **cloud-object-storage** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **cloud-object-storage** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。


### 可用操作
{: #pkg_obstorage_actions}

{{site.data.keyword.cos_full_notm}} 包中包含以下操作：

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/cloud-object-storage`|包|apikey、resource_instance_id、cos_hmac_keys.access_key_id、cos_hmac_keys.secret_access_key|使用 {{site.data.keyword.cos_full_notm}} 实例。|
|`/cloud-object-storage/object-write`|操作|bucket、key、body、endpoint、ibmAuthEndpoint|将对象写入存储区。|
|`/cloud-object-storage/object-read`|操作|bucket、key、endpoint、ibmAuthEndpoint|从存储区读取对象。|
|`/cloud-object-storage/object-delete`|操作|bucket、key、endpoint、ibmAuthEndpoint|从存储区中删除对象。|
|`/cloud-object-storage/bucket-cors-put`|操作|bucket、corsConfig、endpoint、ibmAuthEndpoint|将 CORS 配置分配给存储区。|
|`/cloud-object-storage/sucket-cors-get`|操作|bucket、endpoint、ibmAuthEndpoint|读取存储区的 CORS 配置。|
|`/cloud-object-storage/sucket-cors-delete`|操作|bucket、endpoint、ibmAuthEndpoint|删除存储区的 CORS 配置。|
|`/cloud-object-storage/client-get-signed-url`|操作|bucket、key、operation、expires、endpoint、ibmAuthEndpoint|获取签名的 URL 以限制对存储区中对象的写入、读取和删除操作。|


### 包参数
{: #pkg_obstorage_pkgparams}

以下参数应该绑定到包；这将使参数自动可用于所有操作。还可以在调用其中一个操作时指定这些参数。

**apikey**：`apikey` 参数是 {{site.data.keyword.cos_full_notm}} 实例的 IAM API 密钥。

**resource_instance_id**：`resource_instance_id` 参数是 {{site.data.keyword.cos_full_notm}} 实例标识。

**cos_hmac_keys**：`cos_hmac_keys` 参数是 {{site.data.keyword.cos_full_notm}} 实例 HMAC 凭证，其中包含 `access_key_id` 和 `secret_access_key` 值。这些凭证仅由 `client-get-signed-url` 操作使用。请参阅[使用 HMAC 凭证](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials)，以获取有关如何为 {{site.data.keyword.cos_full_notm}} 实例生成 HMAC 凭证的指示信息。

#### 操作参数
{: #pkg_obstorage_actparams}

调用单个操作时指定了以下参数。每个操作不一定支持所有这些参数；请参阅上表以了解哪个操作支持哪些参数。

**bucket**：`bucket` 参数是 {{site.data.keyword.cos_full_notm}} 存储区的名称。

**endpoint**：`endpoint` 参数是用于连接到 {{site.data.keyword.cos_full_notm}} 实例的 {{site.data.keyword.cos_full_notm}} 端点。可以在 [{{site.data.keyword.cos_full_notm}} 文档](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)中找到您的端点。

**expires**：`expires` 参数表示在多少秒后使预签名的 URL 操作到期。缺省 `expires` 值为 15 分钟。

**ibmAuthEndpoint**：`ibmAuthEndpoint ` 参数是 {site.data.keyword.cos_short}} 用于通过 `apikey` 生成令牌的 IBM Cloud 授权端点。缺省授权端点应该对所有 IBM Cloud 区域都有效。

**key**：`key` 参数是存储区对象键。

**operation**：`operation` 参数是要调用的预签名的 URL 操作。

**corsConfig**：`corsConfig` 参数是存储区的 CORS 配置。


### 写入 {{site.data.keyword.cos_full_notm}} 存储区
{: #pkg_obstorage_write}

可以使用 `object-write` 操作将对象写入 {{site.data.keyword.cos_full_notm}} 存储区。
{: shortdesc}

**注**：在以下步骤中，名称 `testbucket` 用作示例。{{site.data.keyword.cos_full_notm}} 中的存储区必须全局唯一，因此必须将 `testbucket` 替换为唯一的存储区名称。

#### 通过 CLI 写入存储区
{: #pkg_obstorage_write_cli}

使用 `object-write` 操作将对象写入存储区。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

示例输出：
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### 通过 UI 写入存储区
{: #pkg_obstorage_write_ui}

1. 转至 [{{site.data.keyword.openwhisk_short}} 控制台中的“操作”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/actions)。

2. 在 `cloud-object-storage` 包下，单击 **object-write** 操作。

3. 在“代码”窗格中，单击**更改输入**。

4. 输入包含您的存储区、键和主体的 JSON 对象作为对象键。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. 单击**保存**。

6. 单击**调用**。

7. 验证输出是否类似于以下内容：
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

### 从 {{site.data.keyword.cos_full_notm}} 存储区进行读取
{: #pkg_obstorage_read}

可以使用 `object-read` 操作读取 {{site.data.keyword.cos_full_notm}} 存储区中的对象。
{: shortdesc}

**注**：在以下步骤中，名称 `testbucket` 用作示例。{{site.data.keyword.cos_full_notm}} 中的存储区必须全局唯一，因此必须将 `testbucket` 替换为唯一的存储区名称。

#### 通过 CLI 从存储区进行读取
{: #pkg_obstorage_read_cli}

使用 `object-read` 操作读取存储区中的对象。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

示例输出：
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

#### 通过 UI 从存储区进行读取
{: #pkg_obstorage_read_ui}

1. 转至 [{{site.data.keyword.openwhisk_short}} 控制台中的“操作”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/actions)。

2. 在 `cloud-object-storage` 包下，单击 **object-read** 操作。

3. 在“代码”窗格中，单击**更改输入**。

4. 输入包含您的存储区和键的 JSON 对象作为对象键。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. 单击**保存**。

6. 单击**调用**。

7. 验证输出是否类似于以下内容：
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## 使用（试验性) Object Storage 事件源侦听对存储区的更改
{: #pkg_obstorage_ev}

`/whisk.system/cos-experimental` 包可能不稳定，可能会经常发生改变而与早期版本不兼容，还可能被临时通知停用。建议不要在生产环境中使用此包。此试验性包目前仅在美国南部区域可用。
{: important}

可以使用 {{site.data.keyword.openwhisk}} 来侦听对 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) 存储区的更改，并使用操作处理存储区中的一个或多个对象。

<br>

**样本用例：**通过 `/whisk.system/cos-experimental` 包，可以侦听对存储在 {{site.data.keyword.cos_full_notm}} 存储区中的 GPS 街道数据的更改。然后，在发生更改时，可以触发自动重新生成 GPS 地图，以便用户可以使用其 GPS 应用程序访问最新的街道数据。

### （试验性）Object Storage 事件源参数
{: #pkg_obstorage_ev_ch}

`/whisk.system/cos-experimental` 包支持您配置 {{site.data.keyword.cos_full_notm}} 实例中的事件，并包含以下订阅源：

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/cos-experimental`|包|apikey、auth_endpoint、bucket、endpoint、interval|包含 `changes` 订阅源操作的包。|
|`/whisk.system/cos-experimental/changes`|订阅源|apikey、auth_endpoint、bucket、endpoint、interval|对 {{site.data.keyword.cos_full_notm}} 存储区进行更改时触发触发器事件。|
{: shortdesc}

可以使用 `changes` 订阅源来配置 {{site.data.keyword.cos_full_notm}} 事件源服务，以在每次对 {{site.data.keyword.cos_full_notm}} 实例中的存储区进行更改时都触发触发器。

此示例中使用的参数：

**apikey**：_（必需，除非绑定到包）_。`apikey` 参数是 {{site.data.keyword.cos_full_notm}} 实例的 IAM API 密钥。通常，此值会绑定到包。但是，如果使用 `changes` 订阅源操作时指定了 `apikey` 值，那么对于凭证，将使用指定的值，而不使用绑定凭证的 API 密钥。

**auth_endpoint**：_（可选）_。`auth_endpoint` 参数是 {{site.data.keyword.cos_full_notm}} 用于通过 `apikey` 生成令牌的授权端点。缺省端点为 {{site.data.keyword.Bluemix}} 端点。

**bucket**：_（必需）_。`bucket` 参数是 {{site.data.keyword.cos_full_notm}} 存储区的名称。

**endpoint**：_（必需）_。`endpoint` 参数是用于连接到 {{site.data.keyword.cos_full_notm}} 实例的 {{site.data.keyword.cos_full_notm}} 端点。可以在 [{{site.data.keyword.cos_full_notm}} 文档](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)中找到您的端点。

**interval**：_（可选）_。`interval` 参数是存储区轮询时间间隔，以整数分钟为单位。 `interval` 值必须至少为 1 分钟，并且缺省情况下设置为 1 分钟。

### 创建触发器，用于响应 changes 订阅源
{: #pkg_obstorage_ev_trig}

创建触发器时，可以通过将 {{site.data.keyword.cos_full_notm}} 凭证直接绑定到 `cos-experimental` 包来避免将这些凭证传递到 `changes` 订阅源操作。
 {: shortdesc}

 1. 首先，创建可以修改以包含凭证的包绑定。以下命令将在名称空间中创建包绑定 `myCosPkg`。
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. 将 {{site.data.keyword.cos_full_notm}} 凭证绑定到包。
 将 {{site.data.keyword.cos_full_notm}} 凭证绑定到包会将 `apikey` 值绑定到包，以便在调用 `changes` 订阅源操作时不需要指定 `apikey` 值。
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 使用创建的包绑定中的 `changes` 订阅源来创建名为 `myCosTrigger` 的触发器。请使用您的存储区名称和 {{site.data.keyword.cos_full_notm}} 端点参数值。
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

示例输出：
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. 创建一个简单操作，以仅用于验证触发器、更改订阅源和规则是否都已正确配置并在正常运行。例如，创建名为 `showCosChange` 的操作，其中包含以下 `showCosChange.js` JavaScript 代码：
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
这将显示样本代码
 5. 创建规则，用于将 `showCosChange` 操作连接到 `myCosTrigger` 触发器：
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. 在单独的窗口中，开始对激活进行轮询，以清楚地了解正在发生的情况。触发器触发且操作运行时，此命令将列出其中每个操作执行时的激活记录。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. 在 {{site.data.keyword.cos_full_notm}} 仪表板中，修改现有存储区对象或创建存储区对象。要了解如何向存储区添加对象，请参阅[向存储区添加一些对象](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects)。

 8. 对于每个存储区对象更改，请观察 `myCosTrigger` 触发器和 `showCosChange` 操作的新激活。这些激活会显示在配置的存储区轮询时间间隔内运行 `ibmcloud fn activation poll` 命令的窗口中。

如果观察不到新的激活，请验证 `apikey`、`endpoint` 和 `bucket` 参数值是否正确。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Object Storage 触发器事件的数据结构
{: #pkg_obstorage_ev_data}

生成的事件的内容具有以下参数：

  - `file`：文件或对象元数据。此结构在[列出特定存储区中的对象](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets)中进行了描述。
  - `status`：检测到的更改。此值为 `added`、`modified` 或 `deleted`。
  - `bucket`：{{site.data.keyword.cos_full_notm}} 存储区的名称。
  - `endpoint`：用于连接到 {{site.data.keyword.cos_full_notm}} 实例的 {{site.data.keyword.cos_full_notm}} 端点。
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

### 创建用于处理已更改对象的操作
{: #pkg_obstorage_ev_act}

可以创建用于检索和处理对象的单个操作。或者，可以创建一个序列，其中使用一个操作来检索对象，另一个操作来处理对象。

### 创建用于检索和处理对象的操作
{: #pkg_obstorage_ev_act_ret}

此样本操作代码会检索并处理存储区更改通知文档。在手动操作调用期间，可以直接将 `apikey` 和 `serviceInstanceId` 参数传递到操作，但当触发器调用此操作时，这些值必须从 {{site.data.keyword.cos_full_notm}} 中获取，Object Storage 必须使用 `ibmcloud fn service bind` 命令绑定到此操作。

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
exports.main = main;
```
{: codeblock}

由于此操作使用 `ibm-cos-sdk` npm 包，因此该操作必须打包为 [Node.js 模块](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm)或[单个捆绑软件](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg)。

将此操作打包成 .zip 文件 `myCosAction.zip` 后，创建用于通过 {{site.data.keyword.cos_full_notm}} 检索和处理对象的操作：

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### 创建用于检索和处理对象的操作序列
{: #pkg_obstorage_ev_act_seq}

无需在操作中包含对象检索代码，而是可以使用 `cloud-object-storage` 包（必须[手动安装](#pkg_obstorage_install)）中的 `object-read` 操作。操作代码仅需要处理从 `object-read` 返回的结果。

仅处理存储区对象的操作的示例 `myCosAction.js` 代码：
```javascript
  function main(data) {
    if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. 创建用于仅处理 {{site.data.keyword.cos_full_notm}} 中对象的操作：
  ```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
  {: pre}
2. 将 {{site.data.keyword.cos_full_notm}} 凭证绑定到手动安装的 `cloud-object-storage` 包。
  ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
  {: pre}
3. `object-read` 操作可与 `myCosProcessObjectAction` 一起编写以创建操作序列。
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

除了 `object-read` 操作外，还可以使用可安装的 `cloud-object-storage` 包中包含的其他操作。

### 将凭证绑定到操作
{: #pkg_obstorage_ev_bind}

通过以下命令将 {{site.data.keyword.cos_full_notm}} 凭证绑定到操作，可以避免在调用期间传递敏感凭证：
```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
{: pre}

### 创建规则以将操作与更改触发器相关联
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

可以使用[规则](/docs/openwhisk?topic=cloud-functions-rules)中的操作或操作序列来访存和处理与 {{site.data.keyword.cos_full_notm}} 更改事件关联的对象。

创建规则，用于在发生新的 {{site.data.keyword.cos_full_notm}} 触发器事件时激活 `MyCosAction` 操作。
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}

