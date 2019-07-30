---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: triggers, serverless, functions

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



# イベント用トリガーの作成
{: #triggers}

トリガーとは、ユーザーから起動されるか、イベント・ソースによって起動されるかに関わらず、特定のタイプのイベントに対応するための宣言です。
{: shortdesc}

トリガーの例を以下に示します。
- ロケーション更新イベント
- Web サイトへの文書アップロード
- 着信 E メール



## CLI からのトリガーの作成
{: #triggers_create}


1. トリガーを作成します。 トリガーは、名前空間内に直接作成する必要があり、パッケージ内には作成できません。
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    **出力例**
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. トリガーが作成されたことを確認します。
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    **出力例**
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



次に、[トリガーをテスト](/docs/openwhisk?topic=cloud-functions-test#test_triggers)するか、トリガーをアクションと関連付ける[ルールを作成](/docs/openwhisk?topic=cloud-functions-rules)できます。



## フィードとトリガーの違い
{: #triggers_difference}

フィードとトリガーは密接に関連していますが、技術的には別個の概念です。

- {{site.data.keyword.openwhisk_short}} はシステムに流入してくる**イベント**を処理します。

- **トリガー**は、イベントのクラスの名前です。 各イベントは 1 つのトリガーにのみ属します。トリガーは、トピック・ベースのパブリッシュ/サブスクライブ・システムにおけるトピックに似ています。 **ルール**は、トリガーからイベントが到着するたびに、トリガー・ペイロードを使用してアクションを呼び出します。

- **フィード** は、{{site.data.keyword.openwhisk_short}} で消費可能なトリガー・イベントを起動するように、外部イベント・ソースを構成するための便利な方法です。 フィードは、特定のトリガーに属するイベントのみで構成されるイベント・ストリームです。 事前インストールされたパッケージ、インストール可能パッケージ、および独自のカスタム・パッケージには、フィードが含まれていることがあります。  フィードは**フィード・アクション**によって制御され、フィード・アクションが、フィードを構成するイベントのストリームの作成、削除、休止、および再開を処理します。 通常、フィード・アクションは、通知を管理する REST API を使用して、イベントを生成する外部サービスと対話します。

フィードの例:
- データベース内の文書が追加または変更されるたびにトリガー・イベントを起動する、{{site.data.keyword.cloudant}} データ変更フィード
- Git リポジトリーに対するコミットのたびにトリガー・イベントを起動する Git フィード



## フィード用トリガーの作成
{: #triggers_feeds}

この例は、Alarms パッケージ内のフィードを使用して 1 分ごとにトリガーを発生させる方法と、ルールを使用して 1 分ごとにアクションを呼び出す方法を示しています。

1. `/whisk.system/alarms` パッケージ内のエンティティーの説明リストを取得します。

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    **出力例**
    ```
    package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. `/whisk.system/alarms` パッケージ内のフィードの説明を取得し、使用できるパラメーターを確認します。

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  **出力例**
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` フィードには次の 2 つのパラメーターがあります。
  - `cron`: トリガーをいつ起動するのかについての crontab 指定。
  - `trigger_payload`: 各トリガー・イベントに設定するペイロード・パラメーター値。

2. 1 分ごとに起動するトリガーを作成します。
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  **出力例**
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. アプリを作成します。 `hello.js` の例:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. アクションを作成します。
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. `everyOneMinute` トリガーが起動するたびに `hello` アクションを呼び出すルールを作成します。
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  **出力例**
  ```
  ok: created rule myRule
  ```
  {: screen}

6. アクティベーション・ログをポーリングして、アクションが呼び出されていることを確認します。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  トリガー、ルール、およびアクションに 1 分ごとにアクティベーションが実行されていることがわかります。 アクションは、すべての呼び出しでパラメーター `{"name":"Mork", "place":"Ork"}` を受け取ります。



