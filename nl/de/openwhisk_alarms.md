---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Alarme
{: #openwhisk_catalog_alarm}

Das Paket `/whisk.system/alarms` kann verwendet werden, um einen Auslöser mit einer angegebenen Häufigkeit zu aktivieren. Alarme sind nützlich, um sich wiederholende Jobs oder Tasks einzurichten, wie zum Beispiel das stündliche Aufrufen einer Systemsicherungsaktion.
{: shortdesc}

Das Paket enthält die folgenden Feeds.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Paket | - | Alarme und Dienstprogramm für regelmäßige Ausführung. |
| `/whisk.system/alarms/interval` | Feed | minutes, trigger_payload, startDate, stopDate | Aktivieren eines Auslöserereignisses nach einem intervallbasierten Zeitplan. |
| `/whisk.system/alarms/once` | Feed | date, trigger_payload, deleteAfterFire | Einmaliges Aktivieren eines Auslöserereignisses an einem bestimmten Datum. |
| `/whisk.system/alarms/alarm` | Feed | cron, trigger_payload, startDate, stopDate | Aktivieren eines Auslöserereignisses nach einem zeitbasierten Zeitplan unter Verwendung von Cron. |


## Auslöserereignis regelmäßig nach einem intervallbasierten Zeitplan aktivieren
{: #openwhisk_catalog_alarm_fire}

Durch den Feed `/whisk.system/alarms/interval` wird der Alarmservice so konfiguriert, dass ein Auslöserereignis nach einem intervallbasierten Zeitplan aktiviert wird. Die folgenden Parameter sind verfügbar:

- `minutes` (*erforderlich*): Eine ganze Zahl, die die Länge des Intervalls (in Minuten) zwischen Auslöseraktivierungen angibt.
- `trigger_payload` (*optional*): Der Wert dieses Parameters wird jedes Mal zum Inhalt des Auslösers, wenn der Auslöser aktiviert wird.
- `startDate` (*optional*): Das Datum, an dem der Auslöser aktiviert wird. Nachfolgende Aktivierungen erfolgen entsprechend der Intervalllänge, die durch den Parameter `minutes` angegeben wird.
- `stopDate` (*optional*): Das Datum, an dem die Ausführung des Auslösers gestoppt wird. Nach Erreichen dieses Datums werden keine Auslöser mehr aktiviert.

  **Hinweis:** Die Parameter `startDate` und `stopDate` unterstützen einen ganzzahligen Wert oder einen Zeichenfolgewert. Der ganzzahlige Wert stellt die Anzahl der Millisekunden seit dem 1. Januar 1970, 00:00:00 Uhr (UTC), dar. Der Zeichenfolgewert muss das ISO-8601-Format (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) haben.

Im folgenden Beispiel wird ein Auslöser erstellt, der alle 2 Minuten einmal aktiviert wird. Der Auslöser wird so bald, wie möglich, aktiviert und die Aktivierung wird am 31. Januar 2019 um 23:59:00 Uhr (UTC) gestoppt.

  ```
  ibmcloud fn trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

Jedes generierte Ereignis enthält Parameter, die die Eigenschaften sind, die durch den Wert `trigger_payload` angegeben werden. In diesem Fall erhält jedes Auslöserereignis die Parameter `name=Odin` und `place=Asgard`.

## Auslöserereignis einmal aktivieren

Der Feed `/whisk.system/alarms/once` konfiguriert den Alarm-Service so, dass er ein Auslöserereignis an einem angegebenen Datum aktiviert. Die folgenden Parameter sind verfügbar:

- `date` (*erforderlich*): Das Datum, an dem der Auslöser aktiviert wird. Der Auslöser wird zu dem bestimmten Zeitpunkt nur einmal aktiviert.

  **Hinweis:** Der Parameter `date` unterstützt einen ganzzahligen Wert oder einen Zeichenfolgewert. Der ganzzahlige Wert stellt die Anzahl der Millisekunden
seit dem 1. Januar 1970 00:00:00 Uhr (UTC) dar. Der Zeichenfolgewert muss das ISO-8601-Format (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) haben.

- `trigger_payload` (*optional*): Der Wert dieses Parameters wird zum Inhalt des Auslösers, wenn der Auslöser aktiviert wird.

- `deleteAfterFire` (*optional*, default:false): Der Wert dieses Parameters bestimmt, ob der Auslöser und potenziell alle zugeordneten Regeln gelöscht werden, nachdem der Auslöser aktiviert wurde.
  - `false`: Es wird keine Aktion nach dem Aktivieren des Auslösers ausgeführt.
  - `true`: Der Auslöser wird nach dem Aktivieren gelöscht.
  - `rules`: Der Auslöser und alle zugehörigen Regeln werden nach dem Aktivieren gelöscht.

Das folgende Beispiel zeigt, wie ein Auslöser erstellt wird, der einmal am 25. Dezember 2019 um 12:30:00 Uhr (UTC) aktiviert wird. Der Auslöser wird nach dem Aktivieren mit allen zugehörigen Regeln gelöscht.

  ```
  ibmcloud fn trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2019-12-25T12:30:00.000Z" \
    --param deleteAfterFire "rules"
  ```
  {: pre}

## Auslöser nach zeitbasiertem Zeitplan mit 'cron' aktivieren

Der Feed `/whisk.system/alarms/alarm` konfiguriert den Alarm-Service so, dass er ein Auslöserereignis mit einer angegebenen Häufigkeit aktiviert. Die folgenden Parameter sind verfügbar:

- `cron` (*erforderlich*): Eine Zeichenfolge auf der Basis der UNIX-Syntax 'crontab', die angibt, wann der Auslöser zu aktivieren ist (angegeben in koordinierter Weltzeit, UTC). Die Zeichenfolge besteht aus einer Reihe von fünf durch Leerzeichen getrennten Feldern: `X X X X X`.
Weitere Informationen finden Sie unter: http://crontab.org. Die folgenden Beispiele zeigen Zeichenfolgen, die unterschiedliche Zeitdauern für die Häufigkeit angeben.

  - `* * * * *`: Der Auslöser wird zu Beginn jeder Minute aktiviert.
  - `0 * * * *`: Der Auslöser wird zu Beginn jeder Stunde aktiviert.
  - `0 */2 * * *`: Der Auslöser wird alle 2 Stunden (d. h. 02:00:00, 04:00:00, ...) aktiviert.
  - `0 9 8 * *`: Der Auslöser wird um 9:00:00 Uhr (UTC) am achten Tag jedes Monats ausgelöst.

  **Hinweis:** Der Parameter `cron` unterstützt nur fünf Felder.

- `trigger_payload` (*optional*): Der Wert dieses Parameters wird jedes Mal zum Inhalt des Auslösers, wenn der Auslöser aktiviert wird.

- `startDate` (*optional*): Das Datum, an dem die Ausführung des Auslösers gestartet wird. Der Auslöser wird nach dem Zeitplan aktiviert, der durch den Parameter 'cron' angegeben wird.

- `stopDate` (*optional*): Das Datum, an dem die Ausführung des Auslösers gestoppt wird. Wenn dieses Datum erreicht wird, werden keine Auslöser mehr aktiviert.

  **Hinweis:** Die Parameter `startDate` und `stopDate` unterstützen einen ganzzahligen Wert oder einen Zeichenfolgewert. Der ganzzahlige Wert stellt die Anzahl der Millisekunden seit dem 1. Januar 1970, 00:00:00 Uhr (UTC), dar. Der Zeichenfolgewert muss das ISO-8601-Format (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15) haben.

Das folgende Beispiel zeigt, wie ein Auslöser erstellt wird, der einmal alle 2 Minuten aktiviert wird, wobei das Auslöserereignis die Werte für `name` und `place` enthält. Der Auslöser beginnt die Ausführung der Aktivierungen erst am 01. Januar 2019 um 00:00:00 Uhr (UTC) und stoppt sie am 31. Januar 23:59:00 Uhr (UTC).

  ```
  ibmcloud fn trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **Hinweis:** Der Parameter `maxTriggers` ist veraltet und wird in Zukunft entfernt. Verwenden Sie zum Stoppen des Auslösers den Parameter `stopDate`.
