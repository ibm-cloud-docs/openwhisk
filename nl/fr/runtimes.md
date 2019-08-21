---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: runtimes, support, functions

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


# Contextes d’exécution
{: #runtimes}
Vos applications doivent être codées et exécutées dans des langages de programmation tels que JavaScript ou Python. De nombreux contextes d'exécution sont disponibles par défaut avec {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Affichez les contextes d'exécution disponibles pour IBM Cloud Functions dans chaque région. Les liens suivants renvoient une réponse JSON. La section `runtimes` de la réponse contient l'ensemble des contextes d'exécution disponibles. La section `image` contient le nom de l'image du contexte d'exécution sur [Docker Hub](https://hub.docker.com/){: external} et de l'étiquette utilisée.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [` eu-de `](https://eu-de.functions.cloud.ibm.com/){: external}


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

Par défaut, toutes les actions Node.js sont exécutées dans un environnement de version 10.
{: note}

Les actions JavaScript peuvent être exécutées dans Node.js version 8 ou 10. Node.js version 8 est en mode maintenance et est disponible jusqu'à décembre 2019. Consultez le [planning des éditions Node.js](https://github.com/nodejs/Release){: external}. 
{: deprecated}

| Exécution | Description | Journal des modifications |
| --- | --- | --- |
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} | Par défaut, toutes les actions Node.js sont exécutées dans un environnement de version 10. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} | La version 8.15.0 de Node.js est utilisée si l'indicateur `--kind` est spécifié de manière explicite avec la valeur `nodejs:8` lors de la création ou de la mise à jour d'une action. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |


### Migration depuis nodeJS 8 vers nodeJS 10

