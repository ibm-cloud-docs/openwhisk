---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# アラーム
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` パッケージを使用して、指定した頻度でトリガーを起動できます。 アラームは、1 時間ごとにシステム・バックアップ・アクションを起動するといった、ジョブやタスクの繰り返しをセットアップする際に便利です。
{: shortdesc}

このパッケージには、以下のフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | パッケージ | - | アラームおよび定期的ユーティリティー。 |
| `/whisk.system/alarms/interval` | フィード | minutes、trigger_payload、startDate、stopDate | 間隔ベースのスケジュールでトリガー・イベントを起動します。 |
| `/whisk.system/alarms/once` | フィード | date、trigger_payload、deleteAfterFire | 特定の日付に 1 回、トリガー・イベントを起動します。 |
| `/whisk.system/alarms/alarm` | フィード | cron、trigger_payload、startDate、stopDate | cron を使用して、時間ベースのスケジュールでトリガー・イベントを起動します。 |


## 間隔ベースのスケジュールでの定期的なトリガー・イベントの起動
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/interval` フィードは、間隔ベースのスケジュールでトリガー・イベントを起動するようにアラーム・サービスを構成します。 パラメーターは次のとおりです。

- `minutes` (*必須*): トリガー起動の間隔の長さ (分) を表す整数。
- `trigger_payload` (*オプション*): このパラメーターの値は、トリガーが起動するごとにトリガーの内容になります。
- `startDate` (*オプション*): 最初のトリガーが起動する日付。  後続の起動は、`minutes` パラメーターで指定されている間隔の長さに基づいて発生します。
- `stopDate` (*オプション*): トリガーの実行を停止する日付。 この日付に達すると、トリガーは起動しなくなります。

  **注**: `startDate` および `stopDate` パラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC 以降のミリ秒数を表します。ストリング値は、ISO 8601 フォーマット (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) でなければなりません。

以下の例では、2 分ごとに 1 回起動するトリガーを作成します。 トリガーは可能になるとすぐに起動し、2019 年 1 月 31 日 23:59:00 UTC に起動を停止します。

  ```
  ibmcloud fn trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

生成された各イベントにはパラメーターが含まれます。パラメーターは、`trigger_payload` 値で指定されたプロパティーです。 この場合、各トリガー・イベントには、パラメーター `name=Odin` および `place=Asgard` が含まれます。

## トリガー・イベントの 1 回の起動

`/whisk.system/alarms/once` フィードは、特定の日付にトリガー・イベントを起動するようにアラーム・サービスを構成します。 パラメーターは次のとおりです。

- `date` (*必須*): トリガーが起動される日付。トリガーは、指定時刻に 1 回だけ起動します。

  **注**: `date` パラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC 以降のミリ秒数を表します。ストリング値は、ISO 8601 フォーマット (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) でなければなりません。

- `trigger_payload` (*オプション*): このパラメーターの値は、トリガーの起動時におけるトリガーの内容になります。

- `deleteAfterFire` (*オプション*、デフォルト: false): このパラメーターの値によって、トリガーが起動された後にトリガーおよびそのトリガーに関連付けられたすべてのルールが削除されるかどうかが決まります。
  - `false`: トリガーが起動された後にアクションは実行されません。
  - `true`: トリガーは、起動後に削除されます。
  - `rules`: トリガーおよびそのすべての関連ルールは、起動後に削除されます。

以下は、2019 年 12 月 25 日 12:30:00 UTC に 1 回起動するトリガーを作成する例です。 トリガーの起動後、トリガーならびにそのすべての関連ルールが削除されます。

  ```
  ibmcloud fn trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2019-12-25T12:30:00.000Z" \
    --param deleteAfterFire "rules"
  ```
  {: pre}

## cron を使用した時間ベースのスケジュールでのトリガーの起動

`/whisk.system/alarms/alarm` フィードは、指定頻度でトリガー・イベントを起動するようにアラーム・サービスを構成します。 パラメーターは次のとおりです。

- `cron` (*必須*): 協定世界時 (UTC) でトリガーの起動タイミングを示す、UNIX crontab 構文に基づいたストリング。このストリングは、`X X X X X` のようにスペースで区切られた 5 個のフィールドのシーケンスです。
詳しくは、http://crontab.org を参照してください。 以下のストリングは、さまざまな頻度の期間を使用した例です。

  - `* * * * *`: トリガーは、毎分の最初に起動します。
  - `0 * * * *`: トリガーは、毎時間の最初に起動します。
  - `0 */2 * * *`: トリガーは、2 時間ごとに起動します (つまり、02:00:00、04:00:00、...)。
  - `0 9 8 * *`: トリガーは、毎月 8 日の 9:00:00AM (UTC) に起動します。

  **注**: パラメーター `cron` でサポートされるフィールドは 5 つのみです。

- `trigger_payload` (*オプション*): このパラメーターの値は、トリガーが起動するごとにトリガーの内容になります。

- `startDate` (*オプション*): トリガーの実行を開始する日付。 トリガーは、cron パラメーターで指定されたスケジュールに基づいて起動します。

- `stopDate` (*オプション*): トリガーの実行を停止する日付。 トリガーは、この日付に達すると、起動しなくなります。

  **注**: `startDate` および `stopDate` パラメーターでは、整数値またはストリング値がサポートされます。 整数値は、1970 年 1 月 1 日 00:00:00 UTC 以降のミリ秒数を表します。ストリング値は、ISO 8601 フォーマット (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) でなければなりません。

以下は、トリガー・イベントで `name` および `place` の値を指定して、2 分ごとに 1 回起動するトリガーを作成する例です。 トリガーは、2019 年 1 月 1 日 00:00:00 UTC まで起動を開始せず、2019 年 1 月 31 日 23:59:00 UTC に起動を停止します。

  ```
  ibmcloud fn trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **注**: パラメーター `maxTriggers` は非推奨であり、間もなく削除されます。 トリガーを停止する場合は、`stopDate` パラメーターを使用してください。
