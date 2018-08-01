---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotations
{: #openwhisk_annotations}

Les actions, les déclencheurs, les règles et les packages de {{site.data.keyword.openwhisk}} (collectivement désignés comme les actifs) peuvent être décorés avec des `annotations`. Des annotations sont associées à des actifs comme des paramètres avec une `clé` qui définit un nom et une `valeur` qui définit la valeur. Il est pratique de les définir à partir de l'interface de ligne de commande (CLI) en utilisant l'indicateur `--annotation` ou `-a` (forme abrégée).
{: shortdesc}

Explication : les annotations ont été ajoutées à {{site.data.keyword.openWhisk_short}} pour permettre de s'exercer sans modifier le schéma d'actifs sous-jacent. Avant la rédaction du présent document, nous avions délibérément choisi de ne pas définir les `annotations` autorisées. Toutefois, dans la mesure où les annotations sont davantage utilisées pour diffuser les modifications d'ordre sémantique, il est important de commencer à les documenter.

A ce jour, l'utilisation la plus répandue des annotations consiste à documenter des actions et des packages. Un grand nombre de packages dans le catalogue {{site.data.keyword.openwhisk_short}} comportent des annotations, par exemple, une description de la fonctionnalité proposée par leurs actions, les paramètres à utiliser en phase de liaison ou d'appel, ou si un paramètre est une valeur confidentielle ( "secret"), par exemple un mot de passe. Les annotations sont inventées en fonction des besoins, par exemple, pour autoriser l'intégration d'interface utilisateur.

Voici un exemple d'ensemble d'annotations pour une action `echo`, qui renvoie ses arguments d'entrée non modifiés (par exemple, `function main(args) { return args }`). Cette action peut s'avérer utile pour la consignation des paramètres d'entrée, par exemple, dans le cadre d'une séquence ou d'une règle.
```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

Voici les annotations qui décrivent des **packages** :

- `description` : description concise du package.
- `parameters` : tableau décrivant les paramètres définis dans la portée du package.

Voici les annotations qui décrivent des **actions** :

- `description` : description concise de l'action.
- `parameters` : tableau décrivant les actions requises pour exécuter l'action.
- `sampleInput` : exemple illustrant le schéma d'entrée avec des valeurs typiques.
- `sampleOutput` : exemple illustrant le schéma de sortie, généralement pour l'exemple `sampleInput`.

Voici les annotations qui décrivent des **paramètres** :

- `name` : nom du paramètre.
- `description` : description concise du paramètre.
- `doclink` : lien vers de la documentation supplémentaire relative au paramètre (utile pour les jetons OAuth).
- `required` : valeur True pour les paramètres requis et valeur false pour les paramètres facultatifs.
- `bindTime` : valeur True si le paramètre doit être spécifié lorsqu'un package est lié.
- `type` : type du paramètre, `password` ou `array` (mais peut être utilisé plus largement).

Les annotations _ne sont pas_ vérifiées. Par conséquent, s'il est concevable d'utiliser les annotations, par exemple, pour déduire si une composition de deux actions dans une séquence est légale, pour l'instant, le système n'effectue pas cette action. 

## Annotations spécifiques aux actions Web
{: #annotations-specific-to-web-actions}

Récemment, l'API principale a été étendue avec des nouvelles fonctions. Pour permettre aux packages et aux actions d'exploiter ces fonctionnalités, de nouvelles annotations dont la sémantique est significative ont été introduites. La valeur `true` doit être affectée de manière explicite à ces annotations pour que celles-ci aient un effet. Si la valeur `true` est remplacée par `false`, l'actif associé est exclu de la nouvelle API. En effet, si la valeur true ne leur est pas affectée, les annotations n'ont aucune signification dans le système. Examinez les annotations suivantes :

- `web-export` : ne s'applique qu'à une action. Si elle est présente, cette annotation rend l'action qui lui correspond accessible aux appels REST _sans_ authentification. C'est ce que l'on appelle des [_actions Web_](openwhisk_webactions.html) car elles permettent d'utiliser des actions OpenWhisk à partir d'un navigateur, par exemple. Il est important de noter que le _propriétaire_ de l'action Web supporte le coût lié à son exécution dans le système. En d'autres termes, le _propriétaire_ de l'action possède également l'enregistrement des activations.
- `final` : ne s'applique qu'à une action. Rend non modifiables tous les paramètres d'action qui sont déjà définis. Un paramètre d'action véhiculant l'annotation ne peut pas être remplacé par des paramètres d'appel (invoke-time) une fois que la valeur du paramètre est définie via le package qui le contient ou la définition de l'action.
- `raw-http` : s'applique uniquement à une action accompagnée d'une annotation `web-export`. Si celle-ci est présente, les paramètres de demande et de corps de la demande HTTP sont transmis à l'action en tant que propriétés réservées.
- `web-custom-options` : lorsque cette annotation est définie, elle permet à une action Web de répondre à des demandes OPTIONS avec des en-têtes personnalisés, sinon, une [réponse CORS par défaut](openwhisk_webactions.html#options-requests) s'applique.
- `require-whisk-auth` : cette annotation protège l'action Web qui n'est appelée que par des demandes qui fournissent les données d'authentification appropriées. Lorsqu'elle est définie avec une valeur booléenne, elle contrôle si la valeur d'authentification de base de la demande (c'est-à-dire Whisk auth key) sera authentifiée. La valeur `true` authentifie les données d'identification alors que `false` appelle l'action sans authentification. Lorsqu'elle est définie avec un nombre ou une chaîne, cette valeur doit correspondre à la valeur d'en-tête `X-Require-Whisk-Auth` de la demande. Dans les deux cas, il est important de noter que le _propriétaire_ de l'action Web supporte le coût de son exécution dans le système (c'est-à-dire que le _propriétaire_ de l'action possède également l'enregistrement des activations).

## Annotations spécifiques aux activations

Le système peut décorer les enregistrements d'activation avec les annotations suivantes :

- `path` : chemin d'accès complet de l'action qui a généré l'activation. Notez que si cette activation était le résultat d'une action dans une liaison de package, le chemin fait référence au package parent.
- `kind` : type d'action exécutée et un des types d'environnement d'exécution OpenWhisk de support.
- `Limits` : limites de temps, de mémoire et de journal auxquelles cette activation est soumise.

Pour les activations liées aux séquences, le système génère les annotations suivantes :

- `topmost` : présente uniquement pour une action de séquence la plus vers l'extérieur.
- `causedBy` : présente uniquement pour les actions qui sont contenues dans une séquence.

Enfin, et pour fournir une transparence de performance, les activations enregistrent également :

- `waitTime` : temps passé à attendre dans le système OpenWhisk interne. C'est à peu près le temps écoulé entre la réception de la demande d'activation par le contrôleur et la mise à disposition d'un conteneur pour l'action par l'auteur de l'appel. Actuellement, cette valeur est présente uniquement pour les activations qui ne sont pas liées à des séquences. Pour les séquences, ces informations peuvent être dérivées de l'enregistrement d'activation de séquence `topmost`.
- `initTime` : temps passé à initialiser la fonction. Si cette valeur est présente, l'action nécessitait une initialisation et elle représente un démarrage à froid. Une activation à chaud ignore l'initialisation, et dans ce cas, l'annotation n'est pas générée.

Exemple de ces annotations telles qu'elles apparaîtraient dans un enregistrement d'activation :

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
