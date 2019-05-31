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

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.openwhisk}} の操作中に問題が生じた場合は、ここに示すトラブルシューティング手法やヘルプの利用手法を検討してください。
{: shortdesc}



## アクションが失敗しました
{: #ts_action_fails}

{: tsSymptoms}
アクションが失敗しました。

{: tsCauses}
アプリが失敗する 1 つの原因は、非推奨のランタイムが使用されていることです。サポート対象のランタイムに更新するまで、アクションは正常に実行できません。

{: tsResolve}
アクションに使用されているランタイムを確認するには、`ibmcloud fn action get ACTION_NAME` を実行し、照会応答に `deprecated=true` と表示されていないか調べます。ランタイムが非推奨の場合は、[ランタイムを更新](/docs/openwhisk?topic=cloud-functions-actions#actions_update)してください。


<br />


## システム限度に達しました
{: #ts_limit_reached}

{: tsSymptoms}
関数が正常に実行されずに、`{"error":"signal: killed"}` などのメッセージが表示されることがあります。

{: tsCauses}
すべての関数には、最大メモリー使用量やタイムアウトなど、上限と下限の[システム限度](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)があります。

{: tsResolve}
安全性限度値を高くしても対応可能なビジネス・ケースであれば、限度を上げられる場合があります。限度値を上げるには、IBM Cloud Functions の Web コンソールからチケットを直接オープンして、IBM サポートに連絡してください。

1. **「サポート」**を選択します。
2. ドロップダウン・メニューから**「チケットの追加」**を選択します。
3. チケット・タイプに**「技術的」**を選択します。
4. サポートの技術的領域に**「機能」**を選択します。


<br />


## ヘルプおよびサポートの利用
{: #gettinghelp}

まだ関数に問題がありますか?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを調べるために、[{{site.data.keyword.Bluemix_notm}} 状況ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を確認します](https://cloud.ibm.com/status?selected=status)。
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。
    -   {{site.data.keyword.openwhisk}} での関数の開発について技術的な質問がある場合は、[Stack Overflow ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/search?q=ibm-cloud-functions) に質問を投稿し、`ibm-cloud-functions` というタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM Developer Answers Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` タグと `functions` タグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)を参照してください。
-   ケースを開いて、IBM サポートに連絡してください。 IBM サポート・ケースを開く方法や、サポート・レベルとケースの重大度については、[サポートへのお問い合わせ](/docs/get-support?topic=get-support-getting-customer-support)を参照してください。
問題を報告する際は、アクティベーション ID を含めてください。アクティベーション ID を調べるには、`ibmcloud fn activation list` を実行します。
{: tip}
