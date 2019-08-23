---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

Vous pouvez étendre les fonctionnalités de votre application {{site.data.keyword.openwhisk}} en l'intégrant à une instance {{site.data.keyword.cos_full}}.

**Avant de commencer**
Pour en savoir plus sur {{site.data.keyword.cos_full_notm}}, voir [A propos d'Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Pour plus d'informations sur la configuration de l'instance {{site.data.keyword.cos_full_notm}}, voir [Mise à disposition d'une instance {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Packages
{: #obstorage_packages}
| Package | Disponibilité | Description |
| --- | --- | --- |
| [Package {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_install)| Installable | Lecture, écriture et suppression depuis une instance {{site.data.keyword.cos_full_notm}}. |
| [Source d'événements {{site.data.keyword.cos_full_notm}} (expérimental)](#pkg_obstorage_ev) | Préinstallé (Sud des Etats-Unis uniquement) | Ecoute des modifications apportées à une instance {{site.data.keyword.cos_full_notm}}. |

## Création d'une instance de service IBM Cloud Object Storage
{: #pkg_obstorage_service}

Avant d'utiliser l'un de ces packages, vous devez demander une instance d'{{site.data.keyword.cos_full_notm}} et créer au moins un compartiment.

1. [Créez une instance de service {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Créez un ensemble de données d'identification du service HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) pour l'instance de service {{site.data.keyword.cos_full_notm}}. Dans la zone **Ajouter des paramètres de configuration en ligne (facultatif)**, ajoutez `{"HMAC":true}`.

3. [Créez au moins un compartiment](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Installation du package {{site.data.keyword.cos_full_notm}} depuis l'interface CLI {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_install}

Dès que vous disposez d'une instance de service {{site.data.keyword.cos_full_notm}}, vous pouvez utiliser l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.cos_full_notm}} dans votre espace de nom.
{: shortdesc}
{: #pkg_obstorage_cli}

Le package {{site.data.keyword.cos_full_notm}} installable déploie un ensemble d'actions que vous pouvez utiliser pour travailler avec votre instance {{site.data.keyword.cos_full_notm}}. Ces actions sont exécutées en Node.js ou Python. Une fois le package installé, vous pouvez sélectionner un environnement d'exécution. Pour obtenir la liste des actions dans le package `cloud-object-storage`, voir [Entités disponibles](#pkg_obstorage_actions).
{: note}

**Avant de commencer**

[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.cos_full_notm}} :

1. Clonez le référentiel de package {{site.data.keyword.cos_full_notm}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Accédez au répertoire `runtimes/nodejs` ou `runtimes/python` pour sélectionner un environnement d'exécution pour les actions du package.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Déployez le package `cloud-object-storage`.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Réponse**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. Vérifiez que le package `cloud-object-storage` est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Sortie**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Liez les données d'identification de l'instance {{site.data.keyword.cos_full_notm}} que vous avez créée au package. Vous pouvez inclure l'indicateur `--keyname` pour lier des données d'identification du service spécifiques. Pour plus d'informations sur la liaison de services, voir [Commandes de service](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service).

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Exemple de sortie**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Paramètres de liaison

Vous pouvez utiliser la commande [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) pour lier le noeud final d'un compartiment à une action spécifique ou au package `cloud-object-storage`. Remplacez `<bucket_endpoint>` par le noeud final de votre compartiment. 

Lorsque vous mettez à jour des paramètres pour un package, une action ou un déclencheur, vous devez spécifier tous les paramètres précédemment créés. Sinon, les paramètres précédemment créés sont retirés. Tous les services qui étaient liés au package sont également retirés. Par conséquent, après avoir mis à jour d'autres paramètres, vous devez à nouveau [lier les services](/docs/openwhisk?topic=cloud-functions-services) à votre package.
{: important}

**Lier des paramètres à toutes les actions d'un package**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Lier des paramètres à une action spécifique**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

Vous pouvez également lier des paramètres à des actions en utilisant l'onglet **Paramètres** de l'interface utilisateur. Pour ajouter des paramètres dans l'interface utilisateur, accédez à la [page **Actions**](https://cloud.ibm.com/openwhisk/actions){: external} et cliquez sur l'une de vos actions. Cliquez ensuite sur **Paramètres** > **Ajouter un paramètre**. Vous devez ajouter des paramètres dans les paires `<key>` et `<value>`.
{: tip}


## Installation du package {{site.data.keyword.cos_full_notm}} depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer](https://cloud.ibm.com/openwhisk/create){: external}.

2. Sélectionnez l'espace de nom dans lequel vous souhaitez installer le package {{site.data.keyword.cos_full_notm}} via le menu déroulant espace de nom.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM Cloud Object Storage**, puis cliquez sur le package **IBM Cloud Object Storage**.

5. Dans la section **Contextes d'exécution disponibles**, sélectionnez `Node.JS` ou `Python` dans la liste déroulante. Cliquez ensuite sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page **Actions** et pouvez rechercher votre nouveau package, nommé `cloud-object-storage`.

7. Pour utiliser les actions du package `cloud-object-storage`, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez l'étape 5 des [instructions de l'interface de ligne de commande](#pkg_obstorage_cli).
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. 
  
Si vous liez le service de vos données d'identification de service à des actions individuelles, vous devez exécuter la procédure suivante pour chaque action que vous souhaitez utiliser.
{: note}

1. Cliquez sur une action du package `cloud-object-storage` que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
2. Dans le volet de navigation de gauche, cliquez sur **Paramètres**.
3. Entrez un nouveau paramètre. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.


Pour plus d'informations sur les paramètres de liaison, voir [Liaison de paramètres à des packages](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params).


## Entités disponibles
{: #pkg_obstorage_actions}

Le package {{site.data.keyword.cos_full_notm}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Package | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Gestion d'une instance {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | Action | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Ecriture d'un objet dans un compartiment. |
| `/cloud-object-storage/object-read` | Action | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Lecture d'un objet d'un compartiment. |
| `/cloud-object-storage/object-delete` | Action | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Suppression d'un objet d'un compartiment. |
| `/cloud-object-storage/bucket-cors-put` | Action | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Affectation d'une configuration CORS à un compartiment. |
| `/cloud-object-storage/bucket-cors-get` | Action | `bucket`, `endpoint`, `ibmAuthEndpoint` | Lecture de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/bucket-cors-delete` | Action | `bucket`, `endpoint`, `ibmAuthEndpoint` | Suppression de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/client-get-signed-url` | Action | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Obtention d'une URL signée pour restreindre l'écriture, la lecture et la suppression d'un objet d'un compartiment. |

Pour obtenir la liste complète des entités disponibles, exécutez `ibmcloud fn package get cloud-object-storage`.
{: note}

### Paramètres de package
{: #pkg_obstorage_pkgparams}

Les paramètres de package suivants sont censés être liés au package et sont automatiquement disponibles pour toutes les actions. Il est également possible de spécifier ces paramètres lorsque vous appelez l'une des actions. 

| Paramètre de package | Description |
| --- | --- |
| `apikey` | Le paramètre `apikey` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_full_notm}}. |
| `resource_instance_id` | Le paramètre `resource_instance_id` est l'identificateur d'instance {{site.data.keyword.cos_full_notm}}. |
| `cos_hmac_keys` | Le paramètre `cos_hmac_keys` correspond aux donnée d'identification HMAc de l'instance {{site.data.keyword.cos_full_notm}}, et inclut les valeurs `access_key_id` et `secret_access_key`.  Ces données d'identification sont utilisées exclusivement par l'action `client-get-signed-url`.  Consultez la section [Utilisation des données d'identification HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) pour des instructions sur la génération de donnée d'identification HMAC pour votre instance {{site.data.keyword.cos_full_notm}}. |
 
### Paramètres d'action
{: #pkg_obstorage_actparams}

Les paramètres d'action suivants sont spécifiés lorsque vous appelez les actions individuelles. Tous ces paramètres ne sont pas pris en charge par chaque action. Reportez-vous au tableau [Entités disponibles](#pkg_obstorage_actions) pour voir quels paramètres sont pris en charge par quelle action.

| Paramètre d'action | Description |
| --- | --- |
| `bucket` | Le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | Le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_full_notm}}. Vous pouvez localiser votre noeud final dans la [documentation {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | Le paramètre `expires` correspond au nombre de secondes avant expiration de l'opération d'URL pré-signée. La valeur par défaut pour `expires` est de 15 minutes. |
| `ibmAuthEndpoint` | Le paramètre `ibmAuthEndpoint` est le noeud final d'autorisation IBM Cloud utilisé par {site.data.keyword.cos_short}} pour générer un jeton à partir du paramètre `apikey`. Le noeud final d'autorisation par défaut fonctionne pour toutes les régions IBM Cloud. |
| `key` | Le paramètre `key` est la clé d'objet du compartiment. |
| `operation` | Le paramètre `operation` correspond à l'opération de l'URL présignée à appeler. |
| `corsConfig` | Le paramètre `corsConfig` est une configuration CORS d'un compartiment. |


## Ecriture d'objets dans un compartiment
{: #pkg_obstorage_write}

Vous pouvez utiliser l'action `object-write` pour écrire un objet dans un compartiment {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Dans la procédure suivante, le nom `test-bucket` est utilisé comme exemple. Les compartiments d'{{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `test-bucket` par un nom de compartiment unique.
{: note}

### Ecriture d'un objet dans un compartiment via l'interface de ligne de commande
{: #pkg_obstorage_write_cli}
Ecrivez un objet dans votre compartiment à l'aide de l'action `object-write`.
{: shortdesc}


Appelez l'action `object-write` afin d'écrire un objet dans votre compartiment. Si vous avez lié votre noeud final de compartiment à votre package ou à l'action `object-write`, vous n'avez pas besoin d'inclure le noeud final en tant que paramètre. Remplacez `<org_space>` par le nom de vos organisation Cloud Foundry et espace, `<test-bucket>` par le nom de votre compartiment et `<test.txt>` par le nom de l'objet que vous souhaitez écrire.

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Exemple de sortie**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Ecriture d'un objet dans un compartiment via l'interface utilisateur
{: #pkg_obstorage_write_ui}


1. Accédez à la [page Actions](https://cloud.ibm.com/openwhisk/actions){: external} sur la console {{site.data.keyword.openwhisk_short}}.

2. Sous le package `cloud-object-storage`, cliquez sur l'action **object-write**.

3. Dans le volet Code, cliquez sur **Changer l'entrée**.

4. Entrez un objet JSON contenant votre compartiment, ainsi que les clés d'objet key et body.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
    }
    ```
    {: pre}

5. Cliquez sur **Sauvegarder**.

6. Cliquez sur **Appeler**.

7. Vérifiez que la sortie ressemble à ce qui suit :
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Lecture d'objets depuis un compartiment
{: #pkg_obstorage_read}

Vous pouvez utiliser l'action `object-read` pour lire depuis un objet dans un compartiment {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Dans la procédure suivante, le nom `test-bucket` est utilisé comme exemple. Les compartiments d'{{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `test-bucket` par un nom de compartiment unique.
{: note}

### Lecture d'un objet depuis un compartiment via l'interface de ligne de commande
{: #pkg_obstorage_read_cli}

Lisez depuis un objet dans votre compartiment à l'aide de l'action `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Exemple de sortie**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Lecture d'un objet depuis un compartiment via l'interface utilisateur
{: #pkg_obstorage_read_ui}

1. Accédez à la [page Actions](https://cloud.ibm.com/openwhisk/actions){: external}.

2. Sous le package `cloud-object-storage`, cliquez sur l'action `object-read`.

3. Dans le volet Code, cliquez sur **Changer l'entrée**.

4. Entrez un objet JSON contenant votre compartiment, ainsi que les clés d'objet key.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
    }
    ```
    {: pre}

5. Cliquez sur **Sauvegarder**.

6. Cliquez sur **Appeler**.

7. Vérifiez que la sortie ressemble à ce qui suit :
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Ecoute des modifications apportées à un compartiment via la source d'événements Object Storage (Expérimental)
{: #pkg_obstorage_ev}

Le package `/whisk.system/cos-experimental` risque d'être instable, de changer fréquemment d'une façon non compatible avec les versions antérieures et peut être retiré dans un délai court. Ce package n'est pas recommandé pour une utilisation dans des environnements de production. Ce package expérimental est disponible uniquement dans la région Sud des Etats-Unis.
{: important}

Vous pouvez utiliser {{site.data.keyword.openwhisk}} pour écouter les modifications apportées à un compartiment [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) et utiliser une action pour traiter un ou plusieurs objets du compartiment.

<br>

**Exemple de cas d'utilisation :** Le package `/whisk.system/cos-experimental` permet d'écouter les modifications apportées aux données de rue GPS stockées dans un compartiment {{site.data.keyword.cos_full_notm}}. Ensuite, lorsque des modifications ont lieu, vous pouvez déclencher la régénération automatique d'une carte GPS, afin que les utilisateurs puissent avoir accès aux données de rue les plus récentes pour leur application GPS. 

### Paramètres de la source d'événements Object Storage (expérimental)
{: #pkg_obstorage_ev_ch}

Le package `/whisk.system/cos-experimental` permet de configurer des événements à partir d'une instance {{site.data.keyword.cos_full_notm}} et inclut le flux suivant :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Package | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Package contenant l'action de flux `changes`. |
| `/whisk.system/cos-experimental/changes` | Flux | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Exécute des événements déclencheurs en cas de modifications effectuées dans un compartiment {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Vous pouvez utiliser le flux `changes` pour configurer le service de source d'événements {{site.data.keyword.cos_full_notm}} afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à un compartiment dans votre instance {{site.data.keyword.cos_full_notm}}.

Paramètres utilisés dans cet exemple :

| Paramètre | Description |
| --- | --- |
| `apikey` | (Obligatoire, sauf si lié au package) Le paramètre `apikey` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_full_notm}}.  Normalement, cette valeur est liée au package. Toutefois, si la valeur `apikey` est spécifiée lors de l'utilisation de l'action de flux `changes`, la valeur indiquée est utilisée pour les données d'identification au lieu de la clé d'API des données d'identification liées. |
| ` auth_endpoint` |(Facultatif) Le paramètre `auth_endpoint` est le noeud final d'autorisation utilisé par {{site.data.keyword.cos_full_notm}} pour générer un jeton à partir du paramètre `apikey`. Le noeud final par défaut est le noeud final {{site.data.keyword.cloud}}. |
| `bucket` | (Obligatoire)  Le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | (Obligatoire) Le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_full_notm}}. Vous pouvez localiser votre noeud final dans la [documentation {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` |(Facultatif) Le paramètre `interval` est l'intervalle d'interrogation du compartiment, en minutes entières. La valeur `interval` doit être d'au moins 1 minute, qui est la valeur par défaut. |

## Création d'un déclencheur pour répondre au flux changes
{: #pkg_obstorage_ev_trig}

Lors de la création du déclencheur, vous pouvez éviter de transmettre vos données d'identification {{site.data.keyword.cos_full_notm}} à l'action de flux `changes` en liant vos données d'identification directement au package `cos-experimental`.
{: shortdesc}

 1. Créez d'abord une liaison de package qui peut être modifiée pour contenir vos données d'identification. La commande suivante crée une liaison de package `myCosPkg` dans votre espace de nom.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Liez vos données d'identification {{site.data.keyword.cos_full_notm}} au package. La liaison de vos données d'identification {{site.data.keyword.cos_full_notm}} au package associe la valeur `apikey` au package, de sorte que vous n'avez pas besoin de spécifier la valeur `apikey` lors de l'appel de l'action de flux `changes`. 

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Créez un déclencheur nommé `cosTrigger` avec le flux `changes` dans la liaison de package que vous avez créée. Utilisez les valeurs de paramètre de votre nom de compartiment et de votre noeud final {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Exemple de sortie**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

Créez une action pour vérifier que le déclencheur, le flux de modifications et la règle sont tous configurés et fonctionnent correctement.
 
  1. Sauvegardez le code JavaScript suivant en tant que `cosChange.js`. 

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

  2. Créez une action appelée `cosChange` en utilisant le code `cosChange.js`.

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. Créez une règle pour connecter l'action `cosChange` au déclencheur `cosTrigger`. 

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. Dans une fenêtre distincte, commencez à interroger les activations pour obtenir une bonne visibilité de ce qui se passe. Lorsque le déclencheur est exécuté, la commande affiche les enregistrements d'activation pour chacune de ces opérations, à mesure qu'elles se produisent.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. Dans votre tableau de bord {{site.data.keyword.cos_full_notm}}, modifiez un objet de compartiment existant ou créez-en un. Pour savoir comment ajouter un objet à votre compartiment, voir [Ajout d'objets à votre compartiment](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
  6. Pour chaque modification d'objet de compartiment, observez les nouvelles activations pour le déclencheur `cosTrigger` et l'action `cosChange`. Vous pouvez afficher ces activations en exécutant la commande `ibmcloud fn activation poll` dans l'intervalle d'interrogation de compartiment configuré.
  
  7. Si vous ne parvenez pas à observer de nouvelles activations, vérifiez que les valeurs des paramètres `apikey`, `endpoint` et `bucket` sont correctes. 
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Structure de données d'une activation de déclencheur Object Storage
{: #pkg_obstorage_ev_data}

Le contenu des événements générés possède les paramètres suivants :

| Paramètre | Description |
| --- | --- |
| `file` | Métadonnées du fichier ou de l'objet. Cette structure est décrite dans la section d'[affichage de la liste des objets dans un compartiment spécifique](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets). |
| `status` | Modification détectée. Cette valeur peut être `added`, `modified` ou `deleted`. |
| `bucket`| Nom du compartiment {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | Noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à l'instance {{site.data.keyword.cos_full_notm}}. |
| `key` | Identificateur de l'objet compartiment modifié. Cette valeur est identique à `file.Key`, mais elle est disponible en haut du code JSON de l'événement déclencheur. |

**Exemple de représentation JSON de l'activation du déclencheur de changement du compartiment**

```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Création d'une action pour traiter un objet modifié
{: #pkg_obstorage_ev_act}

Vous pouvez créer une action unique pour extraire et traiter l'objet. Vous pouvez également créer une séquence qui utilise une action pour extraire l'objet et une autre action pour le traiter.

### Création d'une action pour extraire et traiter l'objet
{: #pkg_obstorage_ev_act_ret}

Cet exemple de code d'action extrait et traite le document de notification de changement du compartiment. Vous pouvez transmettre les paramètres `apikey` et `serviceInstanceId` directement à l'action lors de l'appel manuel d'une action, mais lorsque cette action est appelée par un déclencheur, ces valeurs doivent être obtenues à partir de votre instance {{site.data.keyword.cos_full_notm}}, qui doit être liée à l'action avec la commande `ibmcloud fn service bind`. 

1. Sauvegardez le code suivant dans un fichier .zip nommé `myCosAction.zip`.

  ```javascript
  const COS = require('ibm-cos-sdk')

  function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

    const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

    return new Promise(function(resolve, reject) {
      client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
        if (err != null) {
          console.log(err)
        reject({ err: err })
        } else {
          console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
      })
    });
}
exports.main = main;
  ```
  {: codeblock}

  Cette action utilisant le package du gestionnaire de package de noeud (NPM) `ibm-cos-sdk`, elle doit être conditionnée sous forme de [module Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) ou de [bundle unique](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg). 

2. Créez l'action permettant d'extraire et de traiter l'objet depuis {{site.data.keyword.cos_full_notm}} :

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Création d'une séquence d'actions pour extraire et traiter l'objet
{: #pkg_obstorage_ev_act_seq}

Au lieu d'inclure le code d'extraction de l'objet dans votre action, vous pouvez utiliser l'action `object-read` à partir du package `cloud-object-storage`, qui doit être [installé manuellement](#pkg_obstorage_install). Votre code d'action a uniquement besoin de traiter les résultats renvoyés par `object-read`.
{: shortdesc}

Pour créer une action qui traite uniquement l'objet de compartiment :

1. Sauvegardez le code suivant en tant que `myCosAction.js`.

  ```javascript
  function main(data) {
    if (data) {
      // Traitement de l'objet
  }
  }
  ```
  {: codeblock}

2. Créez l'action permettant de traiter uniquement l'objet provenant de {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Liez vos données d'identification {{site.data.keyword.cos_full_notm}} au package `cloud-object-storage` que vous avez installé manuellement.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. L'action `object-read` peut être associée à `myCosProcessObjectAction` pour créer une séquence d'actions.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Outre l'action `object-read`, vous pouvez utiliser d'autres actions incluses dans le package `cloud-object-storage` installable.
{: tip}

Pour obtenir la liste des entités disponibles, exécutez la commande suivante.
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Liaison de données d'identification à votre action
{: #pkg_obstorage_ev_bind}

Vous pouvez éviter de transmettre des données d'identification sensibles lors de l'appel en liant vos données d'identification {{site.data.keyword.cos_full_notm}} à l'action via la commande suivante :
  ```
  ibmcloud fn service bind cloud-object-storage myCosAction
  ```
  {: pre}

### Création d'une règle pour associer l'action au déclencheur de changement
{: #pkg_obstorage_ev_rule}

Vous pouvez utiliser une action ou une séquence d'actions dans une [règle](/docs/openwhisk?topic=cloud-functions-rules) pour extraire et traiter l'objet qui est associé à un événement de changement {{site.data.keyword.cos_full_notm}}. 

Créez une règle qui active l'action `MyCosAction` sur les nouveaux événements déclencheurs {{site.data.keyword.cos_full_notm}}.
  ```
  ibmcloud fn rule create myRule myCosTrigger myCosAction
  ```
  {: pre}



