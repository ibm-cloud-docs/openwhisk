---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions

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


# Protokolle anzeigen
{: #logs}

Als Unterstützung bei der Behebung von Problemen ist die Protokollierung in {{site.data.keyword.openwhisk}} automatisch aktiviert. Sie können auch den {{site.data.keyword.cloudaccesstraillong}}-Service verwenden, um nachzuverfolgen, wie Benutzer und Anwendungen mit dem {{site.data.keyword.openwhisk_short}}-Service interagieren.


## Aktionsprotokolle bei ihrem Auftreten anzeigen
{: #logs_poll}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. Um Informationen darüber zu erhalten, wann Aktionen aufgerufen wurden und was die Ausgabe war, kann es nützlich sein, die Aktionsprotokolle zu überwachen.

Sie können die Ausgabe von Aktionen, wenn sie aufgerufen werden, über die {{site.data.keyword.openwhisk_short}}-CLI beobachten.

1. Starten Sie eine Sendeaufrufschleife, die ununterbrochen prüft, ob Protokolle von Aktivierungen vorhanden sind.

    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Wechseln Sie zu einem anderen Fenster und rufen Sie eine Aktion auf.

    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Im Polling-Fenster wird das Aktivierungsprotokoll angezeigt.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    Gegebenenfalls werden auch die Protokolle für Aktionen, die für Sie in {{site.data.keyword.openwhisk_short}} ausgeführt werden, in Echtzeit angezeigt.




## Aktivierungsdetails anzeigen
{: #activation_details}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. Wird eine Aktion aufgerufen, wird ein Aktivierungsdatensatz für diesen Aufruf erstellt. Wenn Sie Informationen zum Ergebnis des Aktionsaufrufs wünschen, können Sie Details zu Aktivierungen abrufen.

Sie können alle Aktivierungsdatensatz-IDs in einem Namensbereich abrufen, indem Sie den folgenden Befehl ausführen. 
```
ibmcloud fn activation list
```
{: pre}

Sie können Details zu einem bestimmten Aktivierungsdatensatz für einen Aktionsaufruf abrufen, indem Sie den folgenden Befehl ausführen. Ersetzen Sie `<activation_ID>` durch die ID der Aktivierung.  
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

**Beispielausgabe**
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Informationen zu der Ausgabe des Befehls <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu der Ausgabe des Befehls <code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Der Namensbereich, in dem sich diese Aktivierung befindet. Dieser Namensbereich kann ein anderer sein als der, in dem die Aktion ausgeführt wird.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Der Name der Aktion.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>Die semantische Version der Aktion.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>Das Benutzerkonto, mit dem das Element aktiviert wurde.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID dieses Aktivierungsdatensatzes.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>Zeit, zu der die Aktivierung begann.</td>
</tr>
<tr>
<td><code>end</code></td>
<td>Zeit, zu der die Aktivierung abgeschlossen wurde.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Die Zeit (in Millisekunden), die benötigt wurde, um die Aktivierung vollständig auszuführen.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: Der Exitstatus der Aktivierung.</li>
<li><code>statusCode</code>: Der Statuscode. Wenn die Aktion zu einem Fehler geführt hat, ist dieser Wert der HTTP-Fehlercode. </li>
<li><code>success</code>: Das Ergebnis, ob die Aktion erfolgreich abgeschlossen wurde. </li>
<li><code>result</code>: Der Rückgabewert von der Aktivierung.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Protokolle für diese Aktivierung.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotationen für diese Aktion. Eine Liste der gültigen Annotationen enthalten die Referenzinformationen zu [Annotationen](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Das Ergebnis, ob die Aktion veröffentlicht wird. </td>
</tr>
</tbody></table>



## Protokolle in {{site.data.keyword.loganalysisfull_notm}} anzeigen
{: #logs_view}

{{site.data.keyword.loganalysislong_notm}}-Protokolle sind nicht für IAM-basierte Namensbereiche verfügbar.
{: note}

Sie können Aktivierungsprotokolle direkt über das Dashboard für {{site.data.keyword.openwhisk_short}}-Überwachung anzeigen. Die Protokolle werden auch an [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) weitergeleitet, wo sie indexiert werden, wodurch eine Volltextsuche für alle generierten Nachrichten und eine komfortable Abfrage auf der Basis bestimmter Felder möglich wird.
{:shortdesc}

Protokollierung ist für die Region 'US East' (USA (Osten)) nicht verfügbar.
{: important}

1. Öffnen Sie die [{{site.data.keyword.openwhisk_short}}-Seite 'Überwachung'](https://cloud.ibm.com/openwhisk/dashboard){: external}. 

2. Optional: Wenn Sie nur für eine bestimmte Aktion Protokolle anzeigen wollen, beschränken Sie die Überwachungszusammenfassung auf die betreffende Aktion. Wählen Sie im Abschnitt 'Filteroptionen' in der Dropdown-Liste **Begrenzen auf** den Namen der entsprechenden Aktion aus.

3. Klicken Sie im Navigationsbereich links auf **Protokolle**. Die {{site.data.keyword.loganalysisshort_notm}}-Seite für Kibana wird geöffnet.

4. Optional: Zum Anzeigen älterer Protokolle ändern Sie den Standardwert von 15 Minuten für den Zeitrahmen. Klicken Sie hierzu auf **Letzte 15 Minuten** und wählen Sie einen anderen Zeitrahmen aus. 

### Protokolle abfragen
{: #logs_query}

Wenn Sie in [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) bestimmte Aktivierungsprotokolle suchen wollen, verwenden Sie hierfür die Abfragesyntax von Kibana.

Die folgenden Beispielabfragen können Ihnen bei der Behebung von Fehlern helfen. 
  * Alle Fehlerprotokolle suchen.
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Alle Fehlerprotokolle suchen, die von `myAction` generiert wurden.
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Ergebnisse abfragen
{: #logs_query_results}

Zusätzlich zu Protokollzeilen indexiert [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) auch die von {{site.data.keyword.openwhisk_short}} generierten Ergebnisse bzw. Aktivierungsdatensätze. Die Ergebnisse enthalten Aktivierungsmetadaten wie etwa die Aktivierungsdauer oder den Aktivierungsergebniscode. Das Abfragen von Ergebnisfeldern kann Ihnen dabei helfen, das Verhalten Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu verstehen.

Sie können bestimmte Aktivierungsprotokolle suchen, indem Sie die Abfragesyntax von Kibana verwenden. Die folgenden Beispielabfragen können Ihnen bei der Behebung von Fehlern helfen.

* Alle fehlgeschlagenen Aktivierungen suchen.
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
In den Ergebnissen gibt `0` an, dass eine Aktion erfolgreich beendet wurde. Alle anderen Werte geben einen Fehler an. 

* Alle Aktivierungen suchen, die mit einem bestimmten Fehler fehlgeschlagen sind.
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}



