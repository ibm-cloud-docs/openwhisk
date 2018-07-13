---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Liaison de services à des actions
{: #binding_services}

Vous pouvez utiliser le [plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk}}](./bluemix_cli.html) pour lier un service à une action ou un package. La commande `ibmcloud wsk service bind` de {{site.data.keyword.openwhisk_short}} rend vos données d'identification au service {{site.data.keyword.Bluemix_notm}} disponibles pour votre code {{site.data.keyword.openwhisk_short}} en phase d'exécution.
{: shortdesc}

Ne confondez pas la commande `ibmcloud wsk service bind` avec la commande `cf bind-service` disponible dans Cloud Foundry.
{: tip}

## Liaison d'un service à une action ou un package
{: #cli_bind}

Liez n'importe quel service {{site.data.keyword.Bluemix_notm}} à une action définie dans {{site.data.keyword.openwhisk_short}}. Cette liaison crée un nouveau paramètre dans votre action existante, qui comprend les données d'identification de l'instance de service.

**Remarque** : vous ne pouvez lier qu'un service de chaque type à une action ou un package. La liaison de plusieurs services du même type n'est pas prise en charge.

Avant de commencer, [définissez les données d'identification](/docs/apps/reqnsi.html#accser_external) du service que vous souhaitez lier.

1. Obtenez le nom de l'instance de service que vous souhaitez lier à une action ou un package.
    ```
    ibmcloud service list
    ```
    {: pre}

    Exemple de sortie :
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenez le nom des données d'identification définies pour l'instance de service que vous avez extraite à l'étape précédente.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Exemple de sortie :
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Liez le service à une action.
    ```
    ibmcloud wsk service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Description des composantes de la commande <code>ibmcloud wsk service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de la commande <code>ibmcloud wsk service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>Type de service concerné par la liaison.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>Nom de l'action ou du package auquel vous souhaitez lier le service.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Facultatif : indiquez un nom d'instance de service (instance_name). A défaut, la première instance du service est sélectionnée.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Facultatif : indiquez un nom d'ensemble de données d'identification. A défaut, le premier ensemble de données d'identification pour l'instance de service est sélectionné.</td>
    </tr>
    </tbody></table>

    Par exemple, pour lier un service de conversation {{site.data.keyword.ibmwatson}} à une action nommée `hello`, exécutez :
    ```
    ibmcloud wsk service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Vérifiez que la liaison des données d'identification a abouti. L'action à laquelle est lié le service ne prend pas en charge les indicateurs personnalisés, mais prend en charge les indicateurs debug et verbose.
    ```
    ibmcloud wsk action get hello parameters
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
                    "conversation": {
                        "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    Dans cet exemple, les données d'identification du service de conversation, ainsi que les autres données d'identification d'autres types de service, appartiennent à un paramètre nommé `__bx_creds`. L'action recherche le paramètre lié `__bx_creds` et supprime la référence au type de service répertorié. S'il s'agit de l'unique service répertorié, l'action retire la valeur du paramètre `__bx_cSreds`. S'il y a plusieurs services liés à l'action, le paramètre `__bx_creds` est conservé avec les services liés.

Pour plus d'informations sur la transmission de paramètres à une action ou un package et comment les données d'identification sont affectées lors des opérations `update`, voir [Utilisation des paramètres](./parameters.html#pass-params-action).


## Suppression de la liaison d'un service d'une action ou d'un package
{: #cli_unbind}

Supprimez la liaison d'un service d'une action ou d'un package. Cette opération supprime les liaisons existantes créées par la commande `service bind`.

```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
