---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: actions, serverless, ruby

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

# Ruby アクションの作成
{: #creating-ruby-actions}

以下のセクションでは、単一 Ruby アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。 

Ruby アクションの実行には Ruby 2.5 を使用します。このランタイムを使用するには、アクションの作成時または更新時に `ibmcloud fn` CLI パラメーター `--kind ruby:2.5` を指定します。

## Ruby アクションの作成と呼び出し
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

アクションは、単にトップレベルの Ruby メソッドです。

例えば、`hello.rb` というファイルを作成します。

1. 以下のコードを `hello.rb` という名前のファイルに保存します。

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby アクションは、常に Hash (辞書のようなコレクション) を取り込んで Hash を返します。
    * このアクションの入り口メソッドはデフォルトで `main` になっていますが、`ibmcloud fn` CLI を使用してアクションを作成する際、`--main` フラグを指定して入り口メソッドを指定することができます。

2. `helloRuby` というアクションを作成します。

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    出力例:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Ruby アクションのパッケージ
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

PHP アクション、およびその他のファイルや依存パッケージを .zip ファイルにパッケージ化することができます。 例えば、`helper.rb` という 2 番目のファイルを組み込んでアクションをパッケージすることもできます。

1. ソース・ファイルを含めたアーカイブを作成します。 **注**: エントリー・ポイントを含むソース・ファイルは `main.rb` という名前でなければなりません。

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. アクションを作成します。

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

バンドルに含まれているデフォルトの gem に加えて、`mechanize` と `jwt` という gem もあります。
zip を使用したアクションですべての依存項目をパッケージするだけであれば、任意の gem を使用できます。
