---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}} 包

{{site.data.keyword.visualrecognitionfull}} 服务使用深度学习算法来识别上传到此服务的图像中的场景、对象和人脸。您可以创建和培训定制分类器以识别符合您需求的主题。
{:shortdesc}

{{site.data.keyword.visualrecognitionshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.visualrecognitionshort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|[`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.visualrecognitionshort}} 服务。|
|[classify](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、images_file、accept_language、url、threshold、owners、classifier_ids、images_file_content_type|对图像进行分类。|
|[detect-faces](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、images_file、url、images_file_content_type|检测图像中的人脸。|
|[create-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、classname_positive_examples、negative_examples|创建分类器。|
|[delete-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id|删除分类器。|
|[get-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id|检索分类器详细信息。|
|[list-classifiers](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、verbose|检索分类器的列表。|
|[update-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id、classname_positive_examples、negative_examples|更新分类器。|
|[get-core-ml-model](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id|检索分类器的核心 ML 模型。|
|[delete-user-data](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id|删除标记的数据。|

## 创建 {{site.data.keyword.}} 服务实例
{: #service_instance}

安装包之前，必须创建 {{site.data.keyword.}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/services/watson_vision_combined)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.}} 包
{: #install}

具有 {{site.data.keyword.}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #visualrecognition_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](bluemix_cli.html#cloudfunctions_cli)。
  2. 安装 [`wskdeploy` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)，并将下载的二进制文件添加到 PATH 中。

要安装 {{site.data.keyword.}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    wskdeploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. 验证包是否已添加到包列表中。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    输出：
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    示例输出：
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.}} 服务实例凭证。
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    示例输出：
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #visualrecognition_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将 {{site.data.keyword.cos_short}} 包安装到其中的名称空间。名称空间由组合的组织和空间名称构成。

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Visual Recognition** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **visual-recognition-v3** 的新包。

7. 要使用 **visual-recognition-v3** 包中的操作，必须将服务凭证绑定到操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。 
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **visual-recognition-v3** 包中要使用的操作。这将打开该操作的详细信息页面。 
    2. 在左侧导航中，单击**参数**部分。 
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.visualrecognitionshort}} 包
{: #usage}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的版本参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。请在 [API 参考](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning)中查看更多详细信息。

此包的函数使用当前版本的 Visual Recognition (2018-03-19)。请试用 `list-classifiers` 操作。
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}
