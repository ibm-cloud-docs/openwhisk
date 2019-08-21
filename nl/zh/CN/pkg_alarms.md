---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: alarms, serverless, triggers, functions

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



# 警报
{: #pkg_alarms}

`/whisk.system/alarms` 包可用于按指定的频率触发触发器。对于设置重复发生的作业或任务（例如，每小时调用一次系统备份）来说，警报非常有用。
{: shortdesc}

此包中包含以下订阅源。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/alarms`|包| - |警报和定期实用程序。|
|`/whisk.system/alarms/once`|订阅源|`date`、`trigger_payload`、`deleteAfterFire`|在特定日期触发一次触发器事件。|
|`/whisk.system/alarms/interval`|订阅源|`minutes`、`trigger_payload`、`startDate`、`stopDate`|根据基于时间间隔的计划触发触发器事件。|
|`/whisk.system/alarms/alarm`|订阅源|`cron`、`timezone`、`trigger_payload`、`startDate`、`stopDate`|使用 cron 根据基于时间的计划触发触发器事件。|



## 触发一次触发器事件
{: #pkg_alarms_one}

`/whisk.system/alarms/once` 订阅源将警报服务配置为在指定日期触发触发器事件一次。要创建触发一次的警报，请运行以下命令。

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption>了解 <code>trigger create fireOnce</code> 命令的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>trigger create fireOnce</code> 命令的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>要创建的警报触发器的类型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>fireOnce 订阅源的警报包文件路径。</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>将 <code>&lt;date&gt;</code> 替换为计划触发触发器的日期。触发器在指定的时间触发一次。`date` 参数支持整数值或字符串值。整数值表示自 `1970 年 1 月 1 日 00:00:00` UTC 以来的毫秒数，字符串值必须采用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（可选）在触发器触发时，将 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 替换为触发器的相应参数。</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>（可选）触发触发器后是否删除触发器和任何关联规则。将 <code>&lt;delete_option&gt;</code> 替换为下列其中一项：<ul><li><code>false</code> -（缺省值）触发器触发后不执行任何操作。</li><li><code>true</code> - 触发器触发后，将删除该触发器。</li><li><code>rules</code> - 触发器触发后，将删除该触发器及其所有关联的规则。</li></ul></td>
</tr>
</tbody></table>

以下命令是创建触发器的示例，此触发器将在 2019 年 12 月 25 日 12:30:00 UTC 触发一次。每个触发器事件都包含参数 `name=Odin` 和 `place=Asgard`。触发器触发后，将删除该触发器和所有关联的规则。

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## 根据基于时间间隔的计划定期触发触发器事件
{: #pkg_alarms_int}

`/whisk.system/alarms/interval` 订阅源将警报服务配置为根据基于时间间隔的计划触发触发器事件。要创建基于时间间隔的警报，请运行以下命令。
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>了解 <code>trigger create interval</code> 命令的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>trigger create interval</code> 命令的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>要创建的警报触发器的类型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>interval 订阅源的警报包文件路径。</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>将 <code>&lt;minutes&gt;</code> 替换为整数，用于表示触发器触发时间间隔长度（以分钟为单位）。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（可选）在触发器触发时，将 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 替换为触发器的相应参数。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>（可选）将 <code>&lt;startDate&gt;</code> 替换为计划触发第一个触发器的日期。后续的触发将根据 minutes 参数指定的时间间隔长度发生。此参数支持整数值或字符串值。整数值表示自 `1970 年 1 月 1 日 00:00:00` UTC 以来的毫秒数，字符串值必须采用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>（可选）将 <code>&lt;stopDate&gt;</code> 替换为计划停止触发器的日期。达到此日期后，触发器即不再触发。此参数支持整数值或字符串值。整数值表示自 `1970 年 1 月 1 日 00:00:00` UTC 以来的毫秒数，字符串值必须采用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

以下示例创建每 2 分钟触发一次的触发器。触发器会尽快触发，并且将在 `2019 年 1 月 31 日 23:59:00` UTC 停止触发。每个触发器事件都包含参数 `name=Odin` 和 `place=Asgard`。

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## 使用 cron 根据基于时间的计划触发触发器
{: #pkg_alarms_cron}

`/whisk.system/alarms/alarm` 订阅源将警报服务配置为按指定的频率触发触发器事件。要创建基于时间的警报，请运行以下命令。
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>了解 <code>trigger create periodic</code> 命令的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>trigger create periodic</code> 命令的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>要创建的警报触发器的类型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>periodic 警报订阅源的警报包文件路径。</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>将 <code>&lt;cron&gt;</code> 替换为字符串，用于指示何时触发触发器，时间采用全球标准时间 (UTC)。字符串基于 <a href="http://crontab.org">UNIX crontab 语法</a>，并且是最多包含 5 个字段的序列。字段之间用空格分隔，格式为 <code>X X X X X</code>。以下字符串是使用不同频率持续时间的示例。<ul><li><code>\* \* \* \* \*</code> - 触发器在每分钟开始时触发。</li><li><code>0 \* \* \* \*</code> - 触发器在每小时开始时触发。</li><li><code>0 \*/2 \* \* \*</code> - 触发器每 2 小时触发一次（即，02:00:00、04:00:00、...）。</li><li><code>0 9 8 \* \*</code> - 触发器在每个月 8 号上午 9:00:00 (UTC) 触发。</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>（可选）将 <code>&lt;timezone&gt;</code> 替换为用于指定时区的字符串。触发触发器的实际时间将相对于指定的时区进行修改。如果时区无效，将抛出错误。可以在 [Moment Timezone Web 站点](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}中检查可用的所有时区。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（可选）在触发器触发时，将 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 替换为触发器的相应参数。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>（可选）将 <code>&lt;startDate&gt;</code> 替换为计划触发第一个触发器的日期。后续的触发将根据 minutes 参数指定的时间间隔长度发生。此参数支持整数值或字符串值。整数值表示自 `1970 年 1 月 1 日 00:00:00` UTC 以来的毫秒数，字符串值必须采用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>（可选）将 <code>&lt;stopDate&gt;</code> 替换为希望触发器停止运行的日期。达到此日期后，触发器即不再触发。此参数支持整数值或字符串值。整数值表示自 `1970 年 1 月 1 日 00:00:00` UTC 以来的毫秒数，字符串值必须采用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

以下命令是创建每 2 分钟触发一次的触发器的示例。触发器直到 `2019 年 1 月 1 日 00:00:00` UTC 才会开始触发，并且将在 `2019 年 1 月 31 日 23:59:00` UTC 停止触发。每个触发器事件都包含参数 `name=Odin` 和 `place=Asgard`。

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}




