---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: alarms, triggers, event, schedule, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Alarme zum Planen von Triggern verwenden
{: #openwhisk_catalog_alarm}

Das Paket `/whisk.system/alarms` kann verwendet werden, um einen Auslöser mit einer angegebenen Häufigkeit zu aktivieren. Alarme sind nützlich, um sich wiederholende Jobs oder Tasks einzurichten, wie zum Beispiel das stündliche Aufrufen einer Systemsicherungsaktion.
{: shortdesc}

Das Paket enthält die folgenden Feeds.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Paket | - | Alarme und Dienstprogramm für regelmäßige Ausführung. |
| `/whisk.system/alarms/once` | Feed | date, trigger_payload, deleteAfterFire | Einmaliges Aktivieren eines Auslöserereignisses an einem bestimmten Datum. |
| `/whisk.system/alarms/interval` | Feed | minutes, trigger_payload, startDate, stopDate | Auslösen eines Auslöserereignisses nach einem intervallbasierten Zeitplan. |
| `/whisk.system/alarms/alarm` | Feed | cron, timezone, trigger_payload, startDate, stopDate | Aktivieren eines Auslöserereignisses nach einem zeitbasierten Zeitplan unter Verwendung von Cron. |

## Auslöserereignis einmal aktivieren

Durch den Feed `/whisk.system/alarms/once` wird der Service 'Alarm' so konfiguriert, dass er ein Auslöserereignis an einem angegebenen Datum einmal aktiviert. Führen Sie den folgenden Befehl aus, um einen Alarm zu erstellen, der lediglich einmal ausgelöst wird:
```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}

<table>
<caption>Informationen zu den Komponenten des Befehls <code>trigger create fireOnce</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten des Befehls <code>trigger create fireOnce</code></th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>Der Typ von Alarmauslöser, den Sie erstellen.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>Der Dateipfad des Alarmpakets für den 'fireOnce'-Feed.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Ersetzen Sie <code>&lt;date&gt;</code> durch das Datum, an dem der Auslöser aktiviert wird. Der Auslöser wird nur einmal zum angegebenen Zeitpunkt ausgelöst. Hinweis: Der Parameter `date` unterstützt sowohl ganzzahlige Werte als auch Zeichenfolgewerte. Der ganzzahlige Wert stellt die Anzahl der seit dem 1. Januar 1970 00:00:00 Uhr (UTC) verstrichenen Millisekunden dar. Der Zeichenfolgewert muss im Format des Standards <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a> angegeben werden.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Optional: Ersetzen Sie <code>&lt;key&gt;</code> und <code>&lt;value&gt;</code> durch die Parameter des Auslösers zum Anwenden des Auslösers.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>Optional: Gibt an, ob der Auslöser und alle zugehörigen Regeln gelöscht werden, nachdem der Auslöser gestartet wurde. Ersetzen Sie <code>&lt;delete_option&gt;</code> durch einen der folgenden Werte:<ul><li><code>false</code> (Standardwert): Es wird keine Aktion ausgeführt, nachdem der Auslöser angewendet wurde.</li><li><code>true</code>: Der Auslöser wird nach seiner Anwendung gelöscht.</li><li><code>rules</code>: Der Auslöser wird zusammen mit den zugehörigen Regeln nach seiner Anwendung gelöscht.</li></ul></td>
</tr>
</tbody></table>

Das folgende Beispiel zeigt, wie ein Auslöser erstellt wird, der einmal am 25. Dezember 2019 um 12:30:00 Uhr (UTC) aktiviert wird. Für jedes Auslöserereignis gelten die Parameter `name=Odin` und `place=Asgard`. Nach seiner Anwendung wird der Auslöser mit allen zugehörigen Regeln gelöscht.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}

## Auslöserereignis regelmäßig nach einem intervallbasierten Zeitplan aktivieren
{: #openwhisk_catalog_alarm_fire}

Durch den Feed `/whisk.system/alarms/interval` wird der Service 'Alarm' so konfiguriert, dass ein Auslöserereignis nach einem intervallbasierten Zeitplan aktiviert wird. Führen Sie den folgenden Befehl aus, um einen intervallbasierten Alarm zu erstellen:
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Informationen zu den Komponenten des Befehls <code>trigger create interval</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten des Befehls <code>trigger create interval</code></th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>Der Typ von Alarmauslöser, den Sie erstellen.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>Der Dateipfad des Alarmpakets für den 'interval'-Feed.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Ersetzen Sie <code>&lt;minutes&gt;</code> durch eine ganze Zahl, die die Länge des Intervalls (in Minuten) zwischen den einzelnen Anwendungen des Auslösers angibt.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Optional: Ersetzen Sie <code>&lt;key&gt;</code> und <code>&lt;value&gt;</code> durch die Parameter des Auslösers zum Anwenden des Auslösers.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Optional: Ersetzen Sie <code>&lt;startDate&gt;</code> durch das Datum, an dem der erste Auslöser aktiviert wird. Nachfolgende Aktivierungen erfolgen entsprechend der Intervalllänge, die durch den Parameter 'minutes' angegeben wird. Hinweis: Dieser Parameter unterstützt sowohl ganzzahlige Werte als auch Zeichenfolgewerte. Der ganzzahlige Wert stellt die Anzahl der seit dem 1. Januar 1970 00:00:00 Uhr (UTC) verstrichenen Millisekunden dar. Der Zeichenfolgewert muss im Format des Standards <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a> angegeben werden.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Optional: Ersetzen Sie <code>&lt;stopDate&gt;</code> durch das Datum, an dem die Anwendung des Auslösers beendet wird. Ab Erreichen dieses Datums werden keine weiteren Auslöser mehr aktiviert. Hinweis: Dieser Parameter unterstützt sowohl ganzzahlige Werte als auch Zeichenfolgewerte. Der ganzzahlige Wert stellt die Anzahl der seit dem 1. Januar 1970 00:00:00 Uhr (UTC) verstrichenen Millisekunden dar. Der Zeichenfolgewert muss im Format des Standards <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a> angegeben werden.</td>
</tr>
</tbody></table>

Im folgenden Beispiel wird ein Auslöser erstellt, der in Zeitabständen von je 2 Minuten aktiviert wird. Die Anwendung des Auslösers erfolgt so bald wie möglich und endet am 31. Januar 2019 um 23:59:00 Uhr (UTC). Für jedes Auslöserereignis gelten die Parameter `name=Odin` und `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}

