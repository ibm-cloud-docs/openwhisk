---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Configuration du plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} offre un plug-in puissant pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} qui permet une gestion complète du système {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Vous pouvez désormais utiliser l'alias `fn` dans vos commandes de plug-in {{site.data.keyword.openwhisk_short}} : `ibmcloud fn <command>`
{: tip}

## Configuration de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Téléchargez et installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} et connectez-vous.
{: shortdesc}

1. Téléchargez et installez l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. ](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)

2. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.openwhisk_short}} est disponible dans les régions {{site.data.keyword.Bluemix_notm}} Sud des Etats-Unis et Royaume-Uni. Si le noeud final d'API {{site.data.keyword.Bluemix_notm}} n'est pas spécifié, indiquez-en un avec l'indicateur `-a`.

    * Pour vous connecter à la région Sud des Etats-Unis :
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * Pour vous connecter à la région Royaume-Uni :
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  Vous pouvez utiliser la commande `ibmcloud api` pour définir de manière explicite le noeud final de l'API {{site.data.keyword.Bluemix_notm}}. Pour afficher le paramètre de noeud final d'API, utilisez la commande `ibmcloud target`.
  {: tip}

3. La commande `ibmcloud login` vous invite à saisir des informations, telles que l'organisation, l'espace et le mot de passe, si elles ne sont pas spécifiées.

  Vous pouvez indiquer l'organisation et l'espace à la connexion pour éviter d'avoir à répondre aux invites. Pour cela, utilisez les indicateurs suivants : `ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

Vous pouvez également utiliser une clé d'API {{site.data.keyword.Bluemix_notm}} pour vous connecter. Cette méthode s'avère pratique lorsque votre compte est configuré avec une connexion fédérée qui nécessite une connexion avec l'indicateur `--sso`. [L'utilisation d'une clé d'API](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) est également avantageuse si vous souhaitez définir une intégration continue et configurer un pipeline automatisé.

1. Créez une nouvelle clé d'API.
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. Utilisez la valeur générée de la clé d'API pour vous connecter.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
Pour plus d'informations sur la commande `ibmcloud login`, utilisez `ibmcloud login --help` ou consultez la rubrique [Commandes IBM Cloud (bx)](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login).

## Configuration du plug-in {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Téléchargez et installez le plug-in {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

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

