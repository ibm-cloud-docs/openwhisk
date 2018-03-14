---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} 獨立式 CLI

**在 2018 年 3 月 9 日，將無法再下載 {{site.data.keyword.openwhisk_short}} 獨立式 CLI。若要在此日期之後繼續管理 {{site.data.keyword.openwhisk_short}} 實體，請針對 {{site.data.keyword.Bluemix}} CLI 使用 [{{site.data.keyword.openwhisk_short}}CLI 外掛程式](./bluemix_cli.html)。

{{site.data.keyword.openwhisk}} 分散式 **wsk** 指令行介面容許管理所有 {{site.data.keyword.openwhisk_short}} 實體。
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* 配置 {{site.data.keyword.openwhisk_short}} 鑑別金鑰
  * 不需要存取 {{site.data.keyword.openwhisk_short}} 主控台來取得鑑別金鑰
  * 在地區、組織及空間之間切換時，會發生自動鑑別金鑰
* 重新整理已過期的 {{site.data.keyword.openwhisk_short}} 鑑別金鑰
* 將 CLI 更新為較新版本


下列管理作業需要您使用外掛程式。

* API Management
  * API 閘道存取記號的配置
  * 重新整理過期的 API 閘道存取記號

## 設定 {{site.data.keyword.openwhisk_short}} CLI 
{: #openwhisk_cli_setup}

您可以使用 {{site.data.keyword.openwhisk_short}} 指令行介面 (CLI) 來設定名稱空間及授權金鑰。移至[配置 CLI](https://console.bluemix.net/openwhisk/cli?loadWsk=true)，並遵循指示進行安裝。

首先，配置下列兩個內容：

1. 您要使用的 {{site.data.keyword.openwhisk_short}} 部署的 **API 主機**（名稱或 IP 位址）。
2. 授與您存取 {{site.data.keyword.openwhisk_short}} API 的**授權金鑰**（使用者名稱及密碼）。

有兩個可用的 {{site.data.keyword.Bluemix_notm}} 地區，其需要自己的專屬 API 主機和「授權」金鑰。

* 美國南部
  * API 主機：`openwhisk.ng.bluemix.net`

* 英國
  * API 主機：`openwhisk.eu-gb.bluemix.net`

執行下列指令，為您想要的 {{site.data.keyword.Bluemix_notm}} 地區設定 API 主機：

美國南部：
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英國：
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

如果您需要切換地區，則必須使用 API 主機及授權金鑰來重新配置 CLI，因為授權金鑰是每個地區專用的。
{: tip}

「動作」、「規則」和套件之類的構件是地區特定的。因此，如果您在多個地區中使用相同的構件，您必須將它部署到每一個想要的地區。

如果您知道您的授權金鑰，則可以配置 CLI 使用它。 

執行下列指令，以設定「授權金鑰」：

```
wsk property set --auth <authorization_key>
```
{: pre}

**提示：**依預設，{{site.data.keyword.openwhisk_short}} CLI 會將內容集儲存在 `~/.wskprops` 中。您可以透過設定 `WSK_CONFIG_FILE` 環境變數來變更此檔案的位置。 

若要驗證 CLI 設定，請嘗試[建立及執行動作](./index.html#openwhisk_start_hello_world)。

## 使用 {{site.data.keyword.openwhisk_short}} CLI
{: #using_openwhisk_cli}

配置環境之後，您可以使用 {{site.data.keyword.openwhisk_short}} CLI 來執行下列作業：

* 在 {{site.data.keyword.openwhisk_short}} 上，執行您的程式碼 Snippet 或「動作」。請參閱[建立及呼叫動作](./openwhisk_actions.html)。
* 使用「觸發程式」和「規則」，讓「動作」回應事件。請參閱[建立觸發程式及規則](./openwhisk_triggers_rules.html)。
* 瞭解套件如何組合動作及配置外部事件來源。請參閱[使用及建立套件](./openwhisk_packages.html)。
* 探索套件的型錄，以及使用外部服務（例如 [Cloudant 事件來源](./openwhisk_cloudant.html)）來加強應用程式。請參閱[前置安裝的 {{site.data.keyword.openwhisk_short}} 套件](./openwhisk_catalog.html)。

## 配置 CLI 以使用 HTTPS Proxy
{: #cli_https_proxy}

您可以設定 CLI 來使用 HTTPS Proxy。若要設定 HTTPS Proxy，必須建立一個稱為 `HTTPS_PROXY` 的環境變數。變數必須設為 HTTPS Proxy 的位址，其埠使用下列格式：`{PROXY IP}:{PROXY PORT}`。
