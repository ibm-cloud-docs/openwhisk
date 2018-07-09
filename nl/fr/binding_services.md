---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Utilisation de services à partir des actions
{: #binding_services}

Vous pouvez optimiser le [plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk}} ](./bluemix_cli.html) pour lier un service à une action. {{site.data.keyword.openwhisk_short}} fournit la commande `service bind` pour mettre vos données d'identification du service {{site.data.keyword.Bluemix}} à la disposition de votre code Cloud Functions lors de l'exécution. Il ne faut pas confondre la commande `service bind` avec la commande `cf bind-service` disponible dans Cloud Foundry. Il s'agit simplement d'un moyen permettant de créer automatiquement un nouveau paramètre sur votre action existante qui contient des données d'identification du service. La commande {{site.data.keyword.openwhisk_short}} `service bind` est plus flexible et vous permet de lier n'importe quel service {{site.data.keyword.Bluemix_notm}} à n'importe quelle action qui est définie dans {{site.data.keyword.openwhisk_short}}. Le seul inconvénient est que vous devez avoir des données d'identification définies pour le service que vous souhaitez lier.
{: shortdesc}

## Comment lier un service à une action
{: #cli_bind}

Liez un service à une action à l'aide de la commande `bx wsk service bind` fournie par le plug-in d'interface de ligne de commande [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html). D'autres informations sont fournies dans la section [Limitations](./binding_services.html#limitations). 

Syntaxe d'utilisation avec `bind` :
```
bx wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

La commande `service bind` requiert un type de service et un nom d'action pour la liaison. Par exemple, si vous souhaitez lier un service de conversation Watson à une action nommée `hello`, votre appel ressemblera à la commande suivante :
```
bx wsk service bind conversation hello
```
{: pre}

Le résultat produit est le suivant :
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

Cette commande recherche les services de conversation Watson existant dans votre espace en cours, prend le premier service de conversation qu'il trouve, puis extrait toutes les données d'identification qui appartiennent à ce service. A l'aide du premier ensemble de données d'identification appartenant à ce service, il lie ces données d'identification en tant que paramètre à l'action `hello` spécifiée. La sortie montre exactement à quel service l'action est liée et quel ensemble de données d'identification issu de ce service a été utilisé pour la liaison. 

Pour vérifier que la liaison des données d'identification a abouti, exécutez la commande suivante :
```
bx wsk action get hello parameters
```
{: pre}

Exemple de résultat :
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

A ce stade, vous pourrez constater que les données d'identification pour ce service de conversation (ainsi que les autres données d'identification pour les autres types de service) appartiennent à un paramètre nommé `__bx_creds`, qui peut désormais être utilisé dans le code d'action comme n'importe quel autre paramètre lié. L'action sélectionne le premier service de conversation disponible incluant le premier ensemble de données d'identification défini dans ce service.  

Pour en savoir plus sur la transmission de paramètres à une action et sur la façon dont les données d'identification sont affectées lors d'une opération `action update`, voir la rubrique [Création et appel d'actions](openwhisk_actions.html#openwhisk_pass_params).

La commande `wsk service` prend en charge les deux indicateurs suivants :

<dl>
    <dt>--instance</dt>
    <dd>Nom du service spécifique correspondant au type que vous souhaitez utiliser. </dd>
    <dt>--keyname</dt>
    <dd>Nom des données d'identification spécifiques dans le service que vous souhaitez utiliser. </dd>
</dl>

Pour savoir comment utiliser ces indicateurs, voir l'exemple ci-après. En utilisant la commande `bx wsk service bind` précédente, imaginons qu'il existait deux services de conversation et que la valeur par défaut de l'action a fini par lier le service/les données d'identification inappropriés. Vous pouvez relancer l'exécution de la commande avec les indicateurs `--instance` et `--keyname` pour faire en sorte de lier le service approprié à l'action correcte. Tout d'abord, vérifiez les services disponibles et les données d'identification qui sont liées à ces services. Si nous souhaitions afficher la liste de nos services, nous obtiendrions un résultat similaire à ce qui suit :

```
bx service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

A partir de ce résultat, nous pouvons constater que **Conversation-qp** est le premier des deux services répertoriés, et qu'il s'agit de celui auquel la commande`bx wsk service bind conversation hello` initiale a fini par se lier. Vous souhaitez peut-être établir une liaison plutôt avec le service **Conversation-uc**. Afin d'être sûr, vous pouvez vérifier les données d'identification contenues dans **Conversation-uc** afin d'être certain d'utiliser l'ensemble de données d'identification approprié pour établir la liaison. 

```
bx service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

Vous souhaitez établir une liaison à "Credentials-2" à partir de ce service. Pour êtes certain que l'action effectue le comportement souhaité, exécutez la commande suivante :
```
bx wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Le résultat produit est le suivant :
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

A partir du résultat, vous pouvez voir que l'ensemble de données d'identification approprié est lié à l'action. Là encore, vous pouvez exécuter la commande `bx wsk action get` pour en être certain. 
```
bx wsk action get hello parameters
```
{: pre}

Le résultat produit est le suivant :
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

Les indicateurs de débogage normaux sont pris en charge et impriment des en-têtes de réponse à partir des appels. 

## Comment déconnecter un service d'une action
{: #cli_unbind}

Déconnectez un service d'une action à l'aide de la commande `bx wsk service unbind`. La commande `service unbind` retire les liaisons existantes créées par la commande `service bind`. 

Syntaxe d'utilisation avec `unbind` :
```
bx wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## Limitations
{: #limitations}

L'action `service` ne prend pas en charge les indicateurs personnalisés, mais prend en charge les indicateurs prolixes et de débogage habituels. L'action recherche le paramètre lié `__bx_creds` et retire la référence au type de service répertorié. Si ce type de service est le seul répertorié, l'action annule la valeur du paramètre `__bx_creds`. Si plus d'un service est lié à l'action, les services liés au paramètre `__bx_creds` sont conservés. 

Vous ne pouvez lier qu'un service de chaque type à une action. Il est impossible de lier plusieurs services du même type dans une seule action.
{: tip}

