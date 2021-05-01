---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-30"

keywords: alarms, serverless, triggers, functions, event, cron

subcollection: openwhisk

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

# Alarms 
{: #pkg_alarms}

The `/whisk.system/alarms` package, available through {{site.data.keyword.openwhisk}}, can be used to fire a trigger or feed at a specified frequency. Alarms are useful for setting up recurring jobs or tasks, such as invoking a system back up every hour.
{: shortdesc}

The package includes the following feeds.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Package | - | Alarms and periodic utility. |
| `/whisk.system/alarms/once` | Feed | `date`, `trigger_payload`, `deleteAfterFire` | Fire trigger event one time on a specific date. |
| `/whisk.system/alarms/interval` | Feed | `minutes`, `trigger_payload`, `startDate`, `stopDate` | Fire trigger event on an interval-based schedule. |
| `/whisk.system/alarms/alarm` | Feed | `cron`, `timezone`, `trigger_payload`, `startDate`, `stopDate` | Fire trigger event on a time-based schedule by using cron. |

## Firing a trigger event one time
{: #pkg_alarms_one}

The `/whisk.system/alarms/once` feed configures the Alarm service to fire a trigger event one time on a specified date. To create a fire-once alarm, run the following command.

```sh
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption>Understanding the <code>trigger create fireOnce</code> command components</caption>
<thead>
<col width="40%">
<col width="60%">
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>trigger create fireOnce</code> command components</th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>The type of alarm trigger you are creating.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
  <td>The alarm package file path for the <code>fireOnce</code> feed.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Replace <code>&lt;date&gt;</code> with the date when you plan to fire the trigger. The trigger fires once at the specified time. The `date` parameter supports an integer or string value. The integer value represents the number of milliseconds since 1 `January 1970 00:00:00` UTC and the string value must be in the <a href="https://262.ecma-international.org/5.1/">ISO 8601 format</a>.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Optional) Replace <code>&lt;key&gt;</code> and <code>&lt;value&gt;</code> with the parameters of the trigger when the trigger is fired.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>(Optional) Whether the trigger and any associated rules are deleted after the trigger is fired. Replace <code>&lt;delete_option&gt;</code> with one of the following.<ul><li><code>false</code> - (default) No action is taken after the trigger fires.</li><li><code>true</code> - The trigger is deleted after it fires.</li><li><code>rules</code> - The trigger and all of its associated rules are deleted after it fires.</li></ul></td>
</tr>
</tbody></table>

The following command is an example of creating a trigger that fires once on December 25, 2019, 12:30:00 UTC. Each trigger event has the parameters `name=Odin` and `place=Asgard`. After the trigger fires, the trigger and all associated rules are deleted.

```sh
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}

## Firing a trigger event periodically on an interval-based schedule
{: #pkg_alarms_int}

The `/whisk.system/alarms/interval` feed configures the Alarm service to fire a trigger event on an interval-based schedule. To create an interval-based alarm, run the following command.

```sh
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Understanding the <code>trigger create interval</code> command components</caption>
<thead>
<col width="40%">
<col width="60%">
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>trigger create interval</code> command components</th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>The type of alarm trigger you are creating.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>The alarm package file path for the interval feed.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Replace <code>&lt;minutes&gt;</code> with an integer that represents the length of the interval, in minutes, between trigger fires.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Optional) Replace <code>&lt;key&gt;</code> and <code>&lt;value&gt;</code> with the parameters of the trigger when the trigger is fired.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Optional) Replace <code>&lt;startDate&gt;</code> with the date that you plan for the first trigger to fire. Subsequent fires occur based on the interval length that is specified by the minutes parameter. This parameter supports an integer or string value. The integer value represents the number of milliseconds since `1 January 1970 00:00:00` UTC and the string value must be in the <a href="https://262.ecma-international.org/5.1/">ISO 8601 format</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Optional) Replace <code>&lt;stopDate&gt;</code> with the date when you plan for the trigger to stop. Triggers do not fire once this date is reached. This parameter supports an integer or string value. The integer value represents the number of milliseconds since `1 January 1970 00:00:00` UTC and the string value must be in the <a href="https://262.ecma-international.org/5.1/">ISO 8601 format</a>.</td>
</tr>
</tbody></table>

The following example creates a trigger that fires once every 2 minutes. The trigger fires as soon as possible, and stops firing `January 31, 2019, 23:59:00` UTC. Each trigger event has the parameters `name=Odin` and `place=Asgard`.

```sh
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}

## Firing a trigger on a time-based schedule by using cron
{: #pkg_alarms_cron}

The `/whisk.system/alarms/alarm` feed configures the Alarm service to fire a trigger event at a specified frequency. To create a time-based alarm, run the following command.

```sh
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Understanding the <code>trigger create periodic</code> command components</caption>
<thead>
<col width="40%">
<col width="60%">
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>trigger create periodic</code> command components</th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>The type of alarm trigger you are creating.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>The alarm package file path for the periodic alarm feed.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Replace <code>&lt;cron&gt;</code> with a string that indicates when to fire the trigger in Coordinated Universal Time (UTC). The string is based on the <a href="http://crontab.org">UNIX crontab syntax</a> and is a sequence of maximum five fields. Fields are separated by spaces in the format <code>X X X X X</code>. The following strings are examples that use varying durations of frequency.<ul><li><code>\* \* \* \* \*</code> - The trigger fires at the start of every minute.</li><li><code>0 \* \* \* \*</code> - The trigger fires at the start of every hour.</li><li><code>0 \*/2 \* \* \*</code> - The trigger fires every 2 hours (that is, 02:00:00, 04:00:00, ...).</li><li><code>0 9 8 \* \*</code> - The trigger fires at 9:00:00AM (UTC) on the eighth day of every month.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>(Optional) Replace <code>&lt;timezone&gt;</code> with a string that specifies the time zone. The actual time to fire the trigger is modified relative to the specified time zone. If the time zone is invalid, an error is thrown. You can check all of the available time zones at the [Moment Time zone Website](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Optional) Replace <code>&lt;key&gt;</code> and <code>&lt;value&gt;</code> with the parameters of the trigger when the trigger is fired.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Optional) Replace <code>&lt;startDate&gt;</code> with the date when you plan to fire the first trigger. Subsequent fires occur based on the interval length that is specified by the minutes parameter. This parameter supports an integer or string value. The integer value represents the number of milliseconds since `1 January 1970 00:00:00` UTC and the string value must be in the <a href="https://262.ecma-international.org/5.1/">ISO 8601 format</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Optional) Replace <code>&lt;stopDate&gt;</code> with the date that you want the trigger to stop running. Triggers do not fire once this date is reached. This parameter supports an integer or string value. The integer value represents the number of milliseconds since `1 January 1970 00:00:00` UTC and the string value must be in the <a href="https://262.ecma-international.org/5.1/">ISO 8601 format</a>.</td>
</tr>
</tbody></table>

The following command is an example of creating a trigger that fires once every 2 minutes. The trigger does not start firing until
`January 1, 2019, 00:00:00` UTC and stops firing `January 31, 2019, 23:59:00` UTC. Each trigger event has the parameters `name=Odin` and `place=Asgard`.

```sh
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}
