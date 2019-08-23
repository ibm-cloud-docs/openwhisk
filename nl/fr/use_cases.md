---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# Scénarios d'utilisation courants
{: #use_cases}

Le modèle d'exécution qui est proposé par {{site.data.keyword.openwhisk_short}} prend en charge divers scénarios d'utilisation. Les sections ci-après présentent des exemples classiques. Pour obtenir une présentation plus détaillée de l'architecture sans serveur, voir l'excellent [article de Mike Roberts sur le blogue de Martin Fowler](https://martinfowler.com/articles/serverless.html){: external} qui contient des scénarios d'utilisation et présente les avantages et les inconvénients, ainsi que les meilleures pratiques d'implémentation de cette architecture.
{: shortdesc}

## Microservices
{: #use_cases_microservices}

En dépit des avantages qu'elles procurent, les solutions basées sur les microservices sont toujours aussi difficiles à générer à l'aide de technologies cloud traditionnelles nécessitant souvent de contrôler une chaîne d'outils complexe et d'avoir recours à des pipelines de génération et d'opérations distincts. Les équipes réduites mais agiles perdent trop de temps à gérer les complexités d'infrastructure et de fonctionnement (par exemple, tolérance aux pannes, équilibrage de charge, mise à l'échelle automatique et journalisation). Elles souhaitent plus précisément pouvoir développer un code simplifié à valeur ajoutée à l'aide de langages de programmation qu'ils connaissent et apprécient et qui conviennent mieux pour la résolution de problèmes spécifiques.

La nature modulaire et intrinsèquement évolutive de {{site.data.keyword.openwhisk_short}} en fait un service idéal pour l'implémentation d'éléments de logique granulaires dans les actions. Les actions {{site.data.keyword.openwhisk_short}} sont indépendantes les unes des autres et peuvent être implémentées à l'aide d'une grande variété de langages qui sont pris en charge par {{site.data.keyword.openwhisk_short}} et accèdent à différents systèmes de back end. Chaque action peut être déployée, gérée et mise à l'échelle indépendamment des autres actions. L'interconnectivité entre les actions est fournie par {{site.data.keyword.openwhisk_short}} sous forme de règles, de séquences et de convention de dénomination. Ce environnement est de bon augure pour les applications basées sur les microservices.

Un autre argument important en faveur de {{site.data.keyword.openwhisk_short}} est le coût d'un système dans une configuration de reprise après incident. Comparons des microservices avec PaaS ou CaaS par rapport à l'utilisation de {{site.data.keyword.openwhisk_short}} en partant du principe que vous possédez 10 microservices utilisant des conteneurs ou des environnements d'exécution Cloud Foundry. En d'autres termes, il s'agit de 10 processus en exécution permanente et facturable, avec une seule zone de disponibilité, de 20 processus dans le cas de 2 zones de disponibilité, et de 40 processus dans le cas de deux régions avec deux zones de disponibilité chacune. Pour atteindre le même objectif avec {{site.data.keyword.openwhisk_short}}, vous pouvez les exécuter dans autant de zones de disponibilité et de régions que vous le désirez sans encourir un centime de frais supplémentaires.

## Applications Web
{: #use_cases_webapps}

Vu que {{site.data.keyword.openwhisk_short}} est géré par des événements, il offre plusieurs avantages aux applications destinées aux utilisateurs, tandis que les demandes HTTP provenant du navigateur de l'utilisateur servent d'événements. Les applications {{site.data.keyword.openwhisk_short}} n'utilisent des capacités de traitement et ne sont facturées que lorsqu'elles traitent des demandes d'utilisateurs. Le mode de secours en veille ou d'attente est inexistant. Cette fonction rend {{site.data.keyword.openwhisk_short}} considérablement moins onéreux que les applications de conteneurs ou Cloud Foundry traditionnelles. Ces deux outils affichent un temps d'inactivité, dans l'attente d'une demande utilisateur entrante, et sont facturées pour tout ce temps de "sommeil".

Une application Web complète peut être construite et exécutée avec {{site.data.keyword.openwhisk_short}}. Le fait de combiner des API sans serveur avec un hébergement de fichier statique pour des ressources de site, c'est-à-dire, des ressources HTML, JavaScript et CSS, signifie que vous pouvez construire entièrement des applications Web sans serveur. La simplicité d'exploitation d'un environnement {{site.data.keyword.openwhisk_short}} hébergé est liée au fait d'en être totalement exempté. Le fait que {{site.data.keyword.openwhisk_short}} soit hébergé sur {{site.data.keyword.cloud_notm}} est un avantage considérable par rapport à la mise en oeuvre et à l'exploitation d'un serveur Node.js Express ou tout autre contexte d'exécution de serveur traditionnel.

## IoT
{: #use_cases_iot}

Souvent, les scénarios Internet of Things dépendent intrinsèquement d'un capteur. Par exemple, une action dans {{site.data.keyword.openwhisk_short}} peut être déclenchée s'il est nécessaire de réagir à un relevé de capteur dépassant une température particulière. Les interactions IoT sont généralement sans état et présentent une charge élevée potentielle dans le cas d'événements spontanés majeurs, tels que des catastrophes naturelles, des tempêtes importantes ou des bouchons de circulation. Ces situations nécessitent un système élastique qui doit pouvoir être capable de passer rapidement d'une charge de travail normale faible à une charge de travail beaucoup plus élevée avec des temps de réponse prévisibles. Par conséquent, la capacité à traiter un grand nombre d'événements simultanés sans avertissement préalable envoyé au système est souhaitable. Il est difficile de construire un système pour répondre à ces exigences qui font appel à des architectures de serveur traditionnelles. Celles-ci manquent généralement de puissance et sont incapables de gérer les pics de trafic ou bien sont surdimensionnées et extrêmement onéreuses.

Il est possible d'implémenter des applications IoT qui utilisent des architectures de serveur traditionnelles. Toutefois, dans de nombreux cas, la combinaison de différents services et ponts de données nécessite des pipelines extrêmement performants et flexibles, allant des périphériques IoT au stockage en cloud et à une plateforme d'analyse. Souvent les ponts préconfigurés ne disposent pas de la programmabilité pour implémenter et ajuster une solution d'architecture spécifique. Etant donné la grande variété de pipelines possibles et le manque de standardisation en matière de fusion des données en général (pour IoT en particulier), il arrive souvent que le pipeline nécessite une transformation des données personnalisées. Ces transformations de données personnalisées concernent la conversion de format, le filtrage ou l'augmentation. {{site.data.keyword.openwhisk_short}} est un outil idéal pour implémenter ce type de transformation selon une méthode "sans serveur" où la logique personnalisée est hébergée sur une plateforme cloud élastique et entièrement gérée.

Examinez l'exemple d'application IoT suivant qui utilise {{site.data.keyword.openwhisk_short}}, Node-RED, Cognitive et d'autres services : [Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}.

![Exemple d'architecture de solution IoT](images/IoT_solution_architecture_example.png)

## Système de back end d'API
{: #use_cases_backend}

Les plateformes informatiques sans serveur permettent aux développeurs de créer rapidement des API sans utiliser de serveurs. {{site.data.keyword.openwhisk_short}} prend en charge la génération automatique d'API REST pour les actions. La [fonction de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-apigateway) peut appeler une action via l'API de passerelle {{site.data.keyword.openwhisk_short}} avec d'autres méthodes HTTP que POST et sans la clé d'API d'autorisation de l'action. Cette possibilité est utile non seulement pour exposer des API à des consommateurs externes, mais aussi pour construire des applications de microservices.

De plus, les actions {{site.data.keyword.openwhisk_short}} peuvent être connectées à un outil de gestion d'API de votre choix (tel qu'[IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} ou un autre outil). A l'instar d'autres scénarios d'utilisation, toutes les remarques concernant l'évolutivité et d'autres qualités de service s'appliquent. 

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} est un exemple d'application qui utilise des actions {{site.data.keyword.openwhisk_short}} au moyen d'une API REST.

Examinez l'exemple ci-après qui inclut une discussion portant sur l'[utilisation d'une plateforme sans serveur en tant que système de back end d'API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}.

## Système de back end mobile
{: #use_cases_mobile}

De nombreuses applications mobiles requièrent une logique côté serveur. Cela dit, les développeurs d'applications mobiles ne sont généralement pas familiers avec la gestion de la logique côté serveur et préfèrent se concentrer sur l'application qui s'exécute sur le périphérique. Cet objectif de développement peut être facilement atteint en utilisant {{site.data.keyword.openwhisk_short}} comme système de back end côté serveur, et représente une solution idoine. En outre, la prise en charge de Swift côté serveur permet aux développeurs de réutiliser leurs compétences existantes en matière de programmation iOS. Les modèles de charge des applications mobiles étant souvent imprévisibles, vous souhaiterez utiliser une solution {{site.data.keyword.openwhisk_short}}, telle que {{site.data.keyword.cloud}}. Cette solution est capable d'évoluer afin de répondre à pratiquement n'importe quelle demande en matière de charge de travail sans avoir à fournir des ressources à l'avance.

## Traitement de données
{: #use_cases_data}

Etant donné la quantité de données désormais disponible, le développement d'applications doit pouvoir traiter de nouvelles données, et éventuellement y réagir. Cette exigence inclut le traitement d'enregistrements de base de données structurés ainsi que de vidéos, d'images ou de documents non structurés. {{site.data.keyword.openwhisk_short}} peut être configuré par des flux fournis par le système ou personnalisés afin de réagir aux changements de données et d'exécuter automatiquement des actions sur les flux de données entrants. Les actions peuvent être programmées pour traiter des modifications, transformer des formats de données, envoyer et recevoir des messages, appeler d'autres actions et mettre à jour différents magasins de données. Les magasins de données pris en charge sont notamment des bases de données relationnelles SQL, des grilles de données en mémoire, une base de données, des fichiers, des courtiers de messages NoSQL et une variété d'autres systèmes. Les règles et séquences {{site.data.keyword.openwhisk_short}} fournissent suffisamment de souplesse pour modifier le pipeline de traitement sans avoir à effectuer de tâche de programmation, simplement via des changements de configuration. Les options de magasin de données et la faible maintenance administrative rendent un système basé sur {{site.data.keyword.openwhisk_short}} extrêmement agile et facilement adaptable aux exigences variables.

## Applications cognitives
{: #use_cases_cognitive}

Les technologies cognitives peuvent être combinées efficacement avec {{site.data.keyword.openwhisk_short}} pour créer des applications puissantes. Par exemple, IBM Alchemy API et Watson Visual Recognition peuvent être utilisés avec {{site.data.keyword.openwhisk_short}} pour extraire automatiquement des informations utiles provenant de vidéos sans avoir à regarder ces dernières. Cette technologie est l'extension "cognitive" du scénario [Traitement de données](#use_cases_data) décrit auparavant. L'emploi de {{site.data.keyword.openwhisk_short}} pour implémenter une fonction Bot qui est combinée à d'autres services cognitifs est une autre utilisation pratique.

C'est ce que fait précisément l'exemple d'application [Dark vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external} fourni. Dans cette application, l'utilisateur télécharge une vidéo ou une image à l'aide de l'application Web Dark Vision, qui la stocke dans une base de données {{site.data.keyword.cloudant_short_notm}}. Une fois la vidéo téléchargée, {{site.data.keyword.openwhisk_short}} la détecte en écoutant les modifications de {{site.data.keyword.cloudant_short_notm}} (déclencheur). {{site.data.keyword.openwhisk_short}} déclenche ensuite l'action de l'extracteur de vidéo. Pendant son exécution, l'extracteur produit des images et les stocke dans {{site.data.keyword.cloudant_short_notm}}. Les images sont ensuite traitées avec Watson Visual Recognition et les résultats sont stockés dans la même base de données {{site.data.keyword.cloudant_short_notm}}. Les résultats peuvent être affichés à l'aide de l'application Web Dark Vision ou d'une application iOS. {{site.data.keyword.cos_full_notm}} peut être utilisé en plus de {{site.data.keyword.cloudant_short_notm}} : les métadonnées des vidéos et des images sont stockées dans {{site.data.keyword.cloudant_short_notm}} et les fichiers de support dans {{site.data.keyword.cos_full_notm}}.

## Traitement d'événements avec Kafka ou {{site.data.keyword.messagehub}}
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} est particulièrement bien adapté à une utilisation conjointe avec Kafka, {{site.data.keyword.messagehub_full}} (basé sur Kafka) et d'autres systèmes de messagerie. Ces systèmes étant par nature gérés par événements, un environnement d'exécution géré lui-aussi par événements est requis pour traiter les messages. L'environnement d'exécution peut appliquer une logique métier à ces messages, ce qui est exactement ce qu'offre {{site.data.keyword.openwhisk_short}} avec les flux, les déclencheurs et les actions. Kafka et {{site.data.keyword.messagehub}} sont souvent utilisés pour les volumes de charge de travail élevés et imprévisibles et nécessitent un redimensionnement au pied levé des consommateurs de ces messages. Cela constitue, là encore, un atout de {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} intègre d'office la capacité de consommer, tout comme de publier, des messages fournis dans le package [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams).



