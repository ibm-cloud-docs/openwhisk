---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Guide d'initiation
{: #index}

{{site.data.keyword.openwhisk}} vous permet d'utiliser votre langage de programmation favori pour écrire un code simple qui exécute la logique d'application de façon évolutive. Vous pouvez exécuter le code à la demande avec des demandés d'API basées sur HTTP provenant d'applications, ou automatiquement en réponse à des événements tiers et services {{site.data.keyword.Bluemix_notm}}. La plateforme de programmation Faas (Function-as-a-Service) repose sur le projet open source Apache OpenWhisk.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} étant sans serveur, le nombre de langages que vous pouvez utiliser est illimité et vous n'avez pas besoin de passer du temps à mettre à disposition explicitement une infrastructure de back end. Vous pouvez vous consacrer à l'écriture de la logique d'application au lieu de vous préoccuper de la mise à l'échelle automatique, de vous soucier de la haute disponibilité, des mises à jour et de la maintenance. Avec une solution prête à l'emploi, IBM fournit du matériel, des réseaux, l'administration des logiciels, l'équilibrage de charge, des plug-ins, et ainsi de suite. Il vous suffit de fournir le code ! 

## Gestion des actions
{: #creating_actions}
{: #openwhisk_actions}

{{site.data.keyword.openwhisk}} vous permet de créer des fragments de code sans état, qui sont définis pour l'exécution d'une tâche spécifique appelée action.
{:shortdesc}

**Qu'est-ce qu'une action ?**
{: #what_is_an_action}

Une action est un petit morceau de code qui peut être appelé ou défini pour s'exécuter automatiquement en réponse à un événement. Dans tous les cas, chaque exécution génère un enregistrement identifié par un ID d'activation unique. L'entrée et le résultat d'une action peuvent être affichés sous forme de paires clé-valeur. La clé est une chaîne et la valeur est une valeur JSON valide. Une action peut être écrite dans le langage de votre choix et fournie au service sous forme de code source ou d'image Docker. Le code d'action s'exécute lorsqu'il est directement appelé par l'API Cloud Functions, l'interface de ligne de commande ou le logiciel SDK iOS. Une action peut répondre automatiquement aux événements d'IBM Cloud ou de services tiers. 

**Pourquoi utiliser une action ?**
{: #why_use_an_action}

Les actions vous permettent de limiter la durée d'exécution de votre code, afin de réduire vos frais généraux. 

Par exemple, vous pouvez utiliser des actions pour détecter des visages dans une image, répondre à des modifications dans une base de données, agréger un ensemble d'appels d'API ou même publier un tweet. 

**Puis-je utiliser plusieurs actions simultanément ?**
{: #more_than_one_action}

Oui. Vous pouvez utiliser des actions pour appeler d'autres actions, ou vous pouvez chaîner des actions pour créer des séquences. Pour que cela fonctionne, la sortie
d'une action doit être utilisée comme entrée pour une autre action, qui fournira une sortie pouvant être utilisée pour déclencher une autre action et ainsi de suite. Vous pouvez même conditionner le groupe d'actions que vous créez pour former un package. Un package vous permet de réutiliser des actions ou des séquences courantes en appelant le package au lieu de configurer à nouveau l'action ou la séquence. 

Cliquez sur une option pour commencer :

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Cliquez sur une icône pour vous initier rapidement à {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="/docs/openwhisk?topic=cloud-functions-index#openwhisk_start_hello_world" alt="Création d'une action" title="Création d'une action" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="Configuration du plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" title="Configuration du plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="Voir l'architecture de la plateforme" title="Voir l'architecture de la platforme" shape="rect" coords="326, -10, 448, 218" />
</map>

## Création d'une action dans l'interface graphique
{: #openwhisk_start_hello_world}

Pour vous initier à {{site.data.keyword.openwhisk_short}}, commencez par utiliser le modèle Quickstart HelloWorld.

1.  Consultez le tableau de bord Functions dans le catalogue [{{site.data.keyword.Bluemix_notm}} **** ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk).

2. Cliquez sur **Commencer la création** > **Modèles Quickstart** et sélectionnez le modèle **Hello World**.

3. Créez un package pour vos actions en entrant un nom unique dans la zone **Nom de package**. 

4. Sélectionnez un environnement d'exécution dans le menu déroulant de la section **Action helloworld**. Vous pouvez prévisualiser le code de l'exemple d'action dans chaque environnement d'exécution disponible avant de déployer le modèle. 

5. Cliquez sur **Déployer**. Vous avez créé une action. Félicitations !

6. Exécutez l'action en cliquant sur **Appeler**. L'appel manuel d'une action exécute la logique d'application définie par l'action. Dans le panneau **Activations**, vous pouvez voir l'annonce d'accueil "Hello stranger!" générée par l'action.

7. Facultatif : Cliquez sur **Changer l'entrée** pour modifier l'action, ou essayez la vôtre. Cliquez ensuite sur **Appeler** pour exécuter l'action avec vos mises à jour. Le résultat s'affiche dans le panneau **Activations**. Vous pouvez répéter ce processus autant de fois que vous le souhaitez. 

Bien joué ! Vous venez de créer votre première action. Pour supprimer cette action, cliquez sur le menu déroulant dynamique et sélectionnez **Supprimer l'action**.

## Création d'une action dans l'interface de ligne de commande
{: #openwhisk_start_hello_world_cli}

Soyez rapidement opérationnel avec l'exemple de code JavaScript HelloWorld. Cet exemple crée une action de base `hello`, que vous pouvez appeler manuellement pour en exécuter la logique d'application.

1. [Configurez le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

2. Sauvegardez le code suivant dans un fichier nommé **hello.js**.

    ```javascript
    /**
     * Hello world en tant qu'action OpenWhisk.
     */
    function main(params) {
        var name = params.name || 'World';
        return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Créez l'action `hello`.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Appelez l'action sans paramètre.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Sortie :
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Appelez à nouveau l'action, avec cette fois-ci le paramètre "name" pour tester la logique d'application.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Sortie :
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Bien joué ! Vous venez de créer votre première action. Pour supprimer cette action, exécutez `ibmcloud fn action delete hello`.

## Etape suivante
{: #next-steps}

Maintenant que vous avez terminé le déploiement de votre premier modèle, que pouvez-vous faire ? Vous pouvez : 

* Vous familiariser avec la [terminologie](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology). 
* Apprendre à utiliser [vos propres actions](/docs/openwhisk?topic=cloud-functions-openwhisk_actions). 
* Découvrir comment organiser des actions en [packages](/docs/openwhisk?topic=cloud-functions-openwhisk_packages). 
* Option avancée : créer une [API REST sans serveur](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway). 
