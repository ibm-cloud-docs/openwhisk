---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# Utilisation de packages installables
{: #installable-packages-overview}

Les packages installables représentent une nouvelle conception dans la gestion et l'interaction avec les packages au sein de {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Présentation
{: #overview}

**Que sont les packages installables ?**

Les packages sont des ensembles regroupant des actions et des flux. Chaque package est conçu pour une interaction spécifique avec des services et des fournisseurs d'événements. Les packages installables sont des packages qui sont disponibles pour vous permettre de choisir, d'installer et de modifier en fonction de vos besoins.

**En quoi les packages installables sont-ils différents des packages préinstallés ?**

Les [packages préinstallés](openwhisk_packages.html#browse-packages) sont automatiquement enregistrés au sein de {{site.data.keyword.openwhisk_short}} dans l'espace de nom `/whisk.system`. Pour stocker les données d'identification ou d'autres paramètres dans un package préinstallé, vous devez créer des [liaisons de package](openwhisk_packages.html#openwhisk_package_bind).

Les packages installables ne se trouvent pas au sein du système {{site.data.keyword.openwhisk_short}}. Ils sont hébergés en externe dans des référentiels Github individuels. Vous pouvez installer ces packages directement dans votre propre espace de nom à l'aide de l'outil [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) et donner un nom personnalisé à chaque package. Le package étant installé dans votre propre espace de nom, vous pouvez modifier ses actions et ses flux autant que nécessaire.

## Installation de packages à l'aide de wskDeploy
{: #installing}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installez la commande [`wskdeploy`![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) et ajoutez le fichier binaire téléchargé à votre variable PATH.

Pour installer un package :

1. Clonez le référentiel de package. Les référentiels de package sont accessibles sur des pages individuelles pour chaque package dans cet ensemble de documentations.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Accédez au répertoire de package contenant un fichier `manifest.yaml` ou `manifest.yml`. Le fichier `manifest.yaml` indique la structure globale du package, y compris le package et les actions à installer dans votre espace de nom ainsi que les métadonnées qui doivent être incluses avec la commande `wskdeploy`. Pour en savoir plus sur les fichiers `manifest.yaml`, consultez la [documentation wskdeploy![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Déployez le package. Certains packages nécessitent des variables d'environnement particulières pour fonctionner correctement.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### Exemple d'utilisation du package {{site.data.keyword.cos_full_notm}}
{: #example}

Pour voir un exemple d'installation d'un package, consultez la rubrique relative au [package {{site.data.keyword.cos_short}} ](cloud_object_storage_actions.html). {{site.data.keyword.cos_full}} est un service qui permet aux utilisateurs de stocker tous les types de fichiers (images, vidéos, musique et texte). Pour interagir avec les fichiers, un magasin de données de paires clé/valeur basé sur le cloud est stocké dans un compartiment. Ainsi, pour utiliser le package [{{site.data.keyword.cos_short}}](cloud_object_storage_actions.html), vous devez d'abord créer une instance de service {{site.data.keyword.cos_short}}, puis un compartiment. Le compartiment est utilisé en tant que variable d'environnement nécessaire à l'installation de ce package.

Une fois l'instance de service et le compartiment créés, les commandes suivantes sont nécessaires pour installer le package :

1. Clonez le référentiel de package.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Accédez au répertoire de package contenant le fichier `manifest.yaml`. Dans cet exemple, la version d'exécution Node.js du package {{site.data.keyword.cos_short}} est utilisée.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Déployez le package, en utilisant votre compartiment comme variable d'environnement. La dépendance sur la variable d'environnement `PACKAGE_NAME` vous permet de donner un nom personnalisé à ce package.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> wskdeploy
    ```
    {: pre}
