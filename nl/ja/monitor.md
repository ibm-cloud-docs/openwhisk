---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: monitoring, viewing, performance, dashboard, metrics, health

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

# アクティビティーのモニタリング
{: #monitor}

{{site.data.keyword.openwhisk}} でデプロイされたアクションのパフォーマンスを洞察します。 メトリックは、アクションの期間、アクション・アクティベーションの結果、アクション・アクティベーションの限度に達したタイミングに基づいて、ボトルネックを見つけたり実動上の問題の発生を予測したりするために役立ちます。
{: shortdesc}

メトリックはすべてのエンティティーに関して自動的に収集されます。 アクションが IAM ベースの名前空間にあるのか、それとも Cloud Foundry ベースの名前空間にあるのかに応じて、メトリックは IBM Cloud のアカウントまたはスペースのいずれかに存在します。 それらのメトリックは {{site.data.keyword.monitoringlong}} に送信され、Grafana を介して使用可能になります。そこでは、ダッシュボードの構成、メトリック・イベント値に基づくアラートの作成、その他を行うことができます。 メトリックについて詳しくは、[{{site.data.keyword.monitoringlong_notm}} の資料](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started)を参照してください。

## ダッシュボードの作成
{: #monitor_dash}

Grafana モニタリング・ダッシュボードを作成して作業を開始します。

1. 以下のいずれかの URL に移動します。
  <table>
    <thead>
      <tr>
        <th>{{site.data.keyword.openwhisk_short}}region</th>
        <th>モニタリング・アドレス</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>中欧</td>
        <td>metrics.eu-de.bluemix.net</td>
      </tr>
      <tr>
        <td>英国南部</td>
        <td>metrics.eu-gb.bluemix.net</td>
      </tr>
      <tr>
        <td>米国南部</td>
        <td>metrics.ng.bluemix.net</td>
      </tr>
      <tr>
        <td>米国東部</td>
        <td>使用不可</td>
      </tr>
    </tbody>
  </table>

2. メトリック・ドメインを選択します。
    * IAM ベースの名前空間の場合は、以下のようにします。
        1. ユーザー名をクリックします。
        2. **「ドメイン」**ドロップダウン・リストで、**「アカウント」**を選択します。
        3. **「アカウント」**ドロップダウン・リストで、IAM ベースの名前空間がある IBM Cloud アカウントを選択します。
    * Cloud Foundry ベースの名前空間の場合は、以下のようにします。
        1. ユーザー名をクリックします。
        2. **「ドメイン」**ドロップダウン・リストで、**「スペース」**を選択します。
        3. **「組織」**および**「スペース」**ドロップダウン・リストを使用して、Cloud Foundry ベースの名前空間を選択します。

3. ダッシュボードを作成します。
    * 事前作成された {{site.data.keyword.openwhisk_short}} ダッシュボードを使用するには、以下のようにします。
        1. **「ホーム」>「インポート」**に移動します。
        3. 事前作成された {{site.data.keyword.openwhisk_short}} ダッシュボードの ID である `8124` を **Grafana.net Dashboard** フィールドに入力します。
        4. **「インポート」**をクリックします。
    * カスタム・ダッシュボードを作成するには、**「ホーム」>「新規作成」**に移動します。

アクションの実行後に、新しいメトリックが生成されて、Grafana で検索可能になります。 注: 実行されたアクションが Grafana に表示されるまでに、最大で 10 分間かかります。




## ダッシュボードの使用
{: #monitor_dash_use}

[{{site.data.keyword.openwhisk_short}} ダッシュボード](https://cloud.ibm.com/openwhisk/dashboard)は、アクティビティーのグラフィック・サマリーを提供します。 ダッシュボードを使用して、{{site.data.keyword.openwhisk_short}} アクションのパフォーマンスおよび正常性を判別できます。
{:shortdesc}

どのアクション・ログを表示するのかを選択することでログをフィルターに掛けて、ログに記録されたアクティビティーの時間フレームを選択することができます。 これらのフィルターは、ダッシュボード上のすべてのビューに適用されます。 いつでも**「再ロード」**をクリックして、最新のアクティベーション・ログ・データでダッシュボードを更新できます。

### アクティビティー・サマリー
{: #monitor_dash_sum}

**「アクティビティー・サマリー」**ビューは、{{site.data.keyword.openwhisk_short}} 環境の概要レベルのサマリーを示します。 このビューを使用して、{{site.data.keyword.openwhisk_short}} 対応サービスの全体的な正常性とパフォーマンスをモニターします。 このビューのメトリックから、以下のことが行えます。
* サービスの {{site.data.keyword.openwhisk_short}} 対応アクションの使用率を、それらのアクションが呼び出された回数を表示することで判別します。
* すべてのアクションで発生した障害の全体的な比率を判別します。 エラーが表示された場合、**「アクティビティー・ヒストグラム」**ビューを表示することによって、エラーが発生したサービスまたはアクションを切り分けることができます。 さらに、**「アクティビティー・ログ」**を表示することによって、エラーそのものを切り分けます。
* 各アクションに関連付けられている平均完了時間を表示することによって、アクションの実行状況を判別します。

### アクティビティー・タイムライン
{: #monitor_dash_time}

**「アクティビティー・タイムライン」**ビューには、過去および現在のアクションのアクティビティーを示す縦棒グラフが表示されます。 赤は、特定のアクションでのエラーを示します。 エラーについてもっと詳しく調べるには、このビューを**「アクティビティー・ログ」**と相互に関連付けます。



### アクティビティー・ログ
{: #monitor_dash_log}

この**「アクティビティー・ログ」**ビューは、アクティベーション・ログのフォーマット済みバージョンを表示します。 このビューには、各アクティベーションの詳細が表示されますが、1 分に 1 回ポーリングして新規のアクティベーションがないかを確認します。 いずれかのアクションをクリックすると、詳細ログが表示されます。

アクティビティー・ログに表示される出力を CLI を使用して取得するには、以下のコマンドを使用します。
```
ibmcloud fn activation poll
```
{: pre}




## メトリック形式
{: #monitor_metric}

メトリックには、1 分ごとに集約される、アクション・アクティベーションから収集されたデータが反映されます。 メトリックは、アクション・パフォーマンスまたはアクション並行性のレベルで検索可能となります。


### アクション・パフォーマンスのメトリック
{: #monitor_metric_perf}

アクション・パフォーマンスのメトリックは、単一のアクションについて計算された値です。 アクション・パフォーマンスのメトリックには、実行のタイミング特性とアクティベーションの状況の両方が含まれます。 注: 作成時にパッケージの名前を指定しない場合、デフォルトのパッケージ名が使用されます。 これらのメトリックの形式は以下のとおりです。

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.<package>.<action>.<metric_name>
```
{: codeblock}

次の文字はダッシュ (`-`) に変換されます: ピリオド (.)、アットマーク (@)、空白文字 ( )、アンパーサンド (&)、アンダースコアー (_)、コロン (:)
{: tip}

例: `us-south` 地域内の Cloud Foundry ベースの名前空間 `user@email.com_dev` に `hello-world` というアクションがある場合、アクション・パフォーマンスのメトリックは次のようになります。

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### アクション並行性のメトリック
{: #monitor_metric_con}

アクション並行性のメトリックは、名前空間内のすべてのアクティブなアクションから収集されたデータに基づいて計算されます。 アクション並行性には、同時に呼び出される数、および並行性の限度を超過したときに生じる可能性のあるシステム・スロットルの数が含まれます。 これらのメトリックの形式は以下のとおりです。

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.action-performance.<package>.<action>.<metric_name>
```
{: codeblock}

例: `us-south` 地域に `myNamespace` という IAM ベースの名前空間がある場合、アクション並行性のメトリックは次のようになります。

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### 使用可能なメトリック
{: #monitor_metric_av}

数千から数百万のアクション・アクティベーションが存在することもあるので、メトリック値は、多数のアクティベーションから生成されるイベントの集約として表されます。 それらの値は以下の方法で集約されます。
* 合計: すべてのメトリック値が加算されます。
* 平均: 算術平均が計算されます。
* 合計平均: 構成要素に基づいて算術平均が計算され、さまざまな構成要素が加算されます。

以下の表で、お客様が使用できるメトリックを確認してください。

<table>
  <thead>
    <tr>
      <th>メトリック名</th>
      <th>説明</th>
      <th>タイプ</th>
      <th>カテゴリー</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>平均アクション期間、アクション実行時に課金。</td>
      <td>平均</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>アクション・コンテナーの初期化に費やされた時間。</td>
      <td>平均</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>アクティベーションがスケジュールされるまで待機するためにキューに入っていた時間の平均。</td>
      <td>平均</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>システムで起動したアクティベーションの総数。</td>
      <td>合計</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>アクション・コードの成功したアクティベーションの数。</td>
      <td>合計</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>アプリケーション・エラーに由来する失敗したアクティベーションの数。 例えば、アクションからの安全なエラーに由来するものなどです。 アクション・パフォーマンスのメトリックを算出する方法について詳しくは、[Understanding the activation record](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record) を参照してください。</td>
      <td>合計</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>開発者に由来する失敗したアクティベーションの数。 例えば、アクション・コード内の未処理の例外による[アクション・プロキシー・インターフェース](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface)の違反に由来するものなどです。</td>
      <td>合計</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>{{site.data.keyword.openwhisk_short}} 内部エラーに由来する失敗したアクティベーションの数。</td>
      <td>合計</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>並行率の限度を越えたために抑制されたアクティベーションの合計。 限度に達していない場合、メトリックは出力されません。</td>
      <td>合計</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>1 分ごとの限度を超えたために抑制されたアクティベーションの合計。 限度に達していない場合、メトリックは出力されません。</td>
      <td>合計</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>システム内の並行呼び出しの数。</td>
      <td>合計平均</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

デフォルトの名前空間の一部として存在するアクションのメトリックは、デフォルト・カテゴリー内にあります。
{: tip}


