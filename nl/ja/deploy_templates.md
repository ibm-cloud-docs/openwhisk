---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# クイック・スタート・テンプレートのデプロイ
{: #serviceauth}

{{site.data.keyword.openwhisk}} は、次のプロジェクトを迅速に開始できるように、テンプレートのカタログを提供します。 テンプレートは、アクション、トリガー、シーケンスの組み合わせであり、{{site.data.keyword.Bluemix}} からのサービス・インスタンスを取り込むこともできます。 テンプレートを使用することで、プロジェクトを迅速かつ簡単に作成し、直ちにコーディングを開始できます。

このチュートリアルでは、{{site.data.keyword.openwhisk_short}} で使用可能なテンプレートのデプロイについて順に説明します。
{: shortdesc}

## 使用可能なクイック・スタート・テンプレート
{: #available-templates}

| 名前 | 説明 | サポートされるランタイム |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | {{site.data.keyword.cloudant}} で文書が編集または追加された場合に、コンソールに変更を出力します。 | Node.js、Swift、Python、PHP |
| [Upload Image](./deploy_templates.html#cos-upload-image) | イメージを {{site.data.keyword.cos_full}} インスタンスのバケットにアップロードし、その後そのイメージのサムネールを取得することのできる Web アクション。| Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | HTTP イベントに対する応答として呼び出され、Yahoo Weather API からデータをフェッチする Web アクション。 | Node.js、Python |
| [Hello World](./deploy_templates.html#hello-world-template) | このアクションは 1 つのパラメーターを受け入れます。このパラメーターは JSON オブジェクトでなければなりません。 | Node.js、Swift、Python、PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | {{site.data.keyword.messagehub_full}} トピックで新しいデータが追加されると、コンソールに変更を出力します。 | Node.js、Swift、Python、PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | 定期的なトリガーに基づいて、Slack に投稿するアクション。 | Node.js、Swift、Python、PHP |

## {{site.data.keyword.cloudant_short_notm}} Events テンプレートのデプロイ
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} テンプレートは、アクション・シーケンス、およびそのシーケンスを開始するトリガーを作成します。 トリガーは、接続されている {{site.data.keyword.cloudant_short_notm}} データベースで変更が生じると起動します。このデータベースは、名前と色を持つ猫のデータベースです。 予期されるデータ項目は猫であり、名前と色が定義されています。 新しい猫がデータベースに追加されるか、現在の猫が編集されると、データがコンソールに出力されます。

1. テンプレートを作成するには、[{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) に移動してから、**「作成の開始」**をクリックします。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Cloudant Events」**をクリックします。

### {{site.data.keyword.cloudant_short_notm}} アクションの作成

1. 次に、パッケージの名前を指定するか、提供されているデフォルト名 `new-cloudant-item` を使用します。

2. **「アクション」**ドロップダウンの下で、所有するアクションのランタイム (nodejs、swift、python、または php) を選択します。 この例では、**「nodejs」**を選択し、**「次へ」**をクリックします。

### {{site.data.keyword.cloudant_short_notm}} トリガーの作成

トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。 {{site.data.keyword.cloudant_short_notm}} テンプレート用のトリガーを作成するには、そのトリガーに、必要な {{site.data.keyword.cloudant_short_notm}} サービス・インスタンス情報を指定します。

#### {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスの作成

以下のいずれかの操作から選択できます。
  * **独自のインスタンスの作成**
  * **独自の資格情報の入力**

1. この例では、**独自のインスタンスの作成**を選択します。

2. ポップアップが開き、{{site.data.keyword.cloudant_short_notm}} セットアップ・ページが含まれた新規タブが表示されます。 {{site.data.keyword.cloudant_short_notm}} インスタンスを作成した後に、サービス資格情報セットを作成してから、**「OK」**をクリックしてタブを閉じてこのページに戻る必要があります。

3. 次に、**独自の資格情報の入力**を選択し、以下の情報を指定します。
  * ユーザー名 - _{{site.data.keyword.cloudant_short_notm}} ユーザー名_
  * パスワード - _{{site.data.keyword.cloudant_short_notm}} パスワード_
  * ホスト - _これは通常、`username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * データベース - _{{site.data.keyword.cloudant_short_notm}} データベースの名前_

### {{site.data.keyword.cloudant_short_notm}} テンプレートのデプロイ

**「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

## Upload Image テンプレートのデプロイ
{: #cos-upload-image}

Upload Image テンプレートは、小さなインターフェースを使用してイメージを {{site.data.keyword.cos_short_notm}} バケットにアップロードできるようにする Web アクションを作成します。次に、このテンプレートはイメージをサムネールとして取得し、それを Web アクションのインターフェースに表示します。

テンプレートをデプロイするには、次のようにします。

1. [{{site.data.keyword.Bluemix_notm}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/) の {{site.data.keyword.openwhisk_short}} コンソールに移動します。

2. **「作成の開始」**をクリックします。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「Upload Image」**テンプレートをクリックします。

4. パッケージの名前を入力するか、またはデフォルト名 `upload-image` を使用します。

5. **「次へ」**をクリックします。

6. テンプレートには、{{site.data.keyword.cos_full_notm}} サービス・インターフェースのサービス資格情報が必要です。**{{site.data.keyword.cos_short}}** リストで、以下のいずれかのオプションを選択します。
  * **新規インスタンスの作成**: 既存のサービス・インスタンスがない場合、このオプションを選択して 1 つ作成します。
      1. 開いた「{{site.data.keyword.cos_full_notm}} サービス・インスタンス作成」ページで、サービス・インスタンスを作成します。
      2. [HMAC サービス資格情報セットを作成します](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials)。
      3. [バケットを少なくとも 1 つ作成します](/docs/services/cloud-object-storage/getting-started.html#create-buckets)。
  * **独自の資格情報の入力**: {{site.data.keyword.cos_short}} サービス・インスタンス用に独自の資格情報を手動で入力するには、このオプションを選択します。資格情報には HMAC 鍵がなければならず、サービス・インスタンスには、バケットが少なくとも 1 つ必要です。
  * **既存のインスタンス**: 既存の {{site.data.keyword.cos_short}} インスタンスがあれば、そのいずれかのインスタンスをリストから選択します。資格情報には HMAC 鍵がなければならず、サービス・インスタンスには、バケットが少なくとも 1 つ必要です。

7. **「デプロイ」**をクリックします。

8. 左側のナビゲーションで、**「Endpoints」**をクリックします。

9. 「Web アクション」セクションで、リンクを .json という接尾部を除いてコピーし、それをご使用のブラウザーのアドレス・バーに貼り付けます。テンプレートの Web アクション用インターフェースが表示されます。

10. オプション: テンプレートがデプロイされた後、アクション・ダッシュボードにナビゲートして、以下の 2 つの新規パッケージに含まれているコードをカスタマイズできます。
    * `cloud-object-storage` パッケージ。このパッケージには、{{site.data.keyword.cos_short}} インスタンスと連携するアクションが含まれています。
    * テンプレート・パッケージ (デフォルト名 `upload-image`)。これには `app` アクションが含まれています。

## Get HTTP Resource テンプレートのデプロイ
{: #get-http-resource-template}

Get HTTP Resource テンプレートは、外部リソースである Yahoo Weather API をフェッチするためのアクションを作成し、その後、データを返します。 アクションは Web アクションとして有効にされるので、CORS 対応の URL を使用して呼び出すことが可能で、認証鍵は必要ありません。これは、Web アプリケーションのバックエンドを構築するのに役立ちます。 **注**: デフォルトでは、`get-http-resource` エンドポイントは、このエンドポイントを呼び出すことができるあらゆるユーザーに対して公開されています。

1. テンプレートを作成するには、[{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) に移動してから、**「作成の開始」**をクリックします。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「パッケージ名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`get-http-resource` と設定されます。

4. 所有するアクションのランタイムを選択します。Node.js 8、Node.js 6、または Python 3 のいずれかです。

5. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

## Hello World テンプレートのデプロイ
{: #hello-world-template}

このアクションは 1 つのパラメーターを受け入れます。このパラメーターは JSON オブジェクトでなければなりません。

1. テンプレートを作成するには、[{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) に移動してから、**「作成の開始」**をクリックします。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「パッケージ名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`hello-world` と設定されます。

4. 所有するアクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。

5. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

## {{site.data.keyword.messagehub}} Events テンプレートのデプロイ
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} Events テンプレートは、アクション、およびそのアクションを開始するトリガーを作成します。テンプレートの作成時に選択された {{site.data.keyword.messagehub}} トピックに新しい項目が追加されるたびにトリガーが起動されます。

1. テンプレートを作成するには、[{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) に移動してから、**「作成の開始」**をクリックします。

2. **「クイック・スタート・テンプレート」**をクリックします。

3. **「パッケージ名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`message-hub-events` と設定されます。

4. 所有するアクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。

5. **「次へ」**をクリックします。

### {{site.data.keyword.messagehub}} トリガーの作成

トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。 {{site.data.keyword.messagehub}} テンプレート用のトリガーを作成するには、そのトリガーに、必要な {{site.data.keyword.messagehub}} サービス・インスタンス情報を指定します。

**「トリガー名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`message-hub-events-trgr` と設定されます。

### {{site.data.keyword.messagehub}} サービス・インスタンスの作成

以下のいずれかの操作から選択できます。
  * **独自のインスタンスの作成**
  * **独自の資格情報の入力**

1. この例では、**独自のインスタンスの作成**を選択します。

2. ポップアップが開き、{{site.data.keyword.messagehub}} セットアップ・ページが含まれた新規タブが表示されます。 {{site.data.keyword.messagehub}} インスタンスを作成した後に、サービス資格情報セットを作成してから、**「OK」**をクリックしてタブを閉じてこのページに戻る必要があります。

3. 次に、**独自の資格情報の入力**を選択し、以下の情報を指定します。
  * ユーザー名 - _{{site.data.keyword.messagehub}} ユーザー名_
  * パスワード - _{{site.data.keyword.messagehub}} パスワード_
  * kafka_admin_url - _{{site.data.keyword.messagehub}} 管理 REST URL_
  * データベース - _{{site.data.keyword.messagehub}} データベースの名前_
  * トピック - _サブスクライブするトピック_

### {{site.data.keyword.messagehub}} テンプレートのデプロイ

**「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

## Periodic Slack Reminder テンプレートのデプロイ
{: #slack-reminder-template}

Periodic Slack Reminder テンプレートは、トリガー作成時にユーザーによって指定された間隔で Slack に通知します。 このテンプレートを作成する前に、https://api.slack.com/incoming-webhooks にアクセスして、必要な着信 Webhook URL をセットアップしてください。

1. **「パッケージ名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`periodic-slack-reminder` と設定されます。

2. 所有するアクションのランタイムを選択します。Node.js 8、Node.js 6、Python 3、Swift 4、または PHP 7.1 のいずれかです。

3. **「パラメーター」**セクションで、**「パラメーター値」**フィールドに Webhook URL を入力し、**「次へ」**をクリックします。 (例: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX)

### Slack Reminder トリガーの作成

トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。 Slack Reminder テンプレート用のトリガーを作成するには、そのトリガーに、必要な {{site.data.keyword.messagehub}} サービス・インスタンス情報を指定します。

1. **「トリガー名」**フィールドを確認します。このフィールドは必要に応じて更新できます。 デフォルトでは、`periodic-slack-reminder-trgr` と設定されます。

2. 次に、パターンまたはクーロン式を使用して、トリガーを起動する間隔を指定できます。 平日、時間、および分に応じた UTC 時刻を選択できます。 希望する間隔オプションを選択すると、テンプレートのデプロイメントの準備ができます。

3. **「デプロイ」**をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。
