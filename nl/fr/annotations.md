---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

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
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Annotations
{: #annotations}

Les actions, déclencheurs, règles et packages {{site.data.keyword.openwhisk}} (désignés collectivement par le terme entités) peuvent inclure des annotations.
{: shortdesc}

Les annotations sont liées aux entités comme des paramètres. Les annotations se composent d'une `clé` qui définit un nom et une `valeur` qui définit la valeur. Les annotations sont le plus souvent utilisés pour documenter des actions et des packages. Les packages du catalogue {{site.data.keyword.openwhisk_short}} comportent des annotations. Ces annotations incluent des descriptions de la fonctionnalité offerte par ses actions, des paramètres à utiliser lors de la liaison de package, des paramètres d'heure d'appel et indiquent si un paramètre est un secret. Les annotations sont inventées en fonction des besoins, par exemple, pour autoriser l'intégration d'interface utilisateur.

Vous pouvez documenter une entité à l'aide de l'interface de ligne de commande en utilisant l'indicateur `--annotation` ou `-a`.

## Annotations d'action
{: #annotations_action}

| Annotation | Description |
| --- | --- |
| `description` | Description de l'action. |
| `parameters` | Tableau décrivant les actions requises pour exécuter l'action. |
| `sampleInput` | Exemple illustrant le schéma d'entrée avec des valeurs typiques. |
| `sampleOutput` | Exemple de schéma de sortie, généralement pour `sampleInput`. |



Le code suivant est un exemple d'ensemble d'annotations pour une action `echo`, qui renvoie ses arguments d'entrée non modifiés. Cette action peut s'avérer utile pour la consignation des paramètres d'entrée, par exemple, dans le cadre d'une séquence ou d'une règle.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Annotations d'action Web
{: #annotations-specific-to-web-actions}

La valeur `true` doit être affectée de manière explicite à ces annotations d'action Web pour permettre l'interactivité d'API. 

| Annotation | Description |
| --- | --- | 
| `web-export` | Lorsqu'elle est appliquée à une action, l'action devient une [action Web](/docs/openwhisk?topic=cloud-functions-actions_web). L'action devient accessible aux appels REST sans authentification de sorte que les utilisateurs peuvent accéder aux actions depuis un navigateur. Le propriétaire de l'action Web supporte le coût de leur exécution sur le système. En d'autres termes, le propriétaire de l'action possède également l'enregistrement des activations. |
| `final` | Quand elle est appliquée à une action, tout paramètre action défini précédemment ne peut pas être remplacé par un paramètre fourni lors de l'appel. |
| `raw-http` | Lorsqu'elle est appliquée à une action comportant l'annotation `web-export` , la requête HTTP et les paramètres de corps sont transmis à l'action en tant que propriétés réservées. |
| `web-custom-options` | Permet à une action Web de répondre aux demandes OPTIONS avec des en-têtes personnalisés. Dans le cas contraire, une [réponse CORS par défaut](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options) s'applique. |
| `require-whisk-auth` | L'action Web peut uniquement être appelée par des demandes qui fournissent les données d'authentification appropriées. Lorsqu'elle est définie sur une valeur booléenne, elle contrôle si la valeur d'authentification de base de la demande est authentifiée. La valeur `true` authentifie les données d'identification alors que `false` appelle l'action sans authentification. Lorsqu'elle est définie sur un entier ou une chaîne, cette valeur doit correspondre à la valeur d'en-tête `X-Require-Whisk-Auth` de la demande. |

## Annotations de package
{: #annotations_package}

| Annotation | Description |
| --- | --- |
| `description` | Description du package. |
| `parameters` | Tableau qui décrit les paramètres qui sont étendus au package. |

## Annotations de paramètre
{: #annotations_parameter}

| Annotation | Description |
| --- | --- |
| `name` | Nom du paramètre. |
| `description` | Description concise du paramètre. |
| `doclink` | Lien vers une documentation supplémentaire pour le paramètre (utile pour les jetons OAuth). |
| `required` | Valeur True pour les paramètres requis et valeur false pour les paramètres facultatifs.|
| `bindTime` | True si le paramètre est spécifié lorsqu'un package est lié. |
| `type` | Type du paramètre, `password` ou `array` (mais peut être utilisé plus largement).|

## Annotations d'activation
{: #annotations_activation}

Vous pouvez documenter des enregistrements d'activation avec les annotations suivantes :

| Annotation | Description |
| --- | --- |
| `path` | Nom de chemin qualifié complet de l'action ayant généré l'activation. Si cette activation était le résultat d'une action dans une liaison de package, le chemin fait référence au package parent.|
| `kind` | Type d'action exécutée et un des types d'environnement d'exécution {{site.data.keyword.openwhisk_short}} pris en charge. |
| `limits` | Limites de temps, mémoire et journal auxquelles cette activation était soumise. |

Pour les activations liées aux séquences, le système génère les annotations suivantes :

| Annotation | Description |
| --- | --- |
| `topmost` | Cette annotation est présente uniquement pour une action de séquence la plus vers l'extérieur. |
| `causedBy` | Cette annotation est présente uniquement pour les actions qui sont contenues dans une séquence. |
| `waitTime` | Temps passé à attendre dans le système {{site.data.keyword.openwhisk_short}} interne. Il s'agit à peu près du temps écoulé entre la réception de la demande d'activation et la mise à disposition, par l'auteur de l'appel, d'un conteneur pour l'action. Cette valeur est uniquement présente pour les activations non liées à la séquence. Pour les séquences, ces informations peuvent être dérivées de l'enregistrement d'activation de séquence `topmost`. |
| `initTime` | Temps passé à initialiser la fonction. Si cette valeur est présente, l'action nécessitait une initialisation et elle représente un démarrage à froid. Une activation à chaud ignore l'initialisation, et dans ce cas, l'annotation n'est pas générée. |

L'exemple suivant présente ces annotations telles qu'elles apparaîtraient dans un enregistrement d'activation :

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}




