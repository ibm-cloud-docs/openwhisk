---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

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


# クイック・スタート・テンプレートのデプロイ
{: #templates}

{{site.data.keyword.openwhisk}} は、次のプロジェクトを迅速に開始できるように、テンプレートのカタログを提供します。 テンプレートは、アクション、トリガー、シーケンスの組み合わせです。 一部のテンプレートは、{{site.data.keyword.cloud_notm}} からの他のサービス・インスタンスも取り込みます。 テンプレートを使用することで、プロジェクトを迅速かつ簡単に作成し、直ちにコーディングを開始できます。
{: shortdesc}


## 使用可能なクイック・スタート・テンプレート
{: #available-templates}

| 名前 | 説明 | サポートされるランタイム | ソース・リポジトリー |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | {{site.data.keyword.cloudantfull}} データベースで文書が編集または追加されると、コンソールに変更を出力します。 | Node.js、Swift、Python、PHP | [コード](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Upload Image](#image-template) | Web アクションを使用して、イメージを {{site.data.keyword.cos_full}} インスタンスのバケットにアップロードしてから、そのイメージのサムネールを取得します。 | Node.js | [コード](https://github.com/ibm-functions/template-cloud-object-storage){: external}
| [Get HTTP Resource](#get-http-resource-template) | HTTP イベントを使用して Web アクションを呼び出し、Yahoo Weather API からデータを取得します。 | Node.js、Python | [コード](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | JSON オブジェクトを 1 つのパラメーターとして受け入れる基本アクションを作成します。 | Node.js、Swift、Python、PHP | [コード](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub}} Events](#messagehub-events-template) | {{site.data.keyword.messagehub_full}} トピックで新規データが追加されると、コンソールに変更を出力します。 | Node.js、Swift、Python、PHP | [コード](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Periodic Slack Reminder](#slack-reminder-template) | 定期的なトリガーに基づいて Webhook を Slack に対して使用します。 | Node.js、Swift、Python、PHP | [コード](https://github.com/ibm-functions/template-reminder-slack){: external}

各テンプレートのコードはコード・リポジトリーで入手できます。自由に変更し、テンプレートを基礎とする独自の機能を作成できます。
{: tip}


## {{site.data.keyword.cloudant_short_notm}} Events テンプレートのデプロイ
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} テンプレートは、アクション・シーケンス、およびそのシーケンスを開始するトリガーを作成します。 接続されている {{site.data.keyword.cloudant_short_notm}} の猫サンプル・データベースに変更が加えられると、トリガーが起動します。 予期されるデータ項目は猫であり、名前と色が定義されています。 新しい猫がデータベースに追加されるか、現在の猫が編集されると、データがコンソールに出力されます。

### UI から {{site.data.keyword.cloudant_short_notm}} Events テンプレートをデプロイする
{: #cloudant-template-ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Cloudant Events」**をクリックします。

4. {{site.data.keyword.cloudant_short_notm}} アクションを作成します。
    1. パッケージの名前を指定するか、提供されているデフォルト名 `new-cloudant-item` を使用します。
    2. **「アクション」**リストで、猫データベース・サンプルの `Node.js 6` を選択し、**「次へ」**をクリックします。

5. {{site.data.keyword.cloudant_short_notm}} トリガーを作成します。 トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。 トリガーを作成するには、{{site.data.keyword.messagehub}} のサービス・インスタンスとサービス資格情報を指定する必要があります。
    1. **「Cloudant インスタンス (Cloudant Instance)」**リストで、**「独自のインスタンスの作成 (Create your own instance)」**を選択します。 {{site.data.keyword.cloudant_short_notm}} セットアップ・ページが開きます。
    2. {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスを作成します。
    3. サービス資格情報のセットを作成します。
    4. **「OK」**をクリックして、{{site.data.keyword.cloudant_short_notm}} セットアップ・ページを閉じ、{{site.data.keyword.openwhisk_short}} コンソールに戻ります。
    5. **「Cloudant インスタンス (Cloudant Instance)」**リストで、**「自分の資格情報の入力」**を選択し、以下の情報を入力します。
        
        * `ユーザー名` - {{site.data.keyword.cloudant_short_notm}} ユーザー名。
        * `パスワード` - {{site.data.keyword.cloudant_short_notm}} パスワード。
        * `ホスト` - `<username>.cloudant.com`。 
        * `データベース` - {{site.data.keyword.cloudant_short_notm}} データベースの名前。

6. **「デプロイ」**をクリックします。 テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

### CLI から {{site.data.keyword.cloudant_short_notm}} Events テンプレートをデプロイする
{: #cloudant-template-cli}

1. テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. 使用するアクション・ランタイムのディレクトリーに移動します。`nodejs-6`、`nodejs`、`php`、`python`、または `swift` のいずれかです。
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. 次の環境変数を使用してテンプレートをデプロイします。
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>環境変数について</caption>
    <thead>
    <th colspan=2>環境変数について</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>{{site.data.keyword.cloudant_short_notm}} のユーザー名</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>{{site.data.keyword.cloudant_short_notm}} のパスワード</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>{{site.data.keyword.cloudant_short_notm}} データベースの名前</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>パッケージのカスタム名</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>ルールのカスタム名</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>トリガーのカスタム名</td></tr>
    </tbody></table>

## Upload Image テンプレートのデプロイ
{: #image-template}

Upload Image テンプレートは、小さなインターフェースを使用してイメージを {{site.data.keyword.cos_full_notm}} バケットにアップロードするために使用できる Web アクションを作成します。次に、このテンプレートはイメージをサムネールとして取得し、それを Web アクションのインターフェースに表示します。


### UI から Upload Image テンプレートをデプロイする
{: #image-template-ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Upload Image」**をクリックします。

4. パッケージの名前を入力するか、またはデフォルト名 `upload-image` を使用し、**「次へ」**をクリックします。

6. テンプレートには、{{site.data.keyword.cos_full_notm}} サービス・インターフェースのサービス資格情報が必要です。 **{{site.data.keyword.cos_full_notm}}** リストで、以下のいずれかのオプションを選択します。
  * **インスタンスの作成**。既存のサービス・インスタンスがない場合、このオプションを選択して 1 つ作成します。
      1. 開いた「{{site.data.keyword.cos_full_notm}} サービス・インスタンス作成」ページで、サービス・インスタンスを作成します。
      2. [HMAC サービス資格情報セットを作成します](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。
      3. [バケットを少なくとも 1 つ作成します](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets)。
  * **自分の資格情報の入力**。{{site.data.keyword.cos_full_notm}} サービス・インスタンス用に独自の資格情報を手動で入力するには、このオプションを選択します。 資格情報には HMAC 鍵がなければならず、サービス・インスタンスには、バケットが少なくとも 1 つ必要です。
  * **既存のインスタンス**。 既存の {{site.data.keyword.cos_full_notm}} インスタンスがあれば、そのいずれかのインスタンスをリストから選択します。 資格情報には HMAC 鍵がなければならず、サービス・インスタンスには、バケットが少なくとも 1 つ必要です。

7. **「デプロイ」**をクリックします。

8. 左側のナビゲーションで、**「エンドポイント」**をクリックします

9. **「Web アクション」**セクションで、リンクを .json という接尾部を除いてコピーし、それをご使用のブラウザーのアドレス・バーに貼り付けます。 テンプレートの Web アクション用インターフェースが表示されます。

10. オプション: テンプレートがデプロイされた後、**アクション**・ダッシュボードにナビゲートして、以下の 2 つの新規パッケージに含まれているコードをカスタマイズできます。
    * `cloud-object-storage` パッケージ。このパッケージには、{{site.data.keyword.cos_full_notm}} インスタンスと連携するアクションが含まれています。
    * テンプレート・パッケージ (デフォルト名 `upload-image`)。これには `app` アクションが含まれています。

### CLI から Upload Image テンプレートをデプロイする
{: #image-template-cli}

1. [{{site.data.keyword.cos_full_notm}} パッケージをインストールします](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。

2. テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. `nodejs` ランタイム・ディレクトリーに移動します。
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. カスタム・パッケージ名と {{site.data.keyword.cos_full_notm}} バケットの名前を環境変数として使用して、テンプレートをデプロイします。
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. {{site.data.keyword.openwhisk_short}} コンソールの[「アクション」ページ](https://cloud.ibm.com/openwhisk/actions){: external}で、`process-change` アクションをクリックします。

5. 左側のナビゲーションで、**「エンドポイント」**をクリックします。

6. **「Web アクション」**セクションで、リンクを .json という接尾部を除いてコピーし、それをご使用のブラウザーのアドレス・バーに貼り付けます。 テンプレートの Web アクション用インターフェースが表示されます。

## Get HTTP Resource テンプレートのデプロイ
{: #get-http-resource-template}

Get HTTP Resource テンプレートは、外部リソースである Yahoo Weather API をフェッチするためのアクションを作成し、その後、データを返します。 アクションは Web アクションとして有効にされるので、CORS 対応の URL を使用して呼び出すことが可能で、認証鍵は必要ありません。これは、Web アプリケーションのバックエンドを構築するのに役立ちます。

デフォルトでは、`get-http-resource` エンドポイントは、これを呼び出すすべてのユーザーに対して公開されています。
{: note}

### UI から Get HTTP Resource テンプレートをデプロイする
{: #get-http-resource-template-ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Get HTTP Resource」**をクリックします。

3. パッケージの名前を入力するか、またはデフォルト名 `get-http-resource` を使用します。

4. アクションのランタイムを選択します。Node.js 8、Node.js 6、または Python 3 のいずれかです。

5. **「デプロイ」**をクリックします。

6. 次の URL を対象に curl を使用して、アクションを呼び出します。`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。 以下に例を示します。
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

### CLI から Get HTTP Resource テンプレートをデプロイする
{: #get-http-resource-template-cli}

1. テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. 使用するアクション・ランタイムのディレクトリーに移動します。`nodejs-6`、`nodejs`、または `python` のいずれかです。
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. カスタム・パッケージ名を環境変数として使用して、テンプレートをデプロイします。
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 次の URL を対象に curl を使用して、アクションを呼び出します。`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。 以下に例を示します。
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Hello World テンプレートのデプロイ
{: #hello-world-template}

この基本的な Hello World アクションをデプロイして、{{site.data.keyword.openwhisk_short}} の使用を開始したり、作成した他のエンティティー (トリガーやルールなど) をテストしたりできます。

### UI から Hello World テンプレートをデプロイする
{: #hello-world-template-ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Hello World」**をクリックします。

4. パッケージの名前を入力するか、またはデフォルト名 `hello-world` を使用します。

5. アクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。

6. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

### CLI から Hello World テンプレートをデプロイする
{: #hello-world-template-cli}

1. Hello World テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. 使用するアクション・ランタイムのディレクトリーに移動します。`nodejs-6`、`nodejs`、`php`、`python`、または `swift` のいずれかです。
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. テンプレートをデプロイします。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## {{site.data.keyword.messagehub}} Events テンプレートのデプロイ
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} Events テンプレートは、アクション、およびそのアクションを開始するトリガーを作成します。 テンプレートの作成時に選択された {{site.data.keyword.messagehub}} トピックに新しい項目が追加されるたびにトリガーが起動されます。

### UI から {{site.data.keyword.messagehub}} Events テンプレートをデプロイする
{: #messagehub-events-template-ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「{{site.data.keyword.messagehub}} Events」**をクリックします。

4. {{site.data.keyword.messagehub}} アクションを作成します。
    1. パッケージの名前を指定するか、提供されているデフォルト名 `message-hub-events` を使用します。
    2. アクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。
    3. **「次へ」**をクリックします。

5. {{site.data.keyword.messagehub}} トリガーを作成します。 トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。 トリガーを作成するには、{{site.data.keyword.messagehub}} のサービス・インスタンスとサービス資格情報を指定する必要があります。
    1. **「MessageHub インスタンス (MessageHub Instance)」**リストで、**「独自のインスタンスの作成 (Create your own instance)」**を選択します。 {{site.data.keyword.messagehub}} 作成ページが開きます。
    2. {{site.data.keyword.messagehub}} サービス・インスタンスを作成します。
    3. サービス資格情報のセットを作成します。
    4. **「OK」**をクリックして、{{site.data.keyword.messagehub}} 作成ページを閉じ、{{site.data.keyword.messagehub}} コンソールに戻ります。
    5. **「MessageHub インスタンス (MessageHub Instance)」**リストで、**「自分の資格情報の入力」**を選択し、以下の情報を入力します。
        * `ユーザー名` - {{site.data.keyword.messagehub}} ユーザー名。
        * `パスワード` - {{site.data.keyword.messagehub}} パスワード。
        * `kafka_admin_url` - {{site.data.keyword.messagehub}} 管理 REST URL。
        * `データベース` - {{site.data.keyword.messagehub}} データベースの名前。
        * `トピック` - サブスクライブするトピック。

6. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

### CLI から {{site.data.keyword.messagehub}} Events テンプレートをデプロイする
{: #messagehub-events-template-cli}

1. テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. 使用するアクション・ランタイムのディレクトリーに移動します。`nodejs-6`、`nodejs`、`php`、`python`、または `swift` のいずれかです。
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. 次の環境変数を使用してテンプレートをデプロイします。
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>環境変数について</caption>
    <thead>
    <th colspan=2>環境変数について</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>{{site.data.keyword.messagehub}} 管理 REST URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>サブスクライブするトピック</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>{{site.data.keyword.messagehub}} のユーザー名</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>{{site.data.keyword.messagehub}} のパスワード</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>パッケージのカスタム名</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>ルールのカスタム名</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>トリガーのカスタム名</td></tr>
    </tbody></table>

## Periodic Slack Reminder テンプレートのデプロイ
{: #slack-reminder-template}

Periodic Slack Reminder テンプレートは、トリガー作成時にユーザーによって指定された間隔で Slack に通知します。

### UI から Periodic Slack Reminder テンプレートをデプロイする
{: #slack-reminder-template-ui}

1. https://api.slack.com/incoming-webhooks にアクセスして、必要な着信 Webhook URL をセットアップします。

1. {{site.data.keyword.openwhisk_short}} コンソールで[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external}に移動します。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Periodic Slack Reminder Events」**をクリックします。

4. Periodic Slack Reminder アクションを作成します。
    1. パッケージの名前を指定するか、提供されているデフォルト名 `periodic-slack-reminder` を使用します。
    2. アクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。
    3. 「パラメーター」セクションで、Webhook URL を**「パラメーター値」**フィールドに入力します (例えば、`https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`)。
    4. **「次へ」**をクリックします。

5. Periodic Slack Reminder トリガーを作成します。 トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。
    1. パターンまたは `Cron` 式を使用して、トリガーの時間間隔を指定します。
        * `パターン` - 平日、時間、および分に応じた UTC 時刻を選択します。
        * `Cron` - <a href="http://crontab.org">UNIX crontab 構文</a>に基づいた cron シーケンスを指定します。 `X X X X X` 形式の、スペースで区切った 5 個以下の項目を使用します。
    2. トリガー JSON ペイロードを追加します。

6. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

### CLI から Periodic Slack Reminder テンプレートをデプロイする
{: #slack-reminder-template-cli}

1. https://api.slack.com/incoming-webhooks にアクセスして、必要な着信 Webhook URL をセットアップします。

1. テンプレート・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. 使用するアクション・ランタイムのディレクトリーに移動します。`nodejs-6`、`nodejs`、`php`、`python`、または `swift` のいずれかです。
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. 次の環境変数を使用してテンプレートをデプロイします。
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>環境変数について</caption>
    <thead>
    <th colspan=2>環境変数について</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>Webhook URL。例: <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td><a href="http://crontab.org">UNIX crontab 構文</a>に基づいた cron シーケンス。 <code>X X X X X</code> 形式の、5 個以下のフィールドを使用します。</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>パッケージのカスタム名</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>ルールのカスタム名</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>トリガーのカスタム名</td></tr>
    </tbody></table>


