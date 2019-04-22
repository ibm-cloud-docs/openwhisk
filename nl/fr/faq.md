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


# Foire aux questions
{: #faq}

Cette FAQ fournit les réponses aux questions courantes sur le service {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## Quelles langues sont prises en charge pour les environnements d'exécution ?
{: #runtimes}
{: faq}

Les langues suivantes sont prises en charge : 

<table>
  <tr>
    <th id="language-col">Langue</th>
    <th id="kind-identifier-col">Identificateur de type</th>
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
    <td headers="language-col" colspan="2">D'autres langues sont prises en charge à l'aide d'actions Docker. </td>
  </tr>
</table>
{: caption="Tableau 1. Environnements d'exécution pris en charge" caption-side="top"}


## Quelle est la durée maximale d'exécution de ma fonction ?
{: #max-runtime}
{: faq}

La durée maximale est 10 minutes. La valeur par défaut est définie sur 1 minute, mais peut être modifiée via l'interface de ligne de commande en spécifiant une nouvelle
valeur en millisecondes à l'aide de l'indicateur `--timeout`. Vous pouvez également modifier la valeur via l'interface graphique dans la section relative aux détails de l'action. 


## Quelle est la quantité maximale de mémoire que ma fonction peut utiliser ?
{: #max-memory}
{: faq}

Vous pouvez utiliser jusqu'à 2048 Mo de mémoire pour chaque fonction. La valeur par défaut est définie sur 256 Mo, mais vous pouvez la modifier à l'aide de
l'indicateur `--memory` ou via l'interface graphique dans la section relative aux détails de l'action. 


## Quelle est la différence entre une action et une action Web ? 
{: #difference}
{: faq}

La principale différence entre une action et une action Web est l'objet de sortie de réponse. Pour les [actions Web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions), le résultat représente une réponse HTTP,
où la sortie JSON doit avoir au moins une zone `body`. Elle peut éventuellement contenir également un code d'état et des en-têtes. 

## Comment puis-je afficher mes journaux des actions ? 
{: #logs}
{: faq}

Après la collecte des métriques, vous pouvez afficher vos journaux à l'aide du service [{{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs). 


## Comment fonctionne la surveillance ? 
{: #monitor}
{: faq}

Vous pouvez obtenir des informations sur les performances de vos actions déployées avec {{site.data.keyword.openwhisk_short}} à l'aide d'{{site.data.keyword.monitoringlong}}. Vous pouvez également surveiller l'intégrité et les performances de vos actions en utilisant le tableau de bord pour afficher un récapitulatif graphique de votre activité. 


