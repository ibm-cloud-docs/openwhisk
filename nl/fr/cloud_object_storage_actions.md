---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: object storage, bucket, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Package Cloud Object Storage
{: #cloud_object_storage_actions}

Le package {{site.data.keyword.cos_full}} fournit un ensemble d'actions pour une interaction avec des instances {{site.data.keyword.cos_full_notm}}. Ces actions vous permettent de lire, écrire et supprimer dans les compartiments qui sont présents sur une instance {{site.data.keyword.cos_short}}.
{: shortdesc}

Le package {{site.data.keyword.cos_short}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Gestion d'une instance {{site.data.keyword.cos_short}}. |
| `/cloud-object-storage/object-write` | action | bucket, key, body, endpoint, ibmAuthEndpoint | Ecriture d'un objet dans un compartiment. |
| `/cloud-object-storage/object-read` | action | bucket, key, endpoint, ibmAuthEndpoint | Lecture d'un objet d'un compartiment. |
| `/cloud-object-storage/object-delete` | action | bucket, key, endpoint, ibmAuthEndpoint | Suppression d'un objet d'un compartiment. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig, endpoint, ibmAuthEndpoint | Affectation d'une configuration CORS à un compartiment. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket, endpoint, ibmAuthEndpoint | Lecture de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket, endpoint, ibmAuthEndpoint | Suppression de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obtention d'une URL signée pour restreindre l'écriture, la lecture et la suppression d'un objet d'un compartiment. |

## Paramètres de package et d'action

#### Paramètres de package

Les paramètres suivants sont censés être liés au package, ce qui les rend automatiquement disponibles pour toutes les actions. Il est également possible de spécifier ces paramètres lorsque vous appelez l'une de ces actions. 

**apikey** : le paramètre `apikey ` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_short}}. 

**resource_instance_id** : le paramètre `resource_instance_id` est l'identificateur de l'instance {{site.data.keyword.cos_short}}. 

**cos_hmac_keys** : le paramètre `cos_hmac_keys` désigne les données d'identification HMAC de l'instance {{site.data.keyword.cos_short}}, qui incluent les valeurs `access_key_id` et `secret_access_key`. Ces données d'identification
sont utilisées exclusivement par l'action `client-get-signed-url`. Consultez [Utilisation des données d'identification HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) pour obtenir des instructions sur la génération de données d'identification HMAC pour votre instance {{site.data.keyword.cos_short}}. 

#### Paramètres d'action

Les paramètres suivants sont indiqués lors de l'appel d'actions individuelles. Tous ces paramètres ne sont pas pris en charge par toutes les actions ; reportez-vous au tableau ci-dessus pour voir quels paramètres sont pris en charge par une action spécifique. 

**bucket** : le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cloud_object_storage_short_notm}}. 

**endpoint** : le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_short}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_short}}. Vous pouvez localiser votre noeud final dans la documentation [{{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints). 

**expires** : le paramètre `expires` est le délai d'expiration (en secondes) de l'opération d'URL présignée. La valeur par défaut pour `expires` est 15 minutes. 

**ibmAuthEndpoint** : le paramètre `ibmAuthEndpoint ` est le noeud final d'autorisation IBM Cloud utilisé par {site.data.keyword.cos_short}} pour générer un jeton à partir du paramètre `apikey`. Le noeud final d'autorisation par défaut doit normalement
fonctionner pour toutes les régions IBM Cloud. 

**key** : le paramètre `key` est la clé d'objet du compartiment. 

**operation** : le paramètre `operation` est l'opération de l'URL présignée à appeler. 

**corsConfig** : le paramètre `corsConfig` est la configuration CORS d'un compartiment. 


## Création d'une instance de service IBM Cloud Object Storage
{: #cloud_object_storage_service_instance}

Avant d'installer le package, vous devez demander une instance de {{site.data.keyword.cos_short}} et créer au moins un compartiment.

1. [Créez une instance de service {{site.data.keyword.cos_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-order-storage#creating-a-new-service-instance). 

2. [Créez un ensemble de données d'identification du service HMAC![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) pour l'instance de service {{site.data.keyword.cos_short}}. Dans la zone **Ajouter des paramètres de configuration en ligne (facultatif)**, ajoutez `{"HMAC":true}`.

3. [Créez au moins un compartiment ![Icône de lien externe](../icons/launch-glyph.svg "Ajouter des paramètres de configuration en ligne (facultatif)")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).

## Installation du package {{site.data.keyword.cos_short}}
{: #cloud_object_storage_installation}

Dès que vous disposez d'une instance de service {{site.data.keyword.cos_short}}, vous pouvez utiliser l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.cos_short}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_cli}

Avant de commencer :

[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Pour installer le package {{site.data.keyword.cos_short}} :

1. Clonez le référentiel de package {{site.data.keyword.cos_short}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Accédez au répertoire `runtimes/nodejs` ou `runtimes/python`. Les actions du package {{site.data.keyword.cos_short}} sont déployés dans l'environnement d'exécution que vous choisissez.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Déployez le package. Vous pouvez répéter les étapes précédentes pour redéployer le package dans un autre environnement d'exécution.
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

5. Liez les données d'identification de l'instance {{site.data.keyword.cos_short}} que vous avez créée au package.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.cos_short}}.
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
{: #cloud_object_storage_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à [Create page ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package {{site.data.keyword.cos_short}}. Les espaces de nom sont formés à partir de la combinaison des espaces de nom `org` et `space`. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM Cloud Object Storage**, puis cliquez sur le package **IBM Cloud Object Storage**.

5. Dans la section **Available Runtimes**, sélectionnez `Node.JS` ou `Python` dans la liste déroulante. Cliquez ensuite sur **Install**. 

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **cloud-object-storage**.

7. Pour utiliser les actions du package **cloud-object-storage**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **cloud-object-storage** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Parameters**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Ecriture dans un compartiment {{site.data.keyword.cos_short}}
{: #cloud_object_storage_write}

Vous pouvez utiliser l'action `object-write` pour écrire un objet dans un compartiment {{site.data.keyword.cos_short}}.
{: shortdesc}

**Remarque **: Dans les étapes qui suivent, le nom `testbucket` est utilisé en tant qu'exemple. Les compartiments de {{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `testbucket` par un nom de compartiment unique.

### Ecriture dans un compartiment depuis l'interface de ligne de commande
{: #write_bucket_cli}

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
{: #write_bucket_ui}

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

## Lecture depuis un compartiment {{site.data.keyword.cos_short}}
{: #cloud_object_storage_read}

Vous pouvez utiliser l'action `object-read` pour lire depuis un objet dans un compartiment {{site.data.keyword.cos_short}}.
{: shortdesc}

**Remarque **: Dans les étapes qui suivent, le nom `testbucket` est utilisé en tant qu'exemple. Les compartiments de {{site.data.keyword.cos_full_notm}} doivent être globalement uniques, vous devez donc remplacer `testbucket` par un nom de compartiment unique.

### Lecture depuis un compartiment dans l'interface de ligne de commande
{: #read_bucket_cli}

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

### Lecture depuis un compartiment dans l'interface utilisateur
{: #read_bucket_ui}

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
