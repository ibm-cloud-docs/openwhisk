---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# はじめに
{: #getting_started}

{{site.data.keyword.openwhisk}} では、任意のプログラミング言語を使用して、スケーラブルな方法で、アプリのロジックを実行する軽量のコードを作成することができます。アプリケーションからの HTTP ベースの API 要求により、または {{site.data.keyword.Bluemix_notm}} サービスおよびサード・パーティーのイベントへの応答として自動的に、コードをオンデマンドで実行させることができます。この Function-as-a-Service (Faas) プログラミング・プラットフォームは、オープン・ソース・プロジェクトである Apache OpenWhisk を基盤としています。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} はサーバーレスであるため、使用可能な言語が制限されず、バックエンド・インフラストラクチャーを明示的にプロビジョンするために時間を費やす必要もありません。自動スケーリング、高可用性、更新、保守などを心配することなく、アプリ・ロジックの作成に集中できます。IBM は、すぐに使用可能なハードウェア、ネットワーキング、ソフトウェア管理、ロード・バランシング、プラグインなどを提供します。お客様はコードを用意するだけです。

## アクションの操作
{: #creating_actions}
{: #openwhisk_actions}

{{site.data.keyword.openwhisk}} を使用して、アクションと呼ばれる 1 つの特定のタスクを実行するように設定した、ステートレス・コード・スニペットを作成することができます。
{:shortdesc}

**アクションとは**

アクションとは、呼び出し可能な小規模なコード断片、またはイベントの応答として自動的に実行されるように設定した小規模なコード断片です。いずれの場合も、実行されるたびに、固有のアクティベーション ID で識別されるレコードが生成されます。アクションの入力と結果は、キーと値のペアで示されます。キーはストリングで、値は有効な JSON 値です。アクションは任意の言語で作成できます。これはソース・コードまたは Docker イメージとしてサービスに渡されます。アクション・コードは、Cloud Functions API、CLI、または iOS SDK によって直接呼び出されると実行されます。IBM Cloud またはサード・パーティーのサービスからのイベントの応答としてアクションを自動的に実行させることもできます。

**アクションを使用するとよい理由**

アクションを使用することで、コードの実行時間が限定されるため、オーバーヘッド・コストが削減されます。

例えば、アクションを使用して、画像内の顔を検出したり、データベースの変更に応答したり、一連の API 呼び出しを集約したり、ツイートを投稿したりできます。

**一度に複数のアクションを使用できますか?**

はい。 複数のアクションを使用して複数の他のアクションを呼び出したり、複数のアクションを並べてシーケンスを作成したりできます。そのように機能させるには、あるアクションの出力が別のアクションの入力となり、次いでそのアクションの出力が別のアクションのトリガーとして使用される、などといった処理が必要になります。作成する複数のアクションのグループを束ねて、1 つのパッケージを形成することさえ可能です。パッケージを利用するなら、そのパッケージを呼び出すことで一般的なアクションやシーケンスを再使用することができるので、アクションやシーケンスを再構成する必要がなくなります。

いずれかのオプションをクリックして、開始します。

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="アイコンをクリックして {{site.data.keyword.openwhisk_short}} で素早く開始します。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="アクションの作成" title="アクションの作成" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="{{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ" title="{{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="プラットフォーム・アーキテクチャーの確認" title="プラットフォーム・アーキテクチャーの確認" shape="rect" coords="326, -10, 448, 218" />
</map>

## GUI でのアクションの作成
{: #openwhisk_start_hello_world}

{{site.data.keyword.openwhisk_short}} の入門として、HelloWorld クイック・スタート・テンプレートを使用してみてください。

1.  [{{site.data.keyword.Bluemix_notm}} **カタログ** ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk) で Functions ダッシュボードにアクセスします。

2. **「作成の開始」**>**「クイック・スタート・テンプレート」**をクリックし、**「Hello World」**テンプレートを選択します。

3. **「パッケージ名」**フィールドに固有の名前を入力して、アクションのパッケージを作成します。

4. **「アクション helloworld (Action helloworld)」**セクションでドロップダウンからランタイムを選択します。テンプレートをデプロイする前に、使用可能な各ランタイムでサンプル・アクションのコードをプレビューできます。

5. **「デプロイ」**をクリックします。 アクションが作成されました。

6. **「起動」**をクリックしてアクションを実行します。 アクションを手動で呼び出すと、アクションで定義されたアプリケーション・ロジックが実行されます。 **「アクティベーション (Activations)」**パネルに、アクションによって生成された「Hello stranger!」という挨拶が表示されます。

7. オプション:**「入力の変更」**をクリックして、アクションを変更するか、独自のアクションを試行します。それから、**「起動」**をクリックして、更新したアクションを実行します。**「アクティベーション」**パネルに結果が表示されます。この処理を必要な回数だけ繰り返すことができます。

おつかれさまでした。 最初のアクションの作成が終了しました。 このアクションをクリーンアップするには、オーバーフロー・メニューをクリックし、**「アクションの削除」**を選択します。

## CLI でのアクションの作成
{: #openwhisk_start_hello_world_cli}

HelloWorld JavaScript サンプル・コードを使用して、素早く稼働させることができます。 この例では、基本的な `hello` アクションを作成します。このアクションを手動で呼び出して、アプリケーション・ロジックを実行できます。

1. [{{site.data.keyword.openwhisk_short}} CLI プラグインをセットアップします](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)。

2. 以下のコードを **hello.js** という名前のファイルに保存します。

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. `hello` アクションを作成します。

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. パラメーターを何も渡さずにアクションを呼び出します。

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    出力:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. アプリケーション・ロジックをテストするために、name パラメーターを渡して再度アクションを呼び出します。

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    出力:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

おつかれさまでした。 最初のアクションの作成が終了しました。 このアクションをクリーンアップするには、`ibmcloud fn action delete hello` を実行します。

## 次の作業
{: #getting_started_next_steps}

テンプレートの初回デプロイが完了しました。次の作業は何でしょうか? 次のことが可能です。

* [用語](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology)について理解します。
* [独自のアクション](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)の使用を開始します。
* [パッケージ](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)内のアクションの編成について学習します。
* 上級オプション: [サーバーレス REST API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) を作成します。
