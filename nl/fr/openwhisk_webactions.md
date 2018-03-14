---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Actions Web
{: #openwhisk_webactions}

Les actions Web sont des actions OpenWhisk qui sont annotées pour permettre aux développeurs de générer rapidement des applications Web. Ces actions Web permettent aux développeurs de programmer une logique de back end à laquelle votre application Web peut accéder de manière anonyme sans avoir besoin de fournir une clé d'authentification OpenWhisk. Il revient au développeur d'action d'implémenter son propre mécanisme d'authentification et ses propres autorisations (c'est-à-dire, son flux de protocole d'autorisation OAuth).
{: shortdesc}

Des activations d'action Web sont associées à l'utilisateur qui a créé l'action. Cette action reporte le coût de l'activation d'une action de l'appelant au propriétaire de l'action.

Examinez l'action JavaScript `hello.js` suivante :
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

Vous pouvez créer une _action Web_ `hello` dans le Package `demo` pour l'espace de nom `guest` à l'aide de l'indicateur `--web` de l'interface de ligne de commande en affectant à ce dernier la valeur `true` ou `yes`:
```
wsk package create demo
```
{: pre}

```
wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

L'utilisation de l'indicateur `--web` avec la valeur `true` ou `yes` rend une action accessible via l'interface REST sans avoir à spécifier des données d'identification. Une action Web peut être appelée via une URL structurée comme suit : `https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

Le nom de package est `default` si l'action n'est pas un package nommé. 

Par exemple, `guest/demo/hello`. Le chemin de l'API d'action Web peut être utilisé avec `curl` ou `wget` sans clé d'API. Il peut même être entré directement dans votre navigateur.

Tentez d'ouvrir [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane) dans votre navigateur Web. Ou essayez d'appeler l'action via `curl` :
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

Dans l'exemple suivant, une action Web effectue une redirection HTTP :
```javascript
function main() {
  return { 
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

Dans l'exemple suivant, une action Web définit un seul cookie :
```javascript
function main() {
  return { 
    headers: { 
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

Dans l'exemple suivant, une action Web définit plusieurs cookies :
```javascript
function main() {
  return { 
    headers: { 
      'Set-Cookie': [
        'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
      ],
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}

L'exemple suivant renvoie un élément `image/png` :
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
```javascript
function main(params) { 
    return {
        statusCode: 200,
        headers: { 'Content-Type': 'application/json' },
        body: params
    };
}
```
{: codeblock}  

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [taille limite de la réponse](./openwhisk_reference.html) des actions car une réponse dont la taille est supérieure à celle prédéfinie par le système échouera. Les objets volumineux ne sont pas envoyés en ligne via OpenWhisk, mais différés vers un conteneur d'objets, par exemple.

## Traitement des demandes HTTP à l'aide d'actions
{: #openwhisk_webactions_http}

Une action OpenWhisk qui n'est pas une Action Web requiert les deux authentifications et doit répondre avec un objet JSON. En revanche, des actions Web peuvent être appelées sans authentification et peuvent être utilisées pour implémenter des gestionnaires HTTP qui répondent avec un contenu _headers_, _statusCode_ et _body_ de différents types. L'action web doit renvoyer un objet JSON. Toutefois, le système OpenWhisk (nommé le `contrôleur`) traite une action Web différemment si son résultat inclut un ou plusieurs des éléments suivants comme propriétés JSON de niveau supérieur :

- `headers` : objet JSON dans lequel les clés sont des noms d'en-tête et les valeurs sont des valeurs de chaîne, numériques ou booléennes pour ces en-têtes (par défaut, il n'y a aucune valeur d'en-tête). Pour que plusieurs valeurs puissent être envoyées pour un seul en-tête, la valeur de l'en-tête est un tableau de valeurs JSON. 
- `statusCode`: code d'état HTTP valide (la valeur par défaut est 200 OK).
- `body` : chaîne qui est un texte en clair ou une chaîne codée en base64 (pour les données binaires).

Le contrôleur transmettra les en-têtes spécifiés par une action, le cas échéant, au client HTTP qui finalise la demande-réponse. De même, le contrôleur répondra avec le code d'état, lorsque celui-ci existe. Enfin, le corps sera transmis en tant que corps de la réponse. Sauf si un en-tête `Content-Type` est déclaré dans la zone `headers` du résultat d'action, le corps est transmis en l'état s'il s'agit d'une chaîne (sinon, une erreur est générée). Lorsque l'élément `Content-Type` est défini, le contrôleur détermine si la réponse contient des données binaires ou un texte en clair et décode la chaîne à l'aide d'un décodeur de base64 si besoin. Si le corps ne peut pas être correctement décodé, une erreur est renvoyée à l'appelant.

_Remarque_ : un objet ou tableau JSON est traité en tant que données binaires et doit être codé en base64. 

## Contexte HTTP

Lorsqu'elles sont appelées, toutes les actions Web reçoivent des informations de demande HTTP en tant que paramètres pour l'argument d'entrée d'action.  

Examinez les paramètres HTTP suivants :

- `__ow_method` (type : chaîne) : méthode HTTP de la demande. 
- `__ow_headers` (type : mappe chaîne à chaîne) : en-têtes de demande. 
- `__ow_path` (type : chaîne) : chemin sans correspondance de la demande (la correspondance cesse une fois l'extension d'action consommée). 
- `__ow_user` (type : chaîne) : espace nom qui identifie l'objet authentifié par OpenWhisk. 
- `__ow_body` (type : chaîne) : entité de corps de demande sous la forme d'une chaîne codée en base64 lorsque le contenu est binaire, sinon, sous la forme d'une chaîne en clair. 
- `__ow_query` (type : chaîne) : paramètres de requête de la demande en tant que chaîne non analysée. 

Une demande ne peut remplacer aucun des paramètres `__ow_` nommés. Si cela se produit, la demande échoue avec l'état 400 Bad Request.

Le paramètre `__ow_user` n'est présent que lorsque l'action Web est [annotée pour indiquer que l'authentification est requise](./openwhisk_annotations.html#openwhisk_annotations_webactions) et permet à une action Web d'implémenter sa propre règle d'autorisation. Le paramètre `__ow_query` n'est disponible que lorsqu'une action Web choisit de traiter la [demande HTTP brute ("raw")](#raw-http-handling). Il s'agit d'une chaîne contenant les paramètres de requête qui sont analysés à partir de l'URI (séparés par `&`). La propriété `__ow_body` est présente dans les demandes HTTP brutes ou lorsque l'entité de demande HTTP n'est pas un objet JSON ni des données de formulaire. Sinon, les actions Web reçoivent les paramètres de requête et de corps comme propriétés de première classe dans les arguments d'action. Les paramètres de corps sont prioritaires sur les paramètres de requête, lesquels sont prioritaires sur les paramètres d'action et de package. 

## Prise en charge de noeud final HTTPS

Protocoles SSL pris en charge : TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3 ([draft version 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Protocoles SSL non pris en charge : SSLv2, SSLv3

## Fonctions supplémentaires
{: #extra-features}

Les actions Web offrent des fonctions supplémentaires, parmi lesquelles :

- Des `extensions de contenu` : la demande doit préciser le type de contenu souhaité, par exemple, `.json`, `.html`, `.http`, `.svg` ou `.text`. Le type est spécifié en ajoutant une extension au nom d'action dans l'URI, ainsi, une action `/guest/demo/hello` est référencée comme `/guest/demo/hello.http`, par exemple, pour recevoir une réponse HTTP en retour. Par souci de commodité, l'extension `.http` est utilisée par défaut lorsqu'aucune extension n'est détectée.
- La `projection de zones à partir du résultat` : le chemin qui suit le nom d'action est utilisé pour projeter un ou plusieurs niveaux de la réponse. `/guest/demo/hello.html/body`. Cette fonction permet à une action qui renvoie un dictionnaire `{body: "..." }` de projeter la propriété `body` et de renvoyer directement sa valeur de chaîne à la place. Le chemin projeté suit un modèle de chemin d'accès absolu (comme dans XPath).
- Des `paramètres de requête et de corps en tant qu'entrées` : l'action reçoit des paramètres de requête, mais également des paramètres dans le corps de la demande. l'ordre de priorité pour la fusion des paramètres est le suivant : paramètres de package, paramètres d'action, paramètres de requête et paramètres de corps. Chacun de ces paramètres peut remplacer les valeurs précédentes en cas de chevauchement. Par exemple, `/guest/demo/hello.http?name=Jane` transmet l'argument `{name: "Jane"}` à l'action.
- Des `données de formulaire` : outre l'élément `application/json` standard, les actions Web peuvent recevoir une URL codée à partir des données  `application/x-www-form-urlencoded` comme entrée.
- Une `activation via plusieurs verbes HTTP` : une action Web peut être appelée via l'une des méthodes HTTP suivantes : `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD` et `OPTIONS`.
- Un `traitement de corps autre que JSON et d'entité HTTP brute` : une action Web peut accepter un corps de demande HTTP autre qu'un objet JSON et peut choisir de toujours recevoir de telles valeurs comme valeurs opaques (texte en clair avec des données non binaires, sinon, chaîne codée en base64).

L'exemple ci-dessous montre de façon sommaire comment utiliser ces fonctions dans une action Web. Imaginons une action `/guest/demo/hello` avec le corps suivant :
```javascript
function main(params) { 
    return { response: params };
}
```

Lorsque cette action est appelée en tant qu'action Web, vous pouvez modifier la réponse de l'action Web en projetant différents chemins à partir du résultat.
Par exemple, pour renvoyer la totalité de l'objet et voir les arguments que l'action reçoit :

```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}
```json
{
  "response": {
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

Pour effectuer une exécution avec un paramètre de requête, examinez l'exemple de commande suivant :
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

Vous pouvez également effectuer une exécution avec des données de formulaire :
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "10",      
      "content-type": "application/x-www-form-urlencoded",      
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

Exécutez la commande suivante pour un objet JSON :
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

Exécutez la commande suivante pour projeter le nom (en tant que texte) :
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}
```
Jane
```

Par souci de commodité, les paramètres de requête, les données de formulaire et les entités de corps d'objet JSON sont tous traités en tant que dictionnaires, car leurs valeurs sont directement accessibles en tant que propriétés d'entrée d'action. Ce n'est pas le cas des actions Web qui choisissent de traiter les entités de demande HTTP plus directement ou lorsque l'action Web reçoit une entité qui n'est pas un objet JSON.

Examinez l'exemple suivant qui utilise un type de contenu "text", comme illustré précédemment :
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}
```json
{
  "response": {
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "4",      
      "content-type": "text/plain",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": "",
    "__ow_body": "Jane"
  }
}
```


## Extensions de contenu
{: #openwhisk_webactions_extensions}

Une extension de contenu est généralement nécessaire pour appeler une action Web. En l'absence d'extension, `.http` est utilisé par défaut. Les extensions `.json` et `.http` ne requièrent pas de chemin de projection, contrairement aux extensions `.html`, `.svg` et `.text`. Par souci de commodité, on considère que le chemin par défaut correspond au nom d'extension. Pour appeler une action Web et recevoir une réponse `.html`, l'action doit répondre avec un objet JSON qui contient une propriété de niveau supérieur nommée `html` (ou bien la réponse doit figurer dans le chemin défini explicitement). En d'autres termes, `/guest/demo/hello.html` revient à projeter la propriété `html` de manière explicite, comme dans `/guest/demo/hello.html/html`. Le nom qualifié complet de l'action doit inclure son nom de package, qui est `default` si l'action ne figure pas dans un package nommé.

## Paramètres protégés
{: #openwhisk_webactions_protected}

Les paramètres d'action sont protégés et traités comme non modifiables. Les paramètres sont finalisés automatiquement pour activer des actions Web.

```
 wsk action create /guest/demo/hello hello.js \
      --parameter name Jane \
      --web true
```

Ces modifications ont pour conséquence de lier `name` à `Jane`, ce qui ne peut pas être remplacé par des paramètres de requête ou de corps en raison de l'annotation finale. Cette conception permet de sécuriser l'action sur les paramètres de requête ou de corps qui tentent de modifier cette valeur de manière intentionnelle ou fortuite. 

## Désactivation d'actions Web

Afin de désactiver l'appel d'une action Web via l'API Web (`https://openwhisk.ng.bluemix.net/api/v1/web/`), attribuez la valeur `false` ou `no` à l'indicateur `--web` pour mettre à jour une action à l'aide de l'interface CLI.

```
 wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## Traitement de demande HTTP brute

Une action Web peut choisir d'interpréter et de traiter directement un corps HTTP entrant sans la promotion d'un objet JSON vers les propriétés de première classe disponibles pour l'entrée d'action (par exemple, `args.name` et `args.__ow_query` d'analyse). Ce processus est effectué via une [annotation](./openwhisk_annotations.html) `raw-http`. Utilisation du même exemple que celui illustré précédemment, mais avec une action Web HTTP brute qui reçoit `name` comme paramètre de requête et comme valeur JSON dans le corps de demande HTTP :
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}' 
```
{: pre}
```json 
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"      
    },
    "__ow_path": ""
  }
}
```

OpenWhisk utilise l'infrastructure [Akka Http](http://doc.akka.io/docs/akka-http/current/scala/http/) pour [déterminer](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html) les types de contenu binaires et les types de contenu de texte en clair.

### Activation du traitement de demande HTTP brute

Les actions Web HTTP brutes sont activées via l'indicateur `--web` en utilisant la valeur `raw`.

```
 wsk action create /guest/demo/hello hello.js --web raw
```

### Désactivation du traitement de demande HTTP brute

La désactivation des actions HTTP brutes est réalisée en attribuant la valeur `false` ou `no` à l'indicateur `--web`.

```
 wsk update create /guest/demo/hello hello.js --web false
```

### Décodage de contenu de code binaire depuis Base64

Lorsqu'un contenu HTTP brut est traité, le contenu `__ow_body` est codé en Base64 lorsque la partie `Content-Type` de la demande est binaire.
Les fonctions ci-dessous montrent comment décoder le contenu du corps dans Node, Python et Swift. Il suffit d'enregistrer une méthode dans un fichier, de créer une action Web HTTP brute qui utilise l'artefact sauvegardé et d'appeler l'action Web.

#### Noeud

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

```swift
extension String {
    func base64Decode() -> String? {
        guard let data = Data(base64Encoded: self) else {
            return nil
        }

        return String(data: data, encoding: .utf8)
    }
}

func main(args: [String:Any]) -> [String:Any] {
    if let body = args["__ow_body"] as? String {
        if let decoded = body.base64Decode() {
            return [ "body" : decoded ]
        }
    }

    return ["body": "Could not decode body from Base64."]
}
```
{: codeblock}

Par exemple, enregistrez la fonction Node en tant que `decode.js` et exécutez les commandes suivantes :
```
 wsk action create decode decode.js --web raw
```
{: pre}

```
ok: created action decode
```

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

```json
{
  "body": "Decoded body"
}
```

## Demandes OPTIONS
{: #options-requests}

Par défaut, une demande OPTIONS adressée à une action Web entraîne l'ajout automatique d'en-têtes CORS aux en-têtes de réponse. Ces en-têtes admettent toutes les origines et les options, ainsi que les termes HTTP get, delete, post, put, head et patch.

Examinez les en-têtes suivants :

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Les demandes OPTIONS peuvent également être traitées manuellement par une action Web. Pour activer cette option, ajoutez une annotation
`web-custom-options` avec la valeur `true` à une action Web. Lorsque cette fonction est activée, des en-têtes CORS ne sont pas ajoutés automatiquement à la réponse à la demande. Il incombe à la place au développeur d'ajouter les en-têtes voulus à l'aide d'un programme. Examinez l'exemple ci-dessous qui illustre la création de réponses personnalisées à des demandes OPTIONS .

```
function main(params) {
  if (params.__ow_method == "options") {
    return {
      headers: {
        'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
      },
      statusCode: 200
    }
  }
}
```

Enregistrez la fonction dans le fichier `custom-options.js` et exécutez les commandes suivantes :

```
$ wsk action create custom-option custom-options.js --web true -a web-custom-options true
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```

## Traitement des erreurs
{: #openwhisk_webactions_errors}

Une action OpenWhisk échoue dans deux modes d'échec possibles. Le permier, nommé _erreur d'application_, est semblable à une exception interceptée : l'action renvoie un objet JSON qui contient une propriété `error` de niveau supérieur. Le second, nommé _erreur de développeur_, est activé lorsque l'action échoue de manière désastreuse et ne génère pas de réponse (semblable à une exception non interceptée). Pour les actions Web, le contrôleur traite les erreurs d'application comme suit :

- Toute projection de chemin spécifiée est ignorée et le contrôleur projette la propriété `error` à la place.
- Le contrôleur applique le traitement de contenu induit par l'extension d'action à la valeur de la propriété `error`.

Les développeurs doivent savoir comment les actions Web peuvent être utilisées et ils doivent générer des réponses d'erreur en conséquence. Par exemple, une action Web qui est utilisée avec l'extension `.http` renvoie une réponse HTTP, par exemple : `{error: { statusCode: 400 }`. Si tel n'est pas le cas, une non-concordance est générée entre le type de contenu (`Content-Type`) induit par l'extension et le type de contenu (`Content-Type`) d'action dans la réponse d'erreur. Une attention particulière doit être accordée aux actions Web qui sont des séquences, de sorte que les composants qui constituent une séquence puissent générer les erreurs appropriées lorsque cela est nécessaire.

