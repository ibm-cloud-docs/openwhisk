---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: services, serverless

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


# Ajout de services IBM Cloud
{: #services}

Vous pouvez utiliser des fonctionnalités de services IBM Cloud pour les incorporer dans votre application.
{: shortdesc}

**Comment ajouter des services IBM Cloud à mon application ?**

1. Vous pouvez coder en dur les appels d'API REST dans votre application. Cette option est peut être le moyen le plus rapide de communiquer avec un service IBM Cloud.
2. Vous pouvez utiliser un package préinstallé ou installable pour incorporer la fonctionnalité. Vous pouvez utiliser les actions et les flux stockés dans les packages dans le code de votre application. Cette option est susceptible d'alléger un peu votre code, ce qui peut s'avérer utile si votre application approche des limites du système.


**Comment configurer des paramètres auxquels doit accéder mon application ?**

Ces paramètres peuvent inclure des valeurs qui rendent votre application réutilisable avec d'autres données ou ils peuvent contenir des valeurs requises par le service, telles que des données d'identification. 
1. Vous pouvez coder en dur des paramètres dans votre application. Cette option n'est sans doute pas le moyen le plus sécurisé pour stocker des informations confidentielles, telles que des données d'identification.
2. Vous pouvez lier les paramètres à votre application en les associant à une action ou un package.


## Liaison d'un service à une action ou un package
{: #services_bind}

Liez le service {{site.data.keyword.Bluemix_notm}} de votre choix à une action. Une fois le service lié, un nouveau paramètre est créé dans votre action existante en incluant les données d'identification de l'instance de service.

**Remarque** : vous ne pouvez pas lier plusieurs instances du même service à une action ou un package. Vous ne pouvez lier qu'une seule instance d'un service. 

Avant de commencer, [créez une action](/docs/openwhisk?topic=cloud-functions-actions) et [définissez les données d'identification](/docs/resources?topic=resources-externalapp#externalapp) du service que vous souhaitez lier à l'action.

1. Obtenez le nom du service et l'instance de service que vous souhaitez lier à une action ou un package. Dans cet exemple de sortie, `composer` est le service et `Composer-qp` est le nom de l'instance de service.
    ```
    ibmcloud service list
    ```
    {: pre}

    Exemple de sortie :
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenez le nom des données d'identification définies pour une instance de service.

    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    Exemple :
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    Exemple de sortie :
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Liez le service à une action. La commande `ibmcloud fn service bind` de {{site.data.keyword.openwhisk_short}} rend vos données d'identification de service {{site.data.keyword.Bluemix_notm}} disponible pour votre code {{site.data.keyword.openwhisk_short}} lors de l'exécution.
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    <table>
    <caption>Description des composants de la commande <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de la commande <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE</code></td>
    <td>Nom du service dont vous effectuez la liaison.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>Nom de l'action ou du package auquel vous souhaitez lier le service.</td>
    </tr>
    <tr>
    <td>--instance <code>INSTANCE_NAME</code></td>
    <td>Facultatif : indiquez un nom d'instance de service. A défaut, la première instance du service est sélectionnée.</td>
    </tr>
    <tr>
    <td>--keyname <code>CREDENTIALS_NAME</code></td>
    <td>Facultatif : indiquez le nom des données d'identification. Si vous n'indiquez pas ce nom, les premières données d'identification correspondant à l'instance de service sont sélectionnées.</td>
    </tr>
    </tbody></table>

    Par exemple, pour lier un service {{site.data.keyword.ibmwatson}} Composer à une action nommée `hello` :
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    Sortie :
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Vérifiez que la liaison des données d'identification a abouti. L'action à laquelle est lié le service ne prend pas en charge les indicateurs personnalisés, mais prend en charge les indicateurs debug et verbose.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got action Hello World
{
        "parameters": [
        {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    Dans cet exemple, les données d'identification du service Composer, ainsi que d'autres données d'identification pour d'autres types de service, appartiennent à un paramètre nommé `__bx_creds`. L'action recherche le paramètre lié `__bx_creds` et supprime la référence au type de service répertorié. S'il s'agit de l'unique service répertorié, l'action retire la valeur du paramètre `__bx_cSreds`. S'il y a plusieurs services liés à l'action, le paramètre `__bx_creds` est conservé avec les services liés.

Pour plus d'informations sur la transmission de paramètres à une action ou un package, voir [Liaison de paramètres à des actions](/docs/openwhisk?topic=cloud-functions-actions#actions_params).




## Suppression de la liaison de service d'une action
{: #services_unbind}

La suppression de la liaison d'un service d'une action ou d'un package supprime les liaisons de service existantes.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
