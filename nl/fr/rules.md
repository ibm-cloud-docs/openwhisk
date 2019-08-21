---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# Association de déclencheurs et d'actions au moyen de règles
{: #rules}

A chaque fois que le déclencheur s'exécute, la règle utilise l'événement déclencheur comme entrée et appelle l'action associée. Avec le jeu de règles approprié, un déclencheur unique peut appeler plusieurs actions, ou une action peut être appelée en réponse à des événements provenant de plusieurs déclencheurs.
{: shortdesc}


## Création de règle depuis l'interface utilisateur
{: #rules_ui}

Dans l'interface utilisateur, une règle permettant d'associer une action et un déclencheur est créée pour vous.
{: shortdesc}

Lorsque vous créez ou accédez aux détails correspondant à une action ou un déclencheur, vous avez la possibilité de connecter une action ou un déclencheur, qu'ils soient nouveaux ou existants. Lorsque vous établissez la connexion, une règle est créée pour vous avec un nom au format `ACTION_NAME-TRIGGER_NAME`.

A partir de l'interface de commande, vous pouvez exécuter la commande `ibmcloud fn rule list` pour vérifier que la règle a été créée pour vous.


## Création de règle depuis l'interface de ligne de commande
{: #rules_create}

Des règles sont utilisées pour associer un déclencheur à une action. A chaque fois qu'un événement déclencheur est exécuté, l'action est appelée avec les paramètres de l'événement déclencheur.

Avant de commencer, créez [une action](/docs/openwhisk?topic=cloud-functions-actions) et [un déclencheur](/docs/openwhisk?topic=cloud-functions-triggers).


Créez une règle pour associer un déclencheur à une action. Les règles doivent être créées directement dans un espace de nom et elles ne peuvent pas être créées dans des packages.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


Pour désactiver la règle, vous pouvez exécuter la commande suivante.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## Création de règles pour des séquences d'actions
{: #rules_seq}

Vous pouvez utiliser des règles pour associer des déclencheurs à des séquences d'actions.

Avant de commencer, créez [une séquence d'actions](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) et [un déclencheur](/docs/openwhisk?topic=cloud-functions-triggers).

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## Association de plusieurs déclencheurs et actions
{: #rules_assoc}

Vous pouvez utiliser différentes combinaisons de déclencheurs et d'actions en créant une règle pour chaque combinaison. Vous n'avez pas besoin d'avoir un rapport de un à un pour les actions et les déclencheurs.

Par exemple, envisagez les actions suivantes.

| Action | Description |
| --- | --- |
| `classifyImage` | Action qui détecte les objets dans une image et les classe. |
| `thumbnailImage` | Action qui crée une version miniature d'une image. |

Supposons également que deux sources d'événements exécutent les déclencheurs suivants.

| Déclencheur | Description |
| --- | --- |
| `newTweet` | Déclencheur qui est exécuté lorsqu'un nouveau tweet est publié. |
| `imageUpload` | Déclencheur qui est exécuté lorsqu'une image est téléchargée sur un site Web. |

Vous pouvez configurer des règles pour qu'un événement déclencheur unique appelle plusieurs actions, et pour que plusieurs déclencheurs appellent la même action .
- La règle `newTweet -> classifyImage`
- La règle `imageUpload -> classifyImage`
- La règle `imageUpload -> thumbnailImage`

Ces trois règles induisent le comportement suivant.
- Les images contenues dans chaque tweet sont classifiées
- Les images téléchargées sont classifiées
- Une version miniature est générée