## Auslöser nach zeitbasiertem Zeitplan mit 'cron' aktivieren

Durch den Feed `/whisk.system/alarms/alarm` wird der Service 'Alarm' so konfiguriert, dass er ein Auslöserereignis in einer angegebenen Häufigkeit aktiviert. Führen Sie den folgenden Befehl aus, um einen zeitbasierten Alarm zu erstellen:
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Informationen zu den Komponenten des Befehls <code>trigger create periodic</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten des Befehls <code>trigger create periodic</code></th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>Der Typ von Alarmauslöser, den Sie erstellen.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>Der Dateipfad des Alarmpakets für den 'periodic'-Feed.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Ersetzen Sie <code>&lt;cron&gt;</code> durch eine Zeichenfolge, die in koordinierter Weltzeit (UTC) angibt, wann die Aktivierung des Auslösers erfolgen soll. Diese Zeichenfolge basiert auf der <a href="http://crontab.org">UNIX-crontab-Syntax</a> und umfasst eine Folge von höchstens 5 Feldern. Die einzelnen Felder werden jeweils durch Leerzeichen im Format <code>X X X X X</code> getrennt. Die folgenden Zeichenfolgen sind Beispiele für die Angabe verschiedener Wiederholungshäufigkeiten:<ul><li><code>\* \* \* \* \*</code>: Der Auslöser wird zu Beginn einer jeden Minute aktiviert.</li><li><code>0 \* \* \* \*</code>: Der Auslöser wird zu Beginn einer jeden Stunde aktiviert.</li><li><code>0 \*/2 \* \* \*</code>: Der Auslöser wird alle zwei Stunden aktiviert (d. h. um 02:00:00, 04:00:00 ...).</li><li><code>0 9 8 \* \*</code>: Der Auslöser wird jeweils um 9:00:00 Uhr (UTC) am achten Tag eines jeden Monats ausgelöst.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>Optional: Ersetzen Sie <code>&lt;timezone&gt;</code> durch eine Zeichenfolge, die die Zeitzone angibt. Die tatsächliche Zeit zum Auslösen des Auslösers wird dann relativ zur angegebenen Zeitzone geändert. Wenn die Zeitzone ungültig ist, wird ein Fehler ausgegeben. Sie können alle verfügbaren Zeitzonen auf der Moment Timezone-Website (http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names) prüfen. </td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Optional: Ersetzen Sie <code>&lt;key&gt;</code> und <code>&lt;value&gt;</code> durch die Parameter des Auslösers zum Anwenden des Auslösers.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Optional: Ersetzen Sie <code>&lt;startDate&gt;</code> durch das Datum, an dem der erste Auslöser aktiviert wird. Nachfolgende Aktivierungen erfolgen entsprechend der Intervalllänge, die durch den Parameter 'minutes' angegeben wird. Hinweis: Dieser Parameter unterstützt sowohl ganzzahlige Werte als auch Zeichenfolgewerte. Der ganzzahlige Wert stellt die Anzahl der seit dem 1. Januar 1970 00:00:00 Uhr (UTC) verstrichenen Millisekunden dar. Der Zeichenfolgewert muss im Format des Standards <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a> angegeben werden.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Optional: Ersetzen Sie <code>&lt;stopDate&gt;</code> durch das Datum, an dem die Anwendung des Auslösers beendet wird. Ab Erreichen dieses Datums werden keine weiteren Auslöser mehr aktiviert. Hinweis: Dieser Parameter unterstützt sowohl ganzzahlige Werte als auch Zeichenfolgewerte. Der ganzzahlige Wert stellt die Anzahl der seit dem 1. Januar 1970 00:00:00 Uhr (UTC) verstrichenen Millisekunden dar. Der Zeichenfolgewert muss im Format des Standards <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a> angegeben werden.</td>
</tr>
</tbody></table>

Das folgende Beispiel veranschaulicht die Erstellung eines Auslösers, der in Zeitabständen von je 2 Minuten aktiviert wird. Die Anwendung des Auslösers beginnt erst ab dem 01. Januar 2019 um 00:00:00 Uhr (UTC) und endet am 31. Januar 23:59:00 Uhr (UTC). Für jedes Auslöserereignis gelten die Parameter `name=Odin` und `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}
