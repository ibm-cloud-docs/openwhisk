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


# Foire aux questions
{: #faq}

Cette FAQ fournit les réponses aux questions courantes sur le service {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## Quels langages d'exécution sont pris en charge ?
{: #supported-runtimes}

Les langages suivants sont pris en charge :

<table>
  <tr>
    <th>Langage</th>
    <th>Identificateur de type</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (valeur par défaut), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>D'autres langages sont pris en charge en utilisant des actions Docker.</td>
  </tr>
</table>
{: caption="Tableau 1. Environnements d'exécution pris en charge" caption-side="top"}


## Quelle est la durée maximale d'exécution de ma fonction ?
{: #max-runtime}

La durée maximale est 10 minutes. La valeur par défaut est définie sur 1 minute, mais peut être modifiée via l'interface de ligne de commande en spécifiant une nouvelle
valeur en millisecondes à l'aide de l'indicateur `--timeout`. Vous pouvez également modifier la valeur via l'interface graphique dans la section relative aux détails de l'action.


## Quelle est la quantité maximale de mémoire que ma fonction peut utiliser ?
{: #max-memory}

Vous pouvez utiliser jusqu'à 2048 Mo de mémoire pour chaque fonction. La valeur par défaut est définie sur 256 Mo, mais vous pouvez la modifier à l'aide de
l'indicateur `--memory` ou via l'interface graphique dans la section relative aux détails de l'action.



## Quelle est la différence entre une action et une action Web ?
{: #difference}

La principale différence entre une action et une action Web est l'objet de sortie de réponse. Pour les [actions Web](/docs/openwhisk?topic=cloud-functions-actions_web), le résultat représente une réponse HTTP,
où la sortie JSON doit avoir au moins une zone `body`. Elle peut éventuellement contenir également un code d'état et des en-têtes.

## Comment puis-je afficher mes journaux d'actions ?
{: #logs_faq}

Après la collecte des métriques, vous pouvez afficher vos journaux à l'aide du [service {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-logs).


## Comment fonctionne la surveillance ?
{: #monitor_faq}

Vous pouvez obtenir des informations sur les performances de vos actions déployées avec {{site.data.keyword.openwhisk_short}} à l'aide d'{{site.data.keyword.monitoringlong}}. Vous pouvez également surveiller l'intégrité et les performances de vos actions en utilisant le tableau de bord pour afficher un récapitulatif graphique de votre activité.




