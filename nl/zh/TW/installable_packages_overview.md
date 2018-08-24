---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# 使用可安裝的套件
{: #installable-packages-overview}

可安裝的套件代表一種新設計，能管理 {{site.data.keyword.openwhisk}} 內的套件並與之進行互動。
{: shortdesc}

## 概觀
{: #overview}

**何謂可安裝的套件？**

套件是相關動作及資訊來源的組合集。每一個套件都是針對與服務及事件提供者的特定互動而設計的。可安裝的套件是可讓您根據需要來選擇、安裝及編輯的套件。

**可安裝的套件與預先安裝的套件有何不同？**

[預先安裝的套件](openwhisk_packages.html#browse-packages)會在 {{site.data.keyword.openwhisk_short}} 內的 `/whisk.system` 名稱空間中自動登錄。若要在預先安裝的套件中儲存認證或其他參數，您必須建立[套件連結](openwhisk_packages.html#openwhisk_package_bind)。

可安裝的套件不位於 {{site.data.keyword.openwhisk_short}} 系統內。相反地，可安裝的套件存在於外部的個別 GitHub 儲存庫中。您可以使用 [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) 工具，將這些套件直接安裝在您自己的名稱空間中，並為此套件提供一個自訂名稱。因為套件是安裝在您自己的名稱空間中，所以您可以視需要修改套件中的動作及資訊來源。

## 使用 wskDeploy 安裝套件
{: #installing}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](bluemix_cli.html#cloudfunctions_cli)。
  2. 安裝 [`wskdeploy` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)，並將下載的二進位檔新增至您的 PATH。

若要安裝套件，請執行下列動作：

1. 複製套件儲存庫。您可以在此文件集的每一個套件的個別頁面上，找到套件儲存庫。
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. 導覽至包含 `manifest.yaml` 或 `manifest.yml` 檔案的套件目錄。`manifest.yaml` 檔案指定套件的整體結構，包括要安裝到您的名稱空間的套件和動作，以及必須包括在 `wskdeploy` 指令中的任何 meta 資料。若要進一步瞭解 `manifest.yaml` 檔案，請參閱 [wskdeploy 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example)。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 部署套件。部分套件需要特定環境變數，套件才能正常運作。
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### 使用 {{site.data.keyword.cos_full_notm}} 套件的範例
{: #example}

若要查看如何安裝套件的範例，請查看 [{{site.data.keyword.cos_short}} 套件](cloud_object_storage_actions.html)。{{site.data.keyword.cos_full}} 是一項服務，容許使用者儲存所有類型的檔案，例如影像、視訊、音樂和文字。為了與檔案互動，鍵值組的雲端型資料儲存庫會儲存在儲存區中。因此，若要使用 [{{site.data.keyword.cos_short}} 套件](cloud_object_storage_actions.html)，您必須先建立一個 {{site.data.keyword.cos_short}} 服務實例，然後再建立儲存區。此儲存區用來作為安裝此套件所需的環境變數。

建立服務實例及儲存區之後，安裝套件需要下列指令：

1. 複製套件儲存庫。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 導覽至包含 `manifest.yaml` 的套件目錄。在此範例中，使用 {{site.data.keyword.cos_short}} 套件的 Node.js 運行環境版本。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 使用您的儲存區作為環境變數，來部署套件。`PACKAGE_NAME` 環境變數的相依關係，容許您為此套件提供一個自訂名稱。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> wskdeploy
    ```
    {: pre}
