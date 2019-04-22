---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration du plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} offre un plug-in puissant pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} qui permet une gestion complète du système {{site.data.keyword.openwhisk_short}}. Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour gérer vos fragments de code dans les actions, créer des déclencheurs et des règles afin de permettre à vos actions de répondre aux événements et de regrouper les actions dans des packages.
{:shortdesc}

Vous pouvez désormais utiliser l'alias `fn` dans vos commandes de plug-in {{site.data.keyword.openwhisk_short}} : `ibmcloud fn <command>`
{: tip}

## Configuration de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Téléchargez et installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} et connectez-vous.
{: shortdesc}

1. Téléchargez et installez l'interface de ligne de commande [{{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Pour spécifier une région IBM Cloud, [incluez le noeud final de l'API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

  Vous pouvez indiquer l'organisation et l'espace à la connexion pour éviter d'avoir à répondre aux invites en utilisant les indicateurs suivants : `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. Si vous n'avez pas spécifié d'organisation et d'espace, répondez aux invites qui suivent la commande login.


## Configuration du plug-in {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Pour utiliser {{site.data.keyword.openwhisk_short}}, téléchargez et installez le plug-in de l'interface de ligne de commande.
{: shortdesc}

Vous pouvez utiliser le plug-in pour :

* Exécuter vos fragments de code, ou vos actions, dans {{site.data.keyword.openwhisk_short}}. Voir [Création et appel d'actions](/docs/openwhisk?topic=cloud-functions-openwhisk_actions).
* Utiliser des déclencheurs et des règles pour que vos actions puissent répondre à des événements. Voir [Création de déclencheurs et de règles](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers).
* Découvrir comment les packages regroupent des actions et configurer des sources d'événements externes. Voir [Création et utilisation de packages](/docs/openwhisk?topic=cloud-functions-openwhisk_packages).
* Explorer le catalogue de packages et améliorer vos applications avec des services externes, par exemple une [source d'événement {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

Pour afficher toutes les opérations possibles avec le plug-in {{site.data.keyword.openwhisk_short}}, exécutez la commande `ibmcloud fn` sans arguments.
{: tip}

1. Installez le plug-in {{site.data.keyword.openwhisk_short}}.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Vérifiez que le plug-in est installé.

  ```
  ibmcloud plugin list cloud-functions
  ```
  {: pre}

  Sortie :
  ```
  Plugin Name          Version
    Cloud-Functions      1.0.16
  ```
  {: screen}

Vous disposez déjà du plug-in mais vous devez le mettre à jour ? Exécutez `ibmcloud plugin update cloud-functions`.
{:tip}



## Utilisation de services à partir d'actions
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}} fournit la commande `service bind` pour mettre vos données d'identification du service {{site.data.keyword.Bluemix_notm}} à la disposition de votre code lors de l'exécution. Vous pouvez ensuite utiliser la commande `service bind` pour lier un service {{site.data.keyword.Bluemix_notm}} à une action définie dans {{site.data.keyword.openwhisk_short}}.

Pour obtenir la procédure détaillée de l'utilisation des services à partir d'actions, voir [Liaisons de service à des actions](/docs/openwhisk?topic=cloud-functions-binding_services).


## Configuration de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour utiliser un proxy HTTPS
{: #cli_https_proxy}

L'interface de ligne de commande {{site.data.keyword.openwhisk_short}} peut être configurée pour utiliser un proxy HTTPS. Pour configurer un proxy HTTPS, vous devez créer une variable d'environnement appelée `HTTPS_PROXY`. Celle-ci doit avoir pour valeur l'adresse du proxy HTTPS et son port définis au format suivant : `{IP PROXY}:{PORT PROXY}`.



## Changement de régions, d'organisations et d'espaces
{: #region_info}

Si vous êtes déjà connecté, vous pouvez exécuter la commande `ibmcloud target` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} pour changer de régions, d'organisations et d'espaces.


Pour créer et gérer des entités, vous devez cibler un espace de nom. L'espace de nom par défaut, qui peut être signalé par un trait de soulignement (`_`) dans certaines situations, correspond à l'espace de nom Cloud Foundry actuellement ciblé. 

Vous pouvez créer des espaces pour gérer vos déploiements de préproduction et de production en créant des espaces pour chacun d'eux. Le fait de créer des espaces permet que deux noms d'espace différents soient définis pour vous par {{site.data.keyword.openwhisk_short}}. Exécutez la commande [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) pour créer d'autres espaces sous votre organisation, par exemple "staging" et "production" :

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} fait l'objet de restrictions en matière de noms d'espace de nom. Pour plus d'informations, consultez la documentation sur les [limites et détails du système](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities).
{: tip}

