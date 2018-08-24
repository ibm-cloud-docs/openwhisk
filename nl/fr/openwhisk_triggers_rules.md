---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Réponse à des événements avec des déclencheurs et des règles
{: #openwhisk_triggers}

Les déclencheurs et les règles {{site.data.keyword.openwhisk}} apportent des capacités gérées par des événements sur la plateforme. Les événements provenant de sources d'événements externes et internes sont canalisés via un déclencheur, et des règles autorisent vos actions à réagir à ces événements.
{: shortdesc}

## Concepts généraux
{: #definitions}

### Déclencheurs
{: #openwhisk_triggers_create}

Les déclencheurs constituent un canal nommé pour une classe d'événements.
{: shortdesc}

Un déclencheur est une déclaration qui doit réagir à un certain type d'événement, provenant d'un utilisateur ou d'une source d'événements. Voici des exemples de déclencheur. 
- Déclencheur d'événements de mise à jour d'emplacement
- Déclencheur de mises à jour de document sur un site Web
- Déclencheur de courriers électroniques entrants

Les déclencheurs peuvent être exécutés, ou activés, à l'aide d'un dictionnaire de paires clé-valeur. Parfois, ce dictionnaire est
appelé événement. Les déclencheurs peuvent être exécutés explicitement par un utilisateur ou pour le compte d'un utilisateur par une source d'événements d'externe. Comme pour les actions, chaque exécution de déclencheur génère un ID d'activation. Un déclencheur qui n'est pas associé à une règle n'a aucun effet visible lorsqu'il est exécuté.

Un flux est pratique pour configurer une source d'événements externe afin d'exécuter des événements déclencheurs qui peuvent être consommés par {{site.data.keyword.openwhisk_short}}. Voici des exemples de flux : 
- Flux de modification de données {{site.data.keyword.cloudant}} qui exécute un déclencheur à chaque ajout ou modification d'un document dans une base de données.
- Flux Git qui exécute un déclencheur pour chaque validation dans un référentiel Git

### Règles
{: #openwhisk_rules_use}

Une règle associe un déclencheur à une action.
{: shortdesc}

A chaque fois que le déclencheur s'exécute, la règle utilise l'événement déclencheur comme entrée et appelle l'action associée. Avec le jeu de règles approprié, un événement déclencheur unique peut appeler plusieurs actions, ou une action peut être appelée en réponse à des événements provenant de plusieurs déclencheurs.

Par exemple, imaginez un système avec les actions suivantes .
- `classifyImage` : action qui détecte les objets dans une image et les classe.
- `thumbnailImage` : action qui crée une version miniature d'une image.

De plus, supposez que deux sources d'événements exécutent les déclencheurs suivants.
- `newTweet` : déclencheur qui est exécuté lorsqu'un nouveau tweet est publié.
- `imageUpload` : déclencheur qui est exécuté lorsqu'une image est téléchargée sur un site Web.

Vous pouvez configurer des règles pour qu'un événement déclencheur unique appelle plusieurs actions, et pour que plusieurs déclencheurs appellent la même action .
- La règle `newTweet -> classifyImage`
- La règle `imageUpload -> classifyImage`
- La règle `imageUpload -> thumbnailImage`

Ces trois règles induisent le comportement suivant.
- Les images contenues dans chaque tweet sont classifiées
- Les images téléchargées sont classifiées
- Une version miniature est générée

## Création de déclencheurs pour des événements de canal
{: #openwhisk_triggers_fire}

Les étapes suivantes vous indiquent comment créer un exemple de déclencheur pour envoyer les mises à jour de l'emplacement d'utilisateur, et exécuter manuellement le déclencheur.

1. Créez le déclencheur. Les déclencheurs doivent être créés directement dans un espace de nom et ils ne peuvent pas être créés dans des packages.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. Vérifiez que le déclencheur est créé.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Exemple de sortie :
    ```
    triggers
  /someNamespace/locationUpdate                            private
    ```
    {: screen}
    Le déclencheur fait office de canal nommé dont les événements peuvent être déclenchés.

3. Exécutez un événement déclencheur.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Etant donné qu'aucune règle n'est associée à ce déclencheur, les paramètres transmis ne sont pas utilisés en entrée par une action. Exemple de sortie :
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

Dans la section suivante, vous pouvez associer le déclencheur à une action en créant une règle.

## Utilisation de règles pour associer des déclencheurs à des actions
{: #openwhisk_rules_assoc}

Des règles sont utilisées pour associer un déclencheur à une action. A chaque fois qu'un événement déclencheur est exécuté, l'action est appelée avec les paramètres de l'événement déclencheur.

Après que vous avez créé le déclencheur [`locationUpdate`](#openwhisk_triggers_fire), les étapes suivantes vous indiquent comment créer un exemple de règle qui appelle l'action `hello` chaque fois qu'une mise à jour d'emplacement est publiée.

1. Créez un fichier nommé 'hello.js' avec le code d'action suivant :
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. Créez l'action `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Créez la règle `myRule` pour associer le déclencheur `locationUpdate` à l'action `hello`. Les règles doivent être créées directement dans un espace de nom et elles ne peuvent pas être créées dans des packages.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Exécutez le déclencheur `locationUpdate`. Chaque fois qu'un événement déclencheur se produit, l'action `hello` est appelée avec les paramètres d'événement.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Vérifiez que l'action `hello` a été appelée en vérifiant l'enregistrement d'activation le plus récent.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    Exemple de sortie :
    ```
    activations
    9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Obtenez davantage d'informations sur l'ID d'activation à partir du résultat de la commande précédente.
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    Vous constatez que l'action `hello` a reçu le contenu de l'événement et a renvoyé la chaîne attendue.

7. Pour désactiver la règle, vous pouvez exécuter la commande suivante.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

Vous pouvez également utiliser des règles pour associer des déclencheurs à des séquences. Vous pouvez ainsi créer une séquence d'actions appelée `recordLocationAndHello`, qui est activée par la règle `anotherRule` : 
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
