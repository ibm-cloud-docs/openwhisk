---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# デバイス・イベントでのモバイル・プッシュ
{: #openwhisk_pushnotifications}

ここでは、指定されたアプリケーションでデバイス・アクティビティー (デバイスの登録/登録解除、サブスクライブ/アンサブスクライブなど) があるとトリガーを起動するプッシュ・サービスを構成する方法を説明します。
{: shortdesc}

`/whisk.system/pushnotifications` パッケージ自体については、『[モバイル・プッシュ](./mobile_push_actions.html)』トピックを参照してください。そのトピックでは、プッシュ・パッケージ・バインディングの作成およびプッシュ通知の送信について説明されています。

## プッシュのパラメーター
{: #push_parameters}

`/whisk.system/pushnotifications/webhook` パラメーターは以下のとおりです。
- **appId:** {{site.data.keyword.Bluemix_notm}} アプリ GUID。
- **appSecret:** {{site.data.keyword.Bluemix_notm}} プッシュ通知サービスの appSecret。
- **events:** _onDeviceRegister_、_onDeviceUnregister_、_onDeviceUpdate_、_onSubscribe_、_onUnsubscribe_

  ワイルドカード文字「`*`」を使用して、すべてのイベントに対して通知されるようにすることができます。

## プッシュ通知サービス・アクティビティーでのトリガー・イベントの起動
{: #trigger_push_notify}

プッシュ通知サービス・アプリケーションに新規デバイスが登録されるたびに起動されるトリガーを作成するには、以下の例を参照してください。

1. **appId** と **appSecret** を使用して、プッシュ通知サービス用に構成されたパッケージ・バインディングを作成します。
  ```
  ibmcloud wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. `myPush/webhook` フィードを使用して、プッシュ通知サービス `onDeviceRegister` イベント・タイプ用のトリガーを作成します。
  ```
  ibmcloud wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 新規デバイスが登録されるたびにメッセージを送信するルールを作成できます。前のアクションとトリガーを使用してルールを作成します。
  ```
  ibmcloud wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. `ibmcloud wsk activation poll` コマンドを使用して結果を確認します。
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

5. {{site.data.keyword.Bluemix_notm}} アプリケーションでデバイスを登録します。 {{site.data.keyword.openwhisk}} [ダッシュボード](https://console.bluemix.net/openwhisk/dashboard)で、`rule`、`trigger`、および `action` が実行されるのを確認できます。

  アクションはプッシュ通知を送信します。
