---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 疑難排解
{: #troubleshooting}

如果您在使用 {{site.data.keyword.openwhisk}} 時遇到問題，請考慮使用這些技術來進行疑難排解並取得協助。
{: shortdesc}



## 動作失敗
{: #ts_action_fails}

{: tsSymptoms}
動作失敗。

{: tsCauses}
對於應用程式失敗的一個解釋是，使用了已淘汰的運行環境。在將運行環境更新為支援的運行環境之前，無法順利完成動作。

{: tsResolve}
若要查看哪個運行環境用於動作，請執行 `ibmcloud fn action get ACTION_NAME`，並在查詢回應中檢查是否 `deprecated=true`。如果這是已淘汰的運行環境，請[更新運行環境](/docs/openwhisk?topic=cloud-functions-actions#actions_update)。


<br />


## 已達到系統限制
{: #ts_limit_reached}

{: tsSymptoms}
函數未正常執行，可能會產生類似於 `{"error":"signal: killed"}` 的訊息。

{: tsCauses}
所有函數都具有最小和最大[系統限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)，例如最大記憶體用量或逾時。

{: tsResolve}
如果業務案例可以證明使用更高安全限制值是合理的，則可以增加某些限制。若要增加限制值，請直接從 IBM Cloud Functions Web 主控台開立問題單，來與 IBM 支援中心聯絡。

1. 選取**支援**。
2. 從下拉功能表中選取**新增問題單**。
3. 針對問題單類型，請選取**技術**。
4. 針對支援的技術領域，請選取**函數**。


<br />


## 取得協助及支援
{: #gettinghelp}

函數仍然有問題？
{: shortdesc}

-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status)。
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
    -   如果您有使用 {{site.data.keyword.openwhisk}} 開發函數的相關技術問題，請將問題張貼在 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/search?q=ibm-cloud-functions)，並使用 `ibm-cloud-functions` 來標記您的問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM Developer Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 和 `functions` 標籤。
    如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   開立案例，以與「IBM 支援中心」聯絡。若要瞭解有關開立 IBM 支援案例或有關支援層次和案例嚴重性的資訊，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support)。報告問題時，請包含啟動 ID。若要取得啟動 ID，請執行 `ibmcloud fn activation list`。
{: tip}
