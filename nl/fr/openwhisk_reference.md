---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-15"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Détails du système {{site.data.keyword.openwhisk_short}}
{: #openwhisk_reference}

Les sections ci-après fournissent davantage de détails sur le système {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Entités {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Espaces de nom et packages
{: #openwhisk_entities_namespaces}

Les actions, les déclencheurs et les règles {{site.data.keyword.openwhisk_short}} appartiennent à un espace de nom, et parfois à un
package.

Un package peut contenir des actions et des flux. Il ne peut pas contenir un autre package ; par conséquent, l'imbrication de package n'est pas
autorisée. De plus, les entités ne doivent pas obligatoirement se trouver dans un package.

Dans {{site.data.keyword.Bluemix_notm}}, une paire organisation+espace correspond à un espace de nom {{site.data.keyword.openwhisk_short}}. Par exemple, l'organisation
`BobsOrg` et l'espace `dev` correspondraient à l'espace de nom {{site.data.keyword.openwhisk_short}} `/BobsOrg_dev`.

Vous pouvez créer vos propres espaces de nom si vous y êtes autorisé. L'espace de nom `/whisk.system` est réservé aux entités qui
sont distribuées avec le système {{site.data.keyword.openwhisk_short}}.


### Noms qualifiés complets
{: #openwhisk_entities_fullyqual}

Le nom qualifié complet d'une entité est `/nom_espace_nom[/nom_package]/nom_entité`. Notez que `/` est utilisé pour
délimiter les espaces de nom, les packages et les entités. De plus, les espaces de nom doivent être précédés du préfixe `/`.

Pour des raisons pratiques, l'espace de nom peut être omis s'il s'agit de l'*espace de nom par défaut* de l'utilisateur.

Par exemple, imaginez un utilisateur dont l'espace de nom par défaut est `/myOrg`. Voici des exemples de nom qualifié complet pour
plusieurs entités et leurs alias :

| Nom qualifié complet | Alias | Espace de nom | Package | Nom |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Vous pouvez utiliser ce schéma de dénomination dans l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, entre autres.

### Noms d'entité
{: #openwhisk_entities_names}

Les noms de toutes les entités, notamment les actions, les déclencheurs, les règles, les packages et les espaces de nom sont une séquence de
caractères au format suivant :

* Le premier caractère doit être alphanumérique ou un trait de soulignement.
* Les caractères qui suivent doivent être des caractères alphanumériques, des espaces ou l'un des caractères suivants :
`_`, `@`, `.`,
`-`.
* Le dernier caractère ne peut pas être un espace.

Plus précisément, un nom doit se conformer à l'expression régulière suivante (exprimée d'après la syntaxe de métacaractères Java) : `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Sémantique d'action
{: #openwhisk_semantics}

Les sections ci-après présentent les actions {{site.data.keyword.openwhisk_short}} en détail.

### Caractéristique "sans état"
{: #openwhisk_semantics_stateless}

Les implémentations d'action sont sans état, ou *idempotentes*. Si le système n'applique pas cette propriété, il n'est pas
garanti qu'un état conservé par une action sera disponible pour tous les appels.

De plus, plusieurs instanciations d'une action peuvent exister, chacune possédant son propre état. Un appel d'action peut être envoyé dans n'importe
laquelle de ces instanciations.

### Entrée et sortie d'appel
{: #openwhisk_semantics_invocationio}

L'entrée et la sortie d'une action représentent un dictionnaire de paires clé-valeur. La clé est une chaîne et la valeur est une valeur JSON valide.

### Ordre des appels d'action
{: #openwhisk_ordering}

Les appels d'une action ne sont pas ordonnés. Si l'utilisateur appelle deux fois une action depuis la ligne de commande ou l'API REST, il est
possible que le second appel soit exécuté avant le premier. Si les actions ont des effets secondaires, ceux-ci peuvent être observés dans n'importe
quel ordre.

En outre, l'exécution automatique des actions n'est pas garantie. Deux actions peuvent s'exécuter simultanément et leurs effets secondaires
peuvent s'imbriquer. OpenWhisk ne garantit aucun un modèle de cohérence spécifique quant aux effets secondaires. Les effets secondaires liés à la simultanéité dépendent de l'implémentation.

### Garanties de l'exécution des actions
{: #openwhisk_atmostonce}

Lorsqu'une demande d'appel est reçue, le système enregistre la demande et attribue l'activation.

Le système renvoie un ID d'activation (avec un appel non bloquant) qui confirme que l'appel a été reçu.
Si une défaillance du réseau, ou toute autre panne, survient avant que vous ne receviez une réponse HTTP, il est tout de même possible
qu'{{site.data.keyword.openwhisk_short}} ait reçu et traité la demande.

Le système tente d'appeler l'action une fois, ce qui génère l'un des quatre résultats suivants :
- *success* : l'appel de l'action a abouti. 
- *application error* : l'appel de l'action a abouti, mais l'action a volontairement renvoyé une valeur d'erreur, par exemple lorsqu'une précondition relative aux
arguments n'a pas été satisfaite. 
- *action developer error* : l'action a été appelée mais s'est terminée anormalement ; par exemple, l'action n'a pas détecté d'exception ou il existe une erreur de syntaxe.
- *whisk internal error* : le système n'est pas parvenu à appeler l'action. Le résultat est enregistré dans la zone
`status` de l'enregistrement d'activation, tel que documenté dans une section ultérieure.

Il existe un enregistrement d'activation pour chaque appel reçu et pour lequel l'utilisateur peut être facturé. 

Dans le cas d'une erreur de type *action developer error*, il se peut que l'action ait été exécutée partiellement et
qu'elle ait
généré des effets secondaires externes visibles. Il revient à l'utilisateur de vérifier si de tels effets secondaires ont été générés et d'émettre une
logique de relance s'il le souhaite. Certaines erreurs de type *whisk internal errors* indiquent qu'une action commence à s'exécute, mais échoue que son achèvement ne soit enregistré. 

## Enregistrement d'activation
{: #openwhisk_ref_activation}

Chaque appel d'action et chaque exécution de déclencheur génèrent un enregistrement d'activation.

Un enregistrement d'activation contient les zones suivantes :

- *activationId* : l'ID d'activation.
- *start* et *end* : les horodatages enregistrant le début et la fin de l'activation. Les valeurs sont au
[format de temps UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* et `name` : espace de nom et nom de l'entité.
- *logs* : tableau de chaînes indiquant les journaux qui sont générés par l'action au cours de son activation. Chaque élément de tableau
correspond à une ligne générée dans `stdout` ou `stderr` par l'action et inclut l'heure et le flux de la sortie de journal. La
structure est la suivante : `HORODATAGE FLUX : JOURNAL_SORTIE`.
- *response* : dictionnaire qui définit les clés `success`, `status` et `result` :
  - *status* : résultat de l'activation, qui peut être "success",
"application error", "action developer
error", "whisk internal error".
  - *success* : `true` si et seulement si le statut est `"success"`.
- *result* : dictionnaire contenant le résultat de l'activation. Si l'activation a abouti, le résultat contient la valeur qui est renvoyée par l'action. Si
l'activation a échoué, `result` contient la clé `error`, généralement accompagnée d'une explication de l'échec.


## Actions JavaScript
{: #openwhisk_ref_javascript}

### Prototype de fonction
{: #openwhisk_ref_javascript_fnproto}

Les actions {{site.data.keyword.openwhisk_short}} JavaScript s'exécutent dans un contexte d'exécution Node.js.

Les actions écrites en JavaScript doivent se trouver dans un seul fichier. Ce dernier peut contenir plusieurs fonctions mais par convention, une fonction appelée `main` doit exister ; c'est celle qui est appelée lorsque l'action est appelée. Voici un exemple d'action avec plusieurs fonctions :
```javascript
function main() {
    return { payload: helper() }
} function helper() {
    return new Date();
}
```
{: codeblock}

Les paramètres d'entrée de l'action sont transmis sous forme d'objet JSON en tant que paramètre à la fonction `main`. Le résultat d'une activation réussie est également un objet JSON ; toutefois, il est renvoyé différemment selon que l'action est synchrone ou asynchrone, comme décrit dans la section suivante.


### Comportement synchrone et asynchrone
{: #openwhisk_ref_javascript_synchasynch}

Il est fréquent que l'exécution des fonctions JavaScript continue dans une fonction de rappel même après leur retour. En conséquence, l'activation
d'une action JavaScript peut être *synchrone* ou *asynchrone*.

L'activation d'une action JavaScript est **synchrone** si la fonction main se termine dans l'une des conditions suivantes :

- La fonction main se termine sans exécuter d'instruction `return`.
- La fonction main se termine en exécutant une instruction `return` qui renvoie n'importe quelle valeur *sauf* une
promesse (objet Promise).

Exemple d'action synchrone :

```javascript
// an Action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

L'activation d'une action JavaScript est **asynchrone** si la fonction main se termine en renvoyant une promesse (objet Promise). Dans ce cas, le système suppose que l'action est toujours en cours d'exécution jusqu'à ce que la promesse (objet Promise) soit satisfaite ou rejetée.
Commencez par instancier une nouvelle promesse (objet Promise) que vous transmettez à une fonction de rappel. La fonction de rappel admet deux arguments, resolve et reject, qui sont tous les deux des fonctions. Tout le code asynchrone est inséré dans ce rappel.

Voici un exemple illustrant la manière de satisfaire une promesse (objet Promise) en appelant la fonction resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

Voici un exemple illustrant la manière de rejeter une promesse (objet Promise) en appelant la fonction reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

Une action peut être synchrone pour certaines entrées et asynchrone pour d'autres, comme illustré dans l'exemple suivant : 
```javascript
  function main(params) {
      if (params.payload) {
         // asynchronous activation
         return new Promise(function(resolve, reject) {
                setTimeout(function() {
                  resolve({ done: true });
       }, 100);
    })
      } else {
         // synchronous activation
         return {done: true};
      }
  }
```
{: codeblock}

Que l'activation soit synchrone ou asynchrone, l'appel de l'action peut être bloquant ou non bloquant.

### Suppression de l'objet global whisk JavaScript

L'objet global `whisk` a été retiré. Faites migrer vos actions nodejs de manière à utiliser des méthodes alternatives.
Pour les fonctions `whisk.invoke()` et `whisk.trigger()`, utilisez la bibliothèque client [openwhisk](https://www.npmjs.com/package/openwhisk) déjà installée.
Pour `whisk.getAuthKey()`, vous pouvez extraire la valeur de la clé d'API à partir de la variable d'environnement `__OW_API_KEY`.
Pour `whisk.error()`, vous pouvez renvoyer une promesse rejetée (c'est-à-dire, Promise.reject).

### Environnements d'exécution JavaScript
{: #openwhisk_ref_javascript_environments}

Les actions JavaScript peuvent être exécutées dans Node.js version 6 ou Node.js version 8.
Actuellement, les actions sont exécutées par défaut dans un environnement Node.js version 6.11.4.   

### Environnement Node.js version 6
{: #openwhisk_ref_javascript_environments_6}
L'environnement Node.js 6.12.2 est utilisé si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:6` lors de la création ou de la mise à jour d'une action. 

Les packages suivants sont disponibles pour être utilisés dans l'environnement Node.js 6.12.2 :

- [apn v2.1.2](https://www.npmjs.com/package/apn) - Module Node.js qui permet d'interagir avec le service Apple Push Notification. 
- [async v2.1.4](https://www.npmjs.com/package/async) - Fournit des fonctions permettant de gérer des fonctions asynchrones. 
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - Port de la fonction btoa du navigateur. 
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - Implémentation rapide, flexible et allégée de jQuery conçue spécifiquement pour le serveur. 
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - Bibliothèque Cloudant officielle pour Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - Solution complète pour les interfaces de ligne de commande Node.js. 
- [consul v0.27.0](https://www.npmjs.com/package/consul) - Client pour Consul, impliquant la configuration et la reconnaissance de service. 
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Faire une analyse syntaxique d'un en-tête de cookie et remplir req.cookies avec un objet indexé par les noms de cookie. 
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Client CouchDB de mise en cache de haut niveau pour Node.js.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware de gestionnaire d'erreurs uniquement pour le développement. 
- [glob v7.1.1](https://www.npmjs.com/package/glob) - Faire correspondre des fichiers en utilisant des modèles que l'interpréteur de commandes utilise, tels que des étoiles et du contenu. 
- [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick et ImageMagick pour Node.js .
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - Bibliothèque Lodash exportée sous la forme de modules Node.js. 
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - Conversion de l'infrastructure log4js conçue pour fonctionner avec Node.js. 
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - Conversion de codage de caractères Pure JS. 
- [marked v0.3.6](https://www.npmjs.com/package/marked) - Programme d'analyse syntaxique et de compilation de démarque avec fonctions complètes, écrit en JavaScript. Conçu pour la vitesse. 
- [merge v1.2.0](https://www.npmjs.com/package/merge) - Fusionner plusieurs objets en un seul afin de créer un nouvel objet cloné.  
- [moment v2.17.0](https://www.npmjs.com/package/moment) - Bibliothèque de date JavaScript simple pour l'analyse syntaxique, la validation, la manipulation et le formatage de dates. 
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Pilote MongoDB officiel pour Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js est une implémentation du système de modèle Mustache en JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Pilote couchdb minimaliste pour Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - Identificateur unique universel obsolète conditionné.  
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Envoyer des courriers électroniques depuis Node.js, c'est du gâteau !
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Module complet, conforme et bien testé permettant d'implémenter un fournisseur/serveur OAuth2 avec Express dans Node.js. 
- [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk. 
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud est une bibliothèque standard pour Node.js qui élimine les différences entre plusieurs fournisseurs de cloud. 
- [process v0.11.9](https://www.npmjs.com/package/process) - ('process') est requis, comme pour n'importe quel autre module.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Implémente le langage de création de modèles Pug. 
- [redis v2.6.3](https://www.npmjs.com/package/redis) - Client Redis complet et riche en fonctionnalités pour Node.js. 
- [request v2.79.0](https://www.npmjs.com/package/request) - Méthode la plus simple pour passer des appels HTTP. 
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - Demande client de demande HTTP simplifiée avec le support Promise. Optimisée pour Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - Commande UNIX rm -rf pour Node.js. 
- [semver v5.3.0](https://www.npmjs.com/package/semver) - Prend en charge la gestion des versions sémantique. 
- [endgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - Fournit la prise en charge par courrier électronique via l'API SendGrid. 
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js pour servir un favicon.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO active une communication bidirectionnelle en temps réel basée sur des événements. 
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Prise en charge côté client de Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent est une bibliothèque de demande HTTP côté client progressive de petite taille et un module Node.js avec la même API, arborant un grand nombre de fonctions client HTTP de haut niveau. 
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - Outils liés à la gestion de Swagger permettant de documenter des API. 
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - Créateur de répertoire et de fichier temporaire simple pour node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - Encapsuleur pour l'API Twilio, lié aux fonctions vocales et vidéo et à la messagerie. 
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js est une bibliothèque d'utilitaires pour JavaScript qui prend en charge les suspects fonctionnels habituels (each, map, reduce, filter...) sans étendre aucun objet JavaScript de mémoire système. 
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - Génération simple et rapide d'identificateurs uniques universels RFC4122. 
- [validator v6.1.0](https://www.npmjs.com/package/validator) - Bibliothèque de valideurs et de nettoyeurs de chaînes. 
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - Bibliothèque client Node.js permettant d'utiliser les services Watson Developer Cloud, collection d'API qui utilisent l'informatique cognitive pour résoudre des problèmes complexes. 
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js est une implémentation Promises/A+ et when() inébranlable, éprouvée, comprenant un modificateur ES6 Promise complet. 
- [winston v2.3.0](https://www.npmjs.com/package/winston) - Bibliothèque de consignation asynchrone multitransport pour Node.js. "CHILL WINSTON! ... I put it in the logs."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws est une implémentation client et serveur WebSocket facile à utiliser, ultra rapide et soigneusement testée. 
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - Convertisseur d'objet XML en JavaScript simple. Il prend en charge la conversion bidirectionnelle.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest est un encapsuleur pour le client HTTP intégré afin d'émuler l'objet XMLHttpRequest de navigateur. 
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Encore une autre bibliothèque de décompression pour Node.js. Pour compression. 

### Environnement Node.js version 8
{: #openwhisk_ref_javascript_environments_8}
L'environnement Node.js version 8.9.3 est également utilisé si l'indicateur `--kind` est explicitement spécifié avec
la valeur `nodejs:8` lors de la création/mise à jour d'une action.


Les packages suivants sont préinstallés dans l'environnement Node.js version 8.9.3 :

  - [apn v2.1.5](https://www.npmjs.com/package/apn) - Module Node.js qui permet d'interagir avec le service Apple Push Notification. 
  - [async v2.6.0](https://www.npmjs.com/package/async) - Fournit des fonctions permettant de gérer des fonctions asynchrones. 
  - [bent v1.1.0](https://www.npmjs.com/package/btoa) - Port de la fonction btoa du navigateur. 
  - [btoa v1.1.2](https://www.npmjs.com/package/btoa) - Port de la fonction btoa du navigateur. 
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - Bibliothèque Cloudant officielle pour Node.js.
  - [commander v2.12.2](https://www.npmjs.com/package/commander) - Solution complète pour les interfaces de ligne de commande Node.js. 
  - [consul v0.30.0](https://www.npmjs.com/package/consul) - Client pour Consul, impliquant la configuration et la reconnaissance de service. 
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Faire une analyse syntaxique d'un en-tête de cookie et remplir req.cookies avec un objet indexé par les noms de cookie. 
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Client CouchDB de mise en cache de haut niveau pour Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware de gestionnaire d'erreurs uniquement pour le développement. 
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - Faire correspondre des fichiers en utilisant des modèles que l'interpréteur de commandes utilise, tels que des étoiles et du contenu. 
  - [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick et ImageMagick pour Node.js .
  - [ibm-cos-sdk v1.1.1](https://www.npmjs.com/package/ibm-cos-sdk) - SDK IBM Cloud Object Storage pour Node.js
  - [ibm_db v2.2.1](https://www.npmjs.com/package/ibm_db) - Interface asynchrone/synchrone pour Node.js avec IBM DB2 et IBM Informix. 
  - [lodash v4.17.4](https://www.npmjs.com/package/lodash) - Bibliothèque Lodash exportée sous la forme de modules Node.js. 
  - [log4js v2.3.12](https://www.npmjs.com/package/log4js) - Conversion de l'infrastructure log4js conçue pour fonctionner avec Node.js.
  - [iconv-lite v0.4.19](https://www.npmjs.com/package/iconv-lite) - Conversion de codage de caractères Pure JS. 
  - [marked v0.3.7](https://www.npmjs.com/package/marked) - Programme d'analyse syntaxique et de compilation de démarque avec fonctions complètes, écrit en JavaScript. Conçu pour la vitesse. 
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - Fusionner plusieurs objets en un seul en créant éventuellement un nouvel objet cloné. 
  - [moment v2.19.3](https://www.npmjs.com/package/moment) - Bibliothèque de date JavaScript simple pour l'analyse syntaxique, la validation, la manipulation et le formatage de dates. 
  - [mongodb v2.2.33](https://www.npmjs.com/package/mongodb) - Pilote MongoDB officiel pour Node.js.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js est une implémentation du système de modèle Mustache en JavaScript.
  - [nano v6.4.2](https://www.npmjs.com/package/nano) - Pilote couchdb minimaliste pour Node.js.
  - [nodemailer v4.4.1](https://www.npmjs.com/package/nodemailer) - Envoyer des courriers électroniques depuis Node.js, c'est du gâteau !
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Module complet, conforme et bien testé permettant d'implémenter un fournisseur/serveur OAuth2 avec Express dans Node.js. 
  - [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk. 
  - [process v0.11.10](https://www.npmjs.com/package/process) - ('process') est requis, comme pour n'importe quel autre module.
  - [pug v2.0.0-rc.4](https://www.npmjs.com/package/pug) - Implémente le langage de création de modèles Pug. 
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - Client Redis complet et riche en fonctionnalités pour Node.js.
  - [request v2.83.0](https://www.npmjs.com/package/request) - Méthode la plus simple pour passer des appels HTTP. 
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - Demande client de demande HTTP simplifiée avec le support Promise. Optimisée pour Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - Commande UNIX rm -rf pour Node.js. 
  - [semver v5.4.1](https://www.npmjs.com/package/semver) - Prend en charge la gestion des versions sémantique. 
  - [@sendgrid/mail@6.1.4](https://www.npmjs.com/package/@sendgrid/mail) - Fournit la prise en charge par courrier électronique via l'API SendGrid. 
  - [serve-favicon v2.4.5](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js pour servir un favicon.
  - [socket.io v2.0.4](https://www.npmjs.com/package/socket.io) - Socket.IO active une communication bidirectionnelle en temps réel basée sur des événements. 
  - [socket.io-client v2.0.4](https://www.npmjs.com/package/socket.io-client) - Prise en charge côté client de Socket.IO.
  - [superagent v3.8.2](https://www.npmjs.com/package/superagent) - SuperAgent est une bibliothèque de demande HTTP côté client progressive de petite taille et un module Node.js avec la même API, arborant un grand nombre de fonctions client HTTP de haut niveau. 
  - [swagger-tools v0.10.3](https://www.npmjs.com/package/swagger-tools) - Outils liés à la gestion de Swagger permettant de documenter des API. 
  - [tmp v0.0.33](https://www.npmjs.com/package/tmp) - Créateur de répertoire et de fichier temporaire simple pour node.js.
  - [twilio v3.10.1](https://www.npmjs.com/package/twilio) - Encapsuleur pour l'API Twilio, lié aux fonctions vocales et vidéo et à la messagerie. 
  - [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js est une bibliothèque d'utilitaires pour JavaScript qui prend en charge les suspects fonctionnels habituels (each, map, reduce, filter...) sans étendre aucun objet JavaScript de mémoire système. 
  - [uuid v3.1.0](https://www.npmjs.com/package/uuid) - Génération simple et rapide d'identificateurs uniques universels RFC4122. 
  - [validator v9.2.0](https://www.npmjs.com/package/validator) - Bibliothèque de valideurs et de nettoyeurs de chaînes. 
  - [watson-developer-cloud v3.0.2](https://www.npmjs.com/package/watson-developer-cloud) - Bibliothèque client Node.js permettant d'utiliser les services Watson Developer Cloud, collection d'API qui utilisent l'informatique cognitive pour résoudre des problèmes complexes. 
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js est une implémentation Promises/A+ et when() inébranlable, éprouvée, comprenant un modificateur ES6 Promise complet. 
  - [winston v2.4.0](https://www.npmjs.com/package/winston) - Bibliothèque de consignation asynchrone multitransport pour Node.js. "CHILL WINSTON! ... I put it in the logs."
  - [ws v3.3.1](https://www.npmjs.com/package/ws) - ws est une implémentation client et serveur WebSocket facile à utiliser, ultra rapide et soigneusement testée. 
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Convertisseur d'objet XML en JavaScript simple. Il prend en charge la conversion bidirectionnelle.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest est un encapsuleur pour le client HTTP intégré afin d'émuler l'objet XMLHttpRequest de navigateur. 
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - Encore une autre bibliothèque de décompression pour Node.js. Pour compression. 

### Conditionnement de packages npm avec vos actions
Vous pouvez regrouper les packages `npm` non pré-installés dans l'environnement Node.js en tant que dépendances lorsque vous créez ou mettez à jour votre action. 

Pour plus d'informations, voir [Conditionnement d'une action en tant que module Node.js](./openwhisk_actions.html#openwhisk_js_packaged_action) ou [Conditionnement d'une action en tant que bundle unique](./openwhisk_actions.html#openwhisk_js_webpack_action).


## Environnements d'exécution Python
{: #openwhisk_ref_python_environments}

OpenWhisk prend en charge l'exécution d'actions Python sous deux versions d'environnement d'exécution différentes.


### Actions Python 3 (basées sur Jessie)
{: #openwhisk_ref_python_environments_jessie}

Les actions Python 3 sont exécutées avec Python 3.6.4. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python-jessie:3` `wsk` dans l'interface CLI lorsque vous créez ou mettez à jour une action.
Lors de la création d'actions Python à l'aide de virtualenv, utilisez l'image Docker `ibmfunctions/action-python-v3`.
L'environnement d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.6 standard. 

Version Python :
- [3.6.4](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Packages Python :
- asn1crypto (0.24.0)
- attrs (17.4.0)
- Automat (0.6.0)
- beautifulsoup4 (4.6.0)
- certifi (2017.11.5)
- cffi (1.11.4)
- chardet (3.0.4)
- click (6.7)
- cloudant (2.7.0)
- constantly (15.1.0)
- cryptography (2.1.4)
- cssselect (1.0.3)
- docutils (0.14)
- Flask (0.12.2)
- gevent (1.2.2)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- hyperlink (17.3.1)
- ibm-cos-sdk (2.0.0)
- ibm-db (2.0.8a0)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.10)
- jmespath (0.9.3)
- kafka-python (1.3.5)
- lxml (4.1.1)
- MarkupSafe (1.0)
- numpy (1.14.0)
- pandas (0.22.0)
- parsel (1.3.1)
- pip (9.0.1)
- pyasn1 (0.4.2)
- pyasn1-modules (0.2.1)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.5.0)
- pysolr (3.7.0)
- python-dateutil (2.6.1)
- pytz (2017.3)
- queuelib (1.4.2)
- requests (2.18.4)
- scikit-learn (0.19.1)
- scipy (1.0.0)
- Scrapy (1.5.0)
- service-identity (17.0.0)
- setuptools (38.4.0)
- simplejson (3.13.2)
- six (1.11.0)
- Twisted (17.9.0)
- urllib3 (1.22)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- watson-developer-cloud (1.0.2)
- Werkzeug (0.14.1)
- wheel (0.30.0)
- zope.interface (4.4.3)


### Actions Python 3 (basées sur Alpine)
{: #openwhisk_ref_python_environments_alpine}

Les actions Python 3 sont exécutées avec Python 3.6.1. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python:3` `wsk` dans l'interface CLI lorsque vous créez ou mettez à jour une action.
Lors de la création d'actions Python à l'aide de virtualenv, utilisez l'image Docker `openwhisk/python3action`.
Les packages suivants sont disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.6 standard. 

- asn1crypto (0.23.0)
- attrs (17.3.0)
- Automat (0.6.0)
- beautifulsoup4 (4.5.3)
- cffi (1.11.2)
- click (6.7)
- constantly (15.1.0)
- cryptography (2.1.3)
- cssselect (1.0.1)
- Flask (0.12)
- gevent (1.2.1)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.4)
- lxml (3.7.3)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.5)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.6.0)
- queuelib (1.4.2)
- requests (2.13.0)
- Scrapy (1.3.3)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.10.0)
- six (1.11.0)
- Twisted (17.1.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)


### Actions Python 2

Les actions Python 2 sont exécutées avec Python 2.7.12, environnement d'exécution par défaut pour les actions Python, sauf si vous spécifiez l'indicateur `--kind` lors de la création ou de la mise à jour d'une action. Pour sélectionner explicitement cet environnement, utilisez l'instruction `--kind python:2`. 
Lors de la création d'actions Python à l'aide de virtualenv, utilisez l'image Docker `openwhisk/python2action`.
Les packages suivants sont disponibles pour être utilisés par des actions Python 2, en supplément des bibliothèques Python 2.7 standard. 

- asn1crypto (0.23.0)
- attrs (17.2.0)
- beautifulsoup4 (4.5.1)
- cffi (1.11.1)
- click (6.7)
- cryptography (2.0.3)
- cssselect (1.0.1)
- enum34 (1.1.6)
- Flask (0.11.1)
- gevent (1.1.2)
- greenlet (0.4.12)
- httplib2 (0.9.2)
- idna (2.6)
- ipaddress (1.0.18)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.1)
- lxml (3.6.4)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.4)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.5.3)
- queuelib (1.4.2)
- requests (2.11.1)
- Scrapy (1.1.2)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.8.2)
- six (1.11.0)
- Twisted (16.4.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)

## Actions Swift
{: #openwhisk_ref_swift3}

### Swift 3
Les actions Swift 3 sont exécutées avec Swift 3.1.1 `--kind swift:3.1.1`. Spécifiez toujours le type `swift:3.1.1` car les versions précédentes de Swift ne sont pas prises en charge.


Vous devez faire migrer toutes les actions Swift pour utiliser le type `swift:3.1.1`. La meilleure pratique consiste à indiquer systématiquement le type spécifique lors de la création ou de mise à jour d'actions.
{: tip}

Les actions Swift 3.1.1 peuvent utiliser les packages suivants :
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- SDK Watson Developer Cloud version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

## Actions PHP
{: #openwhisk_ref_php}

Les Actions PHP sont exécutées avec PHP 7.1. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind php:7.1` `wsk` dans l'interface de ligne de commande lorsque vous créez ou mettez à jour une action. Il s'agit du comportement par défaut lorsque vous créez une action avec un fichier doté de l'extension `.php`.

Les extensions PHP suivantes sont disponibles en plus des extensions standard :

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### Packages Composer
Les packages Composer suivants sont également disponibles :

- guzzlehttp/guzzle       v6.3.0
- ramsey/uuid             v3.6.1

## Actions Docker
{: #openwhisk_ref_docker}

Les actions Docker exécutent un fichier binaire fourni par l'utilisateur dans un conteneur Docker. Le fichier binaire s'exécute dans une image Docker
reposant sur [python:2.7.12-alpine](https://hub.docker.com/r/library/python) ; par conséquent, il doit être compatible avec cette
distribution.

Le squelette Docker est pratique pour générer des images Docker compatibles avec OpenWhisk. Vous pouvez l'installer avec la commande d'interface de ligne de commande `wsk sdk install docker`.

Le programme binaire principal doit se trouver dans `/action/exec` à l'intérieur du conteneur. L'exécutable reçoit les arguments d'entrée via une chaîne d'argument de ligne de commande qui peut être désérialisée en tant qu'objet `JSON`. Il doit renvoyer un résultat via `stdout` sous la forme d'une chaîne d'une ligne `JSON` sérialisée.

Vous pouvez inclure des étapes de compilation ou des dépendances en modifiant le document `Dockerfile` inclus dans `dockerSkeleton`.

## API REST
{: #openwhisk_ref_restapi}
Vous trouverez des informations sur l'API REST [ici](openwhisk_rest_api.html).

## Limites du système
{: #openwhisk_syslimits}

### Actions
{{site.data.keyword.openwhisk_short}} présente quelques limites relatives au système, notamment la quantité de mémoire qu'une action
peut utiliser et le nombre d'appels d'action autorisés par minute.

Le tableau ci-dessous répertorie les limites par défaut pour les actions.

| Limite| Description | Par défaut| Min | Max | 
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | Taille maximale (en Mo) du code d'action. | 48 | 1 | 48 |
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | N activations au maximum peuvent être soumises par espace de nom, que ce soit pour être exécutées ou être placées en file d'attente pour exécution. | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | Un conteneur ne peut pas écrire plus de N Mo de données dans la sortie standard. | 10 | 0 | 10 |
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory) | Un conteneur ne peut pas allouer plus de N Mo de mémoire. | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | N activations au maximum peuvent être soumises par minute par espace de nom. | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | Nombre maximal de fichiers ouverts pour une action Docker. | 64 | 0 | 64 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | Taille maximale (en Mo) des paramètres qui peuvent être joints. | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | Nombre maximal de processus disponibles pour une action Docker. | 512 | 0 | 512 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | Taille maximale (en Mo) du résultat d'appel d'action. | 1 | 0 | 1 |
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) | La durée d'exécution maximale d'un conteneur est limitée à N millisecondes. | 60000 | 100 | 300000|

### Augmentation des limites fixes
{: #increase_fixed_limit}

Les valeurs limite qui se terminent par un astérisque (*) sont fixes, mais il est possible de les augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Si vous souhaitez augmenter la valeur limite, contactez le support IBM en ouvrant directement un ticket à partir de la [console Web {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Sélectionnez **Support**
  2. Sélectionnez **Ajouter un ticket** dans le menu déroulant. 
  3. Sélectionnez **Technique** pour le type de ticket. 
  4. Sélectionnez **Fonctions** pour le domaine technique de support. 

#### codeSize (Mo) (Fixe : 48 Mo)
{: #openwhisk_syslimits_codesize}
* La taille de code maximale pour l'action est 48 Mo.
* Pour une action JavaScript, il est recommandé d'utiliser un outil permettant de concaténer tout le code source, y compris les dépendances, dans un fichier regroupé unique.
* Cette limite est fixe et ne peut pas être modifiée.

#### concurrent (Fixe : 1000*)
{: #openwhisk_syslimits_concurrent}
* Le nombre d'activations qui sont exécutées ou mises en file d'attente pour exécution pour un espace de nom ne peut pas être supérieur à 1000.
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](openwhisk_reference.html#increase_fixed_limit). 

#### logs (Mo) (Valeur par défaut : 10 Mo)
{: #openwhisk_syslimits_logs}
* La limite de journal N est comprise dans la plage [0 Mo - 10 Mo] et est définie par action.
* Un utilisateur peut modifier la limite de journal des actions lorsqu'une action est créée ou mise à jour. 
* Les journaux qui dépassent la limite définie sont tronqués, par conséquent, tout nouvelle entrée de journal est ignorée, et un avertissement est ajouté comme dernière sortie de l'activation pour indiquer que celle-ci a dépassé la limite de journal définie.

#### memory (Mo) (Valeur par défaut : 256 Mo)
{: #openwhisk_syslimits_memory}
* La limite de mémoire M est comprise dans la plage [128 Mo - 512 Mo] et est définie par action en Mo.
* Un utilisateur peut modifier la limite de mémoire lorsqu'une action est créée. 
* Un conteneur ne peut pas utiliser plus de mémoire que celle qui est allouée par la limite.

#### minuteRate (Fixe : 5000*)
{: #openwhisk_syslimits_minuterate}
* La limite de débit N est 5000 et limite le nombre d'appels d'action dans des fenêtres d'une minute.
* Un appel d'interface de ligne de commande ou API dépassant cette limite reçoit un code d'erreur correspondant au code de statut HTTP `429:
TOO MANY REQUESTS`.
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](openwhisk_reference.html#increase_fixed_limit). 

#### openulimit (Fixe : 1024:1024)
{: #openwhisk_syslimits_openulimit}
* Le nombre maximal de fichiers ouverts pour une action Docker est 1024 (pour les limites absolues et les limites souples).
* Cette limite est fixe et ne peut pas être modifiée.
* La commande docker run utilise l'argument `--ulimit nofile=1024:1024`.
* Pour plus d'informations, voir la documentation de référence relative à la ligne de commande [docker run](https://docs.docker.com/engine/reference/commandline/run). 

#### parameters (Fixe : 1 Mo)
{: #openwhisk_syslimits_parameters}
* La taille limite pour le nombre total de paramètres lors de la création ou de la mise à jour d'une action, d'un package ou d'un déclencheur est 1 Mo.
* Une entité comportant des paramètres dont la taille est trop élevée est rejetée lorsqu'elle fait l'objet d'une tentative de création ou de mise à jour.
* Cette limite est fixe et ne peut pas être modifiée.

#### proculimit (Fixe : 1024:1024)
{: #openwhisk_syslimits_proculimit}
* Le nombre maximal de processus disponibles pour le conteneur d'action Docker est 1024. 
* Cette limite est fixe et ne peut pas être modifiée.
* La commande docker run utilise l'argument `--pids-limit 1024`.
* Pour plus d'informations, voir la documentation de référence relative à la ligne de commande [docker run](https://docs.docker.com/engine/reference/commandline/run). 

#### result (Fixe : 1 Mo)
{: #openwhisk_syslimits_result}
* Taille maximale (en Mo) du résultat d'appel d'action. 
* Cette limite est fixe et ne peut pas être modifiée.

#### timeout (ms) (Valeur par défaut : 60s)
{: #openwhisk_syslimits_timeout}
* La limite de délai d'exécution N est comprise dans la plage [100 ms - 300000 ms] et est définie par action en millisecondes.
* Un utilisateur peut modifier la limite de délai d'exécution lorsqu'une action est créée. 
* Un conteneur dont l'exécution dure plus longtemps que N millisecondes est arrêté.

### Déclencheurs

Les déclencheurs sont soumis à un débit de déclenchement par minute, comme indiqué dans le tableau ci-dessous.

| Limite| Description | Par défaut| Min | Max | 
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | N déclencheurs au maximum peuvent être lancés par minute par espace de nom. | 5000* | 5000* | 5000* |

### Augmentation des limites fixes
{: #increase_fixed_tlimit}

Les valeurs limite qui se terminent par un astérisque (*) sont fixes, mais il est possible de les augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Si vous souhaitez augmenter la valeur limite, contactez le support IBM en ouvrant directement un ticket à partir de la [console Web {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Sélectionnez **Support**
  2. Sélectionnez **Ajouter un ticket** dans le menu déroulant. 
  3. Sélectionnez **Technique** pour le type de ticket. 
  4. Sélectionnez **Fonctions** pour le domaine technique de support. 

#### minuteRate (Fixe : 5000*)
{: #openwhisk_syslimits_tminuterate}

* La limite de débit N est 5000 et limite le nombre de déclencheurs qu'un utilisateur peut activer dans des fenêtres d'une minute.
* Un utilisateur ne peut pas modifier la limite de déclencheurs lorsqu'un déclencheur est créé.
* Un appel d'interface de ligne de commande ou API dépassant cette limite reçoit un code d'erreur correspondant au code de statut HTTP `429:
TOO MANY REQUESTS`.
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](openwhisk_reference.html#increase_fixed_tlimit). 
