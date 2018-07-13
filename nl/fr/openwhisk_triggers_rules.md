---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Concepts généraux
{: #openwhisk_triggers}

Les déclencheurs et les règles {{site.data.keyword.openwhisk_short}} apportent des capacités gérées par des événements sur la plateforme. Les événements provenant de sources d'événements externes et internes sont canalisés via un déclencheur, et des règles autorisent vos actions à réagir à ces événements.
{: shortdesc}

## Qu'est-ce qu'un déclencheur ?
{: #openwhisk_triggers_create}

Les déclencheurs constituent un canal nommé pour une classe d'événements. Voici des exemples de déclencheur :
- Déclencheur d'événements de mise à jour d'emplacement
- Déclencheur de mises à jour de document sur un site Web
- Déclencheur de courriers électroniques entrants

Les déclencheurs peuvent être *exécutés* (activés) à l'aide d'un dictionnaire de paires clé-valeur. Parfois, ce dictionnaire est appelé *événement*. Comme pour les actions, chaque exécution de déclencheur génère un **ID d'activation**.

Les déclencheurs peuvent être exécutés explicitement par un utilisateur ou pour le compte d'un utilisateur par une source d'événements d'externe.
Un *flux* est pratique pour configurer une source d'événements externe afin d'exécuter des événements déclencheur pouvant être consommés par {{site.data.keyword.openwhisk_short}}. Voir l'exemple de flux suivant :
- Flux de modification de données {{site.data.keyword.cloudant}} qui exécute un déclencheur à chaque ajout ou modification d'un document dans une base de données.
- Flux Git qui exécute un déclencheur pour chaque validation dans un référentiel Git

## Quel est l'impact des règles sur les déclencheurs ?
{: #openwhisk_rules_use}

Une règle associe un déclencheur à une action, chaque exécution du déclencheur entraînant l'appel de l'action correspondante avec l'événement déclencheur en entrée.

Avec le jeu de règles approprié, un événement déclencheur unique peut appeler plusieurs actions, ou une action peut être appelée en réponse à des événements provenant de plusieurs déclencheurs.

Par exemple, imaginez un système avec les actions suivantes :
- `classifyImage` : action qui détecte les objets dans une image et les classe.
- `thumbnailImage` : action qui crée une version miniature d'une image.

De plus, supposez que deux sources d'événements exécutent les déclencheurs suivants :
- `newTweet` : déclencheur qui est exécuté lorsqu'un nouveau tweet est publié.
- `imageUpload` : déclencheur qui est exécuté lorsqu'une image est téléchargée sur un site Web.

Vous pouvez configurer des règles pour qu'un événement déclencheur unique appelle plusieurs actions, et pour que plusieurs déclencheurs appellent la même action :
- La règle `newTweet -> classifyImage`
- La règle `imageUpload -> classifyImage`
- La règle `imageUpload -> thumbnailImage`

Ces trois règles induisent le comportement suivant :
- Les images contenues dans chaque tweet sont classifiées
- Les images téléchargées sont classifiées
- Une version miniature est générée

## Création et exécution de déclencheurs
{: #openwhisk_triggers_fire}

Des déclencheurs peuvent être exécutés lorsque certains événements surviennent, ou manuellement.

Par exemple, créez un déclencheur pour envoyer les mises à jour de l'emplacement d'utilisateur, et exécutez manuellement le déclencheur.
1. Entrez la commande suivante pour créer le déclencheur :
  ```
  ibmcloud wsk trigger create locationUpdate
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. Vérifiez que le déclencheur a été créé en affichant la liste des déclencheurs.
  ```
  ibmcloud wsk trigger list
  ```
  {: pre}

  Exemple de sortie :
  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  Désormais, il existe un "canal" nommé dans lequel des événements peuvent être déclenchés.

3. Ensuite, exécutez un événement déclencheur en spécifiant le nom du déclencheur et des paramètres :
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

Un déclencheur qui est lancé sans aucune règle associée n'a aucun effet visible.
Les déclencheurs ne peuvent pas être créés au sein d'un package ; ils doivent être créés directement sous un **espace de nom**.

## Utilisation de règles pour associer des déclencheurs à des actions
{: #openwhisk_rules_assoc}

Des règles sont utilisées pour associer un déclencheur à une action. A chaque fois qu'un événement déclencheur est exécuté, l'action est appelée avec les paramètres d'événement.

Par exemple, créez une règle qui appelle l'action `hello` à chaque fois qu'une mise à jour d'emplacement est publiée.
1. Créez un fichier nommé 'hello.js' avec le code d'action suivant :
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Vérifiez que le déclencheur et l'action existent :
  ```
  ibmcloud wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. L'étape suivante consiste à créer la règle. La règle est activée au moment de la création ce qui signifie qu'elle est disponible immédiatement pour répondre aux activations de votre déclencheur. Les trois paramètres sont : _nom de la règle_, _nom du déclencheur_ et _nom de l'actione_.
  ```
  ibmcloud wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  Vous pouvez à tout moment choisir de désactiver une règle :
  ```
  ibmcloud wsk rule disable myRule
  ```
  {: pre}

4. Exécutez le déclencheur **locationUpdate**. A chaque fois que vous déclenchez un événement, l'action **hello** est appelée avec les paramètres d'événement.
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Vérifiez que l'action **hello** a été appelée en vérifiant l'activation la plus récente.
  ```
  ibmcloud wsk activation list --limit 1 hello
  ```
  {: pre}

  Exemple de sortie :
  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  Interrogez maintenant l'ID d'activation répertorié dans la sortie de la commande précédente :
  ```
  ibmcloud wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  Vous constatez que l'action **hello** a reçu le contneu de l'événement et a renvoyé la chaîne attendue.

Vous pouvez créer plusieurs règles qui associent le même déclencheur à des actions différentes.
Les déclencheurs et les règles ne peuvent pas appartenir à un package. En revanche, la règle peut être associée à une action
qui appartient à un package, par exemple :
  ```
  ibmcloud wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

Vous pouvez également utiliser des règles avec des séquences. Vous
pouvez ainsi créer une séquence d'actions `recordLocationAndHello`, qui est activée par la règle `anotherRule`.
  ```
  ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
