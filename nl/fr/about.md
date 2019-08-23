---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: platform architecture, openwhisk, couchdb, kafka, functions

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


# Fonctionnement de {{site.data.keyword.openwhisk_short}}
{: #about}

{{site.data.keyword.openwhisk}} est une plateforme de traitement gérée par des événements également appelée traitement sans serveur ou Function as a Service (FaaS) qui exécute un code en réponse à des événements ou des appels directs.
{: shortdesc}

## Technologie de {{site.data.keyword.openwhisk_short}}
{: #about_technology}

Découvrez les concepts de base de la technologie derrière {{site.data.keyword.openwhisk_short}} :

**Qu'est-ce qu'une action ?**

Une action est un petit morceau de code qui peut être appelé ou défini pour s'exécuter automatiquement en réponse à un événement. Dans tous les cas, chaque exécution génère un enregistrement identifié par un ID d'activation unique. L'entrée et le résultat d'une action peuvent être affichés sous forme de paires clé-valeur. La clé est une chaîne et la valeur est une valeur JSON valide. Une action peut être écrite dans le langage de votre choix et fournie au service sous forme de code source ou d'image Docker. Le code d'action s'exécute lorsqu'il est directement appelé par l'API Cloud Functions, l'interface de ligne de commande ou le logiciel SDK iOS. Une action peut répondre automatiquement aux événements d'IBM Cloud ou de services tiers.

**Pourquoi utiliser une action ?**

Les actions permettent de limiter la durée d'exécution du code, afin de réduire les frais. 

Par exemple, vous pouvez utiliser des actions pour détecter des visages dans une image, répondre à des modifications dans une base de données, agréger un ensemble d'appels d'API ou même publier un tweet.

**Puis-je utiliser plusieurs actions simultanément ?**

Oui. Vous pouvez utiliser des actions pour appeler d'autres actions, ou vous pouvez chaîner des actions pour [créer des séquences](/docs/openwhisk?topic=cloud-functions-actions#actions_seq). Pour que cela fonctionne, la sortie d'une action doit être utilisée comme entrée pour une autre action, qui fournira une sortie pouvant être utilisée pour déclencher une autre action et ainsi de suite. Vous pouvez même conditionner le groupe d'actions que vous créez pour former un package. Un package permet de réutiliser des actions ou des séquences courantes en appelant le package au lieu de configurer à nouveau l'action ou la séquence. 

## Terminologie {{site.data.keyword.openwhisk_short}}

<dl>
  <dt>Espace de nom</dt>
    <dd>Les [espaces de nom](/docs/openwhisk?topic=cloud-functions-namespaces) contiennent des entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs et appartiennent à un groupe de ressources. Vous pouvez permettre à des utilisateurs d'accéder à vos entités {{site.data.keyword.openwhisk_short}} en leur accordant l'accès à l'espace de nom.</dd>
  <dt>Action</dt>
    <dd>Une [action](/docs/openwhisk?topic=cloud-functions-actions) est un élément de code qui effectue une tâche spécifique. Une action peut être écrite dans le langage de votre choix, par exemple en petits fragments de code Swift ou JavaScript, ou de code binaire personnalisé imbriqué dans un conteneur Docker. Vous soumettez votre action à Cloud Functions sous forme de code source ou d'image Docker.
    <br><br>Une action effectue une tâche lorsqu'elle est directement appelée à l'aide de l'API {{site.data.keyword.openwhisk_short}}, de l'interface de ligne de commande ou du logiciel SDK pour iOS. Une action peut également répondre automatiquement à des événements à partir des services {{site.data.keyword.cloud_notm}} et de services tiers au moyen d'un déclencheur. </dd>
  <dt>Séquence</dt>
    <dd>Ensemble d'actions pouvant être chaînées et regroupées dans une [séquence](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) sans avoir à écrire de code. Une séquence est une suite d'actions, appelées dans un certain ordre, où la sortie de la première devient l'entrée de la suivante. Vous pouvez ainsi combiner des actions existantes afin de pouvoir les réutiliser rapidement et facilement. Une séquence peut ensuite être appelée comme une action, via une API REST ou automatiquement, en réponse à des événements.
  </dd>
  <dt>Evénement</dt>
    <dd>Par exemple, les événements peuvent être des modifications apportées aux enregistrements d'une base de données, des relevés de capteur IoT dépassant une certaine température, de nouvelles validations de code dans un référentiel GitHub ou des demandes HTTP simples provenant d'applications Web ou mobiles. Les événements provenant de sources d'événements externes et internes sont canalisés via un déclencheur, et des règles permettent de réagir à ces événements par le biais d'actions.</dd>
  <dt>Déclencheur</dt>
    <dd>Les [déclencheurs](/docs/openwhisk?topic=cloud-functions-triggers) constituent un canal nommé correspondant à une classe d'événements. Un déclencheur est une déclaration qui doit réagir à un certain type d'événement, provenant d'un utilisateur ou d'une source d'événements.</dd>
  <dt>Règle</dt>
    <dd>Une [règle](/docs/openwhisk?topic=cloud-functions-rules) associe un déclencheur à une action. A chaque fois que le déclencheur s'exécute, la règle utilise l'événement déclencheur comme entrée et appelle l'action associée. Avec le jeu de règles approprié, un événement déclencheur unique peut appeler plusieurs actions, ou une action peut être appelée en réponse à des événements provenant de plusieurs déclencheurs.</dd>
  <dt>Flux</dt>
    <dd>Un
[flux](/docs/openwhisk?topic=cloud-functions-triggers#triggers_feeds) est pratique pour configurer une source d'événements externe afin d'exécuter des événements déclencheurs qui peuvent être consommés par {{site.data.keyword.openwhisk_short}}. Par exemple, un flux Git peut exécuter un déclencheur pour chaque validation dans un référentiel Git.</dd>
  <dt>Package</dt>
    <dd>Des intégrations à des services et à des fournisseurs d'événements peuvent être ajoutées à l'aide de packages. Un [package](/docs/openwhisk?topic=cloud-functions-pkg_ov) est un regroupement de flux et d'actions. Un flux est un élément de code qui configure une source d'événements externe en vue de l'exécution d'événements déclencheurs. Par exemple, un déclencheur créé avec un flux de modifications {{site.data.keyword.cloudant}} configure un service de sorte qu'il exécute le déclencheur à chaque fois qu'un document est modifié ou ajouté dans une base de données {{site.data.keyword.cloudant_short_notm}}. Les actions des packages représentent une logique réutilisable qu'un fournisseur de services peut mettre à disposition pour que les développeurs puissent utiliser le service en tant que source d'événements et appeler les API de ce service.
    <br><br>Un catalogue de packages existant permet d'améliorer les applications avec des fonctions utiles et d'accéder à des services externes dans l'écosystème rapidement. Parmi les exemples de services externes comprenant des packages {{site.data.keyword.openwhisk_short}} figurent {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack et GitHub.</dd>
</dl>

### Etape suivante
{: #quiz}
Testez vos connaissances et [répondez à un quiz](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external} !


## Comment s'effectue le traitement interne dans OpenWhisk ?
{: #about_internal}

Pour décrire en détails tous les composants, suivons l'appel d'une action via le système {{site.data.keyword.openwhisk_short}}. Un appel exécute le code que l'utilisateur ajoute dans le système et renvoie les résultats de cette exécution. La figure ci-dessous présente l'architecture générale de {{site.data.keyword.openwhisk_short}}.

![Architecture {{site.data.keyword.openwhisk_short}}](./images/OpenWhisk.png)

### Que se passe-t-il dans les coulisses d'OpenWhisk ?
{: #about_scenes}

OpenWhisk est un projet open source qui combine des composants tels que NGINX, Kafka, Docker et CouchDB pour former un service de programmation sans serveur basé sur les événements.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="Flux interne de traitement dans les coulisses d'OpenWhisk" style="width:550px; border-style: none"/>

#### 1. Entrée dans le système : NGINX
{: #about_ngnix}

L'API utilisateur d'OpenWhisk est basée sur HTTP et repose sur une conception RESTful. Par conséquent, la commande envoyée via l'interface de ligne de commande correspond à une demande HTTP émise sur le système OpenWhisk. La commande spécifique pourrait se traduire ainsi :
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Notez la variable *$userNamespace* ici. Un utilisateur a accès à au moins un espace. Pour plus de simplicité, nous allons supposer que l'utilisateur possède l'espace de nom dans lequel *myAction* est placé.

Le premier point d'entrée dans le système s'effectue via **NGINX**, "un serveur proxy inverse et HTTP". Il est principalement utilisé pour la terminaison SSL et l'acheminement d'appels HTTP appropriés vers le composant suivant.

#### 2. Entrée dans le système : Contrôleur
{: #about_controller}


NGINX transmet la demande HTTP au **contrôleur**, qui est le composant suivant dans le chemin qui passe par OpenWhisk. Il s'agit d'une implémentation Scala de l'API REST réelle (basée sur **Akka** et **Spray**) et qui par conséquent sert d'interface pour toutes les tâches qu'un utilisateur peut réaliser. Ces tâches incluent notamment les demandes [create, retrieve, update et delete](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){: external} pour vos entités dans OpenWhisk et l'appel d'actions.

Le contrôleur commence par clarifier ce que l'utilisateur essaie de faire. Pour cela, il se base sur la méthode HTTP utilisée dans la demande HTTP. L'utilisateur émet une demande POST vers une action existante, que le contrôleur traduit en **appel d'une action**.

Compte tenu du rôle central du contrôleur (d'où son nom), les étapes suivantes l'impliquent toutes dans une certaine mesure.

#### 3. Authentification et autorisation : CouchDB
{: #about_auth}

Le contrôleur vérifie qui vous êtes (*Authentification*) et si vous êtes autorisé à faire ce que vous essayez de faire avec cette entité (*Autorisation*). Les données d'identification incluses dans la demande sont vérifiées par rapport à la base de données nommée **subjects** dans une instance **CouchDB**.

En l'occurrence, le contrôleur vérifie que l'utilisateur existe dans la base de données d'OpenWhisk et qu'il est autorisé à appeler l'action *myAction*, qui est supposée être une action figurant dans un espace de nom dont l'utilisateur est propriétaire. Cela permet de garantir que l'utilisateur dispose des droits nécessaires pour appeler l'action.

Tout est validé et l'étape de traitement suivante peut commencer.

#### 4. Obtention de l'action : CouchDB
{: #about_couchdb}

Maintenant que le contrôleur est certain que l'utilisateur est authentifié et autorisé à appeler son action, il charge cette action (en l'occurrence *myAction*) à partir de la base de données **whisks** dans CouchDB.

L'enregistrement de l'action contient principalement le code à exécuter et les paramètres par défaut que vous souhaitez transmettre à votre action, fusionnés avec les paramètres que vous avez inclus dans la demande d'appel proprement dite. Il contient également les restrictions de ressource imposées lors de son exécution, telles que la mémoire dans laquelle il peut être consommé.

Dans ce cas précis, l'action ne prend aucun paramètre (la définition du paramètre de la fonction est une liste vide). Par conséquent, nous partons du principe qu'aucun paramètre par défaut n'a été défini, y compris les paramètres spécifiques de l'action, ce qui rend ce cas le plus simple possible.


#### 5. Qui peut appeler l'action : Equilibreur de charge
{: #about_lb}

L'équilibreur de charge, qui fait partie du contrôleur, dispose d'une vue globale des programmes d'exécution disponibles dans le système en vérifiant continuellement leur état de santé. On appelle ces programmes d'exécution des **auteurs d'appel**. L'équilibreur de charge, connaissant les auteurs d'appel disponibles, en choisit un pour appeler l'action demandée.

#### 6. Kafka
{: #about_kafka}

A partir de maintenant, deux problèmes peuvent se produire avec la demande d'appel que vous avez envoyée :

1. Le système tombe en panne, ce qui provoque la perte de votre appel.
2. La charge du système est telle que l'appel doit attendre la fin d'autres appels.

La solution à ces deux problèmes est **Kafka**, un "système de messagerie de publication et d'abonnement distribué à débit élevé". Le contrôleur et l'auteur de l'appel communiquent uniquement via des messages qui sont mis en mémoire tampon et conservés par Kafka. Kafka augmente la charge de mise en mémoire tampon, entraînant un risque d'erreur *OutOfMemoryException* aussi bien au niveau du contrôleur que de l'auteur de l'appel, tout en garantissant également que les messages ne seront pas perdus si le système tombe en panne.

Pour que l'action soit appelée, le contrôleur publie un message sur Kafka, qui contient l'action à appeler et les paramètres à transmettre à cette action (en l'occurrence, aucun paramètre n'est à transmettre). Ce message est envoyé à l'auteur de l'appel, choisi précédemment par le contrôleur dans la liste fournie par Consul.

Une fois que Kafka a confirmé la bonne réception du message, un **ID activation** est envoyé en réponse à la demande HTTP émise vers l'utilisateur. Ce dernier pourra se servir de cette information ultérieurement pour accéder aux résultats de cet appel. Notez qu'il s'agit d'un modèle d'appel asynchrone dans lequel la demande HTTP est terminée une fois que le système a accepté la demande d'appel d'une action. Un modèle synchrone (appelé appel bloquant) est disponible, mais il n'est pas abordé dans cet article.

#### 7. Appel du code : auteur de l'appel
{: #about_invoker}

L'**auteur de l'appel** est le centre nerveux d'OpenWhisk. Sa tâche consiste à appeler une action. Il est également implémenté dans Scala. Mais plus encore, pour exécuter des actions de manière et en toute sécurité, il utilise **Docker**.

Docker est utilisé pour configurer un nouvel environnement auto-encapsulé (appelé *conteneur*) pour chaque action que nous appelons de manière rapide, isolée et contrôlée. Pour chaque appel d'action, un conteneur Docker est généré et le code d'action est injecté. Le code est ensuite exécuté à l'aide des paramètres qui lui ont été transmis, le résultat est obtenu et le conteneur est détruit. Il est possible d'optimiser les performances à ce stade pour réduire les exigences de maintenance, ainsi que les temps de réponse.

Dans le cas qui nous intéresse, avec une action basée sur *Node.js* l'auteur de l'appel démarre un conteneur Node.js. Il injecte ensuite le code à partir de *myAction*, l'exécute sans paramètre, procède à l'extraction du résultat, sauvegarde les journaux et détruit à nouveau le conteneur Node.js.

#### 8. Stockage des résultats : CouchDB
{: #about_storing}

Le résultat étant obtenu par l'auteur de l'appel, il est stocké dans la base de données **whisks** en tant qu'activation sous l'ID activation. La base de données **whisks** se situe dans **CouchDB**.

Dans ce cas précis, l'auteur de l'appel reçoit l'objet JSON résultant de l'action, récupère le journal écrit par Docker, place tous ces éléments dans l'enregistrement d'activation et stocke ce dernier dans la base de données. Voir l'exemple ci-dessous :
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

Notez que l'enregistrement contient à la fois le résultat renvoyé et les journaux qui ont été écrits. Il contient également l'heure de début et de fin de l'appel de l'action. Les enregistrements d'activation contiennent d'autres zones, mais par souci de simplicité, celles-ci ont été retirées.

A présent vous pouvez réutiliser l'API REST (en reprenant à partir de l'étape 1) pour obtenir votre activation, puis le résultat de votre action. Pour cela, exécutez la commande suivante :

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Récapitulatif
{: #about_summary}

Vous pouvoir voir de quelle façon une simple action **ibmcloud fn action invoked myAction** transite par différentes étapes du système {{site.data.keyword.openwhisk_short}}. Le système proprement dit n'est constitué que de deux composants personnalisés, le **contrôleur** et l'**auteur de l'appel**. Tout le reste est déjà là, développé par toutes ces personnes de la communauté open source.

Vous trouverez des informations supplémentaires sur {{site.data.keyword.openwhisk_short}} dans les rubriques suivantes :

* [Noms d'entité](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)
* [Sémantique d'action](/docs/openwhisk?topic=cloud-functions-limits#limits_semantics)
* [Limites](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)
* [Référence de l'API REST](/apidocs/functions)




