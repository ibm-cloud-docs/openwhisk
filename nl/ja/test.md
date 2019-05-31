---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless, javascript, node, node.js

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


# サーバーレス・アプリのテスト
{: #test}

CLI で作成した各エンティティーをテストして、サーバーレス・アプリが正常に機能することを検証したり、問題が発生した場合にトラブルシューティングしたりできます。
{: shortdesc}


## アクションのテスト
{: #test-js}

アクションをテストするには、`invoke` コマンドを実行します。アクションのテストは、パラメーターを指定して行うことも指定せずに行うこともできます。
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

Hello world の例:
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

出力:
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### JSON ファイルに格納されているパラメーターのテスト
{: #test_json_file}

JSON 形式のパラメーターのファイルを渡すことができます。
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

出力例:
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### JSON 形式で入力されたパラメーターのテスト
{: #test_json}

呼び出しで JSON 形式のパラメーターを渡すことができます。
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

出力例:
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### ブロッキング・アクションのテスト
{: #test-block}

アクションの呼び出しはブロッキングにすることも非ブロッキングにすることもできます。デフォルトでは、呼び出しは非ブロッキングです。アクションの結果がすぐに必要でない場合は、非ブロッキング呼び出しを使用してください。
{: shortdesc}

ブロッキング呼び出しは、要求/応答形式を使用するため、アクティベーション結果が使用可能になるまで待機します。 待機時間は、60 秒と、アクションの[制限時間値](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)のいずれか小さいほうです。

ブロッキング呼び出しを実行して、クラウド内でアクションを実行します。

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


出力例:
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
  "status": "success",
  "success": true
}
```
{: screen}

このコマンドにより、以下の情報が出力されます。
* 予期される待機時間内に使用可能になった場合は呼び出し結果
* --result オプションを指定しなかった場合は、アクティベーション ID が結果に表示されます。ログや呼び出し結果の取得に使用できるアクティベーション ID (`44794bd6aab74415b4e42a308d880e5b`)。






## トリガーのテスト
{: #test_triggers}

トリガーは、キーと値のペアのディクショナリーを使用して起動またはアクティブ化することができます。 このディクショナリーは、イベントと呼ばれることもあります。 トリガーは、ユーザーが明示的に起動することも、ユーザーの代わりに外部イベント・ソースによって起動することもできます。 アクションと同様に、トリガーの起動のたびに、アクティベーション ID が生成されます。
{: shortdesc}

1. トリガーを起動します。

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    ルールに関連付けられていないトリガーは、起動されても視覚的な効果はありません。 このトリガーにはルールが関連付けられていないため、渡されたパラメーターは、どのアクションによっても入力として使用されません。

    出力例:

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. 最新のアクティベーション・レコードを調べて、アクションが呼び出されたことを確認します。
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    出力例:
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. 前のコマンド出力からアクティベーション ID に関する詳細情報を取得します。
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    出力例:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## アクティベーションの経過時間のテスト
{: #test_time}

アクティベーション・ログを取得して、アクティベーションの実行に要した時間を確認できます。経過時間が長すぎる場合や、関数の実行時間を長くするためにデフォルトのタイムアウトを調整する必要がある場合は、アクションのタイムアウトを更新できます。
{: shortdesc}

1. アクティベーション ID を取得します。

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    出力例:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. そのアクティベーション ID のログを取得します。

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    `duration` はミリ秒単位の時間を示しています。このアクティベーションの実行時間は 2 秒強でした。

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. タイムアウトをミリ秒単位で指定してアクションを更新します。

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    例:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## メモリー使用量のテスト
{: #test_memory}

アプリを Docker イメージとしてパッケージ化した場合は、Docker コマンドを使用してアプリのメモリー使用量を確認できます。
{: shortdesc}

1. Docker イメージを実行するコンテナーをローカルに作成します。

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. コンテナーのリストを取得してコンテナー ID を調べます。

    ```
    docker ps
    ```
    {: pre}

3. 実行中のコンテナーの統計を確認します。

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. コンテナーのメモリー使用量を確認します。値がシステム限度に収まっていない場合は、スクリプトを調整します。

5. 情報を確認したら、実行しているコンテナーを停止できます。

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. コンテナーを削除します。

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}






