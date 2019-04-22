---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Contextes d’exécution

## Contextes d'exécution d'action
Les actions peuvent être codées et exécutées dans plusieurs langages de programmation (par exemple, Javascript, Python, etc.). Les environnements d'exécution disponibles sont présentés dans les sections suivantes. 

Les liens suivants renvoient une réponse JSON qui affiche les environnements d'exécution disponibles pour IBM Cloud Functions dans chaque région. 

La section `runtimes` de la réponse contient l'ensemble des contextes d'exécution disponibles. 

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [ eu-de ](https://eu-de.functions.cloud.ibm.com/)

La section `image` contient le nom de l'image du contexte d'exécution sur [DockerHub](https://hub.docker.com/) et de l'étiquette utilisée.  

Les exemples suivants pointent vers les images `ibmfunctions/action-nodejs-v10` et `openwhisk/nodejs6action`.
Les étiquettes peuvent être des numéros de version comme `1.9.0` ou la forme abrégée d'un hachage de validation de Git,
comme `b99d71e`. 

Exemples de zones d'image.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- Les contextes d'exécution d'action sont mis à jour régulièrement. Ces mises à jour incluent des correctifs de sécurité et des mises à jour de version mineures pour les packages dans les contextes d'exécution. Les mises à jour de version mineures peuvent introduire des ruptures de compatibilité avec les versions antérieures. Les mises à jour de contexte d'exécution peuvent avoir un impact sur vos actions. Il n'existe pas de migration automatique vers un contexte d'exécution plus récent du même type. 
- Les actions figurant dans des contextes d'exécution obsolètes ne peuvent pas être menées à bien correctement tant que le contexte d'exécution n'est pas mis à jour vers une version prise en charge. Lors du traitement des incidents d'une action défaillante, pour identifier si un contexte d'exécution est obsolète, recherchez `deprecated=true` dans la réponse de requête. Pour mettre à jour le contexte d'exécution, voir [Modification du contexte d'exécution d'action](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime). 

## Environnements d'exécution JavaScript
{: #openwhisk_ref_javascript_environments}

Les actions JavaScript peuvent être exécutées dans Node.js version 8 ou 10.  

Node.js version 6 est la version par défaut, mais est obsolète depuis le 6 décembre 2018. Pour continuer à utiliser une action JavaScript, effectuez une mise à jour vers Node.js version 8 ou 10.
{: deprecated}

### Environnement Node.js version 10 avec kits de développement IBM SDK
{: #openwhisk_ref_javascript_environments_10}
L'environnement Node.js version 10 est utilisé si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:10` lors de la création ou de la mise à jour d'une action. 

#### Migration de `nodejs:8` vers `nodejs:10`
- Le package du gestionnaire de package de noeud `ibm_db` n'est pas disponible dans `nodejs:10`. Le package `ibm_db` ne prend pas en charge Node.js 10. Vous pouvez suivre la progression de la résolution de ce problème dans [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541). 
- Le package du gestionnaire de package de noeud `cloudant` n'est pas disponible dans `nodejs:10`, le package est obsolète, vous devez utiliser le package du gestionnaire de package de noeud officiel [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 lors de l'importation du module nodejs (par exemple, `require('@cloudant/cloudant')`) et [v3.x renvoie uniquement Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x). 
- Le package du gestionnaire de package de noeud `cradle` n'est pas disponible dans `nodejs:10`. 
- Le package du gestionnaire de package de noeud `log4js` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre le problème à l'adresse [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- Le package du gestionnaire de package de noeud `watson-developer-cloud` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre la progression de la résolution de ce problème dans la nouvelle version à l'adresse [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Vous trouverez des informations détaillées sur l'environnement d'exécution nodejs version 10 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md). 

### Environnement Node.js version 8 avec kits de développement IBM SDK
{: #openwhisk_ref_javascript_environments_8}
L'environnement Node.js version 8 est utilisé si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:8` lors de la création ou de la mise à jour d'une action. 

Node.js version 8 est en mode maintenance et est disponible jusqu'à décembre 2019. Consultez le [planning des éditions Node.js](https://github.com/nodejs/Release).
{: deprecated}
 
Vous trouverez des informations détaillées sur l'environnement d'exécution Node.js version 8 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md). 

### Environnement Node.js version 6 (obsolète)
{: #openwhisk_ref_javascript_environments_6}
Node.js version 6 est la version par défaut, mais est obsolète. Pour continuer à utiliser une action JavaScript, effectuez une mise à jour vers Node.js version 8 ou 10.
{: deprecated}

Vous trouverez des informations détaillées sur l'environnement d'exécution nodejs version 6 dans [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md). 

## Environnements d'exécution Python
{: #openwhisk_ref_python_environments}

OpenWhisk prend en charge l'exécution d'actions Python sous deux versions d'environnement d'exécution différentes.

### Actions Python 3.7 (basées sur Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Les actions Python 3.7 sont exécutées avec Python 3.7.x. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python:3.7` de l'interface de ligne de commande `wsk` lorsque vous créez ou mettez à jour une action. 

L'environnement d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.7 standard.

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 3.7 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md). 

### Actions Python 3.6 (basées sur Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Les actions Python 3 sont exécutées avec Python 3.6.x. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python:3.6` de l'interface de ligne de commande `wsk` lorsque vous créez ou mettez à jour une action. 

L'environnement d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.6 standard.

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 3.6 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md). 

### Actions Python 2

Les actions Python 2 sont exécutées avec Python 2.7.15 sauf si vous spécifiez l'indicateur `--kind` lorsque vous créez ou mettez à jour une action. Pour sélectionner explicitement cet environnement, utilisez le paramètre `--kind python:2`.

Lors de la création d'actions Python à l'aide de virtualenv, utilisez l'image Docker `openwhisk/python2action`.
Les packages suivants sont disponibles pour leur utilisation dans des actions Python 2, en supplément de la bibliothèque Python 2.7 standard.

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 2 dans [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md). 

## Actions Swift
{: #swift-actions}

Les environnements d'exécution Swift 3.1.1 et 4.1 sont obsolètes depuis le 28 février 2019.
Démarrez de nouvelles actions ou migrez les actions existantes vers l'environnement d'exécution Swift 4.2 en utilisant le type `swift:4.2` et un nouveau processus de compilation.
{: tip}

### Swift 3
Les actions Swift 3 sont exécutées avec Swift 3.1.1 `--kind swift:3.1.1`. Spécifiez toujours le type `swift:3.1.1` car les versions précédentes de Swift ne sont pas prises en charge.

Vous devez faire migrer toutes les actions Swift pour utiliser le paramètre kind `swift:3.1.1`. La meilleure pratique consiste à indiquer systématiquement le paramètre kind spécifique lors de la création ou de mise à jour d'actions.
{: tip}

Les actions Swift 3.1.1 peuvent utiliser les packages suivants avec un fichier source Swift unique :
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- SDK Watson Developer Cloud version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Les actions Swift 4 peuvent être exécutées à l'aide de Swift 4.1 ou 4.2 qui utilisent respectivement `--kind swift:4.1` ou `--kind swift:4.2`.
La valeur par défaut `--kind swift:default` est Swift 4.2. 

Les contextes d'exécution d'action Swift 4.x n'incorporant aucun package, suivez les instructions pour que les [actions Swift conditionnées](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable) incluent les dépendances en utilisant Package.swift. 

Les actions Swift 4.1 peuvent utiliser les packages suivants avec un fichier source Swift unique :
- Watson Developer Cloud SDK version 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Les actions Swift 4.2 peuvent utiliser les packages suivants avec un fichier source Swift unique :
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Migration de Swift 3.1.1 vers Swift 4.1

#### SwiftyJSON utilisant un fichier d'action source unique
Si vous disposez d'une action `swift:3.1.1` non compilée et que vous utilisez le package **SwiftyJSON**, vous devez précompiler votre action et indiquer la version de SwiftyJSON que vous voulez utiliser pour l'action kind `swift:4.2`. Notez qu'à partir de Swift 4.1, des améliorations ont été apportées à la gestion des données JSON. 

## Actions PHP
{: #openwhisk_ref_php}

PHP 7.1 et 7.2 sont obsolètes depuis le 11 janvier 2019. Pour continuer à utiliser une action PHP, effectuez une mise à jour vers PHP 7.3.
{: deprecated}

Les actions PHP sont exécutées avec PHP 7.3.0. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind php:7.3` de l'interface de ligne de commande `wsk` lorsque vous créez ou mettez à jour une action. Il s'agit du comportement par défaut lorsque vous créez une action avec un fichier dont l'extension est `.php`.

Les environnements d'exécution PHP 7.1 et 7.2 sont obsolètes. Migrez toutes les actions vers PHP 7.3 pour améliorer le temps d'attente et accélérer les exécutions de bout en bout. 

Les extensions PHP suivantes sont disponibles en plus des extensions standard :

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Actions Docker
{: #openwhisk_ref_docker}

Les actions Docker exécutent un fichier binaire fourni par l'utilisateur dans un conteneur Docker. Le fichier binaire s'exécute dans une image Docker reposant sur [python:3.6-alpine](https://hub.docker.com/r/library/python) ; par conséquent, il doit être compatible avec cette distribution. 

Le squelette Docker est pratique pour générer des images Docker compatibles avec OpenWhisk. Vous pouvez l'installer avec la commande de plug-in d'interface de ligne de commande `ibmcloud fn sdk install docker`. 

Le programme binaire principal doit se trouver dans `/action/exec` à l'intérieur du conteneur. L'exécutable reçoit les arguments d'entrée d'une chaîne d'arguments de ligne de commande qui peut être désérialisée en tant qu'objet `JSON`. Il doit renvoyer un résultat en utilisant `stdout` sous forme de chaîne comportant une seule ligne de code `JSON` sérialisé.

Vous pouvez inclure des étapes de compilation ou des dépendances en modifiant le document `Dockerfile` inclus dans `dockerSkeleton`.
