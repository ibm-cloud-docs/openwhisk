---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Aktivierungsprotokolle in IBM Cloud anzeigen
{: #openwhisk_logs}

Aktivierungsprotokolle können direkt über die [ {{site.data.keyword.openwhisk}}-Überwachungsseite](https://console.bluemix.net/openwhisk/dashboard/) angezeigt werden. Die Protokolle werden auch an [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) weitergeleitet, wo sie indexiert werden, wodurch eine Volltextsuche über alle generierten Nachrichten und eine komfortable Abfrage auf der Basis bestimmter Felder (wie Protokollebene) möglich wird.
{:shortdesc}

## Protokolle abfragen
{: #query-logs}

Bei der Verwendung von [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) mit Kibana ist die Abfrage Ihrer Protokolle problemlos möglich. Verwenden Sie die Abfragesyntax von Kibana, um nach Protokollen zu suchen.

Mit der {{site.data.keyword.openwhisk_short}}-Benutzerschnittstelle können Sie direkt zu den Protokollen und Ergebnissen Ihrer Aktionen in Kibana navigieren. Der Link für die **Protokolle** befindet sich im linken Navigationsbereich auf der [{{site.data.keyword.openwhisk}}-Überwachungsseite](https://console.bluemix.net/openwhisk/dashboard/). Wenn Sie auf die Detailseite einer bestimmten Aktion zugreifen, führt Sie der Link für die **Protokolle** zu den Ergebnissen (Aktivierungsdatensätzen) der betreffenden Aktion. Der Zeitrahmen, in dem die Protokolle angezeigt werden, ist standardmäßig auf 15 Minuten eingestellt. Sie können diesen Wert direkt in Kibana in der oberen rechten Ecke ändern, wenn Sie ältere Datensätze anzeigen möchten.

Im Folgenden finden Sie einige Beispiele für Abfragen, die für die Fehlerbehebung hilfreich sind.

### Alle Fehlerprotokolle suchen:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Alle Fehlerprotokolle suchen, die von "myAction" generiert werden:
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Ergebnisse abfragen
{: #query-results}

Neben Protokollzeilen indexiert [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) auch die von {{site.data.keyword.openwhisk_short}} generierten Ergebnisse (Aktivierungsdatensätze). Die Ergebnisse enthalten reichhaltige Metadaten, die für Aktivierungen relevant sind, z. B. ihre Dauer oder den Ergebniscode (Erfolg, Fehler). Alle Felder sind abfragbar und sie können Ihnen helfen zu verstehen, wie sich Ihre {{site.data.keyword.openwhisk_short}}-Aktionen verhalten.

Verwenden Sie die Abfragesyntax von Kibana, um nach Aktivierungen zu suchen. Im Folgenden finden Sie einige Beispiele für Abfragen, die für die Fehlerbehebung hilfreich sind.

### Alle fehlgeschlagenen Aktivierungen suchen:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Ähnlich wie bei Unix-Befehlen gibt eine "`0`" eine erfolgreich ausgeführte Aktion an, während alle anderen Angaben auf einen Fehler hinweisen.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Alle Aktivierungen suchen, die mit einem bestimmten Fehler fehlgeschlagen sind:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
