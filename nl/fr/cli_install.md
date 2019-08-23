---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# Installation de l'interface de ligne de commande et du plug-in
{: #cli_install}

{{site.data.keyword.openwhisk}} offre un plug-in puissant pour l'interface de ligne de commande {{site.data.keyword.cloud_notm}} qui permet une gestion complète du système {{site.data.keyword.openwhisk_short}}. Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour gérer vos fragments de code dans les actions, créer des déclencheurs et des règles afin de permettre à vos actions de répondre aux événements et de regrouper les actions dans des packages.
{:shortdesc}


## Configuration de l'interface de ligne de commande {{site.data.keyword.cloud_notm}}
{: #cli_setup}

**Avant de commencer**

Vous devez créer un [compte {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.

Téléchargez et installez l'interface de ligne de commande {{site.data.keyword.cloud_notm}} et connectez-vous.
{: shortdesc}

1. Téléchargez et installez l'[interface CLI {{site.data.keyword.cloud_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.cloud_notm}}. Pour stipuler une région {{site.data.keyword.cloud_notm}}, [incluez son noeud final d'API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

3. Suivez les invites pour sélectionner votre compte {{site.data.keyword.cloud_notm}}.

4. Obtenez la liste des groupes de ressources.  

```
ibmcloud resource groups
```
{: pre}

**Exemple de sortie**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. Facultatif : Ciblez un groupe de ressources autre que celui par défaut en exécutant la commande suivante.
```
ibmcloud target -g <resource_group>
```
{: pre}


**Exemple de sortie**

```
Targeted resource group <resource_group>
```
{: screen}

## Configuration du plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Pour utiliser {{site.data.keyword.openwhisk_short}}, téléchargez et installez le plug-in de l'interface de ligne de commande.
{: shortdesc}

Vous pouvez utiliser le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour effectuer les tâches suivantes.

* Exécuter vos fragments de code, ou vos actions, dans {{site.data.keyword.openwhisk_short}}. Voir [Création et appel d'actions](/docs/openwhisk?topic=cloud-functions-actions).
* Créer des déclencheurs et des règles pour que vos actions puissent répondre à des événements. Voir [Création de déclencheurs et de règles](/docs/openwhisk?topic=cloud-functions-triggers).
* Regrouper des actions et configurer des sources d'événements externes. Voir [Création et utilisation de packages](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Explorer le catalogue des packages et améliorer vos applications avec des services externes. Voir [Ajout d'{{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services).

Exécutez la procédure suivante pour installer le plug-in d'interface CLI {{site.data.keyword.openwhisk_short}}. 

1. Installez le plug-in {{site.data.keyword.openwhisk_short}}.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Vérifiez que le plug-in est installé.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **Sortie**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. Après la connexion, toutes les commandes {{site.data.keyword.openwhisk_short}} commencent par `ibmcloud fn`. Pour afficher toutes les opérations possibles avec le plug-in {{site.data.keyword.openwhisk_short}}, exécutez la commande `ibmcloud fn` sans arguments.
  ```
  ibmcloud fn
  ```
  {: pre}




## Ciblage des espaces de nom {{site.data.keyword.openwhisk_short}} 
{: #cli_regions}
Par défaut, {{site.data.keyword.openwhisk_short}} utilise des [espaces de nom activés pour IAM](/docs/iam?topic=iam-iamoverview){: external}. Vous ne pouvez plus créer d'espaces de nom basés Cloud Foundry.
{: important}

### Créez ou ciblez un espace de nom.
Pour obtenir la liste de vos espaces de nom {{site.data.keyword.openwhisk_short}}, exécutez la commande `ibmcloud fn namespace list`.

#### Créez un espace de nom activé pour IAM. 
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**Réponse**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### Ciblez un espace de nom activé pour IAM. 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**Réponse**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Ciblez un espace de nom basé Cloud Foundry.  
  
Vous pouvez utiliser les indicateurs `-o` et `-s` pour cibler des éléments `org` et `space` spécifiques, ou vous pouvez suivre les invites.

* Ciblez un espace de nom Cloud Foundy en incluant les noms `org` et `space` dans la commande `target`.

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Ciblez Cloud Foundry et suivez les invites pour sélectionner des éléments `org` et `space`.

```
ibmcloud target --cf
```
{: pre}


**Réponse**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### Facultatif : Création d'espaces de nom pour les déploiements de préproduction et de production.

Vous pouvez créer des espaces de nom activés pour IAM pour gérer vos déploiements {{site.data.keyword.openwhisk_short}} de préproduction et de production en créant des espaces pour chacun d'eux. Exécutez la commande [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) pour créer davantage d'espaces de nom sous votre organisation, par exemple "staging" (préproduction) et "production" :

Créez un espace de nom de préproduction. 
```
ibmcloud fn namespace create staging
```
{: pre}

Créez un espace de nom de production.
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} fait l'objet de restrictions en matière de noms d'espace de nom. Pour plus d'informations, consultez la documentation sur les [limites et détails du système](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}


## Configuration de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour utiliser un proxy HTTPS
{: #cli_proxy}

L'interface de ligne de commande {{site.data.keyword.openwhisk_short}} peut être configurée pour utiliser un proxy HTTPS. Pour configurer un proxy HTTPS, vous devez créer une variable d'environnement appelée `HTTPS_PROXY`. Celle-ci doit avoir pour valeur l'adresse du proxy HTTPS et son port définis au format suivant : `{IP PROXY}:{PORT PROXY}`.

Changer le nom de l'élément `org` ou `space` crée un nouvel espace de nom basé sur ce nom. Les entités situées dans l'ancien espace de nom ne sont pas visibles dans le nouvel espace de nom et peuvent être supprimées.
{: important}


## Migration depuis l'interface de ligne de commande OpenWhisk vers le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Vous pouvez désormais utiliser le plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour interagir avec les entités {{site.data.keyword.openwhisk_short}}. Bien que vous puissiez continuer à utiliser l'interface de ligne de commande autonome OpenWhisk, elle ne contient pas les dernières fonctionnalités prises en charge par {{site.data.keyword.openwhisk_short}}, comme les espaces de nom IAM et la `liaison de service`.
{: shortdesc}


### Syntaxe de commande
{: #cli_syntax}

Toutes les options de commande et tous les arguments pour les commandes dans le plug-in de l'interface de ligne de commande Cloud Functions sont identiques aux commandes pour l'[interface CLI autonome OpenWhisk](https://github.com/apache/incubator-openwhisk-cli){: external}. Notez néanmoins les différences suivantes. 

* Le plug-in {{site.data.keyword.openwhisk}} utilise automatiquement les informations de connexion et de cible en cours.
* Les commandes `wsk` s'exécutent désormais en tant que `ibmcloud fn`.
* La commande `wsk ibmcloud login` n'est plus nécessaire. Vous pouvez vous connecter à l'aide de la commande `ibmcloud login`.
* Vous pouvez gérer vos API en utilisant la commande `ibmcloud fn api`.

Pour plus d'informations, reportez-vous à la rubrique [Référence d'interface CLI {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli).

### Authentification et hôte d'API
{: #cli_api_auth}

Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, vous n'avez plus besoin de configurer de manière explicite la clé d'API et l'hôte d'API. A la place, vous pouvez vous connecter avec la commande `ibmcloud login`. Vous pouvez cibler un espace de nom IAM en exécutant la commande `ibmcloud fn property set --namespace <namespace_name>` ou un espace de nom Cloud Foundry avec la commande `ibmcloud target --cf`. Après la connexion, toutes les commandes commencent par `ibmcloud fn`.


Si vous devez utiliser une clé d'API d'authentification pour {{site.data.keyword.openwhisk_short}} dans un client HTTP externe, tel que cURL ou Postman, vous pouvez la récupérer avec les commandes suivantes. 

Obtenez la clé d'API en cours en exécutant la commande suivante.
```
ibmcloud fn property get --auth
```
{: pre}

Obtenez l'hôte d'API en cours en exécutant la commande suivante.
```
ibmcloud fn property get --apihost
```
{: pre}

La clé d'API est propre à chaque région, chaque organisation et chaque espace ciblés par le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}.
{: tip}


### Authentification de passerelle d'API
{: #cli_apigw_authentication}

L'interface de ligne de commande OpenWhisk vous obligeait à exécuter la commande `wsk ibmcloud login` pour pouvoir configurer l'autorisation de la passerelle d'API pour la gestion de vos API en utilisant la commande `wsk api`. Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, il n'est plus nécessaire d'exécuter la commande `wsk ibmcloud login`. A la place, lorsque vous utilisez la commande `ibmcloud login` pour vous connecter à {{site.data.keyword.cloud_notm}}, le plug-in {{site.data.keyword.openwhisk}} utilise automatiquement vos informations de connexion et de destination. A présent, vous pouvez gérer vos API en utilisant la commande `ibmcloud fn api`.


### Migration des scripts de déploiement
{: #cli_migrating_deploy_scripts}

Si vous avez des scripts qui utilisent l'interface de ligne de commande OpenWhisk avec les commandes `wsk`, toutes les commandes fonctionnent de la même manière en utilisant la commande `ibmcloud fn`. Vous pouvez modifier vos scripts afin d'utiliser le plug-in d'interface de ligne de commande {{site.data.keyword.cloud_notm}}, ou créer un alias ou un encapsuleur de sorte que les commandes utilisant actuellement `wsk` soient converties en `ibmcloud fn`. Les commandes `ibmcloud login` et `ibmcloud target` dans l'interface de ligne de commande {{site.data.keyword.cloud_notm}} fonctionnent en mode sans opérateur. Dans ce mode, vous pouvez configurer votre environnement avant d'exécuter des commandes `ibmcloud fn` pour déployer et gérer vos entités {{site.data.keyword.openwhisk_short}}.

## Historique des versions de l'interface de ligne de commande
{: #cli_versions}

Enregistrement historique des versions qui montre les points forts et les correctifs de bogue.

version 1.0.30 (03 avril 2019)
* Amélioration du traitement de la `liaison de service` d'IAM, ainsi que des services basés sur l'organisation et l'espace.
* Ajout d'un correctif pour le traitement du noeud final de l'API https://cloud.ibm.com.

version 1.0.29 (06 février 2019)
* Ajout des commandes `deploy` et `undeploy` pour déployer une collection d'entités Functions ou annuler leur déploiement via un fichier manifeste. Pour plus d'informations, voir la documentation sur le [déploiement](/docs/openwhisk?topic=cloud-functions-deploy#deploy) .

version 1.0.28 (21 janvier 2019)
* Ajout d'un message d'erreur lorsqu'il existe plusieurs occurrences de `update|delete|get namespace name`.

version 1.0.27 (11 décembre 2018)
* Ajout de correctifs `namespace get`.
* Ajout d'un correctif pour `--save-as` lorsqu'une action est de type boîte noire.

version 1.0.26 (30 novembre 2018)
* Activation de `fn property get --auth` pour renvoyer correctement la clé d'autorisation dans un nouvel environnement.

version 1.0.25 (23 novembre 2018)
* Amélioration de l'affichage du résultat du message d'erreur.
* Ajout d'un correctif `fn namespace get` pour afficher correctement les propriétés de l'espace de nom.

1.0.23 (15 octobre 2018)
* Ajout de la prise en charge de la reconnaissance du code d'action ruby (`.rb`).

1.0.22 (20 août 2018)
* Ajout de la prise en charge de la région us-east.

1.0.21 (01 août 2018)
* Les alias `fn` et `functions` peuvent désormais être utilisés pour les commandes {{site.data.keyword.openwhisk_short}} : `ibmcloud fn <command>` et `ibmcloud fn <command>`. Vous pouvez également encore utiliser `ibmcloud wsk <command>`.

1.0.19 (02 juillet 2018)
* Correctifs de bogues mineurs et améliorations.

1.0.18 (20 juin 2018)
* Ajout d'un correctif permettant de supprimer la liaison des instances de service fournies par l'utilisateur.
* Amélioration des performances.

1.0.17 (12 juin 2018)
* Ajout du support de liaison (`ibmcloud wsk service bind`) et de suppression de liaison (`ibmcloud wsk service unbind`) des instances de service fournies par l'utilisateur créées en utilisant la commande `ibmcloud cf create-user-provided-service`.

1.0.16 (24 mai 2018)
* Correctifs de bogues mineurs et améliorations.

1.0.15 (21 mai 2018)
* Correctifs de bogues mineurs et améliorations.

1.0.14 (17 mai 2018)
* Prise en charge du caractère `&` pour les noms d'organisation et d'espace.

1.0.13 (07 mai 2018)
* Correctifs de bogues mineurs et amélioration du traitement des erreurs.

1.0.12 (30 avril 2018)
* Mises à jour du SDK {{site.data.keyword.cloud_notm}} pour assurer la compatibilité avec l'interface de ligne de commande `bx`.

1.0.11 (23 avril 2018)
* Correctifs de bogues mineurs et améliorations.

1.0.10 (09 avril 2018)
* Ajout de la nouvelle option `--web-secure` dans les commandes `ibmcloud wsk action create|update` pour sécuriser les noeuds finaux des actions Web.
* Correction de l'[erreur](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external} liée aux paramètres de chemins consécutifs.

1.0.9 (16 mars 2018)
* Prise en charge de la liaison de service au niveau du package.

1.0.8 (22 février 2018)
* Prise en charge de la liaison de service IAM.

1.0.7 (02 février 2018)
* Mise à jour de `ibmcloud wsk api` pour accepter des paramètres de chemin de type `/api/{id}`. Pour plus d'informations, voir [Passerelle d'API](/docs/openwhisk?topic=cloud-functions-apigateway).
* Rétablissement de la prise en charge de proxy.
* Retrait de `swift:3`.

1.0.6 (30 janvier 2018)
* Correction de bogue pour la commande `ibmcloud wsk service bind` concernant les actions présentes dans un package.



