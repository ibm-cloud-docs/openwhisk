---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# はじめに
{: #index}

{{site.data.keyword.openwhisk}} は、Apache OpenWhisk に基づいた、多言語 Functions-as-a-Service (FaaS) プログラミング・プラットフォームです。 {{site.data.keyword.openwhisk_short}} は、アクションと呼ばれる軽量のコードを開発者が作成できるようにします。アクションは、アプリケーション・ロジックをスケーラブルに実行します。 アクションは、Web アプリケーションまたはモバイル・アプリケーションからの HTTP ベースの API 要求に基づいてオンデマンドで発生するようにセットアップしたり、{{site.data.keyword.Bluemix_notm}} サービスおよびサード・パーティー・イベントからのイベント・ベースの要求への応答として自動的に発生するようにセットアップしたりできます。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} は、サーバーレスのイベント・ドリブン・プラットフォームであるため、サーバーを明示的にプロビジョンする必要はありません。 マイクロサービス、モバイル、IoT、およびその他多くのアプリケーションに取り組んでいる開発者は、自動スケーリング、高可用性、更新、および保守などの心配をする代わりに、アプリケーション・ロジックの作成に集中できます。 すぐに使用可能な自動スケーリングやロード・バランシングが備わっているということは、クラスターや http プラグインなどを手動で構成する必要がないということです。 ハードウェア、ネットワーク、およびソフトウェアの管理はすべて IBM が行います。 ユーザーが行うのは、コードを用意することだけです。

いずれかのオプションをクリックして、開始します。

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="アイコンをクリックして、すぐに {{site.data.keyword.openswhisk_short}} を開始します。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="アクションの作成" title="アクションの作成" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="{{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ" title="{{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="プラットフォーム・アーキテクチャーの確認" title="プラットフォーム・アーキテクチャーの確認" shape="rect" coords="326, -10, 448, 218" />
</map>


## GUI でのアクションの作成
{: #openwhisk_start_hello_world}

{{site.data.keyword.openwhisk_short}} の入門として、HelloWorld クイック・スタート・テンプレートを使用してみてください。

1.  [{{site.data.keyword.Bluemix_notm}} **カタログ** ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/?category=whisk) の**「Functions」**カテゴリーで、「Functions」をクリックします。

2. **「作成の開始」**>**「クイック・スタート・テンプレート (Quickstart templates)」**をクリックし、**「Hello World」**テンプレートを選択します。

5. アクションのコードを確認し、**「デプロイ」**をクリックしてアクションを作成します。 これで、`hello` というアクションができました。

6. **「起動」**をクリックしてアクションを実行します。 アクションを手動で呼び出すと、アクションで定義されたアプリケーション・ロジックが実行されます。 **「アクティベーション (Activations)」**パネルに、アクションによって生成された「Hello stranger!」という挨拶が表示されます。

おつかれさまでした。 最初のアクションの作成が終了しました。 このアクションをクリーンアップするには、オーバーフロー・メニューをクリックし、**「アクションの削除」**を選択します。

## CLI でのアクションの作成
{: #openwhisk_start_hello_world_cli}

HelloWorld JavaScript サンプル・コードを使用して、素早く稼働させることができます。 この例では、基本的な `hello` アクションを作成します。このアクションを手動で呼び出して、アプリケーション・ロジックを実行できます。

1. [{{site.data.keyword.openwhisk_short}} CLI プラグインをセットアップします](bluemix_cli.html)。

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

**次の作業**
* [定期的なイベントが生成されるたびに **hello** アクションを呼び出すアラーム・サービスの例を試してみる。](./openwhisk_packages.html#openwhisk_package_trigger)
* [サーバーレス REST API を作成する。](openwhisk_apigateway.html)
* [Cloudant など、{{site.data.keyword.Bluemix_notm}} サービス用にプリインストールされているアクション・パッケージを確認する。](cloudant_actions.html)
