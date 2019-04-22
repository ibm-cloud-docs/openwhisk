---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

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

# Ballerina アクションの作成
{: #ballerina-actions}

以下のセクションでは、単一 Ballerina アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。 アクションとは、JSON オブジェクトを受け取って返す最上位の Ballerina 機能のことです。 

Ballerina アクションを実行するために、Ballerina [0.990.2](https://ballerina.io/downloads) を使用します。実行可能ファイルを生成するために、互換バージョンのコンパイラーをローカル環境に用意しておく必要があります。Ballerina コンパイラーがないと、アクションを作成できません。

## Ballerina アクションの作成と呼び出し

**始めに:** 以下のソース・コードで `hello.bal` というファイルを作成します。

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

このアクションの入り口メソッドはデフォルトで `main` になります。`wsk` CLI でアクションを作成するときに、`--main` を使用してこの変数を指定できます。 

**注:** Ballerina コンパイラーは、実行可能ファイルを生成する時に `main` という関数が存在することを想定しているので、ソース・ファイルに `main` というプレースホルダーを組み込む必要があります。

`hello` というアクションを作成するには、以下の手順を実行します。

1. .balx ファイルを生成します。
  ```
  ballerina build hello.bal
  ```
{: pre}

2. .balx ファイルを使用してアクションを作成します。
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. CLI はまだ、ソース・ファイルの拡張子からアクションのタイプを判別していません。種類を明示的に指定することが必要です。`.balx` ソース・ファイルの場合、アクションは Ballerina 0.990.2 ランタイムを使用して実行されます。
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

出力例:
```json
{
  "greeting": "Hello World!"
      }
```
{: screen}
