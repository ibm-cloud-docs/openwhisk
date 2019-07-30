---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

subcollection: cloud-functions

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

インストール可能な {{site.data.keyword.visualrecognitionfull}} サービスは、ディープ・ラーニング・アルゴリズムを使用して、このサービスにアップロードするイメージ内のシーン、オブジェクト、および顔を識別します。 カスタム種別を作成して、ニーズに合った対象を識別するようにトレーニングすることができます。
{:shortdesc}

{{site.data.keyword.visualrecognitionshort}} パッケージには、以下のエンティティーが含まれています。 詳しくは、エンティティー名をクリックして、{{site.data.keyword.visualrecognitionshort}} API リファレンスを参照してください。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} | パッケージ | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | {{site.data.keyword.visualrecognitionshort}} サービスを操作します。 |
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`images_file`、`accept_language`、`url`、`threshold`、`owners`、`classifier_ids`、`images_file_content_type`  | イメージを分類します。 |
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`images_file`、`url`、`images_file_content_type`  | イメージ内の顔を検出します。 |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、  `name`、`classname_positive_examples`、`negative_examples` | 種別を作成します。 |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id`  | 種別を削除します。 |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id`  | 種別の詳細を取得します。 |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、  `verbose`  | 種別のリストを取得します。 |
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、  `classifier_id`、`classname_positive_examples`、`negative_examples`  | 種別を更新します。 |
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id`  | 種別の Core ML モデルを取得します。 |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id`  | ラベル付きデータを削除します。 |

## {{site.data.keyword.visualrecognitionshort}} サービス・インスタンスの作成
{: #service_instance_recognition}

パッケージをインストールする前に、{{site.data.keyword.visualrecognitionshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.visualrecognitionshort}} サービス・インスタンスを作成します](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.visualrecognitionshort}} パッケージのインストール
{: #install_recognition}

{{site.data.keyword.visualrecognitionshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.visualrecognitionshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #visualrecognition_cli}

**始める前に**
[{{site.data.keyword.cloud_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.visualrecognitionshort}} パッケージをインストールするため、以下のコマンドを実行します。

1. {{site.data.keyword.visualrecognitionshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. パッケージがパッケージ・リストに追加されたことを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **出力**
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.visualrecognitionshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    **出力例**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.visualrecognitionshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    **出力例**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #visualrecognition_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external} に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Visual Recognition」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**visual-recognition-v3** という名前の新規パッケージを検索できます。

7. **visual-recognition-v3** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、[CLI の手順](#visualrecognition_cli)のステップ 4 と 5 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。
   
  使用するアクションごとに以下のステップを実行する必要があります。
  {: note}

    1. 使用する **visual-recognition-v3** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.visualrecognitionshort}} パッケージの使用
{: #usage_recognition}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。 API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。 [API 参照](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}で詳細を確認してください。

このパッケージの機能は、Visual Recognition の現行バージョンである `2018-03-19` を使用します。 `list-classifiers` アクションを試行します。
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


