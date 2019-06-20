---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

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

# Object Storage
{: #pkg_obstorage}

Vous pouvez étendre les fonctionnalités de votre application {{site.data.keyword.openwhisk}} en l'intégrant à une instance {{site.data.keyword.cos_full}}.

**Avant de commencer :** pour en savoir plus sur {{site.data.keyword.cos_full_notm}}, voir [A propos d'Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Pour plus d'informations sur la configuration de l'instance {{site.data.keyword.cos_full_notm}}, voir [Mise à disposition d'une instance {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Packages
{: #obstorage_packages}
| Package | Disponibilité | Description |
| --- | --- | --- |
| [Package {{site.data.keyword.cos_full_notm}}](#pkg_obstorage)| Installable | Lecture, écriture et suppression depuis une instance {{site.data.keyword.cos_full_notm}}. |
| [Source d'événements {{site.data.keyword.cos_full_notm}} (expérimental)](#pkg_obstorage_ev) | Préinstallé (Sud des Etats-Unis uniquement) | Ecoute des modifications apportées à une instance {{site.data.keyword.cos_full_notm}}. |

## Création d'une instance de service IBM Cloud Object Storage
{: #pkg_obstorage_service}

Avant d'utiliser l'un de ces packages, vous devez demander une instance d'{{site.data.keyword.cos_full_notm}} et créer au moins un compartiment.

1. [Créez une instance de service {{site.data.keyword.cos_full_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Créez un ensemble de données d'identification du service HMAC ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) pour l'instance de service {{site.data.keyword.cos_full_notm}}. Dans la zone **Ajouter des paramètres de configuration en ligne (facultatif)**, ajoutez `{"HMAC":true}`.

3. [Créez au moins un compartiment ![Icône de lien externe](../icons/launch-glyph.svg "Ajouter des paramètres de configuration en ligne (facultatif)")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Opérations de lecture et d'écriture dans un compartiment avec le package {{site.data.keyword.cos_full_notm}}
{: #pkg_obstorage_install}

Dès que vous disposez d'une instance de service {{site.data.keyword.cos_full_notm}}, vous pouvez utiliser l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.cos_full_notm}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_cli}

Avant de commencer :

[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.cos_full_notm}} :

1. Clonez le référentiel de package {{site.data.keyword.cos_full_notm}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Accédez au répertoire `runtimes/nodejs` ou `runtimes/python`. Les actions du package {{site.data.keyword.cos_full_notm}} sont déployées dans le contexte d'exécution que vous choisissez.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Déployez le package. Vous pouvez répéter les étapes précédentes pour redéployer le package dans un autre contexte d'exécution.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Vérifiez que le package `cloud-object-storage` est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Sortie :
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. Liez les données d'identification de l'instance {{site.data.keyword.cos_full_notm}} que vous avez créée au package.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package {{site.data.keyword.cos_full_notm}}. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM Cloud Object Storage**, puis cliquez sur le package **IBM Cloud Object Storage**.

5. Dans la section **Contextes d'exécution disponibles**, sélectionnez `Node.JS` ou `Python` dans la liste déroulante. Cliquez ensuite sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **cloud-object-storage**.

7. Pour utiliser les actions du package **cloud-object-storage**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **cloud-object-storage** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.


### Actions disponibles
{: #pkg_obstorage_actions}

Le package {{site.data.keyword.cos_full_notm}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Gestion d'une instance {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | action | bucket, key, body, endpoint, ibmAuthEndpoint | Ecriture d'un objet dans un compartiment. |
| `/cloud-object-storage/object-read` | action | bucket, key, endpoint, ibmAuthEndpoint | Lecture d'un objet d'un compartiment. |
| `/cloud-object-storage/object-delete` | action | bucket, key, endpoint, ibmAuthEndpoint | Suppression d'un objet d'un compartiment. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig, endpoint, ibmAuthEndpoint | Affectation d'une configuration CORS à un compartiment. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket, endpoint, ibmAuthEndpoint | Lecture de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket, endpoint, ibmAuthEndpoint | Suppression de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obtention d'une URL signée pour restreindre l'écriture, la lecture et la suppression d'un objet d'un compartiment. |


### Paramètres de package
{: #pkg_obstorage_pkgparams}

Les paramètres suivants sont censés être liés au package, ce qui les rend automatiquement disponibles pour toutes les actions. Il est également possible de spécifier ces paramètres lorsque vous appelez l'une de ces actions.

**apikey** : le paramètre `apikey ` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_full_notm}}.

**resource_instance_id** : le paramètre `resource_instance_id` est l'identificateur de l'instance {{site.data.keyword.cos_full_notm}}. 

**cos_hmac_keys** : le paramètre `cos_hmac_keys` désigne les données d'identification HMAC de l'instance {{site.data.keyword.cos_full_notm}}, qui incluent les valeurs `access_key_id` et `secret_access_key`.  Ces données d'identification sont utilisées exclusivement par l'action `client-get-signed-url`.  Consultez la section [Utilisation des données d'identification HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) pour obtenir des instructions sur la génération de données d'identification HMAC pour votre instance {{site.data.keyword.cos_full_notm}}.

#### Paramètres d'action
{: #pkg_obstorage_actparams}

Les paramètres suivants sont indiqués lors de l'appel d'actions individuelles.  Ces paramètres ne sont pas tous pris en charge par toutes les actions ; reportez-vous au tableau ci-dessus pour voir quels sont les paramètres pris en charge par les différentes actions. 

**bucket** : le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cos_full_notm}}.

**endpoint** : le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_full_notm}}. Vous pouvez localiser votre noeud final dans la [documentation {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**expires** : le paramètre `expires` est le délai d'expiration (en secondes) de l'opération d'URL présignée.  La valeur par défaut pour `expires` est 15 minutes.

**ibmAuthEndpoint** : le paramètre `ibmAuthEndpoint ` est le noeud final d'autorisation IBM Cloud utilisé par {site.data.keyword.cos_short}} pour générer un jeton à partir du paramètre `apikey`. Le noeud final d'autorisation par défaut doit normalement fonctionner pour toutes les régions IBM Cloud.

**key** : le paramètre `key` est la clé d'objet du compartiment.

**operation** : le paramètre `operation` est l'opération de l'URL présignée à appeler.

**corsConfig** : le paramètre `corsConfig` est la configuration CORS d'un compartiment.


### Ecriture dans un compartiment {{site.data.keyword.cos_full_notm}}
{: #pkg_obstorage_write}

Vous pouvez utiliser l'action `object-write` pour écrire un objet dans un compartiment {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

**Remarque **: Dans les étapes qui suivent, le nom `testbucket` est utilisé en tant qu'exemple. Les compartiments d'{{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `testbucket` par un nom de compartiment unique.

#### Ecriture dans un compartiment depuis l'interface de ligne de commande
{: #pkg_obstorage_write_cli}

Ecrivez un objet dans votre compartiment à l'aide de l'action `object-write`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Exemple de sortie :
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Ecriture dans un compartiment depuis l'interface utilisateur
{: #pkg_obstorage_write_ui}

1. Accédez à la page [Actions sur la console {{site.data.keyword.openwhisk_short}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/actions).

2. Sous le package `cloud-object-storage`, cliquez sur l'action **object-write**.

3. Dans le volet Code, cliquez sur **Changer l'entrée**.

4. Entrez un objet JSON contenant votre compartiment, ainsi que les clés d'objet key et body.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
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
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

### Lecture depuis un compartiment {{site.data.keyword.cos_full_notm}}
{: #pkg_obstorage_read}

Vous pouvez utiliser l'action `object-read` pour lire depuis un objet dans un compartiment {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

**Remarque **: Dans les étapes qui suivent, le nom `testbucket` est utilisé en tant qu'exemple. Les compartiments d'{{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `testbucket` par un nom de compartiment unique.

#### Lecture depuis un compartiment dans l'interface de ligne de commande
{: #pkg_obstorage_read_cli}

Lisez depuis un objet dans votre compartiment à l'aide de l'action `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Exemple de sortie :
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

#### Lecture depuis un compartiment dans l'interface utilisateur
{: #pkg_obstorage_read_ui}

1. Accédez à la page [Actions sur la console {{site.data.keyword.openwhisk_short}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/actions).

2. Sous le package `cloud-object-storage`, cliquez sur l'action **object-read**.

3. Dans le volet Code, cliquez sur **Changer l'entrée**.

4. Entrez un objet JSON contenant votre compartiment, ainsi que les clés d'objet key.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
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
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Ecoute des modifications d'un compartiment avec la source d'événements Object Storage (expérimental)
{: #pkg_obstorage_ev}

Le package `/whisk.system/cos-experimental` risque d'être instable, de changer fréquemment d'une façon non compatible avec les versions antérieures et peut être retiré dans un délai court. Ce package n'est pas recommandé pour une utilisation dans des environnements de production. Ce package expérimental est actuellement disponible uniquement dans la région Sud des Etats-Unis.
{: important}

Vous pouvez utiliser {{site.data.keyword.openwhisk}} pour écouter les modifications apportées à un compartiment [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) et utiliser une action pour traiter un ou plusieurs objets du compartiment.

<br>

**Exemple de cas d'utilisation :** Le package `/whisk.system/cos-experimental` vous permet d'écouter les modifications apportées aux données de rue GPS stockées dans un compartiment {{site.data.keyword.cos_full_notm}}. Ensuite, lorsque des modifications ont lieu, vous pouvez déclencher la régénération automatique d'une carte GPS, afin que les utilisateurs puissent avoir accès aux données de rue les plus récentes pour leur application GPS.

### Paramètres de la source d'événements Object Storage (expérimental)
{: #pkg_obstorage_ev_ch}

Le package `/whisk.system/cos-experimental` vous permet de configurer des événements à partir d'une instance {{site.data.keyword.cos_full_notm}}, et inclut le flux suivant :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | package | apikey, auth_endpoint, bucket, endpoint, interval | Package contenant l'action de flux `changes`. |
| `/whisk.system/cos-experimental/changes` | flux | apikey, auth_endpoint, bucket, endpoint, interval | Exécute des événements déclencheurs en cas de modifications effectuées dans un compartiment {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Vous pouvez utiliser le flux `changes` pour configurer le service de source d'événements {{site.data.keyword.cos_full_notm}} afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à un compartiment dans votre instance {{site.data.keyword.cos_full_notm}}.

Paramètres utilisés dans cet exemple :

**apikey** : _(obligatoire, sauf s'il est lié au package)_. Le paramètre `apikey` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_full_notm}}.  Normalement, cette valeur est liée au package. Toutefois, si la valeur `apikey` est spécifiée lors de l'utilisation de l'action de flux `changes`, la valeur indiquée est utilisée pour les données d'identification au lieu de la clé d'API des données d'identification liées.

**auth_endpoint** : _(facultatif)_. Le paramètre `auth_endpoint` est le noeud final d'autorisation utilisé par {{site.data.keyword.cos_full_notm}} pour générer un jeton à partir du paramètre `apikey`.  Le noeud final par défaut est le noeud
final {{site.data.keyword.Bluemix}}.

**bucket** : _(obligatoire)_. Le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cos_full_notm}}.

**endpoint** : _(obligatoire)_. Le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_full_notm}}. Vous pouvez localiser votre noeud final dans la [documentation {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**interval** : _(facultatif)_. Le paramètre `interval` est l'intervalle d'interrogation du compartiment, en minutes entières. La valeur `interval` doit être d'au moins 1 minute, qui est la valeur par défaut.

### Création d'un déclencheur pour répondre au flux changes
{: #pkg_obstorage_ev_trig}

Lors de la création du déclencheur, vous pouvez éviter de transmettre vos données d'identification {{site.data.keyword.cos_full_notm}} à l'action de flux `changes` en liant vos données d'identification directement au package `cos-experimental`.
 {: shortdesc}

 1. Créez d'abord une liaison de package qui peut être modifiée pour contenir vos données d'identification. La commande suivante crée une liaison de package `myCosPkg` dans votre espace de nom.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Liez vos données d'identification {{site.data.keyword.cos_full_notm}} au package.
 La liaison de vos données d'identification {{site.data.keyword.cos_full_notm}} au package associe la valeur `apikey` au package, de sorte que vous n'avez pas besoin de spécifier la valeur `apikey` lors de l'appel de l'action de flux `changes`.
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Créez un déclencheur nommé `myCosTrigger` avec le flux `changes` dans la liaison de package que vous avez créée. Utilisez les valeurs de paramètre de votre nom de compartiment et de votre noeud final {{site.data.keyword.cos_full_notm}}.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

    Exemple de sortie :
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. Créez une action simple permettant uniquement de vérifier que le déclencheur, le flux de modifications et la règle sont tous configurés correctement et fonctionnent bien. Par exemple, créez une action nommée `showCosChange` contenant le code JavaScript `showCosChange.js` suivant :
  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}
  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
Affichage du code exemple
 5. Créez une règle pour connecter l'action `showCosChange` au déclencheur `myCosTrigger` :
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. Dans une fenêtre distincte, commencez à interroger les activations pour obtenir une bonne visibilité de ce qui se passe. Lorsque le déclencheur est exécuté, la commande affiche les enregistrements d'activation pour chacune de ces opérations à mesure qu'elles se produisent.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. Dans votre tableau de bord {{site.data.keyword.cos_full_notm}}, modifiez un objet de compartiment existant ou créez-en un. Pour savoir comment ajouter un objet à votre compartiment, voir [Ajout d'objets à votre compartiment](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).

 8. Pour chaque modification d'objet de compartiment, observez les nouvelles activations pour le déclencheur `myCosTrigger` et l'action `showCosChange`. Ces activations apparaissent dans votre fenêtre en exécutant la commande `ibmcloud fn activation poll` dans l'intervalle d'interrogation du compartiment configuré.

Si vous ne parvenez pas à observer de nouvelles activations, vérifiez que les valeurs des paramètres `apikey`, `endpoint` et `bucket` sont correctes.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Structure de données d'un événement déclencheur Object Storage
{: #pkg_obstorage_ev_data}

Le contenu des événements générés possède les paramètres suivants :

  - `file` : Métadonnées du fichier ou de l'objet. Cette structure est décrite dans la section d'[affichage de la liste des objets dans un compartiment spécifique](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets).
  - `status` : Modification détectée.  Cette valeur peut être : `added`, `modified` ou `deleted`.
  - `bucket` : Nom du compartiment {{site.data.keyword.cos_full_notm}}.
  - `endpoint` : Noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à l'instance {{site.data.keyword.cos_full_notm}}.
  - `key` : Identificateur de l'objet de compartiment modifié. Cette valeur est identique à `file.Key`, mais elle est disponible en haut de l'événement déclencheur JSON.

Exemple de représentation JSON de l'événement déclencheur de modification dans le compartiment :
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

### Création d'une action pour traiter l'objet modifié
{: #pkg_obstorage_ev_act}

Vous pouvez créer une action unique pour extraire et traiter l'objet. Vous pouvez également créer une séquence qui utilise une action pour extraire l'objet et une autre action pour le traiter.

### Création d'une action pour extraire et traiter l'objet
{: #pkg_obstorage_ev_act_ret}

Cet exemple de code d'action extrait et traite le document de notification de changement du compartiment. Vous pouvez transmettre les paramètres `apikey` et `serviceInstanceId` directement à l'action lors de l'appel manuel d'une action, mais lorsque cette action est appelée par un déclencheur, ces valeurs doivent
être obtenues à partir de votre instance {{site.data.keyword.cos_full_notm}}, qui doit être liée à l'action avec la commande `ibmcloud fn service bind`.

Exemple de code :

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

Cette action utilisant le package du gestionnaire de package de noeud (npm) `ibm-cos-sdk`, elle doit être conditionnée sous forme de [module Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) ou de [bundle unique](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

Une fois que cette action est intégrée dans un fichier .zip, `myCosAction.zip`, créez l'action qui permet d'extraire et de traiter l'objet à partir d'{{site.data.keyword.cos_full_notm}} : 

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### Création d'une séquence d'actions pour extraire et traiter l'objet
{: #pkg_obstorage_ev_act_seq}

Au lieu d'inclure le code d'extraction de l'objet dans votre action, vous pouvez utiliser l'action `object-read` à partir du package `cloud-object-storage`, qui doit être [installé manuellement](#pkg_obstorage_install). Il suffit que votre code d'action traite les résultats renvoyés par `object-read`.

Exemple de code `myCosAction.js` d'une action qui traite uniquement l'objet de compartiment :
```javascript
function main(data) {
  if (data) {
    // Traitement de l'objet
  }
}
```
{: codeblock}

1. Créez l'action permettant de traiter uniquement l'objet provenant d'{{site.data.keyword.cos_full_notm}} :
  ```
  ibmcloud fn action create myCosProcessObjectAction myCosAction.js
  ```
  {: pre}
2. Liez vos données d'identification {{site.data.keyword.cos_full_notm}} au package `cloud-object-storage` que vous avez installé manuellement.
  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}
3. L'action `object-read` peut être associée à `myCosProcessObjectAction` pour créer une séquence d'actions.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

En plus de l'action `object-read`, vous pouvez utiliser d'autres actions incluses dans le package installable `cloud-object-storage`.

### Liaison de données d'identification à votre action
{: #pkg_obstorage_ev_bind}

Vous pouvez éviter de transmettre des données d'identification sensibles lors d'un appel en associant les données d'identification {{site.data.keyword.cos_full_notm}} à l'action à l'aide de la commande suivante :
```
ibmcloud fn service bind cloud-object-storage myCosAction
```
{: pre}

### Création d'une règle pour associer l'action au déclencheur de changement
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

Vous pouvez utiliser une action ou une séquence d'actions dans une [règle](/docs/openwhisk?topic=cloud-functions-rules) pour extraire et traiter l'objet qui est associé à un événement de changement {{site.data.keyword.cos_full_notm}}.

Créez une règle qui active l'action `MyCosAction` sur les nouveaux événements déclencheurs {{site.data.keyword.cos_full_notm}}.
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}