| Package | Détails |
| --- | --- |
| `ibm_db` | Le package du gestionnaire de package de noeud (NPM) `ibm_db` n'est pas disponible dans `nodejs:10`. Le package `ibm_db` ne prend pas en charge Node.js 10. Vous pouvez suivre la progression dans [ce problème](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external}. |
| `cloudant` | Le package du gestionnaire de package de noeud (NPM) `cloudant` n'est pas disponible dans `nodejs:10`. Le package est obsolète. Vous devez utiliser le package NPM officiel [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} version 3.0.0 lors de l'importation du module Node.js (c'est-à-dire `require('@cloudant/cloudant')`) et [v3.x renvoie uniquement Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | Le package du gestionnaire de package de noeud (NPM) `cradle` n'est pas disponible dans `nodejs:10`. |
| `log4js` | Le package du gestionnaire de package de noeud (NPM) `log4js` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre [ce problème](https://github.com/log4js-node/log4js-node/issues/805){: external}. |
| `watson-developer-cloud` | Le package du gestionnaire de package de noeud (NPM) `watson-developer-cloud` n'est pas disponible dans `nodejs:10`. Vous pouvez suivre la progression dans la nouvelle version via [ce problème](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external}. |


### Packages Node.js

| Packages Node.js 10.15 | Description |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Bibliothèque permettant de créer des clients Advanced Message Queuing Protocol 0-9-1 pour Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Module Node.js pour l'interfaçage avec le service de notifications Push d'Apple. |
| [`async`](https://www.npmjs.com/package/async){: external} | Fournit des fonctions pour utiliser des fonctions asynchrones. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Client HTTP fonctionnel pour Node.js avec fonctions async et await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Analyse les corps des demandes entrantes dans un logiciel intermédiaire avant vos gestionnaires, disponible sous la propriété req.body. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Port de la fonction `btoa` du navigateur. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | Pilote Node.js DataStax pour Apache Cassandra. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external} | Bibliothèque Cloudant officielle pour Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | Solution complète pour les interfaces de ligne de commande node.js. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Traducteur d'adresses de l'API ou de l'interface utilisateur Compose pour les bases de données Scylla. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Client pour Consul, impliquant la configuration et la reconnaissance de service. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Analyse l'en-tête de cookie et remplit req.cookies avec un objet contenant les noms des cookies. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | Client ElasticSearch de bas niveau officiel pour Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware de gestionnaire d'erreurs (développement uniquement). |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Client de qualité supérieure prêt pour la production conçu pour l'API etcdv3 basée sur Protocol Buffers. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Module Node.js utilisé pour analyser des données de formulaire, notamment des téléchargements de fichier. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Utilisé pour la correspondance de fichiers à l'aide de motifs utilisés par le shell, tels que des étoiles, etc. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick et ImageMagick pour Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | SDK {{site.data.keyword.cos_full}} pour Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Interface asynchrone/synchrone pour Node.js vers IBM DB2 et IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | Client Node.js utilisé pour simplifier l'interaction avec IBM Watson Internet of Things Platform. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Conversion de codage de caractères JS Pure. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` est une implémentation en JavaScript pur de nombreuses normes Web, notamment les normes DOM et HTML du Web Hypertext Application Technology Working Group. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Bibliothèque d'API Salesforce pour les applications JavaScript. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Implémentation de jetons Web JSON. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | La bibliothèque `lodash` est exportée sous forme de modules Node.js. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Compilateur et analyseur Markdown complet, écrit en JavaScript. Conçu pour la vitesse. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Utilisé pour fusionner plusieurs objets en un seul, en créant éventuellement un nouvel objet cloné.
| [`moment`](https://www.npmjs.com/package/moment){: external} | Bibliothèque de dates JavaScript légère conçue pour l'analyse, la validation, la manipulation et le formatage des dates. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | Bibliothèque MongoDB officielle pour Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Pilote Node.js pour MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | Mustache.js est une implémentation du système de modèles Mustache en JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Pilote CouchDB minimaliste pour Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Utilisé pour envoyer des e-mails à partir de Node.js – ultra facile ! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Module complet, fiable et testé pour implémenter un serveur/fournisseur OAuth2 avec express dans Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Transforme une chaîne de chemin telle que `/user/:name` en expression régulière, qui peut ensuite être utilisée pour établir une correspondance avec des chemins d'URL.|
| [`pg`](https://www.npmjs.com/package/pg){: external} | Client PostgreSQL non bloquant pour Node.js. Liaisons entre JavaScript pur et la bibliothèque `libpq` native en option. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')` ; comme tout autre module. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implémente le langage de création de modèles Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Client Redis complet avec de multiples fonctions pour Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Passer des appels HTTP. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | Client de demande HTTP simplifié 'request' compatible avec support Promise. Technologie Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | Commande UNIX rm -rf pour node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Gestion des versions sémantique pour nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Prise en charge des courriers électroniques via l'API SendGrid. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Sérialise une erreur en objet simple. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Middleware Node.js pour utiliser une icône favorite (favicon).
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` active la communication bidirectionnelle en temps réel basée sur les événements. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Structure d'application en temps réel pour `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` est une petite bibliothèque de demandes HTTP côté client progressive et un module Node.js avec la même API, compatible avec de nombreuses fonctions client HTTP de haut niveau. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Package fournissant différents outils pour l'intégration et l'interaction avec Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Encapsuleur pour l'API Twilio, lié à la voix, à la vidéo et aux messages. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js est une bibliothèque d'utilitaires pour JavaScript compatible avec les fonctions habituelles (each, map, reduce, filter...) sans étendre d'objet core JavaScript. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Analyse les URL des paramètres de chemin plus facilement qu'à partir d'un comparateur de chaînes de caractères et d'expressions régulières. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Génération simple et rapide d'identificateurs uniques universels (UUID) RFC 4122. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Bibliothèque de valideurs et d'assainisseurs de chaînes. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Analyse et renvoie des données d'identification de service à partir de la variable d'environnement VCAP_SERVICES fournie par IBM Cloud. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js est une implémentation sans faille et éprouvée de `Promises/A+` et `when()`, incluant un correctif (shim) ES6 Promise complet. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Bibliothèque de journalisation multitransport asynchrone pour Node.js. Pas de panique ! Tout est consigné dans les journaux. |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` est une implémentation client et serveur WebSocket simple d'utilisation, ultra-rapide et testée de manière approfondie. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Analyseur et programme d'écriture conçu pour différents formats de feuille de calcul. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Convertisseur simple d'objets XML en JavaScript. Il prend en charge la conversion bidirectionnelle. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | node-XMLHttpRequest est un encapsuleur pour le client HTTP intégré, utilisé pour l'émulation de l'objet de navigateur XMLHttpRequest. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Autre bibliothèque d'extraction pour node. |
{: caption="Tableau 1. Packages Node.js 10.15" caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

| Packages Node.js 8.15 | Description |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Bibliothèque permettant de créer des clients Advanced Message Queuing Protocol 0-9-1 pour Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Module Node.js pour l'interfaçage avec le service de notifications Push d'Apple.
| [`async`](https://www.npmjs.com/package/async){: external} | Fournit des fonctions pour utiliser des fonctions asynchrones. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Client HTTP fonctionnel pour Node.js avec async-await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Analyse les corps des demandes entrantes dans un logiciel intermédiaire avant vos gestionnaires, disponible sous la propriété req.body. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Port de la fonction `btoa` du navigateur. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | Pilote Node.js DataStax pour Apache Cassandra. |
| [`cloudant`](https://www.npmjs.com/package/cloudant){: external} | Bibliothèque Cloudant officielle pour Node.js. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external} | Bibliothèque Cloudant officielle pour Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | Solution complète pour les interfaces de ligne de commande node.js. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Traducteur d'adresses de l'API ou de l'interface utilisateur Compose pour les bases de données Scylla. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Client pour Consul, impliquant la configuration et la reconnaissance de service. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Analyse l'en-tête de cookie et remplit req.cookies avec un objet contenant les noms des cookies. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | Client ElasticSearch de bas niveau officiel pour Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware de gestionnaire d'erreurs (développement uniquement). |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Client de qualité supérieure prêt pour la production conçu pour l'API etcdv3 basée sur Protocol Buffers. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Module Node.js utilisé pour analyser des données de formulaire, notamment des téléchargements de fichier. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Utilisé pour la correspondance de fichiers à l'aide de motifs utilisés par le shell, tels que des étoiles, etc. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick et ImageMagick pour Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | SDK {{site.data.keyword.cos_full}} pour Node.js. |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Interface asynchrone-synchrone pour Node.js vers IBM DB2 et IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | Le client Node.js est utilisé pour simplifier l'interaction avec IBM Watson Internet of Things Platform. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Conversion de codage de caractères JS Pure. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` est une implémentation en JavaScript pur de nombreuses normes Web, notamment les normes DOM et HTML du Web Hypertext Application Technology Working Group. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Bibliothèque d'API Salesforce pour les applications JavaScript. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Implémentation de jetons Web JSON. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | Bibliothèque `lodash` exportée sous forme de modules Node.js. |
| [`log4js`](https://www.npmjs.com/package/log4js){: external} | Conversion de l'infrastructure log4js pour une utilisation avec Node. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Compilateur et analyseur Markdown complet, écrit en JavaScript. Conçu pour la vitesse. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Utilisé pour fusionner plusieurs objets en un seul, en créant éventuellement un nouvel objet cloné. |
| [`moment`](https://www.npmjs.com/package/moment){: external} | Bibliothèque de dates JavaScript légère conçue pour l'analyse, la validation, la manipulation et le formatage des dates. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | Bibliothèque MongoDB officielle pour Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Pilote Node.js pour MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | mustache.js est une implémentation du système de modèles Mustache en JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Pilote CouchDB minimaliste pour Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Utilisé pour envoyer des e-mails à partir de Node.js – ultra facile ! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Module complet, fiable et testé pour implémenter un serveur-fournisseur OAuth2 avec express dans Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | Bibliothèque client JavaScript pour la plateforme OpenWhisk. Fournit un encapsuleur autour des API OpenWhisk. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Transforme une chaîne de chemin telle que `/user/:name` en expression régulière, qui peut ensuite être utilisée pour établir une correspondance avec des chemins d'URL.
| [`pg`](https://www.npmjs.com/package/pg){: external} | Client PostgreSQL non bloquant pour Node.js. Liaisons entre JavaScript pur et la bibliothèque `libpq` native en option. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')` ; comme tout autre module. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implémente le langage de création de modèles Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Client Redis complet avec de multiples fonctions pour Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Passer des appels HTTP. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | Client de demande HTTP simplifié 'request' compatible avec support Promise. Technologie Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | Commande UNIX rm -rf pour node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Gestion des versions sémantique pour nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Prise en charge des courriers électroniques via l'API SendGrid. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Sérialise une erreur en objet simple. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Middleware Node.js pour utiliser une icône favorite (favicon). |
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` active la communication bidirectionnelle en temps réel basée sur les événements. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Structure d'application en temps réel pour `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` est une petite bibliothèque de demandes HTTP côté client progressive et un module Node.js avec la même API, compatible avec de nombreuses fonctions client HTTP de haut niveau. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Package fournissant différents outils pour l'intégration et l'interaction avec Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Encapsuleur pour l'API Twilio, lié à la voix, à la vidéo et aux messages. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js est une bibliothèque d'utilitaires pour JavaScript compatible avec les fonctions habituelles (each, map, reduce, filter...) sans étendre d'objet core JavaScript. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Analyse les URL des paramètres de chemin plus facilement qu'à partir d'un comparateur de chaînes de caractères et d'expressions régulières. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Génération simple et rapide d'identificateurs uniques universels (UUID) RFC 4122. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Bibliothèque de valideurs et d'assainisseurs de chaînes. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Analyse et renvoie des données d'identification de service à partir de la variable d'environnement VCAP_SERVICES fournie par IBM Cloud. |
| [`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external} | Bibliothèque client Node.js pour utiliser les services Watson Developer Cloud, une collection d'API ayant recours au calcul cognitif pour résoudre des problèmes complexes. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js est une implémentation sans faille et éprouvée de `Promises/A+` et `when()`, incluant un correctif (shim) ES6 Promise complet. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Bibliothèque de journalisation multitransport asynchrone pour Node.js. Pas de panique ! Tout est consigné dans les journaux. |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` est une implémentation client et serveur WebSocket simple d'utilisation, ultra-rapide et testée de manière approfondie. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Convertisseur simple d'objets XML en JavaScript. Il prend en charge la conversion bidirectionnelle. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` est un encapsuleur pour le client HTTP intégré, utilisé pour l'émulation de l'objet de navigateur XMLHttpRequest. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Autre bibliothèque d'extraction pour node. |
{: caption="Tableau 2. Packages Node.js 8.15" caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Contextes d'exécution Python
{: #openwhisk_ref_python_environments}

Par défaut, toutes les actions Python sont exécutées dans un environnement version 2.7.15.
{: note}

| Version Python | Description | Journal des modifications |
| --- | --- | --- |
| 2.7.15 | Par défaut, toutes les actions Python sont exécutées dans un environnement version 2.7.15 à moins que vous n'indiquiez l'indicateur `--kind` lorsque vous créez ou mettez à jour une action. Lors de la création d'actions python à l'aide de `virtualenv`, utilisez l'image Docker `openwhisk/python2action`. Les packages suivants sont disponibles pour leur utilisation dans des actions Python 2, en supplément de la bibliothèque Python 2.7 standard. | [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}. |
| [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | Les actions Python 3 sont exécutées avec Python 3.6.x. Pour utiliser cet environnement d'exécution, spécifiez le paramètre `--kind python:3.6` de l'interface de ligne de commande lorsque vous créez ou mettez à jour une action. L'environnement d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.6 standard. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | Les actions Python 3.7 (basées sur Debian Stretch) sont exécutées avec Python 3.7.x. Pour utiliser ce contexte d'exécution, spécifiez le paramètre `--kind python:3.7` de l'interface de ligne de commande lorsque vous créez ou mettez à jour une action. Le contexte d'exécution contient des packages SDK pour des services IBM Cloud disponibles pour être utilisés par des actions Python, en supplément des bibliothèques Python 3.7 standard. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Packages Python

| Packages Python 2.7.15 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `enum34` |
| `Flask` |
| `functools32` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `idna` |
| `incremental` |
| `ipaddress` |
| `itsdangerous` |
| `Jinja2` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `parsel` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pyOpenSSL` |
| `python-dateutil` |
| `queuelib` |
| `requests` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `Twisted` |
| `urllib3` |
| `virtualenv=` |
| `w3lib` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tableau 1. Packages Python 2.7.15" caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Packages Python 3.6.8 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `autobahn` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six=` |
| `soupsieve` |
| `tornado` |
| `Twisted` |
| `txaio` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tableau 2. Packages Python 3.6.8" caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Packages Python 3.7.2 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `etcd3` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `grpcio` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `protobuf` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `soupsieve` |
| `tenacity` |
| `tornado` |
| `Twisted` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `websocket-client` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tableau 3. Packages Python 3.7.2" caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Contexte d'exécution Swift
{: #swift-actions}

Par défaut, toutes les actions Swift sont exécutées dans un environnement de version 4.2.
{: note}

Les contextes d'exécution d'action Swift 4.x n'incorporant aucun package, suivez les instructions pour que les [actions Swift packagées](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) incluent les dépendances en utilisant Package.swift.

Les actions Swift 4.2 peuvent utiliser les packages suivants lorsque vous utilisez un fichier source Swift unique :
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### <ph class="ignoreSpelling">SwiftyJSON</ph> utilisant un fichier d'action source unique
Si vous disposez d'une action  non compilée et que vous utilisez le package `SwiftyJSON`, vous devez précompiler votre action et indiquer la version de `SwiftyJSON` que vous voulez utiliser pour l'action kind `swift:4.2`. 


## Contexte d'exécution PHP
{: #openwhisk_ref_php}

Par défaut, toutes les actions PHP sont exécutées dans un environnement de version 7.3.
{: note}

Les extensions PHP suivantes sont disponibles en plus des extensions standard :

- `bcmath`
- `curl`
- `gd`
- `intl`
- `mbstring`
- `mysqli`
- `pdo_mysql`
- `pdo_pgsql`
- `pdo_sqlite`
- `soap`
- `zip`

## Contexte d'exécution Docker
{: #openwhisk_ref_docker}

Les actions Docker exécutent un exécutable fourni par l'utilisateur dans un conteneur Docker. Le fichier exécutable s'exécute dans une image Docker reposant sur [python:3.6-alpine](https://hub.docker.com/_/python){: external} ; par conséquent, il doit être compatible avec cette distribution.

Le squelette Docker est pratique pour générer des images Docker compatibles avec OpenWhisk. Vous pouvez l'installer avec la commande de plug-in d'interface de ligne de commande `ibmcloud fn sdk install docker`.

Le programme exécutable principal doit se trouver dans `/action/exec` à l'intérieur du conteneur. L'exécutable reçoit les arguments d'entrée d'une chaîne d'arguments de ligne de commande qui peut être désérialisée en tant qu'objet `JSON`. Il doit renvoyer un résultat en utilisant `stdout` sous forme de chaîne comportant une seule ligne de code `JSON` sérialisé.

Vous pouvez inclure des étapes de compilation ou des dépendances en modifiant le document `Dockerfile` inclus dans `dockerSkeleton`.

## Prise en charge supplémentaire des contextes d'exécution

| Exécution |
| --- | --- | 
| Par défaut, toutes les actions Go sont exécutées dans un environnement de version 1.11. |
{: caption="Tableau 1. Go" caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Exécution |
| --- | --- | 
| Par défaut, toutes les actions Java sont exécutées dans un environnement de version 8. |
{: caption="Tableau 2. Java" caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Exécution |
| --- |
| Par défaut, toutes les actions Ruby sont exécutées dans un environnement de version 2.5. |
{: caption="Tableau 3. Ruby" caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Exécution |
| --- |
| Par défaut, toutes les actions .NET Core sont exécutées dans un environnement de version 2.2. |
{: caption="Tableau 4. .NET Core" caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}




