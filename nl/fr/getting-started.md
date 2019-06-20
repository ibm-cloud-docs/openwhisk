
---

copyright :
  années : 2017, 2019
dernière mise à jour : "17 mai 2019"

mots clés : initiation, création d'actions, appel d'actions,

Sous-collection : cloud-functions

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

# Guide d'initiation
{: #getting-started}

{{site.data.keyword.openwhisk}} vous permet d'utiliser votre langage de programmation favori pour écrire un code simple qui exécute la logique d'application de façon évolutive. Vous pouvez exécuter le code à la demande avec des demandes d'API basées sur HTTP provenant d'applications, ou exécuter le code en réponse à des services {{site.data.keyword.Bluemix_notm}} et des événements tiers. La plateforme de programmation Faas (Function-as-a-Service) repose sur le projet open source Apache OpenWhisk.
{: shortdesc}

## Gestion des actions
{: #gs_actions}

{{site.data.keyword.openwhisk}} vous permet de créer des fragments de code sans état, qui sont définis pour l'exécution d'une tâche spécifique appelée action. Pour en savoir plus sur les actions et d'autres termes liés à Functions, voir [Terminologie](/docs/openwhisk?topic=cloud-functions-about).
{:shortdesc}

Cliquez sur une option pour commencer :

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Cliquez sur une icône pour vous initier rapidement à {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Créer une action" title="Créer une action" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="Configurer un plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" title="Configurer un plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="Voir l'architecture de la plateforme" title="Voir l'architecture de la plateforme" shape="rect" coords="326, -10, 448, 218" />
</map>

## Création d'une action dans l'interface graphique
{: #gs_hello_world}

Pour vous initier à {{site.data.keyword.openwhisk_short}}, commencez par utiliser le modèle Quickstart HelloWorld.

1. Créez un compte [{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration) ou connectez-vous à un compte existant.

2. Accédez au [tableau de bord {{site.data.keyword.openwhisk_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk).

2. Cliquez sur **Commencer la création** > **Modèles Quickstart** et sélectionnez le modèle **Hello World**.

3. Créez un package pour vos actions en entrant un nom unique dans la zone **Nom de package**.

4. Sélectionnez un environnement d'exécution dans le menu déroulant de la section **Action helloworld**. Vous pouvez prévisualiser le code de l'exemple d'action dans chaque environnement d'exécution disponible avant de déployer le modèle.

5. Cliquez sur **Déployer**. Vous avez créé une action. Félicitations !

6. Exécutez l'action en cliquant sur **Appeler**. L'appel manuel d'une action exécute la logique d'application définie par l'action. Dans le panneau **Activations**, vous pouvez voir l'annonce d'accueil "Hello stranger!" générée par l'action.

7. Facultatif : Cliquez sur **Changer l'entrée** pour modifier l'action ou essayer la vôtre.

  a. Collez le code suivant dans la case **Changer l'entrée** et modifiez la valeur de nom.
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. Cliquez ensuite sur **Appeler** pour exécuter l'action avec vos mises à jour. Le résultat s'affiche dans le panneau **Activations**. Vous pouvez répéter ce processus autant de fois que vous le souhaitez.

Bien joué ! Vous venez de créer votre première action. Pour supprimer cette action, cliquez sur le menu déroulant dynamique et sélectionnez **Supprimer l'action**.

## Création d'une action dans l'interface de ligne de commande
{: #gs_hello_world_cli}

Soyez rapidement opérationnel avec l'exemple de code [JavaScript HelloWorld](/docs/openwhisk?topic=cloud-functions-prep#prep-js). Cet exemple crée une action de base `hello`, que vous pouvez appeler manuellement pour en exécuter la logique d'application.

## Etape suivante
{: #gs_next_steps}

Maintenant que vous avez terminé le déploiement de votre premier modèle, que pouvez-vous faire ? Vous pouvez :

* Vous familiariser avec la [terminologie](/docs/openwhisk?topic=cloud-functions-about#about_technology).
* Apprendre à utiliser [vos propres actions](/docs/openwhisk?topic=cloud-functions-actions).
* Découvrir comment organiser des actions en [packages](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Option avancée : créer une [API REST sans serveur](/docs/openwhisk?topic=cloud-functions-apigateway).
