---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Aktivität protokollieren und überwachen
{: #openwhisk_logs}

Als Unterstützung bei der Behebung von Problemen und zur Verbesserung des Allgemeinzustands sowie der Leistung Ihrer Aktionen ist in {{site.data.keyword.openwhisk_short}} die Protokollierung und Überwachung automatisch aktiviert.

## Protokolle anzeigen
{: #view-logs}

Sie können Aktivierungsprotokolle direkt über das Dashboard für {{site.data.keyword.openwhisk_short}}-Überwachung anzeigen. Die Protokolle werden auch an [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) weitergeleitet, wo sie indexiert werden, wodurch eine Volltextsuche für alle generierten Nachrichten und eine komfortable Abfrage auf der Basis bestimmter Felder möglich wird.
{:shortdesc}

1. Öffnen Sie die Seite [{{site.data.keyword.openwhisk_short}}-Überwachung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/dashboard/).

2. Optional: Wenn Sie nur für eine bestimmte Aktion Protokolle anzeigen wollen, beschränken Sie die Überwachungszusammenfassung auf die betreffende Aktion. Wählen Sie im Abschnitt 'Filteroptionen' in der Dropdown-Liste **Begrenzen auf** den Namen der entsprechenden Aktion aus.

3. Klicken Sie im Navigationsbereich links auf **Protokolle**. Die {{site.data.keyword.loganalysisshort_notm}}-Seite für Kibana wird geöffnet.

4. Optional: Damit ältere Protokolle angezeigt werden, ändern Sie den Standardwert für den Zeitrahmen in 15 Minuten. Klicken Sie hierzu in der rechten oberen Ecke auf **Letzte 15 Minuten** und wählen Sie einen anderen Zeitrahmen aus.

### Protokolle abfragen
{: #query-logs}

Wenn Sie in [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) bestimmte Aktivierungsprotokolle suchen wollen, verwenden Sie hierfür die Abfragesyntax von Kibana.

Die folgenden Beispielabfragen können Ihnen bei der Behebung von Fehlern helfen.
  * Alle Fehlerprotokolle suchen:
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Alle Fehlerprotokolle suchen, die von 'myAction' generiert wurden:
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Ergebnisse abfragen
{: #query-results}

Zusätzlich zu Protokollzeilen indexiert [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) auch die von {{site.data.keyword.openwhisk_short}} generierten Ergebnisse bzw. Aktivierungsdatensätze. Die Ergebnisse enthalten Aktivierungsmetadaten wie etwa die Aktivierungsdauer oder den Aktivierungsergebniscode. Das Abfragen von Ergebnisfeldern kann Ihnen dabei helfen, das Verhalten Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu verstehen.

Sie können bestimmte Aktivierungsprotokolle suchen, indem Sie die Abfragesyntax von Kibana verwenden. Die folgenden Beispielabfragen können Ihnen bei der Behebung von Fehlern helfen.

* Alle fehlgeschlagenen Aktivierungen suchen:
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    In den Ergebnissen gibt der Wert `0` eine erfolgreich ausgeführte Aktion an, während alle übrigen Werte auf einen Fehler hinweisen.

* Alle Aktivierungen suchen, die mit einem bestimmten Fehler fehlgeschlagen sind:
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

## Aktivität überwachen
{: #openwhisk_monitoring}

Das [{{site.data.keyword.openwhisk_short}}-Dashboard](https://console.bluemix.net/openwhisk/dashboard/) stellt eine grafische Zusammenfassung Ihrer Aktivität bereit. Verwenden Sie das Dashboard, um die Leistung und den Status Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu ermitteln.
{:shortdesc}

Sie können Protokolle filtern, indem Sie zunächst auswählen, welche Aktionsprotokolle angezeigt werden sollen, und dann den Zeitrahmen der protokollierten Aktivität auswählen. Diese Filter werden auf alle Ansichten im Dashboard angewendet. Sie können jederzeit auf die Option **Erneut laden** klicken, um das Dashboard mit den neuesten Aktivierungsprotokolldaten zu aktualisieren.

### Aktivitätenzusammenfassung
{: #summary}

Die Ansicht **Zusammenfassung der Aktivitäten** stellt eine allgemeine Zusammenfassung Ihrer {{site.data.keyword.openwhisk_short}}-Umgebung bereit. Verwenden Sie diese Ansicht zur Überwachung des Allgemeinzustands und der Leistung Ihres für {{site.data.keyword.openwhisk_short}} eingerichteten Service. Die Metriken in dieser Ansicht bieten Ihnen die folgenden Möglichkeiten:
* Sie können die Nutzungsrate der für {{site.data.keyword.openwhisk_short}} eingerichteten Aktionen Ihres Service ermitteln, indem Sie die Häufigkeit anzeigen, mit der sie aufgerufen wurden.
* Sie können die Gesamtrate von Fehlern über alle Aktionen hinweg ermitteln. Wenn Sie auf einen Fehler stoßen, können Sie die Services oder Aktionen eingrenzen, bei denen Fehler aufgetreten sind, indem Sie die Ansicht **Aktivitätenhistogramm** anzeigen. Sie können die Fehler selbst eingrenzen, indem Sie das **Aktivitätenprotokoll** anzeigen.
* Sie können die Leistung Ihrer Aktionen ermitteln, indem Sie die durchschnittliche Ausführungszeit anzeigen, die mit jeder Aktion verbunden ist.

### Aktivitätenzeitachse
{: #timeline}

Die Ansicht **Aktivitätenzeitachse** zeigt ein vertikales Balkendiagramm an, um die Aktivitäten der vergangenen und gegenwärtigen Aktionen darzustellen. Die Farbe Rot weist auf Fehler in den betreffenden Aktionen hin. Korrelieren Sie diese Ansicht mit dem **Aktivitätenprotokoll**, um weitere Details zu Fehlern zu ermitteln.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### Aktivitätenprotokoll
{: #log}

Die Ansicht **Aktivitätenprotokoll** zeigt eine formatierte Version des Aktivierungsprotokolls an. Die Ansicht zeigt die Details zu jeder Aktivierung an, prüft jedoch einmal pro Minute durch Polling auf neue Aktivierungen. Klicken Sie auf eine Aktion, um ein detailliertes Protokoll anzuzeigen.

Wenn Sie die Ausgabe, die im Aktivitätenprotokoll angezeigt wird, über die Befehlszeilenschnittstelle (CLI) abrufen wollen, verwenden Sie den folgenden Befehl:
```
ibmcloud fn activation poll
```
{: pre}
