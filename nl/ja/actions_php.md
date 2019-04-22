---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: actions, serverless, php

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

# PHP アクションの作成
{: #creating-php-actions}

以下のセクションでは、単一 PHP アクションの作成と呼び出し、およびそのアクションへのパラメーターの追加について説明します。
{: shortdesc}

## PHP アクションの作成と呼び出し
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

アクションは、単にトップレベルの PHP 関数です。 PHP のアクションを作成するには以下のようにします。

1. 以下のコードを `hello.php` という名前のファイルに保存します。

    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * PHP アクションは常に連想配列を取り込み、連想配列を返します。
    * このアクションの入り口メソッドはデフォルトで `main` になっていますが、`ibmcloud fn` CLI を使用してアクションを作成する際、`--main` フラグを指定して入り口メソッドを指定することができます。

2. `helloPHP` というアクションを作成します。

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    アクションのタイプは、ソース・ファイルの拡張子を使用して判別されます。`.php` ソース・ファイルの場合、アクションは PHP 7.3 ランタイムを使用して実行されます。 PHP ランタイムについて詳しくは、[ランタイム](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)を参照してください。

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    出力例:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## .zip ファイルへの PHP アクションのパッケージ化
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

PHP アクション、およびその他のファイルや依存パッケージを .zip ファイルにパッケージ化することができます。 例えば、`helper.php` という 2 番目のファイルを組み込んでアクションをパッケージすることもできます。

1. ソース・ファイルを含めたアーカイブを作成します。 **注**: エントリー・ポイントを含むソース・ファイルは `index.php` という名前でなければなりません。

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. アクションを作成します。

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

