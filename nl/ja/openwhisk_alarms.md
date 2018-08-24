---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# アラームを使用したトリガーのスケジュール
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` パッケージを使用して、指定した頻度でトリガーを起動できます。 アラームは、1 時間ごとにシステム・バックアップ・アクションを起動するといった、ジョブやタスクの繰り返しをセットアップする際に便利です。
{: shortdesc}

このパッケージには、以下のフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | パッケージ | - | アラームおよび定期的ユーティリティー。 |
| `/whisk.system/alarms/once` | フィード | date、trigger_payload、deleteAfterFire | 特定の日付に 1 回、トリガー・イベントを起動します。 |
| `/whisk.system/alarms/interval` | フィード | minutes、trigger_payload、startDate、stopDate | 間隔ベースのスケジュールでトリガー・イベントを起動します。 |
| `/whisk.system/alarms/alarm` | フィード | cron、trigger_payload、startDate、stopDate | cron を使用して、時間ベースのスケジュールでトリガー・イベントを起動します。 |

## トリガー・イベントの 1 回の起動

`/whisk.system/alarms/once` フィードは、指定された日付に 1 回トリガー・イベントを起動するようアラーム・サービスを構成します。1 回起動アラームを作成するには、以下のコマンドを実行します。
```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}

<table>
<caption><code>trigger create fireOnce</code> コマンドのコンポーネントの理解</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>trigger create fireOnce</code> コマンドのコンポーネントの理解</th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>作成するアラーム・トリガーのタイプ。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>fireOnce フィードで使用するアラーム・パッケージのファイル・パス。</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td><code>&lt;date&gt;</code> をトリガーが起動される日付に置き換えます。トリガーは指定時刻に 1 回のみ起動します。注: `date` パラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC からのミリ秒数を表し、ストリング値は  <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 形式</a>でなければなりません。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>オプション : トリガーが起動されたときに、<code>&lt;key&gt;</code>  および  <code>&lt;value&gt;</code> をトリガーのパラメーターに置き換えます。</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>オプション : トリガーが起動された後に、トリガーと関連ルールを削除するかどうか。<code>&lt;delete_option&gt;</code> を次のいずれかに置き換えます。<ul><li><code>false</code> (デフォルト): トリガーの起動後アクションは何も実行されません。</li><li><code>true</code>: トリガーは、起動後に削除されます。</li><li><code>rules</code>: トリガーおよびそのすべての関連ルールは、起動後に削除されます。</li></ul></td>
</tr>
</tbody></table>

以下は、2019 年 12 月 25 日 12:30:00 UTC に 1 回起動するトリガーを作成する例です。 各トリガー・イベントには、パラメーター `name=Odin` および `place=Asgard` が含まれます。 トリガーが起動すると、トリガーとすべての関連ルールが削除されます。

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}

## 間隔ベースのスケジュールでの定期的なトリガー・イベントの起動
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/interval` フィードは、間隔ベースのスケジュールでトリガー・イベントを起動するようにアラーム・サービスを構成します。 間隔ベースのアラームを作成するには、次のコマンドを実行します。
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption><code>trigger create interval</code> コマンドのコンポーネントの理解</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>trigger create interval</code> コマンドのコンポーネントの理解</th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>作成するアラーム・トリガーのタイプ。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>interval フィードで使用するアラーム・パッケージのファイル・パス。</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td><code>&lt;minutes&gt;</code> をトリガーの起動間隔の長さ (分単位) を表す整数で置き換えます。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>オプション : トリガーが起動されたときに、<code>&lt;key&gt;</code> および <code>&lt;value&gt;</code> をトリガーのパラメーターに置き換えます。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>オプション: <code>&lt;startDate&gt;</code> を最初のトリガーが起動される日付に置き換えます。後続の起動は、minutes パラメーターで指定されている間隔の長さに基づいて発生します。 注: このパラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC からのミリ秒数を表し、ストリング値は  <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 形式 </a>でなければなりません。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>オプション: <code>&lt;stopDate&gt;</code> をトリガーが実行を停止する日付に置き換えます。この日付に達すると、トリガーは起動しなくなります。注: このパラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC からのミリ秒数を表し、ストリング値は  <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 形式 </a>でなければなりません。</td>
</tr>
</tbody></table>

以下の例では、2 分ごとに 1 回起動するトリガーを作成します。トリガーは可能になるとすぐに起動し、2019 年 1 月 31 日 23:59:00 UTC に起動を停止します。 各トリガー・イベントには、パラメーター `name=Odin` および `place=Asgard` が含まれます。

```
ibmcloud fn trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}

## cron を使用した時間ベースのスケジュールでのトリガーの起動

`/whisk.system/alarms/alarm` フィードは、指定頻度でトリガー・イベントを起動するようにアラーム・サービスを構成します。 時間ベースのアラームを作成するには、次のコマンドを実行します。
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption><code>trigger create periodic</code> コマンドのコンポーネントの理解</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>trigger create periodic</code> コマンドのコンポーネントの理解</th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>作成するアラーム・トリガーのタイプ。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>periodic アラーム・フィードで使用するアラーム・パッケージのファイル・パス。</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td><code>&lt;cron&gt;</code> を、トリガーを起動するタイミングを協定世界時 (UTC) で示すストリングに置き換えます。このストリングは、<a href="http://crontab.org">UNIX の crontab 構文</a>に基づいており、最大 5 つのフィールドのシーケンスです。フィールドは、<code>X X X X X</code> の形式でスペースで区切られます。以下に、トリガーを起動するさまざまな間隔を指定するストリングの例を示します。<ul><li><code>\* \* \* \* \*</code>: トリガーは、毎分の最初に起動します。</li><li><code>0 \* \* \* \*</code>: トリガーは、毎時間の最初に起動します。</li><li><code>0 \*/2 \* \* \*</code>: トリガーは、2 時間ごとに起動します (つまり、02:00:00、04:00:00、...)。</li><li><code>0 9 8 \* \*</code>: トリガーは、毎月 8 日の 9:00:00AM (UTC) に起動します。</li></ul></td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>オプション : トリガーが起動されたときに、<code>&lt;key&gt;</code> および <code>&lt;value&gt;</code> をトリガーのパラメーターに置き換えます。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>オプション: <code>&lt;startDate&gt;</code> を最初のトリガーが起動される日付に置き換えます。後続の起動は、minutes パラメーターで指定されている間隔の長さに基づいて発生します。 注: このパラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC からのミリ秒数を表し、ストリング値は  <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 形式 </a>でなければなりません。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>オプション: <code>&lt;stopDate&gt;</code> をトリガーが実行を停止する日付に置き換えます。この日付に達すると、トリガーは起動しなくなります。注: このパラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC からのミリ秒数を表し、ストリング値は  <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 形式 </a>でなければなりません。</td>
</tr>
</tbody></table>

以下は、2 分ごとに 1 回起動するトリガーを作成する例です。 トリガーは、2019 年 1 月 1 日 00:00:00 UTC まで起動を開始せず、2019 年 1 月 31 日 23:59:00 UTC に起動を停止します。 各トリガー・イベントには、パラメーター `name=Odin` および `place=Asgard` が含まれます。

```
ibmcloud fn trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}
