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

# Affichage des journaux d'activation dans IBM Cloud
{: #openwhisk_logs}

Les journaux d'activation peuvent être consultés directement sur la [page Surveiller de {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Les journaux sont également transférés à [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) où ils sont indexés, ce qui permet d'effectuer des recherches de texte intégral dans tous les messages générés et de lancer des interrogations opportunes en fonction de zones spécifiques (par exemple au niveau des journaux).
{:shortdesc}

## Interrogation des journaux
{: #query-logs}

Lorsque vous utilisez Kibana hébergé sur [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana), vous pouvez interroger vos journaux directement. Utilisez la syntaxe de requête de Kibana pour obtenir les journaux que vous recherchez.

L'interface utilisateur de {{site.data.keyword.openwhisk_short}} vous permet d'accéder directement aux journaux et aux résultats de vos actions dans Kibana. Le lien **Journaux** se trouve dans la barre de navigation située sur la gauche sur la [page Surveiller de {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Lorsque vous accédez à la page des détails d'une action spécifique, le lien **Journaux** vous dirige vers les résultats (enregistrements d'activation) de cette action précise. La valeur par défaut du délai d'affichage des journaux est fixée à 15 minutes. Vous pouvez modifier cette valeur directement dans Kibana en haut à droite si vous désirez afficher des enregistrements plus anciens.

Voici deux exemples de requêtes utiles pour le débogage.

### Recherche de tous les journaux d'erreurs :
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Recherche de tous les journaux d'erreurs générés par l'action nommée "myAction" :
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Interrogation des résultats
{: #query-results}

En plus des lignes de journaux, [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) indexe également les résultats (enregistrements d'activation) générés par {{site.data.keyword.openwhisk_short}}. Ces résultats contiennent des métadonnées précieuses pour les activations, par exemple leur durée ou leur code de résultat (réussite ou erreur). Toutes les zones sont interrogeables, ce qui peut vous aider à comprendre le comportement de vos actions {{site.data.keyword.openwhisk_short}}.

Utilisez la syntaxe de requête de Kibana pour obtenir les activations que vous recherchez. Voici deux exemples de requêtes utiles pour le débogage.

### Recherche de toutes les activations ayant échoué :
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Comme pour les commandes Unix, la valeur "`0`" indique une action qui s'est terminée avec succès alors que toute autre valeur est considérée comme une erreur.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Recherche de toutes les activations ayant échoué avec une erreur spécifique :
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
