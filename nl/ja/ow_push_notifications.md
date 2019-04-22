---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}}  パッケージ 

{{site.data.keyword.mobilepushshort}} パッケージには、{{site.data.keyword.mobilepushfull}} サービス・インスタンスとの対話に必要なアクションのセットが用意されています。 これらのアクションにより、メッセージを送信したり、Webhook を作成、更新、または削除したりすることができます。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} パッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/push-notifications` | パッケージ | apikey、appGuid | {{site.data.keyword.mobilepushshort}} インスタンスと連携します。 |
| `/push-notifications/send-message` | アクション | text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload | 指定された 1 つ以上のデバイスにプッシュ通知を送信する。 |
| `/push-notifications/webhook` | アクション | events | プッシュ・サービスでデバイスのアクティビティー (デバイスの登録、登録解除、サブスクリプション、アンサブスクリプション) に基づいてトリガー・イベントを起動する。 |

## {{site.data.keyword.mobilepushshort}} サービス・インスタンスの作成
{: #service_instance_push}

パッケージをインストールする前に、{{site.data.keyword.mobilepushshort}} インスタンスを作成する必要があります。

1. [{{site.data.keyword.mobilepushshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. Push Notificiations サービス・インスタンス用に[サービス資格情報のセットを作成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)。

3. [{{site.data.keyword.mobilepushshort}} サービス・インスタンスを構成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

## {{site.data.keyword.mobilepushshort}} パッケージのインストール
{: #install_push}

{{site.data.keyword.mobilepushshort}} サービス・インスタンスを作成したら、{{site.data.keyword.openwhisk}} CLI または UI を使用して、{{site.data.keyword.mobilepushshort}} パッケージを自分の名前空間にインストールします。

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #install_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

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
{: #install_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. 右上隅の名前空間スイッチャーを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。

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

## {{site.data.keyword.mobilepushshort}} パッケージの使用
{: #usage_push}

### プッシュ通知の送信
{: #push_notif}

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

## Web フックの作成
{: #webhook}

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
