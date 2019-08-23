---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: faq, runtimes, actions, memory, monitoring, functions

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
{:faq: data-hd-content-type='faq'}



# Häufig gestellte Fragen
{: #faq}

Diese häufig gestellten Fragen geben Antworten auf allgemeine Fragen zum {{site.data.keyword.openwhisk_short}}-Service.
{: shortdesc}


## Welche Sprachlaufzeiten werden unterstützt?
{: #supported-runtimes}

Die folgenden Sprachen werden unterstützt:

<table>
  <tr>
    <th>Sprache</th>
    <th>ID des Typs</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (Standard), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (Standard)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (Standard)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (Standard)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (Standard)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Andere Sprachen werden durch die Verwendung von Docker-Aktionen unterstützt.</td>
  </tr>
</table>
{: caption="Tabelle 1. Unterstützte Laufzeiten" caption-side="top"}


## Wie lange kann meine Funktion maximal ausgeführt werden? 
{: #max-runtime}

Das maximale Zeitlimit beträgt 10 Minuten. Der Standardwert ist 1 Minute, kann aber über die Befehlszeilenschnittstelle geändert werden, indem ein neuer Wert in Millisekunden mithilfe des Flags `--timeout` angegeben wird. Sie können den Wert auch über die grafische Benutzerschnittstelle (GUI) im Abschnitt mit den Aktionsdetails ändern.

## Wie viel Speicher kann meine Funktion maximal belegen? 
{: #max-memory}

Sie können für jede Funktion bis zu 2048 MB Speicher verwenden. Der Standardwert ist 256 MB, aber Sie können ihn mithilfe des Flags `--memory` oder über die grafische Benutzerschnittstelle im Abschnitt mit den Aktionsdetails ändern. 

## Was ist der Unterschied zwischen einer Aktion und einer Webaktion? 
{: #difference}

Der Hauptunterschied zwischen einer Aktion und einer Webaktion ist das Antwortausgabeobjekt. Für [Webaktionen](/docs/openwhisk?topic=cloud-functions-actions_web) stellt das Ergebnis eine HTTP-Antwort dar, wobei die JSON-Ausgabe mindestens ein Feld `body` enthält. Optional kann sie auch einen Statuscode (`statusCode`) und Header (`headers`) enthalten. 

## Wie kann ich meine Aktionsprotokolle anzeigen?
{: #logs_faq}

Nachdem die Metriken gesammelt wurden, können Sie Ihre Protokolle mit dem [{{site.data.keyword.loganalysislong_notm}}-Service](/docs/openwhisk?topic=cloud-functions-logs) anzeigen.

## Wie funktioniert die Überwachung?
{: #monitor_faq}

Erhalten Sie mit {{site.data.keyword.monitoringlong}} Einblick in die Leistung Ihrer Aktionen, die mit {{site.data.keyword.openwhisk_short}} bereitgestellt werden. Sie können den Status und die Leistung Ihrer Aktionen auch mithilfe des Dashboards überwachen, um eine grafische Zusammenfassung Ihrer Aktivität anzuzeigen.










