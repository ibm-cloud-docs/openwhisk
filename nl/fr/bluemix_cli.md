---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} offre un plug-in puissant pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} qui permet une gestion complète du système {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configuration de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Téléchargez et installez l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. ](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)

## Configuration du plug-in {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Pour installer le plug-in {{site.data.keyword.openwhisk_short}}, exécutez la commande suivante :
```
ic plugin install cloud-functions
```
{: pre}


Pour vérifier que l'installation du plug-in {{site.data.keyword.openwhisk_short}} a abouti, exécutez la commande suivante :
```
ic plugin list cloud-functions
```
{: pre}


La sortie affiche les informations sur la version {{site.data.keyword.openwhisk_short}} installée :
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

Vous pouvez mettre à niveau le plug-in {{site.data.keyword.openwhisk_short}} en exécutant la commande suivante :
```
ic plugin update Cloud-Functions
```
{: pre}


Pour plus d'informations sur la commande de plug-in, utilisez `ic plugin --help` ou vérifiez la documentation suivante :
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## Authentification d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} est disponible dans deux régions {{site.data.keyword.Bluemix_notm}}. 

Lorsque vous vous connectez à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, vous pouvez spécifier le noeud final d'API {{site.data.keyword.Bluemix_notm}} pour la région cible tout comme pour l'organisation et l'espace. 

Pour vous connecter à la région Sud des Etats-Unis :
```
ic login -a api.ng.bluemix.net
```
{: pre}


Pour vous connecter à la région Royaume-Uni :
```
ic login -a api.eu-gb.bluemix.net
```
{: pre}


L'indicateur `-a` spécifie le noeud final d'API {{site.data.keyword.Bluemix_notm}} à utiliser. Si le noeud final d'API est spécifié, l'option `-a` n'est pas nécessaire. Vous pouvez utiliser la commande `ic api` pour définir explicitement le noeud final d'API {{site.data.keyword.Bluemix_notm}}. Pour afficher le paramétrage de noeud final d'API en cours, utilisez la commande `ic target`. 

La commande `login` vous invite à saisir des informations, telles que l'organisation, l'espace et le mot de passe, si elles ne sont pas spécifiées. Vous pouvez spécifier l'organisation et l'espace sur la ligne de commande pour ignorer l'invite vous demandant de les saisir. 
```
ic login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


Vous pouvez également utiliser une clé d'API {{site.data.keyword.Bluemix_notm}} pour vous connecter. Cette méthode est utile lorsque votre compte est configuré avec une connexion fédérée qui vous demande de vous connecter avec l'indicateur `- - connexion unique`. L'utilisation d'une clé d'API est également avantageuse si vous souhaitez définir une intégration continue et configurer un pipeline inattendu. 
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Pour créer une nouvelle clé d'API en utilisant l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, exécutez la commande suivante :
```
ic iam api-key-create MyKey
```
{: pre}


Ensuite, utilisez la valeur générée par la clé d'API pour vous connecter, comme dans l'exemple suivant :
```
ic login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


Pour plus d'informations sur la commande login, utilisez `ic login --help` ou lisez la documentation suivante :
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Utilisation du plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_usage}

Vérifiez votre configuration. Effectuez un appel (synchrone) bloquant d'echo, en transmettant `hello` comme argument, comme dans l'exemple suivant :
```
ic wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


Exemple :
```
{
    "message":"hello"
}
```

Une fois votre environnement configuré, vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour effectuer les tâches suivantes :

* Exécuter vos fragments de code, ou vos actions, dans {{site.data.keyword.openwhisk_short}}. Voir [Création et appel d'actions](./openwhisk_actions.html).
* Utiliser des déclencheurs et des règles pour que vos actions puissent répondre à des événements. Voir [Création de déclencheurs et de règles](./openwhisk_triggers_rules.html).
* Découvrir comment les packages regroupent des actions et configurer des sources d'événements externes. Voir [Création et utilisation de packages](./openwhisk_packages.html).
* Explorer le catalogue de packages et améliorer vos applications avec des services externes tels qu'une [source d'événements Cloudant](./openwhisk_cloudant.html). Voir [Utilisation de services compatibles avec {{site.data.keyword.openwhisk_short}}](./openwhisk_catalog.html).

Afin d'obtenir une liste de commandes pour le plug-in {{site.data.keyword.openwhisk_short}}, exécutez `ic wsk` sans arguments.

## Utilisation de services à partir des actions
{: #binding_services}

{{site.data.keyword.openwhisk_short}} fournit la commande `service bind` pour mettre vos données d'identification du service {{site.data.keyword.Bluemix_notm}} à la disposition de votre code lors de l'exécution. La commande `service bind` peut ensuite être exploitée pour vous permettre de lier n'importe quel service {{site.data.keyword.Bluemix_notm}} à n'importe quelle action qui est définie dans {{site.data.keyword.openwhisk_short}}.

Pour obtenir des instructions détaillées sur la façon d'utiliser des services à partir des actions, voir la rubrique [Utilisation de services à partir des actions](./binding_services.html). 

## Configuration de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour l'utilisation d'un proxy HTTPS
{: #cli_https_proxy}

L'interface de ligne de commande {{site.data.keyword.openwhisk_short}} peut être configurée pour utiliser un proxy HTTPS. Pour configurer un proxy HTTPS, vous devez créer une variable d'environnement appelée `HTTPS_PROXY`. Celle-ci doit avoir pour valeur l'adresse du proxy HTTPS et son port définis au format suivant : `{IP PROXY}:{PORT PROXY}`.

## Gestion des régions, des organisations et des espaces
{: #region_info}

Vous pouvez créer des espaces pour gérer vos déploiements de préproduction et de production en créant des espaces pour chacun d'eux. Le fait de créer des espaces permet que deux noms d'espace différents soient définis pour vous par {{site.data.keyword.openwhisk_short}}. 

Vous pouvez utiliser la commande `ic iam space-create` pour créer d'autres espaces sous votre organisation, par exemple, "staging" et "production". Exemple :
```
ic iam space-create "staging"
ic iam space-create "production"
```

Pour plus d'informations sur la commande `ic iam`, voir la documentation suivante : https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} présente des restrictions relatives aux noms d'espace de nom. Pour en savoir plus sur ces restrictions, voir la documentation suivante : https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Si vous êtes déjà connecté, vous pouvez exécuter la commande `ic target` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} pour changer de région, d'organisation et d'espace. 

Utilisez la commande suivante pour afficher toutes les régions {{site.data.keyword.Bluemix_notm}} :

{{site.data.keyword.openwhisk_short}} est pris en charge uniquement dans les régions `us-south` et `eu-gb`.
{: tip}

```
ic regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Utilisez la commande `ic target` pour changer les régions. Par exemple, si vous souhaitez passer dans la région United Kingdom et dans l'espace `staging` :
```
ic target -r eu-gb -s staging
```
{: pre}


