---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-12"

keywords: faq, runtimes, actions, memory, monitoring

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:faq: data-hd-content-type='faq'}


# Häufig gestellte Fragen
{: #faq}

Diese häufig gestellten Fragen geben Antworten auf allgemeine Fragen zum {{site.data.keyword.openwhisk_short}}-Service.
{: shortdesc}


## Welche Sprachlaufzeiten werden unterstützt?
{: #runtimes}
{: faq}

Die folgenden Sprachen werden unterstützt:

<table>
  <tr>
    <th id="language-col">Sprache</th>
    <th id="kind-identifier-col">ID des Typs</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code>, <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code>, <code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift:4.1</code>, <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code>, <code>php:7.1</code></td>
  </tr>
  <tr>
    <td id="language-col-ruby" headers="language-col">Ruby</td>
    <td headers="kind-identifier-col language-col-ruby"><code>ruby:2.5</code></td>
  </tr>
  <tr>
    <td id="language-col-java" headers="language-col">Java</td>
    <td headers="kind-identifier-col language-col-java"><code>java (JDK 8)</code></td>
  </tr>
  <tr>
    <td headers="language-col" colspan="2">Andere Sprachen werden durch die Verwendung von Docker-Aktionen unterstützt.</td>
  </tr>
</table>
{: caption="Tabelle 1. Unterstützte Laufzeiten" caption-side="top"}


## Wie lange kann meine Funktion maximal ausgeführt werden?
{: #max-runtime}
{: faq}

Das maximale Zeitlimit beträgt 10 Minuten. Der Standardwert ist 1 Minute, kann aber über die Befehlszeilenschnittstelle geändert werden, indem ein neuer Wert in Millisekunden mithilfe des Flags `-- timeout` angegeben wird. Sie können den Wert auch über die grafische Benutzerschnittstelle (GUI) im Abschnitt mit den Aktionsdetails ändern. 


## Wie viel Speicher kann meine Funktion maximal belegen? 
{: #max-memory}
{: faq}

Sie können bis zu 2048 MB Speicher für die einzelnen Funktionen verwenden. Der Standardwert ist 256 MB, aber Sie können ihn mithilfe des Flags `--memory` oder über die grafische Benutzerschnittstelle im Abschnitt mit den Aktionsdetails ändern. 


## Was ist der Unterschied zwischen einer Aktion und einer Webaktion?
{: #difference}
{: faq}

Der Hauptunterschied zwischen einer Aktion und einer Webaktion ist das Antwortausgabeobjekt. Für [Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions) stellt das Ergebnis eine HTTP-Antwort dar, wobei die JSON-Ausgabe mindestens ein Feld `body` aufweisen muss. Optional kann es auch einen Statuscode und Header enthalten. 

## Wie kann ich meine Aktionsprotokolle anzeigen?
{: #logs}
{: faq}

Nachdem die Metriken gesammelt wurden, können Sie Ihre Protokolle mit dem [{{site.data.keyword.loganalysislong_notm}}-Service](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs) anzeigen. 


## Wie funktioniert die Überwachung?
{: #monitor}
{: faq}

Erhalten Sie mit {{site.data.keyword.monitoringlong}} Einblick in die Leistung Ihrer Aktionen, die mit {{site.data.keyword.openwhisk_short}} bereitgestellt werden. Sie können den Status und die Leistung Ihrer Aktionen auch mithilfe des Dashboards überwachen, um eine grafische Zusammenfassung Ihrer Aktivität anzuzeigen. 


