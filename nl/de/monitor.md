---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions

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


# Aktivität überwachen
{: #monitor}

Erhalten Sie Einblick in die Leistung Ihrer Aktionen, die mit {{site.data.keyword.openwhisk}} bereitgestellt werden. Metriken können Ihnen auf der Basis der Aktionsdauer, der Ergebnisse von Aktionsaktivierungen oder der Überschreitung von Aktionsaktivierungsgrenzwerten dabei helfen, Engpässe zu erkennen oder mögliche Produktionsprobleme vorherzusagen.
{: shortdesc}

Metriken werden automatisch für alle Entitäten erfasst. Je nachdem, ob Ihre Aktionen in einem IAM-basierten oder einem Cloud Foundry-basierten Namensbereich enthalten sind, befinden sich Metriken im IBM Cloud-Account oder -Bereich. Diese Metriken werden an {{site.data.keyword.monitoringlong}} gesendet und über Grafana verfügbar gemacht, wo Sie Ihre Dashboards konfigurieren können, Alerts basierend auf den Metrikereigniswerten erstellen können usw. Weitere Informationen zu Metriken finden Sie in der [{{site.data.keyword.monitoringlong_notm}}-Dokumentation](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started).

## Dashboard erstellen
{: #monitor_dash}

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
        <td>`metrics.eu-de.bluemix.net`</td>
      </tr>
      <tr>
        <td>Großbritannien (Süden)</td>
        <td>`metrics.eu-gb.bluemix.net`</td>
      </tr>
      <tr>
        <td>USA (Süden)</td>
        <td>`metrics.ng.bluemix.net`</td>
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




## Dashboards verwenden
{: #monitor_dash_use}

Das [{{site.data.keyword.openwhisk_short}}-Dashboard](https://cloud.ibm.com/openwhisk/dashboard){: external} stellt eine grafische Zusammenfassung Ihrer Aktivität bereit. Verwenden Sie das Dashboard, um die Leistung und den Status Ihrer {{site.data.keyword.openwhisk_short}}-Aktionen zu ermitteln.
{:shortdesc}

Sie können Protokolle filtern, indem Sie zunächst auswählen, welche Aktionsprotokolle angezeigt werden sollen, und dann den Zeitrahmen der protokollierten Aktivität auswählen. Diese Filter werden auf alle Ansichten im Dashboard angewendet. Sie können jederzeit auf die Option **Erneut laden** klicken, um das Dashboard mit den neuesten Aktivierungsprotokolldaten zu aktualisieren.

### Aktivitätenzusammenfassung
{: #monitor_dash_sum}

Die Ansicht **Zusammenfassung der Aktivitäten** stellt eine allgemeine Zusammenfassung Ihrer {{site.data.keyword.openwhisk_short}}-Umgebung bereit. Verwenden Sie diese Ansicht zur Überwachung des Allgemeinzustands und der Leistung Ihres für {{site.data.keyword.openwhisk_short}} eingerichteten Service. Die Metriken in dieser Ansicht bieten Ihnen die folgenden Möglichkeiten:
* Sie können die Nutzungsrate der für {{site.data.keyword.openwhisk_short}} eingerichteten Aktionen Ihres Service ermitteln, indem Sie die Häufigkeit anzeigen, mit der sie aufgerufen wurden.
* Sie können die Gesamtrate von Fehlern über alle Aktionen hinweg ermitteln. Wenn Sie auf einen Fehler stoßen, können Sie die Services oder Aktionen eingrenzen, bei denen Fehler aufgetreten sind, indem Sie die Ansicht **Aktivitätenhistogramm** anzeigen. Sie können die Fehler selbst eingrenzen, indem Sie das **Aktivitätenprotokoll** anzeigen.
* Sie können die Leistung Ihrer Aktionen ermitteln, indem Sie die durchschnittliche Ausführungszeit anzeigen, die mit jeder Aktion verbunden ist.

### Aktivitätenzeitachse
{: #monitor_dash_time}

Die Ansicht **Aktivitätenzeitachse** zeigt ein vertikales Balkendiagramm an, um die Aktivitäten der vergangenen und gegenwärtigen Aktionen darzustellen. Die Farbe Rot weist auf Fehler in den betreffenden Aktionen hin. Korrelieren Sie diese Ansicht mit dem **Aktivitätenprotokoll**, um weitere Details zu Fehlern zu ermitteln.



### Aktivitätenprotokoll
{: #monitor_dash_log}

Die Ansicht **Aktivitätenprotokoll** zeigt eine formatierte Version des Aktivierungsprotokolls an. Die Ansicht zeigt die Details zu jeder Aktivierung an, prüft jedoch einmal pro Minute durch Polling auf neue Aktivierungen. Klicken Sie auf eine Aktion, um ein detailliertes Protokoll anzuzeigen.

Wenn Sie die Ausgabe, die im Aktivitätenprotokoll angezeigt wird, über die Befehlszeilenschnittstelle (CLI) abrufen wollen, verwenden Sie den folgenden Befehl:
```
ibmcloud fn activation poll
```
{: pre}




## Metrikformat
{: #monitor_metric}

Die Metriken spiegeln Daten wider, die von Ihren Aktionsaktivierungen erfasst und minütlich zusammengefasst werden. Metriken können auf der Ebene der Leistung oder Nebenläufigkeit von Aktionen durchsucht werden.


### Metriken für Aktionsleistung
{: #monitor_metric_perf}

Metriken für Aktionsleistung sind Werte, die für eine einzelne Aktion berechnet werden. Metriken für Aktionsleistung umfassen sowohl die Taktmerkmale der Ausführungen als auch den Status der Aktivierungen. Hinweis: Wenn Sie während der Erstellung keinen Namen für ein Paket angeben, wird der Standardpaketname verwendet. Diese Metriken haben das folgende Format:

```
ibmcloud.public.functions.<region>.action.namespace.<namensbereich>.<paket>.<aktion>.<metrikname>
```
{: codeblock}

Die folgenden Zeichen werden in Gedankenstriche (`-`) konvertiert: Punkt (.), kommerzielles A (@), Leerzeichen ( ), Et-Zeichen (&), Unterstrich (_), Doppelpunkt (:).
{: tip}

Beispiel: Für eine Aktion mit dem Namen `hello-world` in dem Cloud Foundry-basierten Namensbereich `user@email.com_dev` in der Region `us-south` würde eine Metrik für die Aktionsleistung etwa wie folgt aussehen: 

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### Metriken für Aktionsnebenläufigkeit
{: #monitor_metric_con}

Metriken für Aktionsnebenläufigkeit werden basieren auf den Daten aus allen aktiven Aktionen in einem Namensbereich berechnet. Die Nebenläufigkeit von Aktionen umfasst die Anzahl von gleichzeitigen Aufrufen und die Systemengpässe, die potenziell auftreten können, wenn die Grenzwerte für Nebenläufigkeit überschritten werden. Diese Metriken haben das folgende Format:

```
ibmcloud.public.functions.<region>.action.namespace.<namensbereich>.action-performance.<paket>.<action>.<metrikname>
```
{: codeblock}

Beispiel: Für einen IAM-basierten Namensbereich mit dem Namen `myNamespace` in der Region `us-south` würde eine Metrik für die Aktionsnebenläufigkeit etwa wie folgt aussehen: 

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### Verfügbare Metriken
{: #monitor_metric_av}

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
      <td>Die durchschnittliche Aktionsdauer, in Rechnung gestellte Aktionsausführungszeit.</td>
      <td>Durchschnitt</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>Die Zeit für das Initialisieren des Aktionscontainers. </td>
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
      <td>Die Gesamtzahl von Aktivierungen, die im System ausgelöst wurden.</td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>Die Anzahl von erfolgreichen Aktivierungen des Aktionscodes.</td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die durch Anwendungsfehler verursacht wurden. Dazu gehören nicht kritische Fehler von Aktionen. Weitere Informationen zur Ableitung der Metriken für die Aktionsleistung finden Sie in [Informationen zum Aktivierungsdatensatz](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record){: external}. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die vom Entwickler verursacht wurden. Dazu gehört die Verletzung der [Proxy-Schnittstelle für die Aktion](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface){: external} durch nicht behandelte Ausnahmebedingungen im Aktionscode. </td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>Die Anzahl von nicht erfolgreichen Aktivierungen, die von internen {{site.data.keyword.openwhisk_short}}-Fehlern verursacht wurden.</td>
      <td>Summe</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>Die Summe von Aktivierungen, die von einem Engpass aufgrund einer Überschreitung der Nebenläufigkeitsgrenzwerts betroffen waren. Es wird keine Metrik ausgegeben, wenn der Grenzwert nicht erreicht wird.</td>
      <td>Summe</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>Die Summe von Aktivierungen, die von einem Engpass aufgrund einer Überschreitung des Minutengrenzwerts betroffen waren. Es wird keine Metrik ausgegeben, wenn der Grenzwert nicht erreicht wird.</td>
      <td>Summe</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>Die Anzahl von gleichzeitigen Aufrufen im System.</td>
      <td>Summierter Durchschnitt</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

Metriken für Aktionen, die als Teil eines Standardnamensbereichs vorhanden sind, sind in der Standardkategorie verfügbar.
{: tip}




