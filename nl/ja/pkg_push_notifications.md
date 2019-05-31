---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: push notifications, functions, webhooks

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

# Push Notifications
{: #pkg_push_notifications}


## パッケージ
{: #pkg_push_packages}

| パッケージ | 可用性 | 説明 |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | 事前インストール済み (東京では提供されていません) | 指定された 1 つ以上のデバイスにプッシュ通知を送信する。 |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | 事前インストール済み (東京では提供されていません) | デバイス・イベントでモバイル・プッシュ通知を送信する。 |
| [`/push-notifications`](#pkg_push_mobile_send) | インストール可能 | {{site.data.keyword.mobilepushfull}} サービス・インスタンスと対話する。メッセージを送信したり、Webhook を作成、更新、削除したりする。 |

## プッシュ通知の送信
{: #pkg_push_send}

この事前インストール済みパッケージは、東京地域では提供されていません。IAM 認証を使用する `sendMessage` アクションについては、インストール可能な[プッシュ通知](#pkg_push_mobile_send)パッケージを参照してください。
{: tip}

プッシュ通知パッケージ・バインディングを作成し、`/whisk.system/pushnotifications` パッケージを使用して単純なプッシュ通知を送信する方法を説明します。
{: shortdesc}

このパッケージには、以下のアクションとフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | パッケージ | appId、appSecret、admin_url | プッシュ・サービスを使用した処理を行う。 |
| `/whisk.system/pushnotifications/sendMessage` | アクション | text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload | 指定された 1 つ以上のデバイスにプッシュ通知を送信する。 |


デバイス・アクティビティーが発生したときにトリガー・イベントを起動する方法については、[デバイス・イベントでのモバイル・プッシュ](#pkg_push_mobile)を参照してください。

### プッシュ通知パッケージ・バインディングの作成
{: #pkg_push_create}

プッシュ通知パッケージ・バインディングを作成するには、以下のパラメーターを指定する必要があります。

-  **appId**: {{site.data.keyword.Bluemix}} **アプリ GUID**。
-  **appSecret**: {{site.data.keyword.Bluemix_notm}} プッシュ通知サービスの**アプリ・シークレット**。

以下の手順を参照して、パッケージ・バインディングを作成します。

1. [{{site.data.keyword.Bluemix_notm}} ダッシュボード](http://cloud.ibm.com)で {{site.data.keyword.Bluemix_notm}} アプリケーションを作成します。

2. プッシュ通知サービスを初期化し、{{site.data.keyword.Bluemix_notm}} アプリケーションにバインドします。

3. [プッシュ通知アプリケーション](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)を構成します。

  作成した {{site.data.keyword.Bluemix_notm}} アプリの **App GUID** と **App Secret** を必ず覚えておいてください。

4. `/whisk.system/pushnotifications` を使用してパッケージ・バインディングを作成します。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. パッケージ・バインディングが存在することを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### プッシュ通知パラメーター
{: #pkg_push_params}

`/whisk.system/pushnotifications/sendMessage` アクションは、登録されたデバイスにプッシュ通知を送信します。 パラメーターは次のとおりです。
- `text`: ユーザーに表示する通知メッセージ。 例えば、`-p text "Hi, OpenWhisk send a notification"` です。
- `url`: アラートと一緒に送信できる URL。 例えば、`-p url "https:\\www.w3.ibm.com"` です。
- `apiHost`: API ホストを指定するオプションのストリング。 デフォルトは `mobile.ng.bluemix.net` です。  例: `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`: 指定されるデバイスのリスト。 例えば、`-p deviceIds ["deviceID1"]` です。
- `platforms`: 指定されるプラットフォームのデバイスに通知を送信します。 Apple (iOS) デバイスの場合は「A」、Google (Android) デバイスの場合は「G」です。 例えば、`-p platforms ["A"]` です。
- `userIds`: 指定されるユーザーのデバイスに通知を送信します。 例えば、`-p userIds "[\"testUser\"]"` です。
- `tagNames`: これらのタグのいずれかにサブスクライブしているデバイスに通知を送信します。 例えば、`-p tagNames "[\"tag1\"]"` です。
- `gcmCollapseKey`: このパラメーターは、メッセージのグループを識別します。
- `gcmCategory`: 対話式プッシュ通知に使用されるカテゴリー ID。
- `gcmIcon`: 通知に対して表示されるアイコンの名前を指定します。 このアイコンが既にクライアント・アプリケーションと共にパッケージされていることを確認してください。
- `gcmDelayWhileIdle`: このパラメーターが true に設定されている場合、メッセージはデバイスがアクティブになるまで送信されます。
- `gcmSync`: デバイス・グループ・メッセージングは、グループ内のすべてのアプリ・インスタンスが最新メッセージング状態を反映することを可能にします。
- `gcmVisibility`: private/public - この通知の可視性。これは、通知がいつ、どのように、ロックされた保護画面に表示されるのかに影響します。
- `gcmPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。 例えば、`-p gcmPayload "{\"hi\":\"hello\"}"` です。
- `gcmPriority`: メッセージの優先順位を設定します。
- `gcmSound`: 通知がデバイスに到着したときに再生される (デバイス上の) 音声ファイル。
- `gcmTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `gcmStyleType`: 展開可能な通知のタイプを指定します。 指定できる値は、`bigtext_notification`、`picture_notification`、`inbox_notification` です。
- `gcmStyleTitle`: 通知のタイトルを指定します。 タイトルは通知が展開されると表示されます。 タイトルは、展開可能な 3 つの通知のすべてで指定される必要があります。
- `gcmStyleUrl`: 通知のピクチャーの取得元である URL。 `picture_notification` にはこれを指定する必要があります。
- `gcmStyleText`: `bigtext_notification` の展開時に表示される必要がある大きいテキスト。 `bigtext_notification` にはこれを指定する必要があります。
- `gcmStyleLines`: `inbox_notification` の場合にボックス内スタイルで表示されるストリングの配列。 `inbox_notification` にはこれを指定する必要があります。
- `gcmLightsLedArgb`: LED の色。 ハードウェアはできる限りの近似を行います。
- `gcmLightsLedOnMs`: LED の点滅時に明かりがついている時間 (ミリ秒)。 ハードウェアはできる限りの近似を行います。
- `gcmLightsLedOffMs`: LED の点滅時に明かりが消えている時間 (ミリ秒)。 ハードウェアはできる限りの近似を行います。
- `apnsBadge`: アプリケーション・アイコンのバッジとして表示する番号。
- `apnsCategory`: 対話式プッシュ通知に使用されるカテゴリー ID。
- `apnsIosActionKey`: アクション・キーのタイトル。
- `apnsPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT']。
- `apnsSound`: アプリケーション・バンドル内の音声ファイルの名前。 このファイルの音声がアラートとして再生されます。
- `apnsTitleLocKey`: 現行のローカリゼーション用の `Localizable.strings` ファイル内のタイトル・ストリングのキー。 キー・ストリングは、`titleLocArgs` 配列に指定された変数を取り込むために、%@ および %n$@ の指定子を使用してフォーマット設定できます。
- `apnsLocKey`: (ユーザーの言語設定によって指定された) 現行のローカリゼーション用の `Localizable.strings` ファイル内のアラート・メッセージ・ストリングのキー。 キー・ストリングは、locArgs 配列に指定された変数を取り込むために、%@ および %n$@ の指定子を使用してフォーマット設定できます。
- `apnsLaunchImage`: アプリケーション・バンドル内のイメージ・ファイルのファイル名 (ファイル名拡張子を含めても含めなくてもよい)。 このイメージは、ユーザーがアクション・ボタンをタップした時、またはアクション・スライダーを移動した時に起動イメージとして使用されます。
- `pnsTitleLocArgs`: `title-loc-key` 内のフォーマット指定子を置換する変数ストリング値。
- `apnsLocArgs`: `locKey` 内のフォーマット指定子を置換する変数ストリング値。
- `apnstitle`: リッチ・プッシュ通知のタイトル (iOS 10 以上のみでサポートされます)。
- `apnsSubtitle`: リッチ通知のサブタイトル (iOS 10 以上でのみサポートされます)。
- `apnsAttachmentUrl`: iOS 通知メディアへのリンク (ビデオ、オーディオ、GIF、イメージ - iOS 10 以上のみでサポートされます)。
- `fireFoxTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `fireFoxIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `fireFoxTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `fireFoxPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `chromeTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `chromeIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `chromeTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `chromePayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `safariTitle`: Safari プッシュ通知に対して設定されるタイトルを指定します。
- `safariUrlArgs`: この通知で使用する必要がある URL 引数。 これらの引数は JSON 配列の形式で指定されます。
- `safariAction`: アクション・ボタンのラベル。
- `chromeAppExtTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `chromeAppExtCollapseKey`: このパラメーターは、メッセージのグループを識別します。
- `chromeAppExtDelayWhileIdle`: このパラメーターが true に設定されている場合、デバイスがアクティブになるまでメッセージが送信されないことを示します。
- `chromeAppExtIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `chromeAppExtTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `chromeAppExtPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。

### プッシュ通知の例
{: #pkg_push_ex}

プッシュ通知パッケージからプッシュ通知を送信するには、以下の例を参照してください。

前に作成したパッケージ・バインディング内の **sendMessage** アクションを使用してプッシュ通知を送信します。 `/myNamespace/myPush` は実際のパッケージ名に置き換えてください。
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

出力例:
```
{
  "result": {
  "pushResponse":
    {
      "messageId":"11111H",
      "message":{
        "alert":"this is my message",
        "url":""
      },
      "settings":{
        "apns":{
          "sound":"default"
        },
        "gcm":{
          "sound":"default"
          },
        "target":{
          "deviceIds":["T1","T2"]
        }
      }
    }
  },
  "status": "success",
  "success": true
}
```
{: screen}

## モバイル・デバイス・イベントでのプッシュ通知の送信
{: #pkg_push_mobile}

この事前インストール済みパッケージは、東京地域では利用できません。
{: tip}

ここでは、指定されたアプリケーションでデバイス・アクティビティー (デバイスの登録/登録解除、サブスクライブ/アンサブスクライブなど) があるとトリガーを起動するプッシュ通知サービスを構成する方法を説明します。
{: shortdesc}

### パラメーター
{: #pkg_push_mobile_params}

`/whisk.system/pushnotifications/webhook` パラメーターは以下のとおりです。
- `appId`: {{site.data.keyword.Bluemix_notm}} アプリ GUID。
- `appSecret`: {{site.data.keyword.Bluemix_notm}} プッシュ通知サービスの `appSecret`。
- `events`: `onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe`、`onUnsubscribe`

  ワイルドカード文字「`*`」を使用して、すべてのイベントに対して通知されるようにすることができます。

### プッシュ通知サービス・アクティビティーでのトリガー・イベントの起動
{: #pkg_push_mobile_trigger}

プッシュ通知サービス・アプリケーションに新規デバイスが登録されるたびに起動されるトリガーを作成するには、以下の例を参照してください。

1. `appId` と `appSecret` を使用して、プッシュ通知サービス用に構成されたパッケージ・バインディングを作成します。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. `myPush/webhook` フィードを使用して、プッシュ通知サービス `onDeviceRegister` イベント・タイプ用のトリガーを作成します。
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 新規デバイスが登録されるたびにメッセージを送信するルールを作成できます。 前のアクションとトリガーを使用してルールを作成します。
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. `ibmcloud fn activation poll` コマンドを使用して結果を確認します。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. {{site.data.keyword.Bluemix_notm}} アプリケーションでデバイスを登録します。 {{site.data.keyword.openwhisk}} [ダッシュボード](https://cloud.ibm.com/openwhisk/dashboard)で、`rule`、`trigger`、および `action` が実行されるのを確認できます。

  アクションはプッシュ通知を送信します。


## プッシュ通知の送信または Webhook の作成、更新、削除
{: #pkg_push_mobile_send}

インストール可能な {{site.data.keyword.mobilepushshort}} パッケージには、{{site.data.keyword.mobilepushfull}} サービス・インスタンスとの対話に必要なアクションのセットが用意されています。 これらのアクションにより、メッセージを送信したり、Webhook を作成、更新、または削除したりすることができます。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} パッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/push-notifications` | パッケージ | apikey、appGuid | {{site.data.keyword.mobilepushshort}} インスタンスと連携します。 |
| `/push-notifications/send-message` | アクション | text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload | 指定された 1 つ以上のデバイスにプッシュ通知を送信する。 |
| `/push-notifications/webhook` | アクション | events | プッシュ・サービスでデバイスのアクティビティー (デバイスの登録、登録解除、サブスクリプション、アンサブスクリプション) に基づいてトリガー・イベントを起動する。 |

### {{site.data.keyword.mobilepushshort}} サービス・インスタンスの作成
{: #service_instance_push}

パッケージをインストールする前に、{{site.data.keyword.mobilepushshort}} インスタンスを作成する必要があります。

1. [{{site.data.keyword.mobilepushshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. プッシュ通知サービス・インスタンス用に[サービス資格情報のセットを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) します。

3. [{{site.data.keyword.mobilepushshort}} サービス・インスタンスを構成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

### {{site.data.keyword.mobilepushshort}} パッケージのインストール
{: #pkg_push_mobile_install}

{{site.data.keyword.mobilepushshort}} サービス・インスタンスを作成したら、{{site.data.keyword.openwhisk}} CLI または UI を使用して、{{site.data.keyword.mobilepushshort}} パッケージを自分の名前空間にインストールします。

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #pkg_push_mobile_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.mobilepushshort}} パッケージをインストールするには、以下のようにします。

1. {{site.data.keyword.mobilepushshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. `runtimes/nodejs` ディレクトリーに移動します。
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. `push-notifications` パッケージが自分のパッケージ・リストに追加されていることを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    出力:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. 作成した {{site.data.keyword.mobilepushshort}} サービス・インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. パッケージが {{site.data.keyword.mobilepushshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #pkg_push_mobile_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. 右上隅の名前空間スイッチャーを使用して、{{site.data.keyword.cos_full_notm}} パッケージのインストール先の名前空間を選択します。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **IBM {{site.data.keyword.mobilepushshort}}** パッケージ・グループをクリックしてから、**IBM {{site.data.keyword.mobilepushshort}}** パッケージをクリックします。

5. 「使用可能なランタイム (Available Runtimes)」セクションで、ドロップダウン・リストから NodeJS を選択してから、**「インストール」**をクリックします。

6. パッケージがインストールされたら、アクション・ページにリダイレクトされ、この新しいパッケージを検索できます。このパッケージは **push-notifications** という名前になっています。

7. **push-notifications** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **push-notifications** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

### プッシュ通知の送信
{: #pkg_push_mobile_sendmsg}

`send-message` アクションを使用してプッシュ通知サービスからメッセージを送信するには、以下のようにします。
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

出力例:
```
{
  "response": {
      "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

### Web フックの作成
{: #pkg_push_mobile_hook}

onDeviceRegister イベント用の {{site.data.keyword.mobilepushshort}} サービスの Webhook を作成するには、以下のようにします。

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

出力例:
```
{
  "response": {
    "result": {
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}

