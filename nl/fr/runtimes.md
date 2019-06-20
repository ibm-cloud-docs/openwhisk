---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# Contextes d’exécution
{: #runtimes}
Vos applications doivent être codées et exécutées dans des langages de programmation tels que JavaScript ou Python. De nombreux contextes d'exécution sont disponibles par défaut avec {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Affichez les contextes d'exécution disponibles pour IBM Cloud Functions dans chaque région. Les liens suivants renvoient une réponse JSON.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [ eu-de ](https://eu-de.functions.cloud.ibm.com/)

La section `runtimes` de la réponse contient l'ensemble des contextes d'exécution disponibles.

La section `image` contient le nom de l'image du contexte d'exécution sur [Docker Hub](https://hub.docker.com/) et de l'étiquette utilisée. 


Les exemples suivants pointent vers les images `ibmfunctions/action-nodejs-v10` et `openwhisk/nodejs8action`.
Les étiquettes peuvent être des numéros de version comme `1.9.0` ou la forme abrégée d'un hachage de validation de Git, comme `b99d71e`.

Exemples de zones d'image.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Les contextes d'exécution sont mis à jour régulièrement. Ces mises à jour incluent des correctifs de sécurité et des mises à jour de version mineures pour les packages dans les contextes d'exécution. Les mises à jour de version mineures peuvent introduire des ruptures de compatibilité avec les versions antérieures. Les mises à jour de contexte d'exécution peuvent avoir un impact sur vos actions. Vous devez effectuer la migration des actions qui s'exécutent dans un contexte d'exécution à une nouvelle version en procédant à une mise à jour.

Les applications qui s'exécutent dans des contextes d'exécution obsolètes ne peuvent pas être finalisées tant que le contexte d'exécution n'est pas mis à jour à une version prise en charge. Lors du traitement des incidents d'une action défaillante, pour identifier si un contexte d'exécution est obsolète, recherchez `deprecated=true` dans la réponse de requête. Pour mettre à jour le contexte d'exécution, voir [Modification du contexte d'exécution d'action](/docs/openwhisk?topic=cloud-functions-actions#actions_update).

Ces contextes d'exécution sont obsolètes :
<ul>
  <li><code>nodejs:6</code> (obsolète)</li>
  <li><code>php:7.1</code> (obsolète)</li>
  <li><code>php:7.2</code> (obsolète)</li>
  <li><code>swift:3</code> (obsolète)</li>
  <li><code>swift:3.1.1</code> (obsolète)</li>
  <li><code>swift:4.1</code> (obsolète)</li>
  <li><code>ballerina:0.990</code> (obsolète)</li>
</ul>






## Contextes d'exécution JavaScript
{: #openwhisk_ref_javascript_environments}

Les actions JavaScript peuvent être exécutées dans Node.js version 8 ou 10. Par défaut, toutes les actions Node.js sont exécutées dans un environnement de version 10.



### Environnement Node.js version 10 avec kits de développement IBM SDK
{: #openwhisk_ref_javascript_environments_10}
L'environnement Node.js version 10 est utilisé si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:10` lors de la création ou de la mise à jour d'une action.

#### Migration de `nodejs:8` vers `nodejs:10`
- Le package du gestionnaire de package de noeud (npm) `ibm_db` n'est pas disponible dans `nodejs:10`. Le package `ibm_db` ne prend pas en charge Node.js 10. Vous pouvez suivre la progression de la résolution de ce problème dans [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- Le package du gestionnaire de package de noeud (npm) `cloudant` n'est pas disponible dans `nodejs:10`, le package est obsolète, vous devez utiliser le package du gestionnaire de package de noeud officiel [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 lors de l'importation du module nodejs (par exemple, `require('@cloudant/cloudant')`) et [v3.x renvoie uniquement Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- Le package du gestionnaire de package de noeud (npm) `cradle` n'est pas disponible dans `nodejs:10`.
- Le package du gestionnaire de package de noeud (npm) `log4js` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre le problème à l'adresse [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- Le package du gestionnaire de package de noeud (npm) `watson-developer-cloud` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre la progression de la résolution de ce problème dans la nouvelle version à l'adresse [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Vous trouverez des informations détaillées sur l'environnement d'exécution nodejs version 10 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

#### Packages Node.js 10.15

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 Packages NPM :
   - [amqplib](https://www.npmjs.com/package/amqplib) - Bibliothèque permettant de créer des clients AMQP 0-9-1 pour Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - Module Node.js pour l'interfaçage avec le service de notifications Push d'Apple.
   - [async](https://www.npmjs.com/package/async) - Fournit des fonctions pour utiliser des fonctions asynchrones.
   - [bent](https://www.npmjs.com/package/bent) - Client HTTP fonctionnel pour Node.js avec fonctions async et await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Analyse les corps des demandes entrantes dans un logiciel intermédiaire avant vos gestionnaires, disponible sous la propriété req.body.
   - [btoa](https://www.npmjs.com/package/btoa) - Port de la fonction btoa du navigateur.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Pilote Node.js DataStax pour Apache Cassandra.
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - Bibliothèque Cloudant officielle pour Node.js.
   - [commander](https://www.npmjs.com/package/commander) - Solution complète pour les interfaces de ligne de commande node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Traducteur d'adresses de l'API ou de l'interface utilisateur Compose pour les bases de données Scylla.
   - [consul](https://www.npmjs.com/package/consul) - Client pour Consul, impliquant la configuration et la reconnaissance de service.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Analyse l'en-tête de cookie et remplit req.cookies avec un objet contenant les noms des cookies.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Client Elasticsearch de bas niveau officiel pour Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Middleware de gestionnaire d'erreurs (développement uniquement).
   - [etcd3](https://www.npmjs.com/package/etcd3) - Client de qualité supérieure prêt pour la production conçu pour l'API etcdv3 basée sur Protocol Buffers.
   - [formidable](https://www.npmjs.com/package/formidable) - Module Node.js utilisé pour analyser des données de formulaire, notamment des téléchargements de fichier.
   - [glob](https://www.npmjs.com/package/glob) - Utilisé pour la correspondance de fichiers à l'aide de motifs utilisés par le shell, tels que des étoiles, etc.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick et ImageMagick pour Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - SDK {{site.data.keyword.cos_full}} pour Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - Interface asynchrone/synchrone pour node.js vers IBM DB2 et IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - Le client node.js est utilisé pour simplifier l'interaction avec IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Conversion de codage de caractères JS pur
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom est une implémentation en JavaScript pur de nombreuses normes Web, notamment WHATWG DOM et HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce) - Bibliothèque d'API Salesforce pour les applications JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - Implémentation de jetons Web JSON.
   - [lodash](https://www.npmjs.com/package/lodash) - Bibliothèque Lodash exportée sous forme de modules Node.js.
   - [marked](https://www.npmjs.com/package/marked) - Compilateur et analyseur Markdown complet, écrit en JavaScript. Conçu pour la vitesse.
   - [merge](https://www.npmjs.com/package/merge) - Utilisé pour fusionner plusieurs objets en un seul, en créant éventuellement un nouvel objet cloné.
   - [moment](https://www.npmjs.com/package/moment) - Bibliothèque de dates JavaScript légère conçue pour l'analyse, la validation, la manipulation et le formatage des dates.
   - [mongodb](https://www.npmjs.com/package/mongodb) - Pilote MongoDB officiel pour Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - Pilote node.js pour mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js est une implémentation du système de modèles Mustache en JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - Pilote couchdb minimaliste pour Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Utilisé pour envoyer des e-mails à partir de Node.js – ultra facile !
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Module complet, fiable et testé de manière approfondie pour implémenter un serveur/fournisseur OAuth2 avec express dans Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Transforme une chaîne de chemin telle que /user/:name en expression régulière pouvant être ensuite utilisée pour établir une correspondance avec des chemins d'URL.
   - [pg](https://www.npmjs.com/package/pg) - Client PostgreSQL non bloquant pour node.js. Liaisons entre JavaScript pur et la bibliothèque native libpq en option.
   - [process](https://www.npmjs.com/package/process) - nécessite require('process') ; comme tout autre module.
   - [pug](https://www.npmjs.com/package/pug) - Implémente le langage de modèles Pug.
   - [redis](https://www.npmjs.com/package/redis) - Client Redis complet avec de multiples fonctions pour Node.js.
   - [request](https://www.npmjs.com/package/request) - Request est conçu pour être la méthode la plus simple possible pour créer des appels HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise) - Client de demande HTTP simplifié 'request' compatible avec Promise. Technologie Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - Commande UNIX rm -rf pour node.
   - [semver](https://www.npmjs.com/package/semver) - Gestion des versions sémantique pour Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Prise en charge des courriers électroniques via l'API SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Sérialise une erreur en objet simple.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js pour utiliser une icône favorite (favicon).
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO active la communication bidirectionnelle en temps réel basée sur les événements.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Structure d'application en temps réel pour socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent est une petite bibliothèque de demandes HTTP côté client progressive et un module Node.js avec la même API, compatible avec de nombreuses fonctions client HTTP de haut niveau.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Package fournissant différents outils pour l'intégration et l'interaction avec Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - Encapsuleur pour l'API Twilio, lié à la voix, à la vidéo et aux messages.
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js est une bibliothèque d'utilitaires pour JavaScript compatible avec les fonctions habituelles (each, map, reduce, filter...) sans étendre d'objet core Javascript.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Analyse les URL des paramètres de chemin plus facilement qu'à partir d'un comparateur de chaînes de caractères et d'expressions régulières.
   - [uuid](https://www.npmjs.com/package/uuid) - Génération simple et rapide d'identificateurs uniques universels (UUID) RFC 4122.
   - [validator](https://www.npmjs.com/package/validator) - Bibliothèque de valideurs et d'assainisseurs de chaînes.
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - Analyse et renvoie des données d'identification de service à partir de la variable d'environnement VCAP_SERVICES fournie par IBM Cloud.
   - [when](https://www.npmjs.com/package/when) - When.js est une implémentation sans faille et éprouvée de Promises/A+ et when(), incluant un correctif (shim) ES6 Promise complet.
   - [winston](https://www.npmjs.com/package/winston) - Bibliothèque de journalisation multitransport asynchrone pour node.js. Pas de panique ! Tout est consigné dans les journaux.
   - [ws](https://www.npmjs.com/package/ws) - ws est une implémentation client et serveur WebSocket simple d'utilisation, ultra-rapide et testée de manière approfondie.
   - [xlsx](https://www.npmjs.com/package/xlsx) - Analyseur et programme d'écriture conçu pour différents formats de feuille de calcul.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Convertisseur d'objets XML en JavaScript. Il prend en charge la conversion bidirectionnelle.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest est un encapsuleur pour le client HTTP intégré, utilisé pour l'émulation de l'objet de navigateur XMLHttpRequest.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Autre bibliothèque d'extraction pour Node.js.

### Environnement Node.js version 8 avec kits de développement IBM SDK
{: #openwhisk_ref_javascript_environments_8}
L'environnement Node.js version 8 est utilisé si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:8` lors de la création ou de la mise à jour d'une action.

Node.js version 8 est en mode maintenance et est disponible jusqu'à décembre 2019. Consultez le [planning des éditions Node.js](https://github.com/nodejs/Release).
{: deprecated}

#### Packages Node.js 8.15

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - Bibliothèque permettant de créer des clients AMQP 0-9-1 pour Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - Module Node.js pour l'interfaçage avec le service de notifications Push d'Apple.
   - [async](https://www.npmjs.com/package/async) - Fournit des fonctions pour utiliser des fonctions asynchrones.
   - [bent](https://www.npmjs.com/package/bent) - Client HTTP fonctionnel pour Node.js avec async/await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Analyse les corps des demandes entrantes dans un logiciel intermédiaire avant vos gestionnaires, disponible sous la propriété req.body.
   - [btoa](https://www.npmjs.com/package/btoa) - Port de la fonction btoa du navigateur.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Pilote Node.js DataStax pour Apache Cassandra.
   - [cloudant](https://www.npmjs.com/package/cloudant) - Bibliothèque Cloudant officielle pour Node.js.
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - Bibliothèque Cloudant officielle pour Node.js.
   - [commander](https://www.npmjs.com/package/commander) - Solution complète pour les interfaces de ligne de commande node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Traducteur d'adresses de l'API ou de l'interface utilisateur Compose pour les bases de données Scylla.
   - [consul](https://www.npmjs.com/package/consul) - Client pour Consul, impliquant la configuration et la reconnaissance de service.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Analyse l'en-tête de cookie et remplit req.cookies avec un objet contenant les noms des cookies.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Client Elasticsearch de bas niveau officiel pour Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Middleware de gestionnaire d'erreurs (développement uniquement).
   - [etcd3](https://www.npmjs.com/package/etcd3) - Client de qualité supérieure prêt pour la production conçu pour l'API etcdv3 basée sur Protocol Buffers.
   - [formidable](https://www.npmjs.com/package/formidable) - Module Node.js utilisé pour analyser des données de formulaire, notamment des téléchargements de fichier.
   - [glob](https://www.npmjs.com/package/glob) - Utilisé pour la correspondance de fichiers à l'aide de motifs utilisés par le shell, tels que des étoiles, etc.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick et ImageMagick pour Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - SDK {{site.data.keyword.cos_full}} pour Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - Interface asynchrone/synchrone pour node.js vers IBM DB2 et IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - Le client node.js est utilisé pour simplifier l'interaction avec IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Conversion de codage de caractères JS pur
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom est une implémentation en JavaScript pur de nombreuses normes Web, notamment WHATWG DOM et HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce) - Bibliothèque d'API Salesforce pour les applications JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - Implémentation de jetons Web JSON.
   - [lodash](https://www.npmjs.com/package/lodash) - Bibliothèque Lodash exportée sous forme de modules Node.js.
   - [log4js](https://www.npmjs.com/package/log4js) - Conversion de l'infrastructure log4js pour utiliser Node.
   - [marked](https://www.npmjs.com/package/marked) - Compilateur et analyseur Markdown complet, écrit en JavaScript. Conçu pour la vitesse.
   - [merge](https://www.npmjs.com/package/merge) - Utilisé pour fusionner plusieurs objets en un seul, en créant éventuellement un nouvel objet cloné.
   - [moment](https://www.npmjs.com/package/moment) - Bibliothèque de dates JavaScript légère conçue pour l'analyse, la validation, la manipulation et le formatage des dates.
   - [mongodb](https://www.npmjs.com/package/mongodb) - Pilote MongoDB officiel pour Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - Pilote node.js pour mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js est une implémentation du système de modèles Mustache en JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - Pilote couchdb minimaliste pour Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Utilisé pour envoyer des e-mails à partir de Node.js – ultra facile !
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Module complet, fiable et testé de manière approfondie pour implémenter un serveur/fournisseur OAuth2 avec express dans Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Transforme une chaîne de chemin telle que /user/:name en expression régulière pouvant être ensuite utilisée pour établir une correspondance avec des chemins d'URL.
   - [pg](https://www.npmjs.com/package/pg) - Client PostgreSQL non bloquant pour node.js. Liaisons entre JavaScript pur et la bibliothèque native libpq en option.
   - [process](https://www.npmjs.com/package/process) - nécessite require('process') ; comme tout autre module.
   - [pug](https://www.npmjs.com/package/pug) - Implémente le langage de modèles Pug.
   - [redis](https://www.npmjs.com/package/redis) - Client Redis complet avec de multiples fonctions pour Node.js.
   - [request](https://www.npmjs.com/package/request) - Request est conçu pour être la méthode la plus simple possible pour créer des appels HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise) - Client de demande HTTP simplifié 'request' compatible avec Promise. Technologie Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - Commande UNIX rm -rf pour node.
   - [semver](https://www.npmjs.com/package/semver) - Gestion des versions sémantique pour Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Prise en charge des courriers électroniques via l'API SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Sérialise une erreur en objet simple.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js pour utiliser une icône favorite (favicon).
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO active la communication bidirectionnelle en temps réel basée sur les événements.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Structure d'application en temps réel pour socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent est une petite bibliothèque de demandes HTTP côté client progressive et un module Node.js avec la même API, compatible avec de nombreuses fonctions client HTTP de haut niveau.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Package fournissant différents outils pour l'intégration et l'interaction avec Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - Encapsuleur pour l'API Twilio, lié à la voix, à la vidéo et aux messages.
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js est une bibliothèque d'utilitaires pour JavaScript compatible avec les fonctions habituelles (each, map, reduce, filter...) sans étendre d'objet core JavaScript.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Analyse les URL des paramètres de chemin plus facilement qu'à partir d'un comparateur de chaînes de caractères et d'expressions régulières.
   - [uuid](https://www.npmjs.com/package/uuid) - Génération simple et rapide d'identificateurs uniques universels (UUID) RFC 4122.
   - [validator](https://www.npmjs.com/package/validator) - Bibliothèque de valideurs et d'assainisseurs de chaînes.
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - Analyse et renvoie des données d'identification de service à partir de la variable d'environnement VCAP_SERVICES fournie par IBM Cloud.
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - Bibliothèque client Node.js pour utiliser les services Watson Developer Cloud, une collection d'API ayant recours au calcul cognitif pour résoudre des problèmes complexes.
   - [when](https://www.npmjs.com/package/when) - When.js est une implémentation sans faille et éprouvée de Promises/A+ et when(), incluant un correctif (shim) ES6 Promise complet.
   - [winston](https://www.npmjs.com/package/winston) - Bibliothèque de journalisation multitransport asynchrone pour node.js. Pas de panique ! Tout est consigné dans les journaux.
   - [ws](https://www.npmjs.com/package/ws) - ws est une implémentation client et serveur WebSocket simple d'utilisation, ultra-rapide et testée de manière approfondie.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Convertisseur d'objets XML en JavaScript. Il prend en charge la conversion bidirectionnelle.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest est un encapsuleur pour le client HTTP intégré, utilisé pour l'émulation de l'objet de navigateur XMLHttpRequest.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Autre bibliothèque d'extraction pour Node.js.

Vous trouverez des informations détaillées sur l'environnement d'exécution Node.js version 8 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).


## Contextes d'exécution Python
{: #openwhisk_ref_python_environments}

Vous pouvez choisir entre différentes versions de contexte d'exécution pour les actions Python. Par défaut, toutes les actions Python sont exécutées dans un environnement de version 2.


### Actions Python 3.7 (basées sur Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Les actions Python 3.7 sont exécutées avec Python 3.7.x. Pour utiliser ce contexte d'exécution, spécifiez le paramètre `--kind python:3.7` de l'interface de ligne de commande lorsque vous créez ou mettez à jour une action.

Le contexte d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.7 standard.

#### Packages Python 3.7.2

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Packages Python :
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 3.7 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Actions Python 3.6.8 (basées sur Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Les actions Python 3 sont exécutées avec Python 3.6.x. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python:3.6` de l'interface de ligne de commande lorsque vous créez ou mettez à jour une action.

L'environnement d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.6 standard.

#### Packages Python 3.6.8

Version de Python :
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Packages Python :
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 3.6 dans [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Actions Python 2

Les actions Python 2 sont exécutées avec Python 2.7.15 sauf si vous spécifiez l'indicateur `--kind` lorsque vous créez ou mettez à jour une action.

Lors de la création d'actions Python à l'aide de virtualenv, utilisez l'image Docker `openwhisk/python2action`.
Les packages suivants sont disponibles pour leur utilisation dans des actions Python 2, en supplément de la bibliothèque Python 2.7 standard.

#### Packages Python 2

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Vous trouverez des informations détaillées sur l'environnement d'exécution Python 2 dans [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).


## Contexte d'exécution Swift
{: #swift-actions}

Par défaut, toutes les actions Swift sont exécutées dans un environnement de version 4.2.

Les contextes d'exécution d'action Swift 4.x n'incorporant aucun package, suivez les instructions pour que les [actions Swift packagées](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) incluent les dépendances en utilisant Package.swift.

Les actions Swift 4.2 peuvent utiliser les packages suivants avec un fichier source Swift unique :
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### SwiftyJSON utilisant un fichier d'action source unique
Si vous disposez d'une action  non compilée et que vous utilisez le package **SwiftyJSON**, vous devez précompiler votre action et indiquer la version de SwiftyJSON que vous voulez utiliser pour l'action kind `swift:4.2`. 


## Contexte d'exécution PHP
{: #openwhisk_ref_php}

Par défaut, toutes les actions PHP sont exécutées dans un environnement de version 7.3.

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

## Contexte d'exécution Docker
{: #openwhisk_ref_docker}

Les actions Docker exécutent un fichier binaire fourni par l'utilisateur dans un conteneur Docker. Le fichier binaire s'exécute dans une image Docker reposant sur [python:3.6-alpine](https://hub.docker.com/r/library/python) ; par conséquent, il doit être compatible avec cette distribution.

Le squelette Docker est pratique pour générer des images Docker compatibles avec OpenWhisk. Vous pouvez l'installer avec la commande de plug-in d'interface de ligne de commande `ibmcloud fn sdk install docker`.

Le programme binaire principal doit se trouver dans `/action/exec` à l'intérieur du conteneur. L'exécutable reçoit les arguments d'entrée d'une chaîne d'arguments de ligne de commande qui peut être désérialisée en tant qu'objet `JSON`. Il doit renvoyer un résultat en utilisant `stdout` sous forme de chaîne comportant une seule ligne de code `JSON` sérialisé.

Vous pouvez inclure des étapes de compilation ou des dépendances en modifiant le document `Dockerfile` inclus dans `dockerSkeleton`.



## Contexte d'exécution Go
{: #runtimes_go}

Par défaut, toutes les actions Go sont exécutées dans un environnement de version 1.11.



## Contexte d'exécution Java
{: #runtimes_java}

Par défaut, toutes les actions Java sont exécutées dans un environnement de version 8.



## Contexte d'exécution Ruby
{: #runtimes_ruby}

Par défaut, toutes les actions Ruby sont exécutées dans un environnement de version 2.5.



## Contexte d'exécution .NET Core
{: #runtimes_dotnet}

Par défaut, toutes les actions .NET Core sont exécutées dans un environnement de version 2.2.
