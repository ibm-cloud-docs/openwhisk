---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: serverless, framework, getting started

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# サーバーレス・フレームワーク
{: #openwhisk_goserverless}

[Serverless Framework](https://serverless.com/) は、サーバーレス・アプリケーションを構築するためのオープン・ソース・フレームワークです。 開発者は、シンプルなマニフェスト・ファイルを使用して、サーバーレス機能を定義し、それらをイベント・ソースに接続し、アプリケーションが必要とするクラウド・サービスを宣言することができます。 このフレームワークは、こうしたサーバーレス・アプリケーションのクラウド・プロバイダーへのデプロイを処理します。 また、開発者による実動サービスのモニター、更新のロールアウト、および問題のデバッグの支援も可能にします。 また、フレームワークの機能を拡張するサード・パーティー・プラグインの活力あるエコシステムもあります。 OpenWhisk は、Serverless Framework を支えるテクノロジーです。
{:shortdesc}

OpenWhisk には、[Serverless Framework 向けの固有のプロバイダー・プラグイン](https://github.com/serverless/serverless-openwhisk)があります。 Serverless Framework を使用する開発者は、アプリケーションを任意の OpenWhisk プラットフォーム・インスタンス ({{site.data.keyword.Bluemix_notm}}、あるいは他のクラウドまたはプライベートでホストされる) にデプロイすることを選択できます。 複数プロバイダーのサポートは、プラットフォーム間でのアプリケーションの移動がはるかに容易であることを意味し、開発者がマルチクラウド・サーバーレス・アプリケーションを開発することを可能にします。

## はじめに
{: #openwhisk_goserverless_starting}

公式の Serverless Framework [OpenWhisk 入門ガイド](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/)。
* インストール、開発ワークフロー、ベスト・プラクティス、機能する OpenWhisk アプリケーションの作成とデプロイの詳細手順などが記述されているガイド。

OpenWhisk プロバイダー・プラグインと共に Serverless Framework を使用する方法を説明するビデオ [The Serverless Framework and OpenWhisk](https://youtu.be/GJY10W98Itc) を視聴してください。

## 資料
{: #openwhisk_goserverless_docs}

Serverless Framework と共に OpenWhisk を使用する方法に関する最新資料は[ここにあります](https://serverless.com/framework/docs/providers/openwhisk/)。

## サンプル
{: #openwhisk_goserverless_samples}
[Serverless Framework サンプル GitHub リポジトリー](https://github.com/serverless/examples)では、OpenWhisk を対象に、HTTP API、cron ベースのスケジューラー、チェーニング関数などの作成方法が示されています。

