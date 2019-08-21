---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: functions compared, architecture, limitless, functions

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


# Architecture FaaS (Function as a Service)
{: #faas}

{{site.data.keyword.openwhisk}} fournit OpenWhisk dans un environnement sans serveur hautement évolutif. Vous pouvez comparer l'architecture sans serveur et le traitement informatique abordable de {{site.data.keyword.openwhisk_short}} à d'autres modèles d'architecture.
{: shortdesc}

## Pourquoi choisir une architecture sans serveur ?

- Pas de frais généraux liés à l'infrastructure
- Maintenance minimale
- Bonne rentabilité
- Facilité d'adaptation
- Rapidité


## Comparaison d'architectures OpenWhisk
{: #faas_architecture}

Les architectures OpenWhisk suivantes sont comparées :

1. **Function as a Service (FaaS)** sur [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}. IBM est le seul fournisseur à proposer une architecture OpenWhisk gérée. Une bonne introduction au modèle de programmation sans serveur utilisant une plateforme FaaS est disponible sur le [blogue de Martin Fowler](https://martinfowler.com/articles/serverless.html){: external} et vous pouvez voir des [scénarios d'utilisation](/docs/openwhisk?topic=cloud-functions-use_cases) pour exécuter OpenWhisk avec une conception sans serveur.

2. **IaaS (Infrastructure as a Service)** avec OpenWhisk RYO (Roll Your Own). Vous pouvez télécharger OpenWhisk depuis le site Apache Incubator Project et l'exécuter sur [{{site.data.keyword.cloud_notm}} IaaS](https://cloud.ibm.com/catalog?category=compute){: external}.

3. **Platform as a Service (PaaS)** en tant qu'environnement d'exécution d'application géré. L'environnement d'exécution [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java){: external} géré par l'implémentation {{site.data.keyword.cloud_notm}} Foundry en est un bon exemple.

4. **Container as a Service (CaaS)** en tant qu'environnement de conteneur géré. [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started#container_index) en est un bon exemple.

5. **IaaS (Infrastructure as a Service)** avec environnement d'exécution Java EE. La solution [WebSphere Application Server VM sur {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server){: external} en est un bon exemple.

Le tableau suivant compare les éléments de chaque architecture du point de vue d'un développeur qui crée et exploite des applications :


| Rubrique | (1) FaaS sur {{site.data.keyword.openwhisk_short}} | (2) IaaS avec OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS avec Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unité d'application	|	Fonction unique (généralement un petit bloc de code dans JavaScript, Swift ou un conteneur Docker) - peut peser moins d'un ko, mais aussi être plus volumineux. Ne dépasse généralement pas quelques ko.	|	Identique à la colonne (1)	|	Dépend de l'environnement d'exécution utilisé. Il peut s'agir d'un fichier EAR ou WAR ou d'un autre bundle d'application spécifique au langage, généralement assez volumineux - ko ou même Mo avec de nombreux services dans un bundle, mais peut aussi être aussi petit qu'un service unique	|	Le conteneur Docker est l'unité de déploiement.	|	Machine virtuelle avec serveur d'application, fichier EAR ou WAR et autres dépendances - taille exprimée généralement en Go.	|
|	Empreinte sur les ressources	|	L'utilisateur final ne paye pas et ne se soucie pas de l'utilisation de la mémoire, de l'UC ou d'autres ressources. Bien que l'action ait une certaine empreinte, l'utilisateur n'a pas à s'en soucier.	|	Elevée. L'utilisateur final doit d'abord rendre disponible l'environnement IaaS et seulement alors installer et configurer OpenWhisk par-dessus.	|	Faible. L'utilisateur final paye pour l'utilisation de la mémoire et de l'UC pour les applications en exécution, mais n'est pas facturé pour celles qui ne sont pas en exécution	|	Faible à moyenne	|	Elevée. L'utilisateur est facturé pour le stockage sur disque, la mémoire, les UC, et éventuellement pour d'autres composants quand l'application est en cours d'exécution. Lorsqu'elle est arrêtée, il n'encourt que les coûts du stockage.	|
|	Installation et configuration	|	Non requises	|	Difficiles - entièrement à la charge de l'utilisateur final	|	Non requises	|	Modérées - matériel, mise en réseau, système d'exploitation, outils de gestion de conteneurs fournis par le fournisseur CaaS, images, connectivité et instances par l'utilisateur final.	|	Difficiles - matériel, mise en réseau, système d'exploitation, installation initiale du logiciel Java EE fourni par le fournisseur, configuration additionnelle, mise en cluster, mise à l'échelle par l'utilisateur final	|
|	Délai jusqu'à la mise à disposition	|	Millisecondes	|	Voir les colonnes (4) et (5)	|	Minutes	|	Minutes	|	Heures	|
|	Administration en continu	|	Aucune	|	Difficile	|	Aucune	|	Modérée	|	Difficile	|
|	Mise à l'échelle élastique	|	Chaque action est instantanément et systématiquement mise à l'échelle en fonction de la charge. Il n'est pas nécessaire de mettre à disposition par avance des machines virtuelles ou d'autres ressources	|	Non fournie - L'utilisateur final doit fournir la capacité de traitement sur IaaS et gérer la mise à l'échelle des machines virtuelles. Une fois les machines virtuelles mises à l'échelle, OpenWhisk met automatiquement à l'échelle les actions, mais les ressources doivent être mises à disposition à l'avance	|	Automatique, mais lente. Lorsque la charge est accrue, les utilisateurs doivent attendre pendant plusieurs minutes que l'action de mise à l'échelle se termine. La mise à l'échelle automatique requiert un réglage consciencieux.	|	Automatique, mais lente. Lorsque la charge est accrue, les utilisateurs doivent attendre pendant plusieurs minutes que l'action de mise à l'échelle se termine. La mise à l'échelle automatique requiert un réglage consciencieux.	|	Non fournie	|
|	Planification de la capacité	|	Non nécessaire. FaaS alloue automatiquement la capacité requise	|	Allocation d'avance d'une capacité suffisante ou utilisation d'un script à cet effet.	|	Un certain degré de planification de capacité est requis, mais un certain niveau d'augmentation automatique de la capacité est fourni.	|	Un certain degré de planification de capacité est requis, mais un certain niveau d'augmentation automatique de la capacité est fourni.	|	Nécessité d'allouer une capacité statique suffisante pour gérer les charges de travail en période de pointe	|
|	Connexions persistantes et état	|	Limitée - Impossible de maintenir une connexion persistante, sauf en cas de mise en cache du conteneur. Généralement, l'état doit être mémorisé dans une ressource externe.	|	Identique à la colonne (1)	|	Prise en charge - Possibilité de maintenir ouvert un socket ou une connexion pendant longtemps, possibilité de conserver en mémoire l'état entre les appels.	|	Prise en charge - Possibilité de maintenir ouvert un socket ou une connexion pendant longtemps, possibilité de conserver en mémoire l'état entre les appels.	|	Prise en charge - Possibilité de maintenir ouvert un socket ou une connexion pendant longtemps, possibilité de conserver en mémoire l'état entre les appels.	|
|	Maintenance	|	Néant - La totalité de la pile est gérée par IBM.	|	Significative - Selon l'environnement cible, l'utilisateur doit mettre à disposition le matériel, l'opération en réseau, le système d'exploitation, la mémoire, la base de données, installer et assurer la maintenance d'OpenWhisk, etc.	|	Néant - La totalité de la pile est gérée par le fournisseur.	|	Significative - L'utilisateur doit créer des images personnalisées et en assurer la maintenance, déployer et gérer des conteneurs, des connexions entre les conteneurs, etc.	|	Significative - L'utilisateur doit allouer des machines virtuelles, gérer et mettre à l'échelle individuellement les serveurs Java EE.	|
|	Haute disponibilité (HA) et reprise après incident (DR)	|	Inhérent / aucun coût supplémentaire.	|	RYO (Roll your own) 	|	Disponible pour un coût supplémentaire	|	Les conteneurs en état d'échec peuvent être redémarrés automatiquement.	|	Disponibles pour un coût supplémentaire, semi-automatique. Basculement automatique possible des machines virtuelles.	|
|	Sécurité	|	Fournie par le fournisseur	|	RYO (Roll your own)	|	Combinaison de RYO et de sécurité fournie par le fournisseur	|	Combinaison de RYO et de sécurité fournie par le fournisseur	|	RYO (Roll your own)	|
|	Vitesse de développement	|	Maximale	|	Maximale	|	Maximale	|	Moyenne	|	Lente	|
|	Utilisation des ressources (ressources inactives mais néanmoins facturées)	|	Les ressources ne sont jamais inactives vu qu'elles ne sont appelées que suite à une demande. Lorsque la charge de travail est absente, aucun coût n'est encouru ou aucune allocation de ressource ne se produit.	|	Vu que cette option utilise IaaS ou CaaS - des considérations similaires s'appliquent comme dans les colonnes (4) et (5)	|	Certaines ressources peuvent être inactives, et la mise à l'échelle automatique permet d'éliminer les ressources inactives. Un certain nombre d'instances en cours d'exécution doit toujours être présent et il est probable qu'elles seront utilisées à moins de 50 % de leur capacité. Aucun coût n'est encouru pour les instances arrêtées.	|	Similaire à la colonne (3)	|	Certaines ressources peuvent être inactives, mais la mise à l'échelle automatique n'est pas prise en charge. Un certain nombre d'instances en cours d'exécution doit toujours être présent et il est probable qu'elles seront utilisées à moins de 50 % de leur capacité. Les instances arrêtées peuvent induire un coût de stockage.	|
|	Maturité	|	Maturité rapide	|	Maturité rapide	|	Maturité rapide	|	Maturité modérée	|	Maturité élevée	|
|	Limites de ressources	|	[Certaines limites s'appliquent](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)	|	Selon les ressources allouées.	|	Non	|	Non	|	Non	|
|	Temps d'attente pour les services rarement utilisés	|	Les demandes inhabituelles peuvent encourir un temps de réponse de plusieurs secondes, mais de quelques microsecondes seulement par la suite.	|	Variable	|	Bref	|	Bref	|	Bref - en supposant que le système dispose de ressources suffisantes.	|
|	Type d'application Sweet spot	|	Traitement d'événements, IoT, back end mobile, microservices. Ne convient absolument pas aux applications monolithiques. Voir [Scénarios d'utilisation](/docs/openwhisk?topic=cloud-functions-use_cases)	|	Identique à la colonne (1), mais lorsque l'utilisateur veut opérer sur un cloud non IBM ou sur site.	|	Applications Web avec charge de travail 24 heures sur 24, 7 jours sur 7, services avec état ayant besoin de garder ouverte la connexion sur de longues périodes. Peut être utilisé pour exécuter des microservices ou des applications monolithiques.	|	Convient particulièrement bien aux applications de microservices.	|	Applications d'entreprise traditionnelles qui sont migrées depuis le site vers le cloud. Convient particulièrement bien aux applications monolithiques.	|
|	Facturation de la granularité et facturation	|	[Par blocs de 100 millisecondes](https://cloud.ibm.com/openwhisk/learn/pricing){: external}	|	Dépend de l'implémentation - Si IaaS ou CaaS est utilisé, des considérations similaires s'appliquent - Voir les colonnes (4) et (5)	|	Généralement facturée par heure (rarement par minute) pour le bundle de ressources (UC + mémoire + certain espace disque)	|	Similaire à la colonne (3)	|	Similaire à la colonne (3)	|
|	 Coût total de possession |	Pour ses applications sweet spot, ce coût sera probablement substantiellement moindre que les alternatives. Les ressources étant mises à l'échelle automatiquement, aucune allocation excédentaire ne se produit.	|	Pour les déploiements en cloud, probablement plus onéreux que OpenWhisk FaaS, mais pour un déploiement sur site, peut être moins coûteux que les architectures traditionnelles.	|	Relativement faible - L'utilisateur n'a pas besoin d'allouer ou de gérer des ressources et peut se concentrer sur son application. Un certain degré d'allocation excédentaire intervient comparé à l'architecture sans serveur	|	Modéré - L'utilisateur doit allouer et gérer des conteneurs et l'application, mais un certain degré d'allocation excédentaire intervient comparé à l'architecture sans serveur ou à PaaS	|	Relativement élevé - Du fait que la migration d'applications existantes vers le modèle de cloud natif peut avoir un coût prohibitif, cette solution peut constituer un choix viable et économique pour ces applications.	|

## Considérations en matière de coûts
{: #faas_cost}

L'infrastructure de vos environnement de test, de préproduction, de test de charge, ainsi que d'autres environnements peut s'avérer coûteuse. Il faut du temps pour les configurer et, comme ils fonctionnent en principe 24 heures sur 24 et 7 jours sur 7, ils sont souvent sous-exploités et consomment de la capacité en grandes quantités. En utilisant une architecture sans serveur, les coûts afférant à plusieurs environnements sont générés en fonction de la charge et non pas du nombre d'environnements définis.
{: shortdesc}

Pour estimer les coûts d'une application sans serveur, vous pouvez utiliser la [calculatrice de prix](https://cloud.ibm.com/openwhisk/learn/pricing){: external}.

### Capacité illimitée
{: #faas_capacity}

Dans les architectures traditionnelles, chaque service consomme la quantité de capacité qui lui est alloué et vous êtes facturé pour la capacité consommée. L'architecture sans serveur de {{site.data.keyword.openwhisk_short}} réduit la contrainte liée à la granularité votre architecture de microservices.

Lorsque vous ne l'utilisez pas, {{site.data.keyword.openwhisk_short}} ne coûte rien. Votre code s'exécute chaque fois que se produit un appel HTTP, un changement d'état de base de données ou tout autre type d'événement qui déclenche l'exécution de votre code. Vous êtes facturé en millisecondes de temps d'exécution arrondies aux 100ms les plus proches, et non par heure d'utilisation de la machine virtuelle, peu importe que le travail effectué par cette machine ait été utile ou non. Comme vous ne payez qu'en cas d'événements consommés et non pas en fonction du nombre d'environnements, vous pouvez subdiviser votre application en 100, 1000 microservices voire plus.

### Exécution d'actions dans n'importe quelle région
{: #faas_region}

Dans les architectures traditionnelles, le code doit être exploité et exécuté dans chaque région et l'infrastructure correspondant à ces régions doit être facturée. Avec {{site.data.keyword.openwhisk_short}}, les actions sont déployables et exécutables dans toutes les régions sans frais supplémentaires. Vous pouvez augmenter la disponibilité et la résilience de votre code sans les restrictions habituelles liées aux coûts.

### Redondance par conception
{: #faas_redundancy}

Dans les architectures traditionnelles, les applications doivent être redondantes. Avec {{site.data.keyword.openwhisk_short}}, les processus ne doivent pas être à haute disponibilité car, de par leur conception, les applications sans serveur sont sans état et gérées en fonction des demandes et des événements. En éliminant la nécessité de créer explicitement de la redondance, la nature sans état des applications sans serveur contribue à réduire considérablement les coûts d'infrastructure.




