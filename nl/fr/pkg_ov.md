---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: packages, installable packages

subcollection: cloud-functions

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

# Incorporation de packages
{: #pkg_ov}

Les packages sont des ensembles regroupant des actions et des flux. Chaque package est conçu pour une interaction spécifique avec des services et des fournisseurs d'événements. Certains packages sont déjà installés avec {{site.data.keyword.openwhisk}} et prêts à l'emploi mais vous pouvez également en installer d'autres.
{: shortdesc}

## Présentation
{: #pkg_overview}

Les [packages préinstallés](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) sont automatiquement enregistrés au sein de {{site.data.keyword.openwhisk_short}} dans l'espace de nom `/whisk.system`. Vous pouvez les utiliser sans effectuer d'étapes d'installation.

Les packages installables sont des packages à votre disposition que vous pouvez installer, modifier et utiliser selon vos besoins. Les packages installables ne se trouvent pas au sein du système {{site.data.keyword.openwhisk_short}}. Ils sont hébergés en externe dans des référentiels Github individuels.

Vous pouvez installer directement ces packages ou les vôtres dans votre espace de nom et attribuer un nom personnalisé à un package. Le package étant installé dans votre propre espace de nom, vous pouvez modifier ses actions et ses flux autant que nécessaire.



## Exploration des packages préinstallés
{: #pkg_browse}

Plusieurs packages sont déjà enregistrés pour vous avec {{site.data.keyword.openwhisk_short}}. Vous pouvez obtenir la liste des packages qui se trouvent dans un espace de nom, répertorier les entités qui se trouvent dans un package, et obtenir la description de chaque entité d'un package.
{: shortdesc}

1. Obtenez la liste des packages qui se trouvent dans l'espace de nom `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Liste de packages obtenue :
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Obtenez la liste d'entités d'un package.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemple :
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Cette sortie indique que le package {{site.data.keyword.cloudant_short_notm}} comprend des actions et un flux. Par exemple, les actions `read` et `write`, ainsi qu'un flux déclencheur nommé `changes`. Le flux `changes` entraîne l'exécution de déclencheurs en cas d'ajout de documents dans la base de données {{site.data.keyword.cloudant_short_notm}} spécifiée.

  Le package {{site.data.keyword.cloudant_short_notm}} définit également les paramètres `username`, `password`, `host` et `port`. Ceux-ci doivent être spécifiés pour que les actions et les flux aient un sens. Par exemple, les paramètres autorisent les actions sur un compte {{site.data.keyword.cloudant_short_notm}} spécifique.

3. Obtenez une description d'une action ou d'un flux pour voir les paramètres nécessaires.

  Exemple :
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Exemple de sortie :
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Cette sortie indique que l'action {{site.data.keyword.cloudant_short_notm}} `read` requiert trois paramètres, y compris l'ID de la base de données et du document à récupérer.



## Liaison de paramètres à des packages préinstallés
{: #pkg_bind}

Vous pouvez utiliser les entités d'un package directement, mais il se peut que vous transmettiez les mêmes paramètres à l'action à chaque fois. Vous pouvez simplifier le processus en effectuant la liaison à un package et en indiquant les paramètres par défaut, qui sont hérités par les actions dans le package.
{: shortdesc}

Par exemple, dans le package `/whisk.system/cloudant`, vous pouvez définir les valeurs par défaut `username`, `password` et `dbname` dans une liaison de package ; elles seront transmises automatiquement aux actions du package.

Dans l'exemple suivant, vous effectuez une liaison avec le package `/whisk.system/samples`.

1. Etablissez la liaison au package `/whisk.system/samples` et définissez une valeur de paramètre `place` par défaut.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Obtenez une description de la liaison de package.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Remarquez que toutes les actions dans le package `/whisk.system/samples` sont disponibles dans la liaison de package `valhallaSamples`.

3. Appelez une action dans la liaison de package.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Dans le résultat, vous constatez que l'action hérite du paramètre `place` que vous avez défini lorsque vous avez créé la liaison de package `valhallaSamples`.

4. Appelez une action et remplacez la valeur de paramètre par défaut.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Remarquez que la valeur de paramètre `place` qui est spécifiée avec l'appel d'action remplace la valeur par défaut définie dans la liaison de package `valhallaSamples`.






## Ajout de vos propres packages
{: #pkg_add}

Vous pouvez créer un package de code local ou un clone de n'importe quel référentiel Github.
{: shortdesc}

Avant de commencer :
- [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).
- Créez un fichier `manifest.yaml` ou `manifest.yml` pour votre application et stockez-le dans le répertoire racine. Le fichier `manifest.yaml` spécifie la structure globale du package, y compris les métadonnées à inclure avec la commande `ibmcloud fn deploy`. Pour en savoir plus sur les fichiers `manifest.yaml`, consultez la [documentation wskdeploy![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).

Pour ajouter un package :

1. Clonez le référentiel de package.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Accédez au répertoire contenant le fichier `manifest.yaml`.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Déployez le package.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Certains packages nécessitent des variables d'environnement particulières pour fonctionner correctement. Dans ce cas, incluez ces variables avec la commande `deploy`. Par exemple, vous pouvez choisir un nom pour le package et l'indiquer avec la variable PACKAGE_NAME.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Exemple d'utilisation du package {{site.data.keyword.cos_full_notm}}
{: #pkg_ex}

Pour voir un exemple d'installation d'un package, examinez le [package {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). {{site.data.keyword.cos_full}} est un service qui permet aux utilisateurs de stocker tous les types de fichiers (images, vidéos, musique et texte). Pour interagir avec les fichiers, un magasin de données de paires clé/valeur basé sur le cloud est stocké dans un compartiment. Ainsi, pour utiliser le [package {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage), vous devez d'abord créer une instance de service {{site.data.keyword.cos_full_notm}} et créer ensuite un compartiment. Le compartiment est utilisé en tant que variable d'environnement nécessaire à l'installation de ce package.

Une fois l'instance de service et le compartiment créés, les commandes suivantes sont nécessaires pour installer le package :

1. Clonez le référentiel de package.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Accédez au répertoire de package contenant le fichier `manifest.yaml`. Dans cet exemple, la version d'exécution Node.js du package {{site.data.keyword.cos_full_notm}} est utilisée.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Déployez le package, en utilisant votre compartiment comme variable d'environnement. La dépendance sur la variable d'environnement `PACKAGE_NAME` vous permet de donner un nom personnalisé à ce package.
    ```
    PACKAGE_NAME=<nom_package_personnalisé> BUCKET=<nom_compartiment> ibmcloud fn deploy
    ```
    {: pre}