**Avertissement** : La modification du nom de l'organisation ou de l'espace crée un nouvel espace de nom basé sur le nom modifié. Les entités situées dans l'ancien espace de nom ne sont pas visibles dans le nouvel espace de nom et peuvent être supprimées. 


## Migration depuis l'interface de ligne de commande OpenWhisk vers le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Vous pouvez désormais utiliser le plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour interagir avec les entités {{site.data.keyword.openwhisk_short}}. Bien que vous puissiez continuer à utiliser l'interface de ligne de commande autonome OpenWhisk, elle ne contiendra pas les dernières fonctionnalités prises en charge par {{site.data.keyword.openwhisk_short}}, comme les espaces de nom IAM et la `liaison de service`.
{: shortdesc}

### Syntaxe de commande
{: #command_syntax}

Toutes les commandes `wsk`, à l'exception de la commande `wsk bluemix login` devenue inutile, fonctionnent de la même façon en utilisant la commande `ibmcloud fn`. Toutes les options de commande et tous les arguments pour les commandes dans le plug-in de l'interface de ligne de commande Cloud Functions sont identiques aux commandes pour l'interface de ligne de commande autonome OpenWhisk. Pour plus d'informations, consultez le [projet d'interface de ligne de commande Apache OpenWhisk ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-cli). 

### Authentification et hôte d'API
{: #api_authentication}

Avec l'interface de ligne de commande OpenWhisk, vous deviez configurer la clé d'API d'authentification et l'hôte d'API. Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, vous n'avez plus besoin de configurer de manière explicite la clé d'API et l'hôte d'API. A la place, vous pouvez vous connecter avec `ibmcloud login` et cibler votre région et votre espace de nom en utilisant la commande `ibmcloud target`. Après la connexion, toutes les commandes commencent par `ibmcloud fn`.


Si vous devez utiliser une clé d'API d'authentification pour {{site.data.keyword.openwhisk_short}} dans un client HTTP externe, tel que cURL ou Postman, vous pouvez la récupérer avec les commandes suivantes :

Pour obtenir la clé d'API en cours :
```
ibmcloud fn property get --auth
```
{: pre}

Pour obtenir l'hôte d'API en cours :
```
ibmcloud fn property get --apihost
```
{: pre}

La clé d'API est propre à chaque région, chaque organisation et chaque espace ciblés par le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}.
{: tip}

### Authentification de passerelle d'API
{: #apigw_authentication}

L'interface de ligne de commande OpenWhisk vous obligeait à exécuter la commande `wsk bluemix login` pour pouvoir configurer l'autorisation de la passerelle d'API pour la gestion de vos API en utilisant la commande `wsk api`. Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, il n'est plus nécessaire d'exécuter la commande `wsk bluemix login`. A la place, lorsque vous utilisez la commande `ibmcloud login` pour vous connecter à {{site.data.keyword.Bluemix_notm}}, le plug-in {{site.data.keyword.openwhisk}} utilise automatiquement vos informations de connexion et de destination. A présent, vous pouvez gérer vos API en utilisant la commande `ibmcloud fn api`.

### Migration des scripts de déploiement
{: #migrating_deploy_scripts}

Si vous avez des scripts qui utilisent l'interface de ligne de commande OpenWhisk avec le binaire `wsk`, toutes les commandes fonctionnent de la même manière en utilisant la commande `ibmcloud fn`. Vous pouvez modifier vos scripts afin d'utiliser le plug-in d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, ou créer un alias ou un encapsuleur de sorte que les commandes utilisant actuellement `wsk` soient converties en `ibmcloud fn`. Les commandes `ibmcloud login` et `ibmcloud target` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} fonctionnent en mode sans opérateur. Dans ce mode, vous pouvez configurer votre environnement avant d'exécuter des commandes `ibmcloud fn` pour déployer et gérer vos entités {{site.data.keyword.openwhisk_short}}.





## Historique des versions de l'interface de ligne de commande
{: #version_history}

Enregistrement historique des versions qui montre les points forts et les correctifs de bogue.

v1.0.30 (2019-04-03)
* Amélioration du traitement de la `liaison de service` d'IAM, ainsi que des services basés sur l'organisation et l'espace. 
* Ajout d'un correctif pour le traitement du noeud final de l'API https://cloud.ibm.com.

v1.0.29 (2019-02-06)
* Ajout des commandes `deploy` et `undeploy` pour déployer une collection d'entités Functions ou annuler leur déploiement via un fichier manifeste. Pour plus d'informations, consultez la documentation [Déploiement](/docs/openwhisk?topic=cloud-functions-deploy#deploy). 

v1.0.28 (2019-01-21)
* Ajout d'un message d'erreur lorsqu'il existe plusieurs occurrences de `update|delete|get namespace name`. 

v1.0.27 (2018-12-11)
* Ajout de correctifs `namespace get`. 
* Ajout d'un correctif pour `--save-as` lorsqu'une action est de type boîte noire. 
* Ajout de l'indicateur `--concurrency` pour les commandes action create et action update. 

v1.0.26 (2018-11-30)
* Activation de `fn property get --auth` pour renvoyer correctement la clé d'autorisation dans un nouvel environnement. 

v1.0.25 (2018-11-23)
* Amélioration de l'affichage du résultat du message d'erreur. 
* Ajout d'un correctif `fn namespace get` pour afficher correctement les propriétés de l'espace de nom. 

1.0.23 (2018-10-15)
* Ajout de la prise en charge de la reconnaissance du code d'action Ruby (.rb). 

1.0.22 (2018-08-20)
* Ajout de la prise en charge de la région us-east. 

1.0.21 (2018-08-01)
* Les alias `fn` et `functions` peuvent désormais être utilisés pour les commandes {{site.data.keyword.openwhisk_short}} : `ibmcloud fn <command>` et `ibmcloud fn <command>`. Vous pouvez également encore utiliser `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Correctifs de bogues mineurs et améliorations.

1.0.18 (20/06/2018)
* Ajout d'un correctif permettant de supprimer la liaison des instances de service fournies par l'utilisateur.
* Amélioration des performances.

1.0.17 (12/06/2018)
* Ajout du support de liaison (`ibmcloud wsk service bind`) et de suppression de liaison (`ibmcloud wsk service unbind`) des instances de service fournies par l'utilisateur créées en utilisant la commande `ibmcloud cf create-user-provided-service`.

1.0.16 (24/05/2018)
* Correctifs de bogues mineurs et améliorations.

1.0.15 (21/05/2018)
* Correctifs de bogues mineurs et améliorations.

1.0.14 (17/05/2018)
* Prise en charge du caractère `&` pour les noms d'organisation et d'espace.

1.0.13 (07/05/2018)
* Correctifs de bogues mineurs et amélioration du traitement des erreurs.

1.0.12 (30/04/2018)
* Mises à jour du SDK {{site.data.keyword.Bluemix_notm}} pour assurer la compatibilité avec l'interface de ligne de commande `bx`.

1.0.11 (23/04/2018)
* Correctifs de bogues mineurs et améliorations.

1.0.10 (09/04/2018)
* Ajout de la nouvelle option `--web-secure` dans les commandes `ibmcloud wsk action create|update` pour sécuriser les noeuds finaux des actions Web.
* Correction de l'[erreur](https://github.com/apache/incubator-openwhisk-cli/issues/237) liée aux paramètres de chemins consécutifs.

1.0.9 (16/03/2018)
* Prise en charge de la liaison de service au niveau du package.

1.0.8 (22/02/2018)
* Prise en charge de la liaison de service IAM.

1.0.7 (02/02/2018)
* Mise à jour de `ibmcloud wsk api` pour accepter des paramètres de chemin de type `/api/{id}`. Pour plus d'informations, voir [Passerelle d'API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
* Rétablissement de la prise en charge de proxy.
* Retrait de `swift:3`.

1.0.6 (30/01/2018)
* Correction de bogue pour la commande `ibmcloud wsk service bind` concernant les actions présentes dans un package.
