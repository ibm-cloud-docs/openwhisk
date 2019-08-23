---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications, functions

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



# 檢視 {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #activity_tracker}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服務，在 {{site.data.keyword.openwhisk}} 服務實例中檢視、管理及審核使用者起始的活動。
{: shortdesc}


如需服務運作方式的相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}} 文件](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)。


## 檢視事件的位置
{: #view}

事件在 {{site.data.keyword.openwhisk_short}} 名稱空間資源可用的 {{site.data.keyword.cloud_notm}} 地區中可用的 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**中可用。如需相關資訊，請參閱[檢視帳戶事件](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events)。

1. 登入 {{site.data.keyword.cloud_notm}} 帳戶。
2. 從型錄中，在與 {{site.data.keyword.openwhisk}} 實例相同的帳戶中佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務實例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 儀表板的**管理**標籤上，按一下**在 Kibana 中檢視**。
4. 設定您要檢視其日誌的時間範圍。預設值為 15 分鐘。
5. 在**可用的欄位**清單中，按一下**類型**。按一下 **Activity Tracker** 的放大鏡圖示，將日誌限制為僅服務所追蹤的日誌。
6. 您可以使用其他可用的欄位，來縮小搜尋範圍。

若要讓帳戶擁有者以外的使用者檢視日誌，您必須使用超值方案。若要讓其他使用者檢視事件，請參閱[授與許可權來查看帳戶事件](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)。
{: tip}


## 事件清單
{: #events}

請參閱下表，以取得傳送至 {{site.data.keyword.cloudaccesstrailshort}} 的事件清單。
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>動作</th>
      <th>說明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>建立以 IAM 為基礎的名稱空間資源</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>移轉以 Cloud Foundry 為基礎的名稱空間以啟用 IAM 功能</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>更新以 IAM 為基礎的名稱空間資源內容，例如顯示名稱或說明</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>刪除名稱空間資源</td>
    </tr>
  </tbody>
</table>



