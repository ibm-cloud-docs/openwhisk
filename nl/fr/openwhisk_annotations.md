---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotations
{: #openwhisk_annotations}

Les actions, déclencheurs, règles et packages {{site.data.keyword.openwhisk}} (désignés collectivement par le terme entités) peuvent être accompagnés d'annotations.
{: shortdesc}

Les annotations sont liées aux entités comme des paramètres. Les annotations se composent d'une `clé` qui définit un nom et une `valeur` qui définit la valeur. Les annotations sont le plus souvent utilisés pour documenter des actions et des packages. Nombre de packages du catalogue {{site.data.keyword.openwhisk_short}} comportent des annotations, par exemple une description de la fonctionnalité qui est proposée par ses actions, paramètres à utiliser aux paramètres d'heure de liaison de package, d'heure d'appel, ou si un paramètre est une valeur confidentielle. Les annotations sont inventées en fonction des besoins, par exemple, pour autoriser l'intégration d'interface utilisateur.

Vous pouvez documenter une entité à l'aide de l'interface de ligne de commande en utilisant l'indicateur `--annotation` ou `-a`.

## Annotations d'action
{: #action}

Les annotations décrivant les actions sont les suivantes :

- `description` : description concise de l'action.
- `parameters` : tableau décrivant les actions requises pour exécuter l'action.
- `sampleInput` : exemple illustrant le schéma d'entrée avec des valeurs typiques.
- `sampleOutput` : exemple illustrant le schéma de sortie, généralement pour l'exemple `sampleInput`.

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

- `web-export` : lorsqu'elle est appliquée à une action, cette action devient une [action Web](openwhisk_webactions.html). L'action devient accessible aux appels REST sans authentification de sorte que les utilisateurs peuvent accéder aux actions depuis un navigateur.
    * **Remarque **: Le propriétaire de l'action Web supporte le coût de leur exécution sur le système. En d'autres termes, le propriétaire de l'action possède également l'enregistrement des activations.
- `final` : lorsqu'elle est appliquée à une action, tout paramètre action défini préalablement devient immuable. Les paramètres ne peuvent pas être remplacés par des paramètres fournis lors de l'appel.
- `raw-http` : lorsqu'elle est appliquée à une action comportant l'annotation `web-export`, les paramètres de demande et de corps de la demande HTTP sont transmis à l'action en tant que propriétés réservées.
- `web-custom-options` : permet à une action Web de répondre à des demandes OPTIONS avec des en-têtes personnalisés. Dans le cas contraire, une [réponse CORS par défaut](openwhisk_webactions.html#options-requests) s'applique.
- `require-whisk-auth` : l'action Web peut uniquement être appelée par des demandes qui fournissent des données d'authentification appropriées.
    * Lorsqu'elle est définie sur une valeur booléenne, elle contrôle si la valeur d'authentification de base de la demande est authentifiée. La valeur `true` authentifie les données d'identification alors que `false` appelle l'action sans authentification.
    * Lorsqu'elle est définie sur un entier ou une chaîne, cette valeur doit correspondre à la valeur d'en-tête `X-Require-Whisk-Auth` de la demande. 
    * **Remarque **: Le propriétaire de l'action Web supporte le coût de leur exécution sur le système. En d'autres termes, le propriétaire de l'action possède également l'enregistrement des activations.

## Annotations de package
{: #package}

Les annotations décrivant les packages sont les suivantes :

- `description` : description concise du package.
- `parameters` : tableau décrivant les paramètres définis dans la portée du package.

## Annotations de paramètre
{: #parameter}

Les annotations décrivant les paramètres sont les suivantes :

- `name` : nom du paramètre.
- `description` : description concise du paramètre.
- `doclink` : lien vers de la documentation supplémentaire relative au paramètre (utile pour les jetons OAuth).
- `required` : valeur True pour les paramètres requis et valeur false pour les paramètres facultatifs.
- `bindTime` : valeur True si le paramètre doit être spécifié lorsqu'un package est lié.
- `type` : type du paramètre, `password` ou `array` (mais peut être utilisé plus largement).

## Annotations d'activation
{: #activation}

Vous pouvez documenter des enregistrements d'activation avec les annotations suivantes :

- `path` : chemin d'accès complet de l'action qui a généré l'activation. Notez que si cette activation était le résultat d'une action dans une liaison de package, le chemin fait référence au package parent.
- `kind` : type d'action exécutée et un des types d'environnement d'exécution OpenWhisk de support.
- `Limits` : limites de temps, de mémoire et de journal auxquelles cette activation est soumise.

Pour les activations liées aux séquences, le système génère les annotations suivantes :

- `topmost` : présente uniquement pour une action de séquence la plus vers l'extérieur.
- `causedBy` : présente uniquement pour les actions qui sont contenues dans une séquence.

Pour fournir une transparence de performance, les activations enregistrent également :

- `waitTime` : temps passé à attendre dans le système OpenWhisk interne. C'est à peu près le temps écoulé entre la réception de la demande d'activation par le contrôleur et la mise à disposition d'un conteneur pour l'action par l'auteur de l'appel. Actuellement, cette valeur est présente uniquement pour les activations qui ne sont pas liées à des séquences. Pour les séquences, ces informations peuvent être dérivées de l'enregistrement d'activation de séquence `topmost`.
- `initTime` : temps passé à initialiser la fonction. Si cette valeur est présente, l'action nécessitait une initialisation et elle représente un démarrage à froid. Une activation à chaud ignore l'initialisation, et dans ce cas, l'annotation n'est pas générée.

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