Si vous devez changer d'espace au sein d'une même région, par exemple, aller de staging à production, exécutez la commande suivante :
```
ic target -s production
```
{: pre}


Pour plus d'informations sur la commande `target`, utilisez `ic target --help` ou lisez la documentation suivante :
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migration depuis l'interface de ligne de commande OpenWhisk vers le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Avec l'introduction du plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, l'interface de ligne de commande OpenWhisk n'est plus nécessaire. 

### Syntaxe de commande
{: #command_syntax}

Toutes les commandes `wsk`, à l'exception de la commande `wsk bluemix login` devenue inutile, fonctionnement de la même manière en utilisant la commande `ic wsk`.  Toutes les options et tous les arguments de commande sont identiques. 

### Authentification et hôte d'API
{: #api_authentication}

Pour l'interface de ligne de commande OpenWhisk, vous devez configurer la clé d'API d'authentification et l'hôte d'API.
Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, vous n'avez pas besoin de configurer explicitement la clé d'API et l'hôte d'API. Vous devez vous connecter avec `ic login` et cibler votre région et votre espace de nom en utilisant la commande `ic target`. Après quoi, toutes les commandes suivantes commenceront par `ic wsk`. Ce type d'authentification permet également d'éviter d'accéder à la page Web {{site.data.keyword.Bluemix_notm}} afin d'obtenir des informations spécifiques pour configurer votre environnement d'interface de ligne de commande. 

Si vous devez vous procurer la clé d'API d'authentification afin d'utiliser {{site.data.keyword.openwhisk_short}} à partir d'un client HTTP externe, tel que cURL ou Postman, vous pouvez l'extraire à l'aide des commandes suivantes :

Pour obtenir la clé d'API en cours, exécutez la commande suivante :
```
ic wsk property get --auth
```
{: pre}


Pour obtenir l'hôte d'API en cours, exécutez la commande suivante :
```
ic wsk property get --apihost
```
{: pre}


La clé d'API est propre à chaque région, chaque organisation et chaque espace ciblés par le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}.
{: tip}

### Authentification de passerelle d'API
{: #apigw_authentication}

Actuellement, l'interface de ligne de commande OpenWhisk vous oblige à exécuter la commande `wsk bluemix login` dans le but de pouvoir configurer l'autorisation de passerelle d'API pour la gestion de vos API à l'aide de la commande `wsk api`. 

Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk}}, il n'est plus nécessaire d'exécuter la commande `wsk bluemix login`. A la place, utilisez la commande `ic login` pour vous connecter à {{site.data.keyword.Bluemix_notm}} ; le plug-in{{site.data.keyword.openwhisk}} utilisera automatiquement vos informations cible et de connexion actuelles. A présent, vous pouvez gérer vos API en utilisant la commande `ic wsk api`. 

### Migration des scripts de déploiement
{: #migrating_deploy_scripts}

Si vous avez des scripts qui utilisent l'interface de ligne de commande OpenWhisk avec le binaire `wsk`, toutes les commandes fonctionnent de la même manière en utilisant la commande `ic wsk`. Vous pouvez modifier vos scripts afin d'utiliser le plug-in d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ou vous pouvez créer un alias ou un encapsuleur de sorte que les exécutions en cours de `wsk` soient converties en `ic wsk`. Les commandes `ic login` et `ic target` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} fonctionnent en mode sans opérateur. Avec le mode sans opérateur, vous pouvez configurer votre environnement avant d'exécuter des commandes `ic wsk` pour déployer et gérer vos entités {{site.data.keyword.openwhisk_short}}. 


## Historique des versions
{: #version_history}

Enregistrement historique de versions qui montre les points forts et les correctifs de bogue. 

1.0.7 (2018-02-02)
* `ic wsk api` accepte désormais des paramètres de chemin, tels que `/api/{id}`. Pour plus d'informations, voir[Passerelle d'API](./openwhisk_apigateway.html). 
* Restaurez la prise en charge de proxy. 
* Retirez `swift:3`.

1.0.6 (2018-01-30)
* Bogue corrigé pour la commande `ic wsk service bind` pour les actions présentes dans un package.
