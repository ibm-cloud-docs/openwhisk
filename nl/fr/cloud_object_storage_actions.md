---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Package Object Storage
{: #cloud_object_storage_actions}

Le package {{site.data.keyword.cos_full_notm}} fournit un ensemble d'actions pour une interaction avec des instances {{site.data.keyword.cos_full}}. Ces actions vous permettent de lire, écrire et supprimer dans les compartiments qui sont présents sur une instance {{site.data.keyword.cos_short}}.
{: shortdesc}

Le package {{site.data.keyword.cos_short}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key |Gestion d'une instance {{site.data.keyword.cos_full_notm}}.  |
| `/cloud-object-storage/object-write` | action | bucket, key, body | Ecriture d'un objet dans un compartiment. |
| `/cloud-object-storage/object-read` | action | bucket, key | Lecture d'un objet d'un compartiment. |
| `/cloud-object-storage/object-delete` | action | bucket, key | Suppression d'un objet d'un compartiment. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig | Affectation d'une configuration CORS à un compartiment. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket | Lecture de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket | Suppression de la configuration CORS d'un compartiment. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation | Obtention d'une URL signée pour restreindre l'écriture, la lecture et la suppression d'un objet d'un compartiment. |

## Création d'une instance de service {{site.data.keyword.cos_full_notm}}
{: #cloud_object_storage_service_instance}

Avant d'installer le package, vous devez demander une instance de {{site.data.keyword.cos_short}} et créer au moins un compartiment.

1. [Créez une instance de service {{site.data.keyword.cos_full_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance).

2. [Créez un ensemble de données d'identification du service HMAC![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials) pour l'instance de service {{site.data.keyword.cos_short}}. Dans la zone **Ajouter des paramètres de configuration en ligne (facultatif)**, ajoutez `{"HMAC":true}`.

3. [Créez au moins un compartiment ![Icône de lien externe](../icons/launch-glyph.svg "Ajouter des paramètres de configuration en ligne (facultatif)")](/docs/services/cloud-object-storage/getting-started.html#create-buckets).

## Installation du package {{site.data.keyword.cos_short}}
{: #cloud_object_storage_installation}

Dès que vous disposez d'une instance de service {{site.data.keyword.cos_short}}, utilisez l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.cos_short}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installez la commande `wskdeploy`. Consultez la documentation [Apache OpenWhisk![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project).

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

3. Déployez le package. Si vous décidez ultérieurement d'exécuter les actions dans ce package dans l'autre environnement d'exécution, vous pouvez reprendre l'étape précédente et cette étape afin de redéployer le package.
    ```
    wskdeploy -m manifest.yaml
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
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. Liez les données d'identification de l'instance {{site.data.keyword.cos_short}} que vous avez créée au package.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.cos_short}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
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

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à [Create page ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/create).

2. A l'aide des listes **Cloud Foundry Org** et **Cloud Foundry Space**, sélectionnez l'espace de nom dans lequel vous voulez installer le package {{site.data.keyword.cos_short}}. Les espaces de nom sont formés à partir de la combinaison des espaces de nom org et space.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM Cloud Object Storage**, puis cliquez sur le package **IBM Cloud Object Storage**.

5. Dans la section Available Runtimes, sélectionnez NodeJS ou Python dans la liste déroulante, puis cliquez sur **Install**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **cloud-object-storage**.

7. Pour utiliser les Actions du package **cloud-object-storage**, vous devez lier les données d'identification du service aux actions.
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

1. Accédez à la page [Actions sur la console {{site.data.keyword.openwhisk_short}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/actions).

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

1. Accédez à la page [Actions sur la console {{site.data.keyword.openwhisk_short}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/actions).

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
