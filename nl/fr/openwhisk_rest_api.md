---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation des API REST OpenWhisk
{: #openwhisk_rest_api}

Une fois votre environnement OpenWhisk activé, vous pouvez utiliser OpenWhisk avec vos applications Web ou mobiles à l'aide d'appels d'API REST.
{: shortdesc}

Pour plus de détails sur les API pour les actions, les activations, les packages, les règles et les déclencheurs, voir la [documentation de l'API OpenWhisk](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


Toutes les fonctions du système sont disponibles via une API REST. Des noeuds finaux de collection et d'entité sont présents pour les actions, les déclencheurs, les packages, les activations et les espaces de nom.

Les noeuds finaux de collection disponibles sont les suivants :
- `https://{HOTEAPI}/api/v1/namespaces`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/actions`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/triggers`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/rules`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/packages`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/activations`

`{HOTEAPI}` est le nom d'hôte de l'API OpenWhisk (par exemple, openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13, etc.).
Pour `{espaceNom}/`, vous pouvez utiliser le caractère `_` afin de spécifier l'*espace de nom par défaut*
de l'utilisateur.

Vous pouvez lancer une requête GET sur les noeuds finaux de collection pour extraire une liste d'entités dans la collection.

Les noeuds finaux d'entité suivants sont disponibles pour chaque type d'entité :
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/actions/[{nomPackage}/]{nomAction}`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/triggers/{nomDéclencheur}`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/rules/{nomRègle}`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/packages/{nomPackage}`
- `https://{HOTEAPI}/api/v1/namespaces/{espaceNom}/activations/{nomActivation}`

Les noeuds finaux d'espace de nom et d'activation prennent en charge les demandes GET. Les noeuds finaux pour les actions, les déclencheurs, les règles et
les packages prennent en charge les demandes GET, PUT et DELETE. Les noeuds finaux pour les actions, les déclencheurs et les règles prennent également en charge les
demandes POST, lesquelles sont utilisées pour appeler des Actions et des déclencheurs et pour activer ou désactiver des règles. 

Toutes les API sont protégées via une authentification HTTP de base. 
Vous pouvez utiliser l'outil [wskadmin ![ibmcloudône de lien externe](../icons/launch-glyph.svg "ibmcloudône de lien externe")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) pour générer un nouvel espace de nom et une nouvelle authentification.
Les données d'identification pour l'authentification de base figurent dans la propriété
`AUTH` de votre fichier `~/.wskprops` et sont délimitées par un signe deux-points. 
Vous pouvez également extraire ces données d'identification via l'interface de ligne de commande en exécutant `wsk property get --auth`.


Dans l'exemple suivant, l'outil de commande [cURL](https://curl.haxx.se) est utilisé pour extraire la liste de tous les packages présents dans l'espace de nom `whisk.system` : 
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

Dans cet exemple, l'authentification est transmise à l'aide de l'indicateur `-u`. Vous pouvez également transmettre cette valeur dans l'adresse URL, sous la forme `https://$AUTH@{APIHOST}`.

L'API OpenWhisk prend en charge les appels demande-réponse de clients Web. OpenWhisk répond aux demandes `OPTIONS` avec des en-têtes CORS (Cross-Origin Resource Sharing). Actuellement, toutes les origines sont autorisées (la valeur de Access-Control-Allow-Origin est "`*`") et les en-têtes Access-Control-Allow-Header fournissent l'autorisation et le type de contenu.

**Attention :** étant donné qu'OpenWhisk ne prend en charge qu'une seule clé par espace de nom, il n'est pas recommandé d'utiliser CORS au-delà de simples expérimentations. Utilisez les [actions Web](./openwhisk_webactions.html) ou la [passerelle d'API](./openwhisk_apigateway.html) pour exposer vos actions au public et ne pas utiliser la clé d'autorisation OpenWhisk pour les applications client requérant CORS.

## Utilisation du mode prolixe de l'interface de ligne de commande
{: #openwhisk_rest_api_cli_v}

L'interface de ligne de commande OpenWhisk assure l'interface avec l'API REST d'OpenWhisk.
Vous pouvez exécuter l'interface de ligne de commande en mode prolixe avec l'indicateur `-v` pour imprimer toutes les informations sur la demande et la réponse HTTP. 

Affichez la valeur d'espace de nom pour l'utilisateur en cours en exécutant la commande suivante :
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

Les informations imprimées indiquent les propriétés de la demande HTTP ; la méthode HTTP `GET` est exécutée sur l'adresse URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` avec un en-tête User-Agent `OpenWhisk-CLI/1.0 (
<CLI-Build-version>)` et un en-tête d'autorisation de base `Basic XXXYYYY`.
Notez que la valeur d'autorisation est votre chaîne d'autorisation OpenWhisk codée en base64.
Le type de contenu de la réponse est `application/json`.

## Actions
{: #openwhisk_rest_api_actions}

Pour créer ou mettre à jour une action, envoyez une demande HTTP à l'aide de la méthode `PUT` à la collection d'actions. Par exemple, pour créer une action `nodejs:6` avec le nom `hello` en utilisant un contenu de fichier unique, entrez la commande suivante :
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

Pour procéder à un appel bloquant sur une action, envoyer une demande HTTP à l'aide d'une méthode `POST` et un corps contenant le paramètre d'entrée `name`, entrez la commande suivante : 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

Vous obtenez la réponse suivante :
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
Si vous voulez obtenir la partie `response.result`, réexécutez la commande avec le paramètre de requête `result=true`, comme illustré dans l'exemple suivant :
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
Vous obtenez la réponse suivante :
```json
{
  "payload": "hello John"
}
```

## Annotations et actions Web
{: #openwhisk_rest_api_webactions}

Pour créer une action en tant qu'action Web, vous devez ajouter une [annotation](./openwhisk_annotations.html) de `web-export=true` pour les actions Web. Etant donné que les actions Web sont accessibles publiquement, vous souhaitez protéger les paramètres prédéfinis (c'est-à-dire de les traiter en tant que paramètres finaux) à l'aide de l'annotation `final=true`. Si vous créez ou mettez à jour une action à l'aide de l'indicateur d'interface de ligne de commande `--web true`, cette commande ajoute les annotations `web-export=true` et `final=true`.

Exécutez la commande curl suivante pour fournir la liste complète des annotations à définir sur l'action :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

A présent, vous pouvez appeler cette action sous forme d'adresse URL publique sans autorisation OpenWhisk. Essayez de l'appeler en utilisant l'adresse URL publique de l'action Web et en incluant une extension telle que `.json` ou `.http`, par exemple à la fin de l'adresse URL.

```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

Cet exemple de code source ne fonctionne pas avec `.http`. Voir la documentation relative aux [actions Web](./openwhisk_webactions.html) pour savoir comment le modifier.



## Séquences
{: #openwhisk_rest_api_sequences}

Pour créer une séquence d'actions, fournissez les noms des actions devant composer la séquence dans l'ordre souhaité. Ainsi, le résultat de la première action sera transmis en tant qu'entrée à l'action suivante. 

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

Créez une séquence avec les actions `/whisk.system/utils/split` et `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

Sachez que vous devez spécifier des noms d'action qualifiés complets.

## Déclencheurs
{: #openwhisk_rest_api_triggers}

Pour créer un déclencheur, vous devez au minimum indiquer son nom. Vous pouvez aussi inclure des paramètres par défaut qui seront transmis à l'action via une règle lorsque le déclencheur sera exécuté.

Créez un déclencheur dont le nom est `events` avec la valeur `webhook` affectée au paramètre par défaut `type`. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Désormais, à chaque fois qu'un événement devra exécuter ce déclencheur, il suffira d'émettre une demande HTTP avec la méthode `POST` en utilisant la clé d'autorisation OpenWhisk.

Pour exécuter le déclencheur `events` avec un paramètre `temperature`, envoyez la demande HTTP suivante :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Déclencheurs avec actions de flux
{: #openwhisk_rest_api_triggers_feed}

Vous pouvez créer des déclencheurs spéciaux à l'aide d'une action de flux. L'action de flux est une action qui contribue à la configuration d'un fournisseur de flux en charge de l'exécution du déclencheur à chaque fois qu'un événement correspond au déclencheur. Lisez la documentation [feeds.md] pour en savoir plus sur ces fournisseurs de flux.

Certains des déclencheurs disponibles qui optimisent une action de flux sont des déclencheurs périodiques/alarmes, Slack, Github, Cloudant/Couchdb et messageHub/Kafka. Vous pouvez aussi créer votre propre action de flux et votre propre fournisseur de flux.

Créez un déclencheur dont le nom est `periodic` et qui sera déclenché à la fréquence spécifiée, toutes les 2 heures (c'est-à-dire 02:00:00, 04:00:00, ...).

A l'aide de l'interface de ligne de commande, exécutez la commande suivante pour créer le déclencheur :
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

L'indicateur `-v` étant utilisé, deux demandes HTTP sont envoyées. L'une consiste à créer un déclencheur appelé `periodic` et l'autre consiste à appeler l'action de flux. L'action de flux, `/whisk.system/alarms/alarm`, est envoyée aux paramètres afin de configurer le fournisseur de flux pour l'exécution du déclencheur toutes les 2 heures. 

Pour effectuer cette activité avec l'API REST, créez d'abord le déclencheur comme suit :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

Comme vous pouvez le constater, l'annotation `Feed` est stockée dans le déclencheur. Par la suite, cette annotation pourra être utilisée afin d'identifier l'action de flux à utiliser pour supprimer le déclencheur.

Maintenant que le déclencheur a été créé, appelons l'action de flux :
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

L'opération de suppression et de création d'un déclencheur sont similaires. Cette fois-ci, supprimez le déclencheur à l'aide de l'action de flux pour configurer le fournisseur de flux de sorte qu'il supprime également le gestionnaire pour le déclencheur.

Pour appeler l'action de flux afin de supprimer le gestionnaire de déclencheur à partir du fournisseur de flux, exécutez la commande suivante :
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

A présent, supprimez le déclencheur avec une demande HTTP à l'aide de la méthode `DELETE` :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Règles
{: #openwhisk_rest_api_rules}

Pour créer une règle qui associe un déclencheur à une action, envoyez une demande HTTP à l'aide de la méthode `PUT` afin d fournir le déclencheur et l'action dans le corps de la demande.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

Les règles peuvent être activées ou désactivées. De plus, vous pouvez changer le statut de la règle en mettant à jour sa propriété de statut. Par exemple, pour désactiver la règle `t2a`, envoyez `status: "inactive"` dans le corps de la demande à l'aide de la méthode `POST`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Packages
{: #openwhisk_rest_api_packages}

Pour créer une action dans un package, vous devez créer d'abord celui-ci. Créez un package dont le nom est `iot` et envoyez une demande HTTP à l'aide d'une méthode `PUT` en exécutant la commande suivante : 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Activations
{: #openwhisk_rest_api_activations}

Pour obtenir la liste des trois dernières activations, envoyez une demande HTTP à l'aide de la méthode `GET` en transmettant le paramètre de requête `limit=3` comme suit :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

Pour obtenir tous les détails d'une activation incluant les résultats et les journaux, envoyez une demande HTTP à l'aide de la méthode `GET` en transmettant l'identificateur de l'activation comme paramètre de chemin, comme suit :
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
