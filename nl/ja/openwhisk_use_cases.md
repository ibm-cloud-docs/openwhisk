---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 一般的なユース・ケース
{: #openwhisk_common_use_cases}

{{site.data.keyword.openwhisk_short}} が提供する実行モデルは、さまざまなユース・ケースをサポートします。 以下のセクションで、典型的な例を示します。 サーバーレス・アーキテクチャーについての詳しい解説、ユース・ケース例、長所と短所、および実装最良事例については、[Martin Fowler のブログにある Mike Roberts の優れた記事](https://martinfowler.com/articles/serverless.html)をお読みください。
{: shortdesc}

## マイクロサービス
{: #openwhisk_common_use_cases_microservices}

マイクロサービス・ベースのソリューションは、その利点にもかかわらず、主流のクラウド・テクノロジーを使用して構築するのは依然として難しく、複雑なツールチェーンの制御やビルド・パイプラインと運用パイプラインの分離が必要とされることも少なくありません。 小規模なアジャイルのチームが、インフラストラクチャーや運用の複雑な事柄 (フォールト・トレランス、ロード・バランシング、自動スケーリング、ロギングなど) に過大な時間をかけています。 こうしたチームは、対象の問題の解決に最適で、かつ、既に理解して慣れ親しんでいるプログラミング言語を使用して、合理化された、付加価値のあるコードを開発する方法を求めています。

{{site.data.keyword.openwhisk_short}} のモジュラー性と本質的な拡張容易性は、小さなロジック断片をアクションに実装するのに理想的です。 {{site.data.keyword.openwhisk_short}} アクションは互いに独立しており、{{site.data.keyword.openwhisk_short}} でサポートされている多様な言語を使用して実装可能であり、さまざまなバックエンド・システムにアクセスします。 各アクションは、個別にデプロイして管理でき、他のアクションとは独立してスケーリングされます。 アクション間の接続性は、{{site.data.keyword.openwhisk_short}} によって、ルール、シーケンス、および命名規則の形で提供されています。 このタイプの環境は、マイクロサービス・ベースのアプリケーションに適しています。

{{site.data.keyword.openwhisk_short}} が支持されているもう 1 つの重要な根拠は、災害復旧構成でのシステムのコストです。 コンテナーまたは CloudFoundry ランタイムを使用する 10 個のマイクロサービスがあるという想定で、PaaS または CaaS を使用したマイクロサービスと、{{site.data.keyword.openwhisk_short}} を使用した場合とを比較します。 この比較は、単一のアベイラビリティー・ゾーン (AZ) で継続的に実行する 10 個の課金対象プロセスに相当します。2 つの AZ にまたがって実行する場合は 20 個、2 つの地域にまたがってそれぞれ 2 つのゾーンで実行する場合は 40 個に相当します。 {{site.data.keyword.openwhisk_short}} では同じ目標を達成するために、コストの追加なしで、AZ および地域をいくつでもまたがってそれらを実行することができます。

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) は、{{site.data.keyword.openwhisk_short}} と CloudFoundry を活用して Twelve-Factor スタイルのアプリケーションを作成する、エンタープライズ・グレードのサンプル・アプリケーションです。 これは、ERP システムが稼働する環境をシミュレートすることを目的とした、スマートなサプライ・チェーン・マネジメント・ソリューションです。 サプライ・チェーン・マネージャーの可視性と俊敏性を向上させるためのアプリケーションを使用して、この ERP システムを強化します。

## Web アプリケーション
{: #openwhisk_common_use_cases_webapps}

{{site.data.keyword.openwhisk_short}} は、そのイベント・ドリブン特性により、ユーザー向けアプリケーションにいくつかの利点を提供するのに対して、ユーザーのブラウザーからの HTTP 要求はイベントとしての役目を果たします。 {{site.data.keyword.openwhisk_short}} アプリケーションは計算能力を使用し、ユーザー要求にサービス提供しているときにのみ料金が請求されます。 アイドル・スタンバイや待機モードは存在しません。 この機能により、{{site.data.keyword.openwhisk_short}} では、従来のコンテナーまたは CloudFoundry アプリケーションと比較して、コストがかなり低くなります。 従来のコンテナーおよび CloudFoundry アプリケーションでは、インバウンドのユーザー要求を待機して大部分の時間をアイドルで費やす可能性があり、そうした「スリープ中」のすべての時間も課金対象となります。

{{site.data.keyword.openwhisk_short}} では、完全な Web アプリケーションを作成して実行できます。 HTML、JavaScript、および CSS などのサイト・リソースのためにホスティングしている静的ファイルとサーバーレス API を結合するということは、完全にサーバーレスな Web アプリケーションを構築できることを意味します。 ホストされた {{site.data.keyword.openwhisk_short}} 環境の運用の単純さは、全く何も運用する必要がないことです。 {{site.data.keyword.openwhisk_short}} は {{site.data.keyword.Bluemix_notm}} でホストされるため、Node.js Express やその他の従来型サーバー・ランタイムの立ち上げや運用と比較したとき、それが大きな利点になります。

{{site.data.keyword.openwhisk_short}} を使用して Web アプリケーションを作成する方法については、以下の例を参照してください。
- [Web actions: Serverless Web Apps with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [Build a user-facing {{site.data.keyword.openwhisk_short}} application with {{site.data.keyword.Bluemix_notm}} and Node.js](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Serverless HTTP handlers with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #openwhisk_common_use_cases_iot}

モノのインターネット (IoT) のシナリオでは、その性質上、センサーで駆動されることがよくあります。 例えば、一定の気温を超えたセンサーへの対応が必要になった場合に、{{site.data.keyword.openwhisk_short}} のアクションが起動されることがあります。 IoT の相互作用は通常ステートレスで、自然災害、大きな嵐、交通渋滞などの大規模な自然発生イベントにおいて、負荷が高まる可能性があります。 通常の作業負荷は小さいが、予測可能な応答時間で迅速にスケーリングする必要がある弾力性のあるシステムが必要になります。 したがって、システムへの事前警告なしで、多くの同時イベントを処理する機能が求められます。 従来のサーバー・アーキテクチャーを使用して、これらの要件に合うシステムを構築することは困難です。 パワーが足りずにトラフィックのピーク負荷を処理できないか、オーバープロビジョンによりコストが高くつくか、のいずれかの傾向があるためです。

従来のサーバー・アーキテクチャーを使用する IoT アプリケーションを実装することは可能です。 ただし、多くの場合、異なるサービスとデータ・ブリッジの組み合わせには、ハイパフォーマンスで柔軟なパイプラインが必要となります。 それは、IoT デバイスからクラウド・ストレージ、分析プラットフォームまでにわたります。 特定のソリューション・アーキテクチャーを実装して微調整するにためにプログラム可能であることが必要であっても、事前構成されているブリッジにはそれが欠けていることがよくあります。 パイプラインは多様で、一般的に (特に IoT では) データ融合に関する標準化がないことから、パイプラインでカスタムのデータ変換を必要とする環境がよく見受けられます。 このようなカスタム・データ変換は、フォーマット変換、フィルタリング、または拡張に適用されます。 {{site.data.keyword.openwhisk_short}} は、そういった変換を「サーバーレス」な方法で実装するのに適した優れたツールであり、弾力性のある完全管理のクラウド・プラットフォームでカスタム・ロジックがホストされます。

{{site.data.keyword.openwhisk_short}}、NodeRed、Cognitive、およびその他のサービスを使用する、以下のサンプル IoT アプリケーションを参照してください。[Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c#.akt3ocjdt)

![IoT ソリューション・アーキテクチャーの例](images/IoT_solution_architecture_example.png)

## API バックエンド
{: #openwhisk_api_backend}

サーバーレス・コンピューティング・プラットフォームは、サーバーなしで API を素早く作成する方法を開発者に提供します。 {{site.data.keyword.openwhisk_short}} では、アクションに対する REST API の自動生成がサポートされます。 {{site.data.keyword.openwhisk_short}} の[試験的フィーチャー](./openwhisk_apigateway.html)を使用すると、{{site.data.keyword.openwhisk_short}} API ゲートウェイを介して、アクションの許可 API キーなしで、POST 以外の HTTP メソッドを使用してアクションを起動することができます。 この機能は、API を外部利用者に公開するだけでなく、マイクロサービス・アプリケーションの作成にも役立ちます。

さらに、{{site.data.keyword.openwhisk_short}} アクションは、適した API 管理ツール ([IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) など) に接続できます。 他のユース・ケースと同様に、スケーラビリティーおよび他の QoS (Qualities of Services) に関するすべての考慮事項が当てはまります。

[Emoting](https://github.com/l2fprod/openwhisk-emoting) は、REST API を通して {{site.data.keyword.openwhisk_short}} アクションを使用するサンプル・アプリです。

[API バックエンドとしてのサーバーレスの使用](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples)の説明を含む例を参照してください。

## モバイル・バックエンド
{: #openwhisk_common_use_cases_mobile}

多くのモバイル・アプリケーションでは、サーバー・サイドのロジックが必要です。 しかし、モバイル開発者は通常、サーバー・サイドのロジックを管理する経験がなく、デバイス上で実行されるアプリケーションに注目しています。 この開発の目標は、サーバー・サイドのバックエンドとして {{site.data.keyword.openwhisk_short}} を使用することで簡単に達成され、優れたソリューションとなります。 また、サーバー・サイド Swift のサポートが組み込まれていることにより、開発者は既に持っている iOS プログラミング・スキルを再利用できます。 モバイル・アプリケーションでは多くの場合、負荷パターンが予期不能なため、{{site.data.keyword.Bluemix}} のような {{site.data.keyword.openwhisk_short}} ソリューションが使用されます。 このソリューションでは、事前にリソースをプロビジョンする必要がなく、作業負荷のほぼすべての需要に合わせてスケールすることができます。

[Skylink](https://github.com/IBM-Bluemix/skylink) は、{{site.data.keyword.openwhisk_short}}、IBM {{site.data.keyword.cloudant_short_notm}}、IBM Watson、および Alchemy Vision を活用した、ほぼリアルタイムでのイメージ分析により、iPad を使用してドローンを IBM Cloud に接続するサンプル・アプリケーションです。

[BluePic](https://github.com/IBM-Swift/BluePic) は、写真を撮影して他の BluePic ユーザーと共有するための、写真および画像共有アプリケーションです。 このアプリケーションは、{{site.data.keyword.openwhisk_short}}、{{site.data.keyword.cloudant_short_notm}}、イメージ・データ用の{{site.data.keyword.cos_full_notm}}を使用して、モバイル iOS 10 アプリケーション内で、Swift で作成された Kitura ベースのサーバー・アプリケーションを活用する方法を示します。 AlchemyAPI も {{site.data.keyword.openwhisk_short}} シーケンス内で使用され、イメージを分析し、イメージの内容に基づいてテキスト・タグを抽出し、最後にプッシュ通知をユーザーに送信します。

## データ処理
{: #data-processing}

現在使用可能なデータ量に加えて、アプリケーション開発では、新規データを処理する能力が必要とされ、新規データに対応できることも示唆されます。 この要件には、構造化されたデータベース・レコードの処理と、構造化されていない文書、イメージ、またはビデオの処理の両方が含まれます。 {{site.data.keyword.openwhisk_short}} をシステム提供フィードまたはカスタム・フィードで構成して、データの変更に反応するようにしたり、データのフィードが着信したときに自動的にアクションを実行するようにしたりできます。 アクションは、変更の処理、データ・フォーマットの変換、メッセージの送受信、その他のアクションの呼び出し、および各種データ・ストアの更新を行うようにプログラムすることができます。 サポートされるデータ・ストアには、SQL ベースのリレーショナル・データベース、メモリー内データ・グリッド、NoSQL データベース、ファイル、メッセージング・ブローカー、およびその他の各種システムが含まれます。 {{site.data.keyword.openwhisk_short}} のルールとシーケンスは、プログラミングなしで処理パイプラインで変更を行う柔軟性を提供し、これは、単純な構成更新によって実行されます。 データ・ストア・オプションと低い管理オーバーヘッドにより、{{site.data.keyword.openwhisk_short}} ベースのシステムは、高度にアジャイルで、変化する要件に簡単に適応可能になります。

[OpenChecks](https://github.com/krook/openchecks) プロジェクトは、光学式文字認識を使用して銀行口座への小切手の預金を処理するための {{site.data.keyword.openwhisk_short}} の使用方法を示す、PoC (概念検証) です。 これは、パブリック {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.openwhisk_short}} サービス上に構築されており、{{site.data.keyword.cloudant}} および {{site.data.keyword.cos_full_notm}}に依存します。 オンプレミスでは、CouchDB および OpenStack Swift を使用できます。 その他のストレージ・サービスには、FileNet や Cleversafe があります。 Tesseract が OCR ライブラリーを提供します。

## コグニティブ
{: #openwhisk_common_use_cases_cognitive}

コグニティブ・テクノロジーを {{site.data.keyword.openwhisk_short}} と効果的に結合して、強力なアプリケーションを作成することができます。 例えば、IBM Alchemy API および Watson Visual Recognition を {{site.data.keyword.openwhisk_short}} と共に使用して、視聴せずにビデオから有用な情報を自動的に抽出することができます。 このテクノロジーは、前述の[データ処理](#data-processing)ユース・ケースを「コグニティブ」に拡張するものです。 コグニティブ・サービスと組み合わせたボット機能の実装も、{{site.data.keyword.openwhisk_short}} の活用として挙げられます。

提供されているサンプル・アプリケーション [Dark Vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp) は、まさにそれを行います。 このアプリケーションでは、ユーザーが Dark Vision Web アプリケーションを使用してビデオまたはイメージをアップロードすると、それが {{site.data.keyword.cloudant_short_notm}} DB に保管されます。 ビデオがアップロードされると、{{site.data.keyword.openwhisk_short}} は {{site.data.keyword.cloudant_short_notm}} 変更 (トリガー) を listen することによって、その新規ビデオを検出します。 {{site.data.keyword.openwhisk_short}} はその後、ビデオ抽出アクションを起動します。 抽出の実行中に、フレーム (イメージ) が生成されて {{site.data.keyword.cloudant_short_notm}} に保管されます。 次に、それらのフレームは Watson Visual Recognition を使用して処理され、同じ {{site.data.keyword.cloudant_short_notm}} DB に結果が保管されます。 結果は Dark Vision Web アプリケーションまたは iOS アプリケーションを使用して表示できます。 {{site.data.keyword.cloudant_short_notm}} に加えて{{site.data.keyword.cos_full_notm}}も使用できます。ビデオおよびイメージのメタデータは {{site.data.keyword.cloudant_short_notm}} に、メディア・ファイルは{{site.data.keyword.cos_short}}に保管されます。

{{site.data.keyword.openwhisk_short}}、IBM Mobile Analytics、Watson を使用してトーン分析と Slack チャネルへのポストを行う[サンプル iOS Swift アプリケーション](https://github.com/gconan/BluemixMobileServicesDemoApp)が利用可能です。

## Kafka または {{site.data.keyword.messagehub}} を使用したイベント処理
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}} は、Kafka、{{site.data.keyword.messagehub_full}} (Kafka ベース)、およびその他のメッセージング・システムと組み合わせて理想的に使用されます。 こうしたシステムのイベント・ドリブンの特性により、メッセージを処理するためにイベント・ドリブンのランタイムが必要になります。 このランタイムは、ビジネス・ロジックをこれらのメッセージに適用できます。これはまさに、{{site.data.keyword.openwhisk_short}} がフィード、トリガー、アクションを使用して実現していることです。 Kafka と {{site.data.keyword.messagehub}} は多くの場合、大量の予測不能な作業負荷に使用され、メッセージのコンシューマーは即座に拡張可能であることが要求されます。 これもまた、{{site.data.keyword.openwhisk_short}} に最適な状況です。 {{site.data.keyword.openwhisk_short}} には、[openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka) パッケージで提供される、組み込みのメッセージ・コンシュームおよびメッセージ・パブリッシュの機能があります。

{{site.data.keyword.openwhisk_short}}、{{site.data.keyword.messagehub}}、および Kafka に[イベント処理シナリオを実装したサンプル・アプリケーション](https://github.com/IBM/openwhisk-data-processing-message-hub)が提供されています。
