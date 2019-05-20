---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: use cases, microservices, web apps, iot, serverless, cognitive

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Scénarios d'utilisation courants
{: #use_cases}

Le modèle d'exécution qui est proposé par {{site.data.keyword.openwhisk_short}} prend en charge divers scénarios d'utilisation. Les sections ci-après présentent des exemples classiques. Pour obtenir une présentation plus détaillée de l'architecture sans serveur, voir l'excellent [article de Mike Roberts sur le blogue de Martin Fowler](https://martinfowler.com/articles/serverless.html) qui contient des scénarios d'utilisation et présente les avantages et les inconvénients, ainsi que les meilleures pratiques d'implémentation de cette architecture.
{: shortdesc}

## Microservices
{: #use_cases_microservices}

En dépit des avantages qu'elles procurent, les solutions basées sur les microservices sont toujours aussi difficiles à générer à l'aide de technologies cloud traditionnelles nécessitant souvent de contrôler une chaîne d'outils complexe et d'avoir recours à des pipelines de génération et d'opérations distincts. Les équipes réduites mais agiles consacrent trop de temps à gérer les complexités d'infrastructure et de fonctionnement (par exemple, tolérance aux pannes, équilibrage de charge, mise à l'échelle automatique et journalisation). Elles souhaitent plus précisément pouvoir développer un code simplifié à valeur ajoutée à l'aide de langages de programmation qu'ils connaissent et apprécient et qui conviennent mieux pour la résolution de problèmes spécifiques.

La nature modulaire et intrinsèquement évolutive de {{site.data.keyword.openwhisk_short}} en fait un service idéal pour l'implémentation d'éléments de logique granulaires dans les actions. Les actions {{site.data.keyword.openwhisk_short}} sont indépendantes les unes des autres et peuvent être implémentées à l'aide d'une grande variété de langages qui sont pris en charge par {{site.data.keyword.openwhisk_short}} et accèdent à différents systèmes de back end. Chaque action peut être déployée, gérée et mise à l'échelle indépendamment des autres actions. L'interconnectivité entre les actions est fournie par {{site.data.keyword.openwhisk_short}} sous forme de règles, de séquences et de convention de dénomination. Ce environnement est de bon augure pour les applications basées sur les microservices.

Un autre argument important en faveur de {{site.data.keyword.openwhisk_short}} est le coût d'un système dans une configuration de reprise après incident. Comparons des microservices avec PaaS ou CaaS par rapport à l'utilisation de {{site.data.keyword.openwhisk_short}} en partant du principe que vous possédez 10 microservices utilisant des conteneurs ou des environnements d'exécution Cloud Foundry. Autrement dit, il s'agit de 10 processus en exécution permanente et facturable, avec une seule zone de disponibilité, de 20 processus dans le cas de 2 zones de disponibilité, et de 40 processus dans le cas de deux régions avec deux zones de disponibilité chacune. Pour atteindre le même objectif avec {{site.data.keyword.openwhisk_short}}, vous pouvez les exécuter dans autant de zones de disponibilité et de régions que vous le désirez sans encourir un centime de frais supplémentaires.

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) est un exemple d'application au niveau de l'entreprise qui tire parti de {{site.data.keyword.openwhisk_short}} et de Cloud Foundry pour construire des applications de type 12 facteurs. Il s'agit d'une solution intelligente de gestion de la chaîne logistique globale qui vise à simuler un environnement qui exécute un système de planification des ressources d'entreprise (ERP). Elle enrichit le système ERP avec des applications améliorant la visibilité et l'agilité des gestionnaires de la chaîne logistique.

## Applications Web
{: #use_cases_webapps}

Vu que {{site.data.keyword.openwhisk_short}} est géré par des événements, il offre plusieurs avantages aux applications destinées aux utilisateurs, tandis que les demandes HTTP provenant du navigateur de l'utilisateur servent d'événements. Les applications {{site.data.keyword.openwhisk_short}} n'utilisent des capacités de traitement et ne sont facturées que lorsqu'elles traitent des demandes d'utilisateurs. Le mode de secours en veille ou d'attente est inexistant. Cette fonction rend {{site.data.keyword.openwhisk_short}} considérablement moins onéreux que les applications de conteneurs ou Cloud Foundry traditionnelles. Ces applications peuvent passer le plus clair de leur temps simplement en attente d'une demande utilisateur entrante et sont facturées pour tout ce temps de "sommeil".

Une application Web complète peut être construite et exécutée avec {{site.data.keyword.openwhisk_short}}. Le fait de combiner des API sans serveur avec un hébergement de fichier statique pour des ressources de site, c'est-à-dire, des ressources HTML, JavaScript et CSS, signifie que vous pouvez construire entièrement des applications Web sans serveur. La simplicité d'exploitation d'un environnement {{site.data.keyword.openwhisk_short}} hébergé est liée au fait d'en être totalement exempté. Le fait que {{site.data.keyword.openwhisk_short}} soit hébergé sur {{site.data.keyword.Bluemix_notm}} est un avantage considérable par rapport à la mise en oeuvre et à l'exploitation d'un serveur Node.js Express ou tout autre contexte d'exécution de serveur traditionnel.

Examinez les exemples ci-dessous illustrant l'utilisation de {{site.data.keyword.openwhisk_short}} pour construire une application Web :
- [Web actions: Serverless Web Apps with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [Build a user-facing {{site.data.keyword.openwhisk_short}} application with {{site.data.keyword.Bluemix_notm}} and Node.js](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Serverless HTTP handlers with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #use_cases_iot}

Souvent, les scénarios Internet of Things dépendent intrinsèquement d'un capteur. Par exemple, une action dans {{site.data.keyword.openwhisk_short}} peut être déclenchée s'il est nécessaire de réagir à un relevé de capteur dépassant une température particulière. Les interactions IoT sont généralement sans état et présentent une charge élevée potentielle dans le cas d'événements spontanés majeurs, tels que des catastrophes naturelles, des tempêtes importantes ou des bouchons de circulation. Ces situations nécessitent un système élastique qui doit pouvoir être capable de passer rapidement d'une charge de travail normale faible à une charge de travail beaucoup plus élevée avec des temps de réponse prévisibles. Par conséquent, la capacité à traiter un grand nombre d'événements simultanés sans avertissement préalable envoyé au système est souhaitable. Il est difficile de construire un système pour répondre à ces exigences qui font appel à des architectures de serveur traditionnelles. Celles-ci manquent généralement de puissance et sont incapables de gérer les pics de trafic ou bien sont surdimensionnées et extrêmement onéreuses.

Il est possible d'implémenter des applications IoT qui utilisent des architectures de serveur traditionnelles. Toutefois, dans de nombreux cas, la combinaison de différents services et ponts de données nécessite des pipelines extrêmement performants et flexibles, allant des périphériques IoT au stockage en cloud et à une plateforme d'analyse. Souvent les ponts préconfigurés ne disposent pas de la programmabilité souhaitée pour implémenter et ajuster une solution d'architecture spécifique. Etant donné la grande variété de pipelines possibles et le manque de standardisation en matière de fusion des données en général (pour IoT en particulier), il arrive souvent que le pipeline nécessite une transformation des données personnalisées. Ces transformations de données personnalisées concernent la conversion de format, le filtrage ou l'augmentation. {{site.data.keyword.openwhisk_short}} est un outil idéal pour implémenter ce type de transformation selon une méthode "sans serveur" où la logique personnalisée est hébergée sur une plateforme cloud élastique et entièrement gérée.

Examinez l'exemple d'application IoT suivant qui utilise {{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive et d'autres services : [Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c).

![Exemple d'architecture de solution IoT](images/IoT_solution_architecture_example.png)

## Système de back end d'API
{: #use_cases_backend}

Les plateformes informatiques sans serveur permettent aux développeurs de créer rapidement des API sans utiliser de serveurs. {{site.data.keyword.openwhisk_short}} prend en charge la génération automatique d'API REST pour les actions. La [fonction ](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) de {{site.data.keyword.openwhisk_short}} peut appeler une action via la passerelle d'API {{site.data.keyword.openwhisk_short}} avec des méthodes HTTP autres que POST et sans la clé d'API d'autorisation de l'action. Cette possibilité est utile non seulement pour exposer des API à des consommateurs externes, mais aussi pour construire des applications de microservices.

De plus, les actions {{site.data.keyword.openwhisk_short}} peuvent être connectées à un outil de gestion d'API de votre choix (tel qu'[IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) ou un autre). A l'instar d'autres scénarios d'utilisation, toutes les remarques concernant l'évolutivité et d'autres qualités de service s'appliquent.

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting) est un exemple d'application qui utilise des actions {{site.data.keyword.openwhisk_short}} au moyen d'une API REST.

Examinez l'exemple ci-après qui inclut une discussion portant sur l'[utilisation d'une plateforme sans serveur en tant que système de back end d'API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples).

## Système de back end mobile
{: #use_cases_mobile}

De nombreuses applications mobiles requièrent une logique côté serveur. Cela dit, les développeurs d'applications mobiles ne sont généralement pas familiers avec la gestion de la logique côté serveur et préfèrent se concentrer sur l'application qui s'exécute sur le périphérique. Cet objectif de développement peut être facilement atteint en utilisant {{site.data.keyword.openwhisk_short}} comme système de back end côté serveur, et représente une solution idoine. En outre, la prise en charge de Swift côté serveur permet aux développeurs de réutiliser leurs compétences existantes en matière de programmation iOS. Les modèles de charge des applications mobiles étant souvent imprévisibles, vous souhaiterez utiliser une solution {{site.data.keyword.openwhisk_short}}, telle que {{site.data.keyword.Bluemix}}. Cette solution est capable d'évoluer afin de répondre à pratiquement n'importe quelle demande en matière de charge de travail sans avoir à fournir des ressources à l'avance.

[Skylink](https://github.com/IBM-Bluemix/skylink) est un exemple d'application qui permet de connecter, via un iPad, un aéronef de type drone à IBM Cloud avec une capacité d'analyse d'image quasiment en temps réel qui tire parti de {{site.data.keyword.openwhisk_short}}, d'IBM {{site.data.keyword.cloudant_short_notm}}, d'IBM Watson et d'Alchemy Vision.

[BluePic](https://github.com/IBM-Swift/BluePic) est une application de partage de photos et d'images qui permet de prendre des photos et de les partager avec d'autres utilisateurs de BluePic. Cette application montre comment exploiter, dans une application mobile iOS 10, une application serveur basée sur Kitura composée dans Swift et utilisant {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.cos_full_notm}} pour les données image. AlchemyAPI est également utilisée dans la séquence {{site.data.keyword.openwhisk_short}} pour analyser l'image, extraire les balises de texte en fonction du contenu de l'image, puis envoyer une notification push à l'utilisateur.

## Traitement de données
{: #use_cases_data}

Etant donné la quantité de données désormais disponible, le développement d'applications doit pouvoir traiter de nouvelles données, et éventuellement y réagir. Cette exigence inclut le traitement d'enregistrements de base de données structurés ainsi que de vidéos, d'images ou de documents non structurés. {{site.data.keyword.openwhisk_short}} peut être configuré par des flux fournis par le système ou personnalisés afin de réagir aux changements de données et d'exécuter automatiquement des actions sur les flux de données entrants. Les actions peuvent être programmées pour traiter des modifications, transformer des formats de données, envoyer et recevoir des messages, appeler d'autres actions et mettre à jour différents magasins de données. Les magasins de données pris en charge sont notamment des bases de données relationnelles SQL, des grilles de données en mémoire, une base de données, des fichiers, des courtiers de messages NoSQL et une variété d'autres systèmes. Les règles et séquences {{site.data.keyword.openwhisk_short}} fournissent suffisamment de souplesse pour modifier le pipeline de traitement sans avoir à effectuer de tâche de programmation, simplement via des changements de configuration. Les options de magasin de données et la faible surcharge d'administration rendent un système basé sur {{site.data.keyword.openwhisk_short}} extrêmement agile et facilement adaptable aux exigences variables.

[OpenChecks](https://github.com/krook/openchecks) est un projet de validation de concept qui illustre comment {{site.data.keyword.openwhisk_short}} peut être exploité pour le traitement des dépôts de chèques sur un compte bancaire à l'aide d'une reconnaissance optique de caractères. Il est généré dans le service {{site.data.keyword.openwhisk_short}} public d'{{site.data.keyword.Bluemix_notm}} et s'appuie sur {{site.data.keyword.cloudant}} et {{site.data.keyword.cos_full_notm}}. Sur site, il peut utiliser CouchDB et OpenStack Swift. D'autres services de stockage, comme FileNet ou Cleversafe, pourraient être utilisés. Tesseract fournit la bibliothèque OCR.

## Applications cognitives
{: #use_cases_cognitive}

Les technologies cognitives peuvent être combinées efficacement avec {{site.data.keyword.openwhisk_short}} pour créer des applications puissantes. Par exemple, IBM Alchemy API et Watson Visual Recognition peuvent être utilisés avec {{site.data.keyword.openwhisk_short}} pour extraire automatiquement des informations utiles provenant de vidéos sans avoir à regarder ces dernières. Cette technologie est l'extension "cognitive" du scénario [Traitement de données](#data-processing) décrit auparavant. L'emploi de {{site.data.keyword.openwhisk_short}} pour implémenter une fonction Bot qui est combinée à d'autres services cognitifs est une autre utilisation pratique.

C'est ce que fait précisément l'exemple d'application [Dark vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp) fourni. Dans cette application, l'utilisateur télécharge une vidéo ou une image à l'aide de l'application Web Dark Vision, qui la stocke dans une base de données {{site.data.keyword.cloudant_short_notm}}. Une fois la vidéo téléchargée, {{site.data.keyword.openwhisk_short}} la détecte en écoutant les modifications de {{site.data.keyword.cloudant_short_notm}} (déclencheur). {{site.data.keyword.openwhisk_short}} déclenche ensuite l'action de l'extracteur de vidéo. Pendant son exécution, l'extracteur produit des images et les stocke dans {{site.data.keyword.cloudant_short_notm}}. Les images sont ensuite traitées avec Watson Visual Recognition et les résultats sont stockés dans la même base de données {{site.data.keyword.cloudant_short_notm}}. Les résultats peuvent être affichés à l'aide de l'application Web Dark Vision ou d'une application iOS. {{site.data.keyword.cos_full_notm}} peut être utilisé en plus de {{site.data.keyword.cloudant_short_notm}} : les métadonnées des vidéos et des images sont stockées dans {{site.data.keyword.cloudant_short_notm}} et les fichiers de support dans {{site.data.keyword.cos_short}}.

Il existe un [exemple d'application iOS Swift](https://github.com/gconan/BluemixMobileServicesDemoApp) qui illustre l'utilisation de {{site.data.keyword.openwhisk_short}}, d'IBM Mobile Analytics et de Watson pour analyser le ton et poster les conclusions sur un canal Slack.

## Traitement d'événements avec Kafka ou {{site.data.keyword.messagehub}}
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} est particulièrement bien adapté à une utilisation conjointe avec Kafka, {{site.data.keyword.messagehub_full}} (basé sur Kafka) et d'autres systèmes de messagerie. Ces systèmes étant par nature gérés par événements, un environnement d'exécution géré lui-aussi par événements est requis pour traiter les messages. L'environnement d'exécution peut appliquer une logique métier à ces messages, ce qui est exactement ce qu'offre {{site.data.keyword.openwhisk_short}} avec les flux, les déclencheurs et les actions. Kafka et {{site.data.keyword.messagehub}} sont souvent utilisés pour les volumes de charge de travail élevés et imprévisibles et nécessitent un redimensionnement au pied levé des consommateurs de ces messages. Cela constitue, là encore, un atout de {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} intègre d'office la capacité de consommer, tout comme de publier, des messages fournis dans le package [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka).

Un [exemple d'application qui implémente le scénario de traitement d'événement](https://github.com/IBM/openwhisk-data-processing-message-hub) est fourni avec {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}} et Kafka.