3. Effectuez un appel (synchrone) bloquant de `echo`. Indiquez `hello` comme argument.
    ```
    ibmcloud fn action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Vérifiez que le message `hello` est renvoyé dans la sortie.
    Exemple de sortie :
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. Vous pouvez mettre à niveau le plug-in {{site.data.keyword.openwhisk_short}} en exécutant la commande suivante :
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour :

* Exécuter vos fragments de code, ou vos actions, dans {{site.data.keyword.openwhisk_short}}. Voir [Création et appel d'actions](./openwhisk_actions.html).
* Utiliser des déclencheurs et des règles pour que vos actions puissent répondre à des événements. Voir [Création de déclencheurs et de règles](./openwhisk_triggers_rules.html).
* Découvrir comment les packages regroupent des actions et configurer des sources d'événements externes. Voir [Création et utilisation de packages](./openwhisk_packages.html).
* Explorer le catalogue de packages et améliorer vos applications avec des services externes, par exemple une [source d'événement {{site.data.keyword.cloudant}}](./openwhisk_cloudant.html).

Pour afficher la liste des commandes du plug-in {{site.data.keyword.openwhisk_short}}, exécutez la commande `ibmcloud fn` sans arguments.
{: tip}

## Utilisation de services à partir d'actions
{: #binding_services}

{{site.data.keyword.openwhisk_short}} fournit la commande `service bind` pour mettre vos données d'identification du service {{site.data.keyword.Bluemix_notm}} à la disposition de votre code lors de l'exécution. Vous pouvez ensuite utiliser la commande `service bind` pour lier un service {{site.data.keyword.Bluemix_notm}} à une action définie dans {{site.data.keyword.openwhisk_short}}.

Pour obtenir la procédure détaillée de l'utilisation des services à partir d'actions, voir [Liaisons de service à des actions](./binding_services.html).

## Configuration de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour utiliser un proxy HTTPS
{: #cli_https_proxy}

L'interface de ligne de commande {{site.data.keyword.openwhisk_short}} peut être configurée pour utiliser un proxy HTTPS. Pour configurer un proxy HTTPS, vous devez créer une variable d'environnement appelée `HTTPS_PROXY`. Celle-ci doit avoir pour valeur l'adresse du proxy HTTPS et son port définis au format suivant : `{IP PROXY}:{PORT PROXY}`.

## Changement de régions, d'organisations et d'espaces
{: #region_info}

Si vous êtes déjà connecté, vous pouvez exécuter la commande `ibmcloud target` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} pour changer de régions, d'organisations et d'espaces.

{{site.data.keyword.openwhisk_short}} est disponible dans les régions {{site.data.keyword.Bluemix_notm}} Sud des Etats-Unis et Royaume-Uni. Pour changer de région, utilisez la commande `ibmcloud target`. Par exemple, pour passer à la région Royaume-Uni et à l'espace `staging` figurant dans cette région, exécutez :
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

Vous pouvez créer des espaces pour gérer vos déploiements de préproduction et de production en créant des espaces pour chacun d'eux. Le fait de créer des espaces permet que deux noms d'espace différents soient définis pour vous par {{site.data.keyword.openwhisk_short}}. Exécutez la commande [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) pour créer d'autres espaces sous votre organisation, par exemple "staging" et "production" :

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} fait l'objet de restrictions en matière de noms d'espace de nom. Pour plus d'informations, consultez la documentation sur les [limites et détails du système](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities).
{: tip}

## Migration depuis l'interface de ligne de commande OpenWhisk vers le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Avec l'introduction du plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, l'interface de ligne de commande (`wsk`) autonome n'est plus nécessaire.

### Syntaxe de commande
{: #command_syntax}

Toutes les commandes `wsk`, à l'exception de la commande `wsk bluemix login` devenue inutile, fonctionnent de la même façon en utilisant la commande `ibmcloud fn`.  Toutes les options et tous les arguments de commande sont identiques.

### Authentification et hôte d'API
{: #api_authentication}

Avec l'interface de ligne de commande OpenWhisk, vous deviez configurer la clé d'API d'authentification et l'hôte d'API.
Avec le plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, vous n'avez plus besoin de configurer de manière explicite la clé d'API et l'hôte d'API. A la place, vous pouvez vous connecter avec `ibmcloud login` et cibler votre région et votre espace de nom en utilisant la commande `ibmcloud target`. Après la connexion, toutes les commandes commencent par `ibmcloud fn`.

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

## Historique des versions
{: #version_history}

Enregistrement historique des versions qui montre les points forts et les correctifs de bogue.

1.0.21 (2018-08-01)
* Les alias `fn` et `functions` peuvent désormais être utilisés pour les commandes {{site.data.keyword.openwhisk_short}} : `ibmcloud fn <command>` et `ibmcloud fn <command>`. Vous pouvez également encore utiliser `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Correctifs de bogues mineurs et améliorations.

1.0.18 (20/06/2018)
* Correctif permettant de supprimer la liaison des instances de service fournies par l'utilisateur.
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
* `ibmcloud wsk api` accepte désormais des paramètres de chemin de type `/api/{id}`. Pour plus d'informations, voir [Passerelle d'API](./openwhisk_apigateway.html).
* Rétablissement de la prise en charge de proxy.
* Retrait de `swift:3`.

1.0.6 (30/01/2018)
* Correctif de bogue pour la commande `ibmcloud wsk service bind` concernant les actions présentes dans un package.
