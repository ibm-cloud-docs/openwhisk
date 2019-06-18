---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: packages, installable packages

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
{:gif: data-image-type='gif'}

# 包含套件
{: #pkg_ov}

套件是相關動作及資訊來源的組合集。每一個套件都是針對與服務及事件提供者的特定互動而設計的。某些套件已隨 {{site.data.keyword.openwhisk}} 一起安裝以供您使用，但您還可以安裝其他套件。
{: shortdesc}

## 概觀
{: #pkg_overview}

[預先安裝的套件](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse)會在 {{site.data.keyword.openwhisk_short}} 內的 `/whisk.system` 名稱空間中自動登錄。您可以直接使用這些套件，而無需完成任何安裝步驟。

可安裝的套件是可供您根據自己的需要來安裝、編輯和使用的套件。可安裝的套件不位於 {{site.data.keyword.openwhisk_short}} 系統內。相反地，可安裝的套件存在於外部的個別 GitHub 儲存庫中。

您可以將這些套件直接安裝到自己的名稱空間中，並且可以為套件提供自訂名稱。因為套件是安裝在您自己的名稱空間中，所以您可以視需要修改套件中的動作及資訊來源。



## 瀏覽預先安裝的套件
{: #pkg_browse}

已向 {{site.data.keyword.openwhisk_short}} 登錄了多個套件。您可以取得名稱空間中的套件清單、列出套件中的實體，以及取得套件中個別實體的說明。
{: shortdesc}

1. 取得 `/whisk.system` 名稱空間中的套件清單。
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  套件清單輸出：
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. 取得套件中實體的清單。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  範例：
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  輸出範例：
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  此輸出顯示 {{site.data.keyword.cloudant_short_notm}} 套件中包含多個動作和一個資訊來源。例如，包含兩個動作（`read` 和 `write`）和一個名為 `changes` 的觸發程式資訊來源。「`changes` 資訊來源」會導致在指定的 {{site.data.keyword.cloudant_short_notm}} 資料庫中新增文件時發動觸發程式。

  {{site.data.keyword.cloudant_short_notm}} 套件也定義參數 `username`、`password`、`host` 及 `port`。您必須指定這些參數，動作及資訊來源才有意義。例如，這些參數容許動作在特定 {{site.data.keyword.cloudant_short_notm}} 帳戶上運作。

3. 取得動作或資訊來源的說明，以查看所需的參數。

  範例：
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  輸出範例：
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  此輸出顯示 {{site.data.keyword.cloudant_short_notm}} 的 `read` 動作需要三個參數（包括要擷取的資料庫及文件 ID）。



## 將參數連結到預先安裝的套件
{: #pkg_bind}

雖然您可以直接使用套件中的實體，但是可能會發現您每次都將相同的參數傳遞給動作。您可以透過連結至套件並指定預設參數（由套件中的動作所繼承）來簡化處理程序。
{: shortdesc}

例如，在 `/whisk.system/cloudant` 套件中，您可以在套件連結中設定預設 `username`、`password` 及 `dbname` 值，而這些值會自動傳遞給套件中的所有動作。

在下列範例中，將連結到 `/whisk.system/samples` 套件。

1. 連結至 `/whisk.system/samples`，並設定預設 `place` 參數值。
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  輸出範例：
  ```
ok: created binding valhallaSamples
  ```
  {: screen}

2. 取得套件連結的說明。
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  輸出範例：
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  請注意，`valhallaSamples` 套件連結中提供 `/whisk.system/samples` 套件中的所有動作。

3. 在套件連結中呼叫動作。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  輸出範例：
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  請注意動作繼承您在建立 `valhallaSamples` 套件連結時所設定的 `place` 參數的結果。

4. 呼叫動作，並且改寫預設參數值。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  輸出範例：
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  請注意，透過動作呼叫指定的 `place` 參數值會改寫 `valhallaSamples` 套件連結中所設定的預設值。






## 新增您自己的套件
{: #pkg_add}

可以建立本端程式碼的套件或任何 Github 儲存庫的複製。
{: shortdesc}

開始之前：
- [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。
- 為應用程式建立 `manifest.yaml` 或 `manifest.yml` 檔案，並將其儲存在根目錄中。`manifest.yaml` 檔案指定套件的整體結構，包括 `ibmcloud fn deploy` 指令必須包含的任何 meta 資料。若要進一步瞭解 `manifest.yaml` 檔案，請參閱 [wskdeploy 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example)。
    

若要新增套件，請執行下列動作：

1. 複製套件儲存庫。
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. 導覽至包含 `manifest.yaml` 檔案的目錄。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 部署套件。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    部分套件需要特定環境變數，套件才能正常運作。
    如果是這樣，請在 `deploy` 指令中包含這些環境變數。例如，可以為套件選擇名稱，並使用 PACKAGE_NAME 變數指定該名稱。

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### 使用 {{site.data.keyword.cos_full_notm}} 套件的範例
{: #pkg_ex}

若要查看如何安裝套件的範例，請參閱 [{{site.data.keyword.cos_full_notm}} 套件](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。{{site.data.keyword.cos_full}} 是一項服務，容許使用者儲存所有類型的檔案，例如影像、視訊、音樂和文字。為了與檔案互動，鍵值組的雲端型資料儲存庫會儲存在儲存區中。因此，若要使用 [{{site.data.keyword.cos_full_notm}} 套件](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)，您必須先建立一個 {{site.data.keyword.cos_full_notm}} 服務實例，然後再建立一個儲存區。此儲存區用來作為安裝此套件所需的環境變數。

建立服務實例及儲存區之後，安裝套件需要下列指令：

1. 複製套件儲存庫。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 導覽至包含 `manifest.yaml` 的套件目錄。在此範例中，使用 {{site.data.keyword.cos_full_notm}} 套件的 Node.js 運行環境版本。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 使用您的儲存區作為環境變數，來部署套件。`PACKAGE_NAME` 環境變數的相依關係，容許您為此套件提供一個自訂名稱。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}

