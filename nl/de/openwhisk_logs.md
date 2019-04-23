---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Aktivität protokollieren und überwachen
{: #openwhisk_logs}

Als Unterstützung bei der Behebung von Problemen und zur Verbesserung des Allgemeinzustands sowie der Leistung Ihrer Aktionen ist in {{site.data.keyword.openwhisk}} die Protokollierung und Überwachung automatisch aktiviert. Sie können auch den {{site.data.keyword.cloudaccesstraillong}}-Service verwenden, um nachzuverfolgen, wie Benutzer und Anwendungen mit dem {{site.data.keyword.openwhisk_short}}-Service interagieren. 

## Protokolle anzeigen
{: #view-logs}

Sie können Aktivierungsprotokolle direkt über das Dashboard für {{site.data.keyword.openwhisk_short}}-Überwachung anzeigen. Die Protokolle werden auch an [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) weitergeleitet, wo sie indexiert werden, wodurch eine Volltextsuche für alle generierten Nachrichten und eine komfortable Abfrage auf der Basis bestimmter Felder möglich wird.
{:shortdesc}

**Hinweis**: Protokollierung ist für die Region 'US East' (USA (Osten)) nicht verfügbar. 

1. Öffnen Sie die Seite [{{site.data.keyword.openwhisk_short}}-Überwachung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/dashboard).

2. Optional: Wenn Sie nur für eine bestimmte Aktion Protokolle anzeigen wollen, beschränken Sie die Überwachungszusammenfassung auf die betreffende Aktion. Wählen Sie im Abschnitt 'Filteroptionen' in der Dropdown-Liste **Begrenzen auf** den Namen der entsprechenden Aktion aus.

3. Klicken Sie im Navigationsbereich links auf **Protokolle**. Die {{site.data.keyword.loganalysisshort_notm}}-Seite für Kibana wird geöffnet.

4. Optional: Damit ältere Protokolle angezeigt werden, ändern Sie den Standardwert für den Zeitrahmen in 15 Minuten. Klicken Sie hierzu in der rechten oberen Ecke auf **Letzte 15 Minuten** und wählen Sie einen anderen Zeitrahmen aus.

### Protokolle abfragen
{: #query-logs}

Wenn Sie in [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) bestimmte Aktivierungsprotokolle suchen wollen, verwenden Sie hierfür die Abfragesyntax von Kibana.

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

Zusätzlich zu Protokollzeilen indexiert [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) auch die von {{site.data.keyword.openwhisk_short}} generierten Ergebnisse bzw. Aktivierungsdatensätze. Die Ergebnisse enthalten Aktivierungsmetadaten wie etwa die Aktivierungsdauer oder den Aktivierungsergebniscode. Das Abfragen von Ergebnisfeldern kann Ihnen dabei helfen, das Verhalten Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu verstehen.

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


## Leistung von Aktionen überwachen
{: #monitoring_performance}

Erhalten Sie Einblick in die Leistung Ihrer Aktionen, die mit {{site.data.keyword.openwhisk}} bereitgestellt werden. Metriken können Ihnen dabei helfen, Engpässe ausfindig zu machen oder mögliche Produktionsprobleme vorherzusagen, basierend auf der Aktionsdauer, den Ergebnissen von Aktionsaktivierungen oder wenn Sie auf Aktionsaktivierungsgrenzwerte gestoßen sind.
{: shortdesc}

Metriken werden automatisch für alle Entitäten erfasst. Je nachdem, ob Ihre Aktionen in einem IAM-basierten oder einem Cloud-Foundry-basierten Namensbereich enthalten sind, befinden sich Metriken im IBM Cloud-Account oder -Bereich. Diese Metriken werden an {{site.data.keyword.monitoringlong}} gesendet und über Grafana verfügbar gemacht, wo Sie Ihre Dashboards konfigurieren können, Alerts basierend auf den Metrikereigniswerten erstellen können usw. Weitere Informationen zu Metriken finden Sie in der [{{site.data.keyword.monitoringlong_notm}}-Dokumentation](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started-with-ibm-cloud-monitoring).

### Dashboard erstellen
{: #create_dashboard}

Beginnen Sie, indem Sie ein Grafana-Dashboard zur Überwachung erstellen. 

1. Rufen Sie eine der folgenden URLs auf. 
  <table>
    <thead>
      <tr>
        <th>{{site.data.keyword.openwhisk_short}}-Region</th>
        <th>Überwachungsadresse</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Mitteleuropa</td>
        <td>metrics.eu-de.bluemix.net</td>
      </tr>
      <tr>
        <td>Großbritannien (Süden)</td>
        <td>metrics.eu-gb.bluemix.net</td>
      </tr>
      <tr>
        <td>USA (Süden)</td>
        <td>metrics.ng.bluemix.net</td>
      </tr>
      <tr>
        <td>USA (Osten)</td>
        <td>Nicht verfügbar</td>
      </tr>
    </tbody>
  </table>

2. Wählen Sie die Metrikdomäne aus. 
    * IAM-basierte Namensbereiche: 
        1. Klicken Sie auf Ihren Benutzernamen. 
        2. Wählen Sie in der Dropdown-Liste **Domäne** den Eintrag **Konto**. 
        3. Wählen Sie in der Dropdown-Liste **Konto** das IBM Cloud-Konto aus, in dem sich Ihr IAM-basierter Namensbereich befindet. 
    * Cloud Foundry-basierte Namensbereiche: 
        1. Klicken Sie auf Ihren Benutzernamen. 
        2. Wählen Sie in der Dropdown-Liste **Domäne** den Eintrag **Bereich** aus. 
        3. Verwenden Sie die Dropdown-Listen **Organisation** und **Bereich**, um Ihren Cloud Foundry-basierten Namensbereich auszuwählen. 

3. Erstellen Sie ein Dashboard. 
    * So verwenden Sie das vorgefertigte {{site.data.keyword.openwhisk_short}}-Dashboard: 
        1. Navigieren Sie zu **Start > Importieren**. 
        3. Geben Sie die ID für das vorgefertigte {{site.data.keyword.openwhisk_short}}-Dashboard, `8124`, in das Feld **Grafana.net-Dashboard** ein. 
        4. Klicken Sie auf **Importieren**. 
    * Navigieren Sie zum Erstellen eines angepassten Dashboards zu **Start > Neu erstellen**. 

Nachdem eine Aktion ausgeführt wurde, werden neue Metriken generiert und können in Grafana gesucht werden. Hinweis: Es kann bis zu 10 Minuten dauern, bis die ausgeführte Aktion in Grafana angezeigt wird. 


### Metrikformat
{: #metric_format}

Die Metriken spiegeln Daten wider, die von Ihren Aktionsaktivierungen erfasst wurden und minütlich zusammengefasst werden. Metriken können auf der Ebene der Leistung oder Nebenläufigkeit von Aktionen durchsucht werden. 


**Metriken für Aktionsleistung**

Metriken für Aktionsleistung sind Werte, die für eine einzelne Aktion berechnet werden. Metriken für Aktionsleistung umfassen sowohl die Taktmerkmale der Ausführungen als auch den Status der Aktivierungen. Hinweis: Wenn Sie während der Erstellung keinen Namen für ein Paket angeben, wird der Standardpaketname verwendet. Diese Metriken haben das folgende Format: 

```
ibmcloud.public.functions.<region>.action.namespace.<namensbereich>.<paket>.<aktion>.<metrikname>
```
{: codeblock}

Die folgenden Zeichen werden in Gedankenstriche (`-`) konvertiert: Punkt (.), kommerzielles A (@), Leerzeichen ( ), Et-Zeichen (&), Unterstrich (_), Doppelpunkt (:).
{: tip}

Beispiel: Wenn Sie eine Aktion namens `hello-world` im Cloud Foundry-basierten Namensbereich `user@email.com_dev` in der Region `us-south` haben, sähe eine Aktionsleistungsmetrik wie folgt aus: 

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

**Metriken für Aktionsnebenläufigkeit**

Metriken für Aktionsnebenläufigkeit werden basieren auf den Daten aus allen aktiven Aktionen in einem Namensbereich berechnet. Die Nebenläufigkeit von Aktionen umfasst die Anzahl von gleichzeitigen Aufrufen und die Systemengpässe, die potenziell auftreten können, wenn die Grenzwerte für Nebenläufigkeit überschritten werden. Diese Metriken haben das folgende Format: 

```
ibmcloud.public.functions.<region>.action.namespace.<namensbereich>.action-performance.<paket>.<action>.<metrikname>
```
{: codeblock}

Beispiel: Wenn Sie einen IAM-basierten Namensbereich namens `myNamespace` in der Region `us-south` haben, sähe eine Metrik für Aktionsnebenläufigkeit wie folgt aus: 

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

**Verfügbare Metriken**

Da es möglicherweise Tausende oder Millionen von Aktionsaktivierungen gibt, werden die Metrikwerte als Aggregation von Ereignissen dargestellt, die von vielen Aktivierungen erzeugt wurden. Die Werte werden auf die folgenden Arten aggregiert: 
* Summe: Alle Metrikwerte werden addiert.
* Durchschnitt: Ein arithmetisches Mittel wird berechnet.
* Summierter Durchschnitt: Ein arithmetisches Mittel wird auf der Basis von Komponenten und dem Summieren verschiedener Komponenten berechnet. 

Prüfen Sie die folgende Tabelle, um die verfügbaren Metriken anzuzeigen. 

<table>
  <thead>
    <tr>
      <th>Metrikname</th>
      <th>Beschreibung</th>
      <th>Typ</th>
      <th>Kategorie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>Die durchschnittliche Aktionsdauer, in Rechnung gestellte Aktionsausführungszeit. </td>
      <td>Durchschnitt</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>Die Zeit zum Initialisieren des Aktionscontainers. </td>
      <td>Durchschnitt</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>Die durchschnittliche Zeit in einer Warteschlange bis zur Planung einer Aktivierung. </td>
      <td>Durchschnitt</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>Die Gesamtzahl von Aktivierungen, die im System ausgelöst wurden. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>Die Anzahl von erfolgreichen Aktivierungen des Aktionscodes. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die durch Anwendungsfehler verursacht wurden. Beispielsweise Teilausfälle von Aktionen. Weitere Informationen zu den 'action-performance'-Metriken finden Sie unter [Informationen zum Aktivierungsdatensatz](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record). </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die vom Entwickler verursacht wurden. Beispielsweise die Verletzung der [Aktions-Proxy-Schnittstelle](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface) durch nicht behandelte Ausnahmen im Aktionscode. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die von internen {{site.data.keyword.openwhisk_short}}-Fehlern verursacht wurden. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>Die Summe von Aktivierungen, die von einem Engpass aufgrund einer Überschreitung der Nebenläufigkeitsgrenzwerts betroffen waren. Es wird keine Metrik ausgegeben, wenn der Grenzwert nicht erreicht wird. </td>
      <td>Summe</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>Die Summe von Aktivierungen, die von einem Engpass aufgrund einer Überschreitung des Minutengrenzwerts betroffen waren. Es wird keine Metrik ausgegeben, wenn der Grenzwert nicht erreicht wird. </td>
      <td>Summe</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>Die Anzahl von gleichzeitigen Aufrufen im System. </td>
      <td>Summierter Durchschnitt</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

Metriken für Aktionen, die als Teil eines Standardnamensbereichs vorhanden sind, sind in der Standardkategorie verfügbar.
{: tip}



## Status von Aktionen überwachen
{: #openwhisk_monitoring}

Das [{{site.data.keyword.openwhisk_short}}-Dashboard](https://cloud.ibm.com/openwhisk/dashboard) stellt eine grafische Zusammenfassung Ihrer Aktivität bereit. Verwenden Sie das Dashboard, um die Leistung und den Status Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu ermitteln.
{:shortdesc}

Sie können Protokolle filtern, indem Sie zunächst auswählen, welche Aktionsprotokolle angezeigt werden sollen, und dann den Zeitrahmen der protokollierten Aktivität auswählen. Diese Filter werden auf alle Ansichten im Dashboard angewendet. Sie können jederzeit auf die Option **Erneut laden** klicken, um das Dashboard mit den neuesten Aktivierungsprotokolldaten zu aktualisieren.

### Aktivitätenzusammenfassung
{: #activity_summary}

Die Ansicht **Zusammenfassung der Aktivitäten** stellt eine allgemeine Zusammenfassung Ihrer {{site.data.keyword.openwhisk_short}}-Umgebung bereit. Verwenden Sie diese Ansicht zur Überwachung des Allgemeinzustands und der Leistung Ihres für {{site.data.keyword.openwhisk_short}} eingerichteten Service. Die Metriken in dieser Ansicht bieten Ihnen die folgenden Möglichkeiten:
* Sie können die Nutzungsrate der für {{site.data.keyword.openwhisk_short}} eingerichteten Aktionen Ihres Service ermitteln, indem Sie die Häufigkeit anzeigen, mit der sie aufgerufen wurden.
* Sie können die Gesamtrate von Fehlern über alle Aktionen hinweg ermitteln. Wenn Sie auf einen Fehler stoßen, können Sie die Services oder Aktionen eingrenzen, bei denen Fehler aufgetreten sind, indem Sie die Ansicht **Aktivitätenhistogramm** anzeigen. Sie können die Fehler selbst eingrenzen, indem Sie das **Aktivitätenprotokoll** anzeigen.
* Sie können die Leistung Ihrer Aktionen ermitteln, indem Sie die durchschnittliche Ausführungszeit anzeigen, die mit jeder Aktion verbunden ist.

### Aktivitätenzeitachse
{: #timeline}

Die Ansicht **Aktivitätenzeitachse** zeigt ein vertikales Balkendiagramm an, um die Aktivitäten der vergangenen und gegenwärtigen Aktionen darzustellen. Die Farbe Rot weist auf Fehler in den betreffenden Aktionen hin. Korrelieren Sie diese Ansicht mit dem **Aktivitätenprotokoll**, um weitere Details zu Fehlern zu ermitteln.



### Aktivitätenprotokoll
{: #log}

Die Ansicht **Aktivitätenprotokoll** zeigt eine formatierte Version des Aktivierungsprotokolls an. Die Ansicht zeigt die Details zu jeder Aktivierung an, prüft jedoch einmal pro Minute durch Polling auf neue Aktivierungen. Klicken Sie auf eine Aktion, um ein detailliertes Protokoll anzuzeigen.

Wenn Sie die Ausgabe, die im Aktivitätenprotokoll angezeigt wird, über die Befehlszeilenschnittstelle (CLI) abrufen wollen, verwenden Sie den folgenden Befehl:
```
ibmcloud fn activation poll
```
{: pre}
