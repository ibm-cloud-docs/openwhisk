---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}


# {{site.data.keyword.cloudaccesstrailshort}} イベントの表示
{: #activity_tracker}

{{site.data.keyword.cloudaccesstrailshort}} サービスを使用すると、{{site.data.keyword.openwhisk}} サービス・インスタンスで実行されたユーザー開始のアクティビティーを表示、管理、および監査できます。
{: shortdesc}


このサービスの仕組みについて詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} の資料](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla)を参照してください。


## イベントを確認できる場所
{: #view}

イベントは、{{site.data.keyword.openwhisk_short}} 名前空間リソースが入っている {{site.data.keyword.Bluemix_notm}} 地域の {{site.data.keyword.cloudaccesstrailshort}} の**アカウント・ドメイン**で確認できます。詳しくは、[アカウント・イベントの表示](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events)を参照してください。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。
2. カタログから、{{site.data.keyword.openwhisk}} のインスタンスと同じアカウントに、{{site.data.keyword.cloudaccesstrailshort}} サービスのインスタンスをプロビジョンします。
3. {{site.data.keyword.cloudaccesstrailshort}} ダッシュボードの **「管理」** タブで、**「Kibana で表示」**をクリックします。
4. ログを表示する時間フレームを設定します。 デフォルトは 15 分です。
5. **「使用可能なフィールド」**リストで、**「タイプ」**をクリックします。 **アクティビティー・トラッカー**の虫眼鏡のアイコンをクリックして、サービスによって追跡されるもののみにログを制限します。
6. その他の使用可能なフィールドを使用して、検索を絞り込むことができます。

アカウント所有者以外のユーザーがログを表示するには、プレミアム・プランを使用している必要があります。 他のユーザーがイベントを表示できるようにするには、[アカウント・イベントを表示する許可の付与](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)を参照してください。
{: tip}


## イベント・リスト
{: #events}

{{site.data.keyword.cloudaccesstrailshort}} に送信されるイベントのリストについては、以下の表を確認してください。
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>アクション</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>IAM ベースの名前空間リソースを作成します</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Cloud Foundry ベースの名前空間をマイグレーションして IAM 対応の名前空間にします</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>IAM ベースの名前空間リソースのプロパティー (表示名や説明など) を更新します</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>名前空間リソースを削除します</td>
    </tr>
  </tbody>
</table>
