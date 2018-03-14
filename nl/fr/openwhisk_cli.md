---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Interface de ligne de commande autonome d'{{site.data.keyword.openwhisk_short}}

**A compter du 9 mars 2018, l'interface de ligne de commande autonome d'{{site.data.keyword.openwhisk_short}} ne sera plus disponible pour téléchargement. Pour continuer de gérer des entités {{site.data.keyword.openwhisk_short}} au-delà de cette date, vous devrez utiliser le [plug-in d'interface de ligne de commande d'{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) pour l'interface de ligne de commande de {{site.data.keyword.Bluemix}}.**

L'interface de ligne de commande **wsk** distribuée d'{{site.data.keyword.openwhisk}} permet de gérer toutes les entités {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* Configuration de la clé d'authentification {{site.data.keyword.openwhisk_short}}
  * Il n'est pas nécessaire d'accéder à la console {{site.data.keyword.openwhisk_short}} pour obtenir la clé d'authentification
  * La génération automatique de clé d'authentification se produit lors du passage d'une région, d'une organisation et d'un espace à une autre région, une autre organisation et un autre espace 
* Actualisation d'une clé d'authentification {{site.data.keyword.openwhisk_short}} étant arrivée à expiration
* Mise à jour de l'interface de ligne de commande vers une version ultérieure


L'utilisation du plug-in est requise pour les tâches de gestion suivantes :

* Gestion d'API
  * Configuration du jeton d'accès API GW
  * Actualisation d'un jeton d'accès API GW étant arrivé à expiration

## Configuration de l'interface de ligne de commande d'{{site.data.keyword.openwhisk_short}} 
{: #openwhisk_cli_setup}

Vous pouvez utiliser l'interface de ligne de commande d'{{site.data.keyword.openwhisk_short}} pour configurer votre espace de nom et votre clé d'autorisation.
Accédez à la page de [configuration de l'interface de ligne de commande](https://console.bluemix.net/openwhisk/cli?loadWsk=true) et suivez les instructions d'installation.

Commencez par configurer les deux propriétés suivantes :

1. L'**hôte d'API** (nom ou adresse IP) pour le déploiement d'{{site.data.keyword.openwhisk_short}} que vous souhaitez utiliser.
2. La **clé d'autorisation** (nom d'utilisateur et mot de passe) qui vous permet d'accéder à l'API {{site.data.keyword.openwhisk_short}}.

Deux régions {{site.data.keyword.Bluemix_notm}} requérant leur propre hôte d'API et leur propre clé d'autorisation uniques sont disponibles. 

* Sud des Etats-Unis
  * Hôte d'API : `openwhisk.ng.bluemix.net`

* Royaume-Uni
  * Hôte d'API : `openwhisk.eu-gb.bluemix.net`

Exécutez la commande suivante afin de définir l'hôte d'API pour la région {{site.data.keyword.Bluemix_notm}} concernée :

Sud des Etats-Unis :
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Royaume-Uni :
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Si vous avez besoin de basculer vers une autre région, vous devez reconfigurer l'interface de ligne de commande avec l'hôte d'API et la clé d'autorisation car celle-ci est spécifique de la région.
{: tip}

Les artefacts, tels que les actions, les règles et les packages sont spécifiques des régions. Par conséquent, si vous utilisez le même artefact dans plusieurs régions, il vous faut le déployer dans chaque région concernée. 

Si vous connaissez votre clé d'autorisation, vous pouvez configurer l'interface de ligne de commande afin de l'utiliser. 

Exécutez la commande suivante pour définir la clé d'autorisation :

```
wsk property set --auth <clé_autorisation>
```
{: pre}

**Astuce :** l'interface CLI d'{{site.data.keyword.openwhisk_short}} stocke par défaut dans `~/.wskprops` les propriétés définies. L'emplacement de ce fichier peut être modifié par le biais de la variable d'environnement `WSK_CONFIG_FILE`. 

Pour vérifier votre configuration d'interface de ligne de commande, essayez de [créer et d'exécuter une action](./index.html#openwhisk_start_hello_world).

## Utilisation de l'interface de ligne de commande d'{{site.data.keyword.openwhisk_short}}
{: #using_openwhisk_cli}

Une fois l'environnement configuré, vous pouvez utiliser l'interface de ligne de commande d'{{site.data.keyword.openwhisk_short}} pour effectuer les tâches suivantes :

* Exécuter vos fragments de code, ou vos actions, dans {{site.data.keyword.openwhisk_short}}. Voir [Création et appel d'actions](./openwhisk_actions.html).
* Utiliser des déclencheurs et des règles pour que vos actions puissent répondre à des événements. Voir [Création de déclencheurs et de règles](./openwhisk_triggers_rules.html).
* Découvrir comment les packages regroupent des actions et configurer des sources d'événements externes. Voir [Utilisation et création de packages](./openwhisk_packages.html).
* Explorer le catalogue de packages et améliorer vos applications avec des services externes tels qu'une [source d'événements Cloudant](./openwhisk_cloudant.html). Voir [Packages {{site.data.keyword.openwhisk_short}} préinstallés](./openwhisk_catalog.html).

## Configuration de l'interface de ligne de commande pour l'utilisation d'un proxy HTTPS
{: #cli_https_proxy}

L'interface de ligne de commande peut être configurée en vue de l'utilisation d'un proxy HTTPS. Pour configurer un proxy HTTPS, vous devez créer une variable d'environnement appelée `HTTPS_PROXY`. Celle-ci doit avoir pour valeur l'adresse du proxy HTTPS et son port définis au format suivant : `{IP PROXY}:{PORT PROXY}`.
