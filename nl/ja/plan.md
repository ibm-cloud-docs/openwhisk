---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# サーバーレス・アプリの計画
{: #plan}

関数の作成を開始する前に、作成中に決定する必要がある内容について確認してください。
{: shortdesc}

## ランタイム・サポートの確認
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} には、コードを実行するために標準的な[ランタイム](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)が備えられています。 しかし、{{site.data.keyword.openwhisk_short}} はサーバーレスであるため、使用可能なランタイムに制限されません。 独自の[カスタム Docker イメージ](/docs/openwhisk?topic=cloud-functions-actions#actions-docker)を作成してコードをパッケージ化すれば、カスタム・ランタイムを作成できます。
{: shortdesc}



## コードの設計
{: #plan_architect}

{{site.data.keyword.openwhisk}} で実行するために、既存のコードをいくらか調整しなければならないことがあります。 まだコードがない場合は、作成する際に以下の点に留意してください。
{: shortdesc}

1. コードをコンポーネント化する。

    既存のアプリをサーバーレスにする場合は、アプリを細分化する必要があるかどうかを検討してください。 各関数には、実行したいコードの一連のトリガーが含まれています。 例えば、GitHub で問題が生じた場合にはこの JavaScript コードを実行するなどのトリガーです。 アプリにこのようなイベントまたはアクションが複数含まれている場合は、別々の関数に分けることを検討してください。

2. フレームワークではなく {{site.data.keyword.cloud_notm}} サービスを使用する。

    フレームワークを使用して実行時に機能を関数内で使用できるようにする代わりに、{{site.data.keyword.cloud}} サービスを使用できます。 フレームワークを利用して実行できる一般的なタスクの多くは、{{site.data.keyword.cloud}} 上でサービスとして提供されています。
    {: shortdesc}

    例えば、フレームワークを使用して認証を行う代わりに、{{site.data.keyword.appid_full}} を試してください。 外部ファイル・ストレージが必要な場合は、{{site.data.keyword.cos_full}} を試してください。

    組み込みたい機能が {{site.data.keyword.cloud}} 上でサービスとして提供されていない場合は、その機能をフィードや API に組み込むこともできるので、フレームワークは常に不要です。

3. [コードがシステム限度を満たしていることを確認する。](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    コードを実行するには、コードがシステム限度内に収まっている必要があります。 例えば、パッケージ化されたコードが 48 MB より大きい場合は、細分化するか、Docker イメージとしてパッケージ化する必要があります。

    多くのサード・パーティーのモジュール、ネイティブ・ライブラリー、外部ツールが含まれたアプリケーションでは、この制限に達する可能性があります。 48 MB より大きい .zip パッケージまたは .jar パッケージのアクションを作成する場合は、依存関係を含めてランタイム・イメージを拡張します。その後、単一のソース・ファイルか 48 MB より小さいアーカイブを使用します。例えば、必要な共有ライブラリーが含まれているカスタム Docker ランタイムをビルドすれば、依存関係はアーカイブ・ファイルに存在する必要がありません。 プライベート・ソース・ファイルは、引き続き、アーカイブにバンドルし、実行時に注入できます。

4. コードに注入する必要があるパラメーターを決定する。

    サーバーレス・アクションでは、パラメーターをアクションに追加することによってデータが供給されます。 パラメーターは、main サーバーレス関数の引数として宣言されます。 このようなパラメーターとして最も一般的なものはサービスの資格情報です。しかし、トリガーを変えてコードを再使用できるような値はすべてパラメーターになり得ます。

5. [関数内でコードを使用するための構造上の要件を確認する。](/docs/openwhisk?topic=cloud-functions-prep)

    既存のアプリを使用するのか、スクリプトを作成して使用するのかにかかわらず、{{site.data.keyword.openwhisk}} で実行するには、コードをいくらか調整しなければならない可能性があります。 コード自体がいくつかの構造上の要件 (入力パラメーターや出力結果など) を満たす必要があります。すべての依存関係を含む単一のファイルにパッケージすることが必要な場合もあります。







## イベント・ソースの決定
{: #plan_source}

どのようなイベントでコードの実行をトリガーしたいかという観点で関数について考えてください。GitHub リポジトリーにコミットされるたびに実行したいコードもあるでしょう。 Cloudant データベースが更新されるたびに実行したいコードもあるかもしれません。
{: shortdesc}

関数をトリガーするイベントを選択したら、既に使用できる状態のパッケージを確認します。 その 1 つを使用して関数を簡単に作成できることがあります。 ない場合は、イベント・ソース用に独自のパッケージを作成できます。

選択したイベントに応じて、コードを見直し、いくらか修正しなければならないこともあります。


## 複数環境へのデプロイメントの編成
{: #plan_environments}

開発環境、ステージング環境、実稼働環境など、複数環境に関数をデプロイする方法を決めます。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} は Cloud Foundry ベースのサービスであるため、{{site.data.keyword.cloud_notm}} で提供されている Cloud Foundry の組織やスペースで関数のデプロイメントを管理できます。 これらの環境間で関数を編成するには、関数 1 つにつき 1 つの組織を作成することができます。それから、必要な環境ごとにスペースを作成します。 代わりに、環境ごとに組織を 1 つ作成し、関数 1 つにつき 1 つのスペースを作成するという方法もあります。 組織とスペースをどのように編成するかにかかわらず、関数エンティティーを効果的に管理するために使用できる構造を選択してください。

[名前空間](/docs/openwhisk?topic=cloud-functions-namespaces)を使用してリソースを分離することもできます。 各 {{site.data.keyword.cloud_notm}} スペースには、デフォルトで Open Whisk 名前空間が含まれています。 名前空間内でアクションやトリガーなどのエンティティーをグループ化してから、Identity and Access (IAM) ポリシーを作成して、そのグループのユーザー許可を管理できます。




