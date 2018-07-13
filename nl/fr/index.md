---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Guide d'initiation
{: #index}

{{site.data.keyword.openwhisk}} est une plateforme de programmation de type Functions-as-a-Service (FaaS) basée sur Apache OpenWhisk. {{site.data.keyword.openwhisk_short}} permet aux développeurs d'écrire un code simple, désignés par "actions", chargé d'exécuter la logique d'application de manière évolutive. Vous pouvez configurer des actions pour produire des demandes d'API de type HTTP à la demande à partir d'applications Web ou d'applications mobiles, ou automatiquement en réponse à des demandes basées sur des événements provenant de services {{site.data.keyword.Bluemix_notm}} et d'événements tiers.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} étant une plateforme sans serveur gérée par les événements, vous n'avez pas besoin d'ajouter de serveurs de manière explicite. Les développeurs utilisant des microservices ou des applications mobile, IoT, et de nombreuses autres applications peuvent se consacrer à l'écriture de la logique applicative au lieu de se préoccuper de la mise à l'échelle automatique, de se soucier de la haute disponibilité, des mises à jour et de la maintenance. La mise à l'échelle automatique et l'équilibrage de charge prêts à l'emploi signifient que vous n'avez pas à configurer manuellement des clusters, des plug-in HTTP, etc. IBM se charge de tout l'aspect matériel, mise en réseau et administration des logiciels. Il ne vous reste qu'à fournir le code.

Cliquez sur une option pour commencer :

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Cliquez sur une icône pour commencer rapidement à utiliser {{site.data.keyword.openswhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Création d'une action" title="Création d'une action" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="Configuration du plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" title="Configuration du plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="Voir l'architecture de la plateforme" title="Voir l'architecture de la platforme" shape="rect" coords="326, -10, 448, 218" />
</map>


## Création d'une action dans l'interface graphique
{: #openwhisk_start_hello_world}

Pour vous initier à {{site.data.keyword.openwhisk_short}}, commencez par utiliser le modèle Quickstart HelloWorld.

1.  Dans la catégorie **Fonctions** du [**catalogue** {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/?category=whisk), cliquez sur Fonctions.

2. Cliquez sur **Commencer la création** > **Modèles Quickstart** et sélectionnez le modèle **Hello World**.

5. Vérifiez le code de l'action, puis créez l'action en cliquant sur **Déployer**. Vous disposez maintenant d'une action appelée `hello`.

6. Exécutez l'action en cliquant sur **Appeler**. L'appel manuel d'une action exécute la logique d'application définie par l'action. Dans le panneau **Activations**, vous pouvez voir l'annonce d'accueil "Hello stranger!" générée par l'action.

Bien joué ! Vous venez de créer votre première action. Pour supprimer cette action, cliquez sur le menu déroulant dynamique et sélectionnez **Supprimer l'action**.

## Création d'une action dans l'interface de ligne de commande
{: #openwhisk_start_hello_world_cli}

Soyez rapidement opérationnel avec l'exemple de code JavaScript HelloWorld. Cet exemple crée une action de base `hello` que vous pouvez appeler manuellement pour en exécuter la logique d'application.

1. [Configurez le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

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
    ibmcloud wsk action create hello hello.js
    ```
    {: pre}

4. Appelez l'action sans paramètre.
    ```
    ibmcloud wsk action invoke hello --blocking --result
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
    ibmcloud wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Sortie :
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Bien joué ! Vous venez de créer votre première action. Pour supprimer cette action, exécutez `ibmcloud wsk action delete hello`.

**Etape suivante ?**
* [Tester l'exemple de service Alarm pour appeler l'action **hello** chaque fois qu'un événement périodique est généré.](./openwhisk_packages.html#openwhisk_package_trigger)
* [Créer une API REST sans serveur.](openwhisk_apigateway.html)
* [Consulter les packages d'actions préinstallés pour des services {{site.data.keyword.Bluemix_notm}}, tels que Cloudant.](cloudant_actions.html)
