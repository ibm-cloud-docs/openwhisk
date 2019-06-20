---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: faq, runtimes, actions, memory, monitoring

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faq}

Questa FAQ fornisce risposte a domande comuni sul servizio {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## Quali runtime di linguaggi sono supportati?
{: #supported-runtimes}

Sono supportati i seguenti linguaggi:

<table>
  <tr>
    <th>Linguaggio</th>
    <th>Identificativo del tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (predefinito), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (predefinito)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Sono supportati altri linguaggi utilizzando le azioni Docker.</td>
  </tr>
</table>
{: caption="Tabella 1. Runtime supportati" caption-side="top"}


## Qual è il tempo massimo di esecuzione della mia funzione?
{: #max-runtime}

Il timeout massimo è 10 minuti. Il valore predefinito è impostato su 1 minuto, ma può essere modificato tramite la CLI specificando un nuovo valore in millisecondi utilizzando l'indicatore `--timeout`. Puoi anche modificare il valore tramite la GUI nella sezione dei dettagli dell'azione.


## Qual è la memoria massima che la mia funzione può utilizzare?
{: #max-memory}

Puoi utilizzare fino a 2048MB di memoria per ogni funzione. Il valore predefinito è impostato su 256MB, ma puoi modificarlo utilizzando l'indicatore `--memory` o tramite la GUI nella sezione dei dettagli dell'azione.



## Qual è la differenza tra un'azione e un'azione web?
{: #difference}

La differenza principale tra un'azione e un'azione web è l'oggetto di output della risposta. Per le [azioni web](/docs/openwhisk?topic=cloud-functions-actions_web), il risultato rappresenta una risposta HTTP, in cui come minimo, l'output JSON dovrebbe avere un campo `body`. Facoltativamente, può contenere uno statusCode e delle intestazioni.

## Come posso visualizzare i miei log di azione?
{: #logs_faq}

Dopo aver raccolto le metriche, puoi visualizzare i tuoi log utilizzando il [servizio {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-logs).


## Come funziona il monitoraggio?
{: #monitor_faq}

Puoi ottenere delle informazioni approfondite sulle prestazioni delle tue azioni che vengono distribuite con {{site.data.keyword.openwhisk_short}} utilizzando {{site.data.keyword.monitoringlong}}. Puoi anche monitorare l'integrità e le prestazioni delle tue azioni utilizzando il dashboard per visualizzare un riepilogo grafico della tua attività.




