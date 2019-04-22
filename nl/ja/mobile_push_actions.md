---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: mobile, push notifications, binding, notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# モバイル・プッシュ・パッケージ
{: #openwhisk_catalog_pushnotifications}

この事前インストール済みパッケージは、東京地域では利用できません。IAM 認証を使用する `sendMessage` アクションについては、インストール可能な[プッシュ通知](/docs/openwhisk?topic=cloud-functions-push-notifications-package)パッケージを参照してください
{: tip}

プッシュ・パッケージ・バインディングを作成し、`/whisk.system/pushnotifications` パッケージを使用して単純なプッシュ通知を送信する方法を説明します。このパッケージは、プッシュ・サービスを操作する機能を提供します。
{: shortdesc}

このパッケージには、以下のアクションとフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | パッケージ | appId、appSecret、admin_url | プッシュ・サービスを使用した処理を行う。 |
| `/whisk.system/pushnotifications/sendMessage` | アクション | text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload | 指定された 1 つ以上のデバイスにプッシュ通知を送信する。 |


デバイス・アクティビティーが発生したときにトリガー・イベントを起動する方法については、トピック『[デバイス・イベントでのモバイル・プッシュ](/docs/openwhisk?topic=cloud-functions-openwhisk_pushnotifications)』を参照してください。

## プッシュ通知パッケージ・バインディングの作成
{: #create_push_binding}

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

## プッシュ通知パラメーター
{: #push_parameters}

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

## プッシュ通知の送信
{: #send_push_notifications}

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
