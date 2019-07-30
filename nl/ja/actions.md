---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

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



# アクションの作成
{: #actions}

アクションを作成します。アクションは、JSON オブジェクトを返す最上位の関数です。 複数のアクションをまとめて 1 つのパッケージにするとアクションの管理が簡単になります。
{: shortdesc}

始める前に: アクションを作成するには、ソース・コードがいくつかの要件を満たしていなければなりません。 例えば、複数のファイルに分かれているコードからアクションを作成する場合は、アクションを作成する前にコードを単一のファイルとしてパッケージ化します。 ランタイムごとの要件について詳しくは、[サーバーレスにするアプリ・コードを準備する方法](/docs/openwhisk?topic=cloud-functions-prep)を参照してください。


## CLI からのアクションの作成
{: #actions_cli}

1. アクションを作成します。
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **例**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **出力例**

  ```
  ok: created action hello
  ```
  {: screen}

  ヒント:
  - コストを節約するために、制限を設定できます。
      - メモリー使用量の制限を設定するには、create コマンドに `--memory VALUE` を指定します。値は M バイト単位です。
      - タイムアウトを設定するには、create コマンドに `--timeout VALUE` を指定します。値はミリ秒単位です。
  - コードを Docker イメージとしてパッケージ化した場合は、アプリのローカル・パスと --kind フラグの代わりに `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` を create コマンドに指定します。 可能であれば、`latest` タグを使用せずに、イメージをうまく管理してください。 `latest` タグを使用すると、このタグが付いたイメージが使用されますが、それが常に最も新しく作成されたイメージであるとは限りません。

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. 作成したアクションがアクション・リストに表示されることを確認します。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **出力例**

  ```
  actions
  hello       private
  ```
  {: screen}


## アクション内のアプリまたはランタイムの更新
{: #actions_update}

アプリのコードを更新したり、新しいバージョンのランタイムに移行したりする必要が生じたら、いつでも update コマンドを実行できます。 例えば、Node.js バージョン 8 は保守モードになっているので、ランタイムを Node.js 10 に切り替えることもできます。

新しいランタイム・バージョンに移行する場合は、新しいランタイム・バージョンに適合するように、アプリ内のコードを変更しなければならないことがあります。 ほとんどの場合、ランタイム・バージョンには互換性があります。
{: tip}

1. アプリをローカルで更新します。

2. アプリを Docker イメージとしてパッケージ化した場合は、最新のイメージを Docker Hub にアップロードします。 これにより、システムは、アクション用のコードの次回実行時に新規 Docker イメージをプルできるようになります。 前のバージョンの Docker イメージを使用している実行中のコンテナーがあると、新たな呼び出しでもそのイメージが使用されます。新たな呼び出しで新しいイメージを実行するためには、update コマンドを実行しなければなりません。

3. アクションを更新してアプリのローカル・パスまたは Docker イメージを指定します。

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **例**

    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **出力例**

    ```
    ok: updated action hello
    ```
    {: screen}

    コードを Docker イメージとしてパッケージ化した場合は、ローカル・アプリのパスと `--kind` フラグの代わりに `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` を create コマンドに指定します。 可能であれば、`latest` タグを使用せずに、イメージをうまく管理してください。 `latest` タグを使用すると、このタグが付いたイメージが使用されますが、それが常に最も新しく作成されたイメージであるとは限りません。 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## アクションへのパラメーターのバインド
{: #actions_params}

アクションにパラメーターをバインドして、デフォルトのパラメーターを設定できます。 呼び出し時にパラメーターを渡さない限り、バインドされたパラメーターが、アクションのデフォルト・パラメーターとして機能します。
{: shortdesc}

始めに、[アクションを作成します](#actions_cli)。

パラメーターをバインドするには、以下のようにします。

1. アクションを更新してデフォルトのパラメーターをバインドします。

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **例**

    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **出力例**

    ```
    ok: updated action MyApp
    ```
    {: screen}

    非サービス資格情報パラメーターを変更する場合、新しいパラメーターを指定して `action update` コマンドを実行すると、現在は存在しているが `action update` コマンドで指定されていないパラメーターはすべて削除されます。 例えば、`action update -p key1 new-value -p key2 new-value` を実行するが、設定されていた他のすべてのパラメーターを省略すると、それらのパラメーターはこのアクションが更新された後は存在しなくなります。 アクションにバインドされていたサービスも削除されます。 サービスをバインドしていた場合は、もう一度、[アクションにサービスをバインドする](/docs/openwhisk?topic=cloud-functions-services)必要があります。
    {: tip}

3. パラメーターがアクションにバインドされたことを確認します。

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **出力例**

    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

オプション: 前にバインドしたパラメーターを消去するには、パラメーターを何も指定せずにアクションを更新します。

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## アクションをアクション・シーケンスとしてチェーニングする
{: #actions_seq}

一連のアクションをチェーニングして 1 つのアクションを作成できます。 1 つのアクションの結果が次のアクションへの引数として渡されます。
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

シーケンス中のアクション間で渡されるパラメーターは、デフォルト・パラメーターを除いて、明示的です。 したがって、アクション・シーケンスに渡されるパラメーターを使用できるのはシーケンス中の先頭アクションのみです。 シーケンス中の最初のアクションの結果が、シーケンス中の 2 番目のアクションへの入力 JSON オブジェクトになり、以降同様に渡されます。 このオブジェクトには、最初にシーケンスに渡されたパラメーターはどれも含まれません (ただし、先頭アクションがその結果にそれらのパラメーターを組み込んだ場合は除きます)。 あるアクションへの入力パラメーターは、そのアクションのデフォルト・パラメーターとマージされます。その際、入力パラメーターが優先され、一致するデフォルト・パラメーターはオーバーライドされます。

シーケンスには、シーケンス内の各アクションのタイムアウトとは別の、全体的なタイムアウトはありません。 シーケンスは操作のパイプラインなので、1 つのアクションで失敗すると、パイプラインが中断します。 1 つのアクションでタイムアウトが発生すると、その失敗でシーケンス全体が終了します。

次は、ルールの作成時またはアクションの呼び出し時に、シーケンスの名前を使用します。


## アクションのパッケージ化
{: #actions_pkgs}

{{site.data.keyword.openwhisk}} では、関連する一連のアクションとフィードをパッケージとしてまとめて、他のユーザーと共有することができます。 パッケージを使用すると、パッケージ内のすべてのエンティティーでパラメーターを共有することもできます。
{: shortdesc}

パッケージは、*アクション* および*フィード* を含むことができます。
- アクションは、{{site.data.keyword.openwhisk_short}} で実行されるコードです。 例えば、{{site.data.keyword.cloudant}} パッケージには、{{site.data.keyword.cloudant_short_notm}} データベースのレコードの読み取りや書き込みのためのアクションが含まれています。
- フィードは、トリガー・イベントを起動するための外部イベント・ソースを構成するために使用されます。 例えば、Alarm パッケージには、指定された頻度でトリガーを起動できるフィードが含まれています。


1. パッケージを作成します。

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. パッケージのサマリーを取得します。 パッケージは空であることに注目してください。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **出力例**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. アクションを作成してパッケージ内に含めます。 パッケージ内にアクションを作成するには、アクション名にパッケージ名の接頭部を付ける必要があります。 パッケージのネスティングは許可されません。 パッケージにはアクションのみを含めることができ、別のパッケージを含めることはできません

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. パッケージのサマリーを取得します。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **出力例**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## パッケージへのパラメーターのバインド
{: #actions_pkgs_params}

パッケージ内のすべてのアクションで継承されるパッケージ・レベルのパラメーターを設定することによって、パッケージ内のすべてのエンティティーのデフォルト・パラメーターを設定できます。

以下の場合を除いて、バインドされたパラメーターは、パッケージ内のアクションのデフォルト・パラメーターとして機能します。

- アクション自体にデフォルト・パラメーターがある
- アクションに呼び出し時に指定されたパラメーターがある

始めに、1 つ以上のアクションを含むパッケージを作成します。

1. パッケージを更新してデフォルトのパラメーターをバインドします。

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **例**

    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    **出力例**

    ```
    ok: updated package MyApp
    ```
    {: screen}

    サービス資格情報以外のパラメーターを変更する場合に、新しいパラメーターを指定して `package update` コマンドを実行すると、現在は存在しているのに `package update` コマンドで指定されなかったパラメーターがすべて削除されます。 例えば、`package update -p key1 new-value -p key2 new-value` を実行し、これまで設定されていた他のパラメーターはすべて省略した場合、それらのパラメーターはパッケージの更新後は存在しなくなります。 パッケージにバインドされていたサービスも削除されるので、他のパラメーターを更新した後にもう一度、[パッケージにサービスをバインドする](/docs/openwhisk?topic=cloud-functions-services)必要があります。
    {: tip}

3. パラメーターがパッケージにバインドされたことを確認します。

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **出力例**

    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. パッケージでパラメーターが継承されたことを確認します。

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **出力例**

    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## アクションのパッケージの共有
{: #actions_pkgs_share}

パッケージを構成するアクションおよびフィードのデバッグとテストが完了したら、そのパッケージをすべての {{site.data.keyword.openwhisk_short}} ユーザーで共有できます。 パッケージを共有すると、ユーザーはパッケージをバインドしたり、パッケージ内のアクションを呼び出したり、{{site.data.keyword.openwhisk_short}} ルールおよびシーケンス・アクションを作成したりできます。 共有パッケージ内のアクションおよびフィードは、_パブリック_ です。 パッケージがプライベートの場合は、そのコンテンツもすべてプライベートになります。
{: shortdesc}

1. すべてのユーザーでパッケージを共有します。

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. パッケージの `publish` プロパティーを表示して、このプロパティーが true になっていることを確認します。

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **出力例**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. パッケージの説明を表示し、他のユーザーにパッケージの完全修飾名を伝えます。これにより、他のユーザーがパッケージのバインドやパッケージ内のアクションの呼び出しを行えるようになります。 完全修飾名には名前空間が含まれます。この例では、`myNamespace` が名前空間です。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **出力例**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## アクションの環境変数
{: #actions_envvars}

アクション環境には、実行中のアクションに固有の環境変数がいくつかあります。 これらのプロパティーは、すべてのサポート対象ランタイムのシステム環境で利用できます。 これらのプロパティーを使用すると、アクションで REST API を介して資産をプログラマチックに処理したり、アクションに割り当てられた時間を使い切りそうなことを知らせる内部アラームを設定したりできます。
{: shortdesc}

| プロパティー | 説明 |
| -------- | ----------- |
| `__OW_API_HOST` | このアクションを実行しているデプロイメントの API ホスト。 |
| `__OW_API_KEY` | アクションを呼び出すサブジェクトの API キー。 この変数は、CF ベースのクラシック名前空間専用です。|
| `__OW_NAMESPACE` | 名前空間 ID (GUID)。CF ベースのクラシック名前空間の場合、この ID は組織名とスペース名から構成されます。|
| `__OW_NAMESPACE_CRN` | 名前空間のクラウド・リソース名 ([CRN](/docs/overview?topic=overview-crn))。CRN は、IAM 対応の名前空間に限り使用できます
| `__OW_ACTION_NAME` | 実行中のアクションの完全修飾名。 |
| `__OW_IAM_NAMESPACE_API_KEY` | IAM 対応の名前空間の API キー。使用法については、[アクセス・ポリシーの設定](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access)を参照してください。|
| `__OW_IAM_API_URL` | API キーからのトークンの取得などの IAM 操作で使用されるサービス・エンドポイント。この変数は、IAM 対応の名前空間に限り使用できます |
| `__OW_ACTIVATION_ID` | 実行中のアクション・インスタンスのアクティベーション ID。 |
| `__OW_DEADLINE` | このアクションが期間割り当て量全体を消費する概算時間 (エポック・ミリ秒)。 |

### アクション環境変数をアプリに組み込む
{: #actions_envvars_app}

アクションの値を表示するには、アプリのコード内に値の表示形式を指定し、結果として値を出力します。

**Python の例**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

アクション内のコードを更新して起動すると、アクションの完全修飾名が結果に含まれています。
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}



