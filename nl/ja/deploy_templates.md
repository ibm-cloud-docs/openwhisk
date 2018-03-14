---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}} でのテンプレートのデプロイ
{: #serviceauth}

{{site.data.keyword.openwhisk}} は、次のプロジェクトを迅速に開始できるように、テンプレートのカタログを提供します。テンプレートは、アクション、トリガー、シーケンスの組み合わせであり、{{site.data.keyword.Bluemix}} からのサービス・インスタンスを取り込むこともできます。テンプレートを使用することで、プロジェクトを迅速かつ簡単に作成し、直ちにコーディングを開始できます。 

このチュートリアルでは、Cloudant テンプレートのデプロイについて順に説明します。
{: shortdesc}

## 使用可能なテンプレート
{: #available-templates}

| 名前 | 説明 | サポートされるランタイム | 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events | Cloudant DB で文書が編集または追加された場合に、コンソールに変更を出力します。| Node.js、Swift、Python、PHP |
| Get HTTP Resource | HTTP イベントに対する応答として呼び出され、Yahoo Weather API からデータをフェッチする Web アクション。| Node.js、Python |
| Hello World | このアクションは 1 つのパラメーターを受け入れます。このパラメーターは JSON オブジェクトでなければなりません。| Node.js、Swift、Python、PHP |
| Message Hub Events | メッセージ・ハブ・トピックで新しいデータが追加されると、コンソールに変更を出力します。| Node.js、Swift、Python、PHP | 
| Periodic Slack Reminder | 定期的なトリガーに基づいて、Slack に投稿するアクション。| Node.js、Swift、Python、PHP |

## Cloudant Events テンプレートのデプロイ
{: #cloudant-template}

Cloudant テンプレートは、アクション・シーケンス、およびそのシーケンスを開始するトリガーを作成します。トリガーは、接続されている Cloudant DB で変更が生じると起動します。この DB は、名前と色を持つ猫のデータベースです。予期されるデータ項目は猫であり、名前と色が定義されています。新しい猫がデータベースに追加されるか、現在の猫が編集されると、データがコンソールに出力されます。

1. テンプレートを作成するには、{{site.data.keyword.Bluemix_notm}} で [{{site.data.keyword.openwhisk_short}} に移動してから、**「作成の開始」**をクリックします。 

2. 「テンプレートのデプロイ (Deploy Template)」をクリックします。

3. 「新規 Cloudant 項目 (New Cloudant Item)」をクリックします。

### Cloudant アクションの作成

1. 次に、パッケージの名前を指定するか、提供されているデフォルト名 new-cloudant-item を使用します。 

2. 「アクション」ドロップダウンの下で、所有するアクションのランタイム (nodejs、swift、python、または php) を選択します。この例では、「nodejs」を選択し、「次へ」をクリックします。

### Cloudant トリガーの作成

トリガーは、イベント・ソースからイベントを受信するとアクションを呼び出します。Cloudant テンプレート用のトリガーを作成するには、トリガーに対し、必要な Cloudant サービス・インスタンス情報を指定します。

#### Cloudant サービス・インスタンスの作成

以下のいずれかの操作から選択できます。
  * 独自のインスタンスの作成
  * 独自の資格情報の入力 

1. この例では、独自のインスタンスの作成を選択します。

2. ポップアップが開き、Cloudant セットアップ・ページが含まれた新規タブが表示されます。Cloudant インスタンスを作成した後に、サービス資格情報セットを作成してから、「OK」をクリックしてタブを閉じてこのページに戻る必要があります。

3. 次に、独自の資格情報の入力を選択し、以下の情報を指定します。
  * ユーザー名 - Cloudant ユーザー名
  * パスワード - Cloudant パスワード
  * ホスト - これは通常、username.cloudant.com
  * データベース - Cloudant データベースの名前

### Cloudant テンプレートのデプロイ

1. 「デプロイ」をクリックします。

テンプレートのデプロイ後、必要に応じてカスタマイズするためにさらにコードを編集するか、戻って使用可能なテンプレートのカタログを確認できます。

