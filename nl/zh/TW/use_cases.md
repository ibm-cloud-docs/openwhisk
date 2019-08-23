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


# 常見使用案例
{: #use_cases}

{{site.data.keyword.openwhisk_short}} 所提供的執行模型支援各種使用案例。下列各節包括一般範例。如需「無伺服器」架構、範例使用案例、正反雙方討論及實作最佳作的詳細討論，請閱讀出色的 [Martin Fowler 部落格上的 Mike Roberts 文章](https://martinfowler.com/articles/serverless.html){: external}。
{: shortdesc}

## 微服務
{: #use_cases_microservices}

儘管有其好處，以微服務為基礎的解決方案仍然很難使用主流雲端技術進行建置，這通常需要控制複雜的工具鏈、不同的建置及作業管線。小型和敏捷團隊會浪費太多時間來處理複雜的基礎架構和操作事項，例如容錯、負載平衡、自動調整和記載。這些小組想要有一種方式可以使用已知、愛用且最適合解決特定問題的程式設計語言來開發有效率且具有附加價值的程式碼。

{{site.data.keyword.openwhisk_short}} 的模組及固有的可擴充本質，讓它適用於實作動作中邏輯的精細部分。{{site.data.keyword.openwhisk_short}} 動作彼此獨立，可透過使用 {{site.data.keyword.openwhisk_short}} 支援的各種不同語言來實作，並存取各種後端系統。每一個動作都可以獨立部署及管理，並與其他動作分開進行調整。{{site.data.keyword.openwhisk_short}} 以規則、序列和命名慣例的形式，提供動作之間的交互連接。此類型的環境類型亦適用於微服務型應用程式。

有利於 {{site.data.keyword.openwhisk_short}} 的另一個重要論點，就是災難回復配置中的系統成本。對使用 PaaS 或 CaaS 的微服務與使用 {{site.data.keyword.openwhisk_short}} 的微服務進行比較；假設您有 10 個微服務，它們使用的是容器或 Cloud Foundry 運行環境。此比較相當於在單一可用性區域中有 10 個持續執行的計費處理程序，跨 2 個可用性區域執行時處理程序有 20 個，而跨 2 個地區（每個地區 2 個區域）執行時處理程序有 40 個。使用 {{site.data.keyword.openwhisk_short}} 時要達到同樣目標，可以根據需要跨任意數量的可用性區域或地區來執行微服務，而不會產生絲毫的遞增成本。

## Web 應用程式
{: #use_cases_webapps}

由於 {{site.data.keyword.openwhisk_short}} 的事件導向本質，它為面對使用者的應用程式提供多項好處，而從使用者瀏覽器提出的 HTTP 要求則作為事件。{{site.data.keyword.openwhisk_short}} 應用程式使用運算能力，而且只有在負責處理使用者要求時，才會計費。閒置待命或等待模式不存在。此特性讓 {{site.data.keyword.openwhisk_short}} 成本遠低於傳統容器或 Cloud Foundry 應用程式。後兩種工具會有閒置時間，用於等待入埠使用者要求，並且即使針對所有這些「睡眠」時間也照樣要計費。

使用 {{site.data.keyword.openwhisk_short}} 可以建置及執行完整的 Web 應用程式。合併無伺服器 API 與網站資源的靜態檔案管理（例如 HTML、JavaScript 及 CSS），表示您可以建置整個無伺服器 Web 應用程式。可簡易操作所管理的 {{site.data.keyword.openwhisk_short}} 環境，不需要任何操作。由於 {{site.data.keyword.openwhisk_short}} 是在 {{site.data.keyword.cloud_notm}} 上管理，因此，相較於使用及操作 Node.js Express 或其他傳統伺服器運行環境，這是很棒的優點。

## IoT
{: #use_cases_iot}

Internet of Things 情境本質上經常是由感應器所驅動。例如，如果需要針對感應器超過特定溫度做出反應，可能會觸發 {{site.data.keyword.openwhisk_short}} 中的動作。IoT 互動通常是 Stateless，且有可能在主要自發事件（例如自然災害、重大氣象風暴或塞車）中發生高負載。需要建立一個彈性系統，該系統的一般工作量可能較小，但需要以可預測的回應時間快速調整。因此，最好能處理多個同時發生但未事先警告系統的事件。對於使用傳統伺服器架構時的需求，很難建置一個系統來符合這些需求。因為它們很容易性能不足，而無法處理資料流量尖峰負載，或是過度供應，而產生高昂費用。

還是有可能實作使用傳統伺服器架構的 IoT 應用程式。不過，在許多情況下，不同服務與資料橋接器的組合需要高效能及彈性管線。範圍從 IoT 裝置到雲端儲存空間及分析平台。預先配置的橋接器往往缺乏用於實作和細部調整特定解決方案架構的程式設計。如果有各種管線，而且一般會缺乏資料融合的標準化（特別是在 IoT 中），常會看到管線需要自訂資料轉換的環境。這些自訂資料轉換適用於格式轉換、過濾或擴增。{{site.data.keyword.openwhisk_short}} 是一個可使用「無伺服器」方式來實作這類轉換的傑出工具，其中，自訂邏輯是在完整受管理及彈性的雲端平台上進行管理。

請查看使用 {{site.data.keyword.openwhisk_short}}、Node-RED、Cognitive 和其他服務的下列範例 IoT 應用程式：[使用 {{site.data.keyword.openwhisk_short}} 進行 IoT 動態資料的無伺服器轉換](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}。

![IoT 解決方案架構範例](images/IoT_solution_architecture_example.png)

## API 後端
{: #use_cases_backend}

無伺服器運算平台可讓開發人員在沒有伺服器的情況下快速建置 API。{{site.data.keyword.openwhisk_short}} 支援為動作自動產生 REST API。[{{site.data.keyword.openwhisk_short}} 特性](/docs/openwhisk?topic=cloud-functions-apigateway)可以使用 POST 以外的 HTTP 方法來呼叫動作，而不需要動作的授權 API 金鑰（透過「{{site.data.keyword.openwhisk_short}} API 閘道」）。此功能不僅有助於向外部消費者公開 API，也有助於建置微服務應用程式。

此外，{{site.data.keyword.openwhisk_short}} 動作還可以連接至所選擇的 API Management 工具（例如 [IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} 或其他工具）。與其他使用案例類似，所有關於可調整性和其他服務品質的考量都適用。

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} 是透過 REST API 使用 {{site.data.keyword.openwhisk_short}} 動作的範例應用程式。

請參閱下列範例，其中包括[使用無伺服器作為 API 後端](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}的討論。

## 行動後端
{: #use_cases_mobile}

許多行動應用程式都需要伺服器端邏輯。不過，行動開發人員通常沒有管理伺服器端邏輯的經驗，而是比較著重於正在裝置上執行的應用程式。將 {{site.data.keyword.openwhisk_short}} 當作伺服器端後端使用，便可輕易達成此開發目標，且是個很好的解決方案。此外，對於伺服器端 Swift 的內建支援可讓開發人員再利用他們現有的 iOS 程式設計技術。由於行動式應用程式通常會有無法預期的載入型樣，您會想要使用 {{site.data.keyword.openwhisk_short}} 解決方案，例如，{{site.data.keyword.cloud}}。此解決方案可以調整以滿足任何工作負載需求，而不需要事先佈建資源。

## 資料處理
{: #use_cases_data}

運用現在可用的資料量，應用程式開發需要處理新資料的能力，而且可能會對它做出反應。此需求包括處理結構化資料庫記錄以及非結構化文件、映像檔或視訊。{{site.data.keyword.openwhisk_short}} 可由系統提供的資訊來源或自訂資訊來源進行配置來反應資料變更，以及自動對送入的資料資訊來源執行動作。您可以對動作進行程式設計，以處理變更、轉換資料格式、傳送及接收訊息、呼叫其他動作，以及更新各種資料儲存庫。支援的資料儲存庫包括 SQL 型關聯式資料庫、記憶體內資料網格、 NoSQL 資料庫、檔案、訊息分配管理系統及各種其他系統。{{site.data.keyword.openwhisk_short}} 規則及序列提供彈性可在處理管線時進行變更，而不需要進行程式設計，只需要透過簡單的配置變更即可執行。利用資料儲存庫選項和低管理維護，可讓 {{site.data.keyword.openwhisk_short}} 型系統具有高度的靈活性，能輕鬆適應不斷變化的需求。

## 認知
{: #use_cases_cognitive}

認知技術可以有效率地與 {{site.data.keyword.openwhisk_short}} 合併，以建立功能強大的應用程式。例如，IBM Alchemy API 及 Watson Visual Recognition 可以與 {{site.data.keyword.openwhisk_short}} 搭配使用，自動從視訊擷取有用的資訊，而不需要觀看它們。此技術是先前討論之[資料處理](#use_cases_data)使用案例的一項「認知」延伸。{{site.data.keyword.openwhisk_short}} 另一個很好的用法，就是實作與認知服務結合的 Bot 功能。

提供的範例應用程式 [Dark vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external} 剛好就這麼做。在此應用程式中，使用者會使用 Dark Vision Web 應用程式來上傳視訊或影像，這會將它儲存在 {{site.data.keyword.cloudant_short_notm}} 資料庫中。上傳視訊之後，{{site.data.keyword.openwhisk_short}} 會接聽 {{site.data.keyword.cloudant_short_notm}} 變更（觸發程式）來偵測新視訊。{{site.data.keyword.openwhisk_short}} 接著會觸發視訊擷取程式動作。在其執行期間，擷取程式會產生訊框（影像），並將它們儲存在 {{site.data.keyword.cloudant_short_notm}} 中。然後，使用 Watson Visual Recognition 來處理訊框，而且結果會儲存在相同的 {{site.data.keyword.cloudant_short_notm}} 資料庫中。可以使用 Dark Vision Web 應用程式或 iOS 應用程式來檢視結果。除了 {{site.data.keyword.cloudant_short_notm}} 之外，還可以使用 {{site.data.keyword.cos_full_notm}}，這麼做的話，視訊及影像 meta 資料會儲存在 {{site.data.keyword.cloudant_short_notm}} 中，而媒體檔案會儲存在 {{site.data.keyword.cos_full_notm}} 中。

## 使用 Kafka 或 {{site.data.keyword.messagehub}} 來處理事件
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} 非常適合與 Kafka、{{site.data.keyword.messagehub_full}}（以 Kafka 為基礎）及其他傳訊系統組合使用。那些系統的事件導向本質，需要事件驅動的運行環境才能處理訊息。運行環境可以將商業邏輯（這其實就是 {{site.data.keyword.openwhisk_short}} 所提供）及其資訊來源、觸發程式及動作套用至那些訊息。Kafka 和 {{site.data.keyword.messagehub}} 通常會用於大量且無法預期的工作負載量，需要那些訊息的消費者一接到通知就能調整。此狀況又是 {{site.data.keyword.openwhisk_short}} 的一大優點。{{site.data.keyword.openwhisk_short}} 具有內建功能，可使用以及發佈 [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams) 套件中提供的訊息。



