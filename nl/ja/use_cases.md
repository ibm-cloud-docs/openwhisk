---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# 一般的なユース・ケース
{: #use_cases}

{{site.data.keyword.openwhisk_short}} が提供する実行モデルは、さまざまなユース・ケースをサポートします。 以下のセクションで、典型的な例を示します。 サーバーレス・アーキテクチャーについての詳しい解説、ユース・ケース例、長所と短所、および実装最良事例については、[Martin Fowler のブログにある Mike Roberts の優れた記事](https://martinfowler.com/articles/serverless.html){: external}をお読みください。
{: shortdesc}

## マイクロサービス
{: #use_cases_microservices}

マイクロサービス・ベースのソリューションは、その利点にもかかわらず、主流のクラウド・テクノロジーを使用して構築するのは依然として難しく、複雑なツールチェーンの制御やビルド・パイプラインと運用パイプラインの分離が必要とされることも少なくありません。 小規模のアジャイル・チームは、インフラストラクチャーや運用の複雑さ (フォールト・トレランス、ロード・バランシング、自動スケーリング、ロギングなど) で時間を無駄にしすぎています。こうしたチームは、対象の問題の解決に最適で、かつ、既に理解して慣れ親しんでいるプログラミング言語を使用して、合理化された、付加価値のあるコードを開発する方法を求めています。

{{site.data.keyword.openwhisk_short}} のモジュラー性と本質的な拡張容易性は、小さなロジック断片をアクションに実装するのに理想的です。 {{site.data.keyword.openwhisk_short}} アクションは互いに独立しており、{{site.data.keyword.openwhisk_short}} でサポートされている多様な言語を使用して実装可能であり、さまざまなバックエンド・システムにアクセスします。 各アクションは、個別にデプロイして管理でき、他のアクションとは独立してスケーリングされます。 アクション間の接続性は、{{site.data.keyword.openwhisk_short}} によって、ルール、シーケンス、および命名規則の形で提供されています。 このタイプの環境は、マイクロサービス・ベースのアプリケーションに適しています。

{{site.data.keyword.openwhisk_short}} が支持されているもう 1 つの重要な根拠は、災害復旧構成でのシステムのコストです。 コンテナーまたは Cloud Foundry ランタイムを使用する 10 個のマイクロサービスがあるという想定で、PaaS または CaaS を使用したマイクロサービスと、{{site.data.keyword.openwhisk_short}} を使用した場合とを比較します。 この比較は、単一のアベイラビリティー・ゾーンで継続的に実行する 10 個の課金対象プロセスに相当します。2 つのアベイラビリティー・ゾーンにまたがって実行する場合は 20 個、2 つの地域にまたがってそれぞれ 2 つのゾーンで実行する場合は 40 個に相当します。{{site.data.keyword.openwhisk_short}} では同じ目標を達成するために、コストの追加なしで、アベイラビリティー・ゾーンおよび地域をいくつでもまたがって実行することができます。

## Web アプリケーション
{: #use_cases_webapps}

{{site.data.keyword.openwhisk_short}} は、そのイベント・ドリブン特性により、ユーザー向けアプリケーションにいくつかの利点を提供するのに対して、ユーザーのブラウザーからの HTTP 要求はイベントとしての役目を果たします。 {{site.data.keyword.openwhisk_short}} アプリケーションは計算能力を使用し、ユーザー要求にサービス提供しているときにのみ料金が請求されます。 アイドル・スタンバイや待機モードは存在しません。 この機能により、{{site.data.keyword.openwhisk_short}} では、従来のコンテナーまたは Cloud Foundry アプリケーションと比較して、コストがかなり低くなります。 これらのツールのどちらにも、インバウンドのユーザー要求を待機するアイドル時間があり、そうした「スリープ中」の時間はすべて課金対象になります。

{{site.data.keyword.openwhisk_short}} では、完全な Web アプリケーションを作成して実行できます。 HTML、JavaScript、および CSS などのサイト・リソースのためにホスティングしている静的ファイルとサーバーレス API を結合するということは、完全にサーバーレスな Web アプリケーションを構築できることを意味します。 ホストされた {{site.data.keyword.openwhisk_short}} 環境の運用の単純さは、全く何も運用する必要がないことです。 {{site.data.keyword.openwhisk_short}} は {{site.data.keyword.cloud_notm}} でホストされるため、Node.js Express やその他の従来型サーバー・ランタイムの立ち上げや運用と比較したとき、それが大きな利点になります。

## IoT
{: #use_cases_iot}

モノのインターネット (IoT) のシナリオでは、その性質上、センサーで駆動されることがよくあります。 例えば、一定の気温を超えたセンサーへの対応が必要になった場合に、{{site.data.keyword.openwhisk_short}} のアクションが起動されることがあります。 IoT の相互作用は通常ステートレスで、自然災害、大きな嵐、交通渋滞などの大規模な自然発生イベントにおいて、負荷が高まる可能性があります。 通常の作業負荷は小さいが、予測可能な応答時間で迅速にスケーリングする必要がある弾力性のあるシステムが必要になります。 したがって、システムへの事前警告なしで、多くの同時イベントを処理する機能が求められます。 従来のサーバー・アーキテクチャーを使用して、これらの要件に合うシステムを構築することは困難です。 パワーが足りずにトラフィックのピーク負荷を処理できないか、オーバープロビジョンによりコストが高くつくか、のいずれかの傾向があるためです。

従来のサーバー・アーキテクチャーを使用する IoT アプリケーションを実装することは可能です。 ただし、多くの場合、異なるサービスとデータ・ブリッジの組み合わせには、ハイパフォーマンスで柔軟なパイプラインが必要となります。 それは、IoT デバイスからクラウド・ストレージ、分析プラットフォームまでにわたります。 特定のソリューション・アーキテクチャーを実装して微調整するためにプログラム可能であることが、事前構成されているブリッジに欠けていることがよくあります。パイプラインは多様で、一般的に (特に IoT では) データ融合に関する標準化がないことから、パイプラインでカスタムのデータ変換を必要とする環境がよく見受けられます。 このようなカスタム・データ変換は、フォーマット変換、フィルタリング、または拡張に適用されます。 {{site.data.keyword.openwhisk_short}} は、そういった変換を「サーバーレス」な方法で実装するのに適した優れたツールであり、弾力性のある完全管理のクラウド・プラットフォームでカスタム・ロジックがホストされます。

{{site.data.keyword.openwhisk_short}}、Node-RED、Cognitive、その他のサービスを使用するサンプル IoT アプリケーションが、[Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external} にあります。

![IoT ソリューション・アーキテクチャーの例](images/IoT_solution_architecture_example.png)

## API バックエンド
{: #use_cases_backend}

サーバーレス・コンピューティング・プラットフォームは、サーバーなしで API を素早く作成する方法を開発者に提供します。 {{site.data.keyword.openwhisk_short}} では、アクションに対する REST API の自動生成がサポートされます。 [{{site.data.keyword.openwhisk_short}} フィーチャー](/docs/openwhisk?topic=cloud-functions-apigateway)を使用すると、{{site.data.keyword.openwhisk_short}} API ゲートウェイを介して、アクションの許可 API キーなしで、POST 以外にも HTTP メソッドを使用してアクションを呼び出すことができます。 この機能は、API を外部利用者に公開するだけでなく、マイクロサービス・アプリケーションの作成にも役立ちます。

さらに、{{site.data.keyword.openwhisk_short}} アクションは、適した API 管理ツール ([IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} など) に接続できます。 他のユース・ケースと同様に、スケーラビリティーなどのサービスの品質に関するすべての考慮事項が当てはまります。

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} は、REST API を通して {{site.data.keyword.openwhisk_short}} アクションを使用するサンプル・アプリです。

[API バックエンドとしてのサーバーレスの使用](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}の説明を含む例を参照してください。

## モバイル・バックエンド
{: #use_cases_mobile}

多くのモバイル・アプリケーションでは、サーバー・サイドのロジックが必要です。 しかし、モバイル開発者は通常、サーバー・サイドのロジックを管理する経験がなく、デバイス上で実行されるアプリケーションに注目しています。 この開発の目標は、サーバー・サイドのバックエンドとして {{site.data.keyword.openwhisk_short}} を使用することで簡単に達成され、優れたソリューションとなります。 また、サーバー・サイド Swift のサポートが組み込まれていることにより、開発者は既に持っている iOS プログラミング・スキルを再利用できます。 モバイル・アプリケーションでは多くの場合、負荷パターンが予期不能なため、{{site.data.keyword.cloud}} のような {{site.data.keyword.openwhisk_short}} ソリューションが使用されます。 このソリューションでは、事前にリソースをプロビジョンする必要がなく、作業負荷のほぼすべての需要に合わせてスケールすることができます。

## データ処理
{: #use_cases_data}

現在使用可能なデータ量に加えて、アプリケーション開発では、新規データを処理する能力が必要とされ、新規データに対応できることも示唆されます。 この要件には、構造化されたデータベース・レコードの処理と、構造化されていない文書、イメージ、またはビデオの処理の両方が含まれます。 {{site.data.keyword.openwhisk_short}} をシステム提供フィードまたはカスタム・フィードで構成して、データの変更に反応するようにしたり、データのフィードが着信したときに自動的にアクションを実行するようにしたりできます。 アクションは、変更の処理、データ・フォーマットの変換、メッセージの送受信、その他のアクションの呼び出し、および各種データ・ストアの更新を行うようにプログラムすることができます。 サポートされるデータ・ストアには、SQL ベースのリレーショナル・データベース、メモリー内データ・グリッド、NoSQL データベース、ファイル、メッセージング・ブローカー、およびその他の各種システムが含まれます。 {{site.data.keyword.openwhisk_short}} のルールとシーケンスは、プログラミングなしで処理パイプラインで変更を行う柔軟性を提供し、これは、単純な構成更新によって実行されます。 データ・ストアのオプションと少ない管理保守により、{{site.data.keyword.openwhisk_short}} ベースのシステムは高度にアジャイルであり、変化する要件に容易に適応できます。

## コグニティブ
{: #use_cases_cognitive}

コグニティブ・テクノロジーを {{site.data.keyword.openwhisk_short}} と効果的に結合して、強力なアプリケーションを作成することができます。 例えば、IBM Alchemy API および Watson Visual Recognition を {{site.data.keyword.openwhisk_short}} と共に使用して、視聴せずにビデオから有用な情報を自動的に抽出することができます。 このテクノロジーは、前述の[データ処理](#use_cases_data)ユース・ケースを「コグニティブ」に拡張するものです。 コグニティブ・サービスと組み合わせたボット機能の実装も、{{site.data.keyword.openwhisk_short}} の活用として挙げられます。

提供されているサンプル・アプリケーション [Dark Vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external} は、まさにそれを行います。 このアプリケーションでは、ユーザーが Dark Vision Web アプリケーションを使用してビデオまたはイメージをアップロードすると、それが {{site.data.keyword.cloudant_short_notm}} DB に保管されます。 ビデオがアップロードされると、{{site.data.keyword.openwhisk_short}} は {{site.data.keyword.cloudant_short_notm}} 変更 (トリガー) を listen することによって、その新規ビデオを検出します。 {{site.data.keyword.openwhisk_short}} はその後、ビデオ抽出アクションを起動します。 抽出の実行中に、フレーム (イメージ) が生成されて {{site.data.keyword.cloudant_short_notm}} に保管されます。 次に、それらのフレームは Watson Visual Recognition を使用して処理され、同じ {{site.data.keyword.cloudant_short_notm}} DB に結果が保管されます。 結果は Dark Vision Web アプリケーションまたは iOS アプリケーションを使用して表示できます。 {{site.data.keyword.cloudant_short_notm}} に加えて{{site.data.keyword.cos_full_notm}}も使用できます。ビデオおよびイメージのメタデータは {{site.data.keyword.cloudant_short_notm}} に、メディア・ファイルは{{site.data.keyword.cos_full_notm}}に保管されます。

## Kafka または {{site.data.keyword.messagehub}} を使用したイベント処理
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} は、Kafka、{{site.data.keyword.messagehub_full}} (Kafka ベース)、およびその他のメッセージング・システムと組み合わせて理想的に使用されます。 こうしたシステムのイベント・ドリブンの特性により、メッセージを処理するためにイベント・ドリブンのランタイムが必要になります。 このランタイムは、ビジネス・ロジックをこれらのメッセージに適用できます。これはまさに、{{site.data.keyword.openwhisk_short}} がフィード、トリガー、アクションを使用して実現していることです。 Kafka と {{site.data.keyword.messagehub}} は多くの場合、大量の予測不能な作業負荷に使用され、メッセージのコンシューマーは即座に拡張可能であることが要求されます。 これもまた、{{site.data.keyword.openwhisk_short}} に最適な状況です。 {{site.data.keyword.openwhisk_short}} には、[Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams) パッケージで提供されるメッセージをコンシュームするとともにパブリッシュする機能が組み込まれています。



