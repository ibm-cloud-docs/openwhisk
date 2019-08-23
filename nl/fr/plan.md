---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# Planification d'applications sans serveur
{: #plan}

Avant de commencer à créer votre fonction, tenez compte des décisions que vous aurez à prendre au cours de ce processus.
{: shortdesc}

## Examen de la prise en charge des contextes d'exécution
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} fournit des [contextes d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) standard pour exécuter le code. Cependant, comme {{site.data.keyword.openwhisk_short}} est sans serveur, vous n'êtes pas limité en termes de contextes d'exécution à utiliser. Vous pouvez créer des contextes d'exécution personnalisés en créant votre propre [image Docker personnalisée](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) pour intégrer votre code.
{: shortdesc}



## Architecture de votre code
{: #plan_architect}

Vous pouvez être amené à personnaliser votre code existant pour qu'il puisse s'exécuter dans {{site.data.keyword.openwhisk}}. Si vous n'avez pas encore votre code, tenez-en compte lorsque vous le ferez.
{: shortdesc}

1. Développez votre code en composants.

    Si vous disposez déjà d'une application que vous voulez rendre sans serveur, envisagez la méthode à utiliser pour la décomposer en plusieurs parties de plus petite taille. Chaque fonction contient un ensemble de déclencheurs pour le code que vous souhaitez exécuter. Si, par exemple, une erreur est générée dans GitHub, exécutez ce code JavaScript. Si votre application comprend plusieurs événements ou actions de ce type, envisagez de les séparer en différentes fonctions.

2. Utilisez des services {{site.data.keyword.cloud_notm}} à la place d'infrastructures (frameworks).

    Au lieu d'utiliser des infrastructures pour rendre les fonctionnalités accessibles dans votre fonction en phase d'exécution, vous pouvez recourir à des services {{site.data.keyword.cloud}}. De nombreuses tâches courantes que les infrastructures vous aident à exécuter sont disponibles sous forme de services dans {{site.data.keyword.cloud}}.
    {: shortdesc}

    Par exemple, au lieu d'utiliser une infrastructure pour l'authentification, essayez {{site.data.keyword.appid_full}}. Si vous avez besoin de stockage de fichiers externes, essayez {{site.data.keyword.cos_full}}.

    Si la fonctionnalité que vous voulez incorporer n'est pas disponible sous forme de service dans {{site.data.keyword.cloud}}, vous pouvez toujours l'intégrer à des flux et à des API, sans recourir à des infrastructures.

3. [Vérifiez que votre code respecte les limites du système.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    Votre code doit respecter les limites du système pour être exécuté. Par exemple, si votre package de code est supérieur à 48 Mo, vous aurez peut-être à fractionner le code en plus petites parties ou à l'intégrer sous forme d'image Docker.

    Les applications contenant un grand nombre de modules tiers, de bibliothèques natives ou d'outils externes peuvent être exécutées dans le respect de cette limite. Si vous créez une action de package .zip ou .jar qui dépasse 48 Mo, vous devez étendre l'image d'exécution avec des dépendances. Utilisez ensuite un fichier source unique ou une archive plus petite que 48 Mo. Par exemple, si vous créez un environnement d'exécution Docker personnalisé, incluant les bibliothèques partagées nécessaires, ces dépendances ne sont pas tenues d'être présentes dans le fichier archive. Des fichiers source privés peuvent toujours être intégrés dans l'archive et injectés lors de l'exécution.

4. Déterminez les paramètres qui doivent être injectés dans votre code.

    Dans les actions sans serveur, les données sont fournies en ajoutant des paramètres aux actions. Les paramètres sont déclarés sous forme d'argument dans la fonction main serverless. Le plus souvent, ces paramètres sont des données d'identification de service, mais ils peuvent être composés de tout ce qui rend votre code réutilisable avec différents déclencheurs.

5. [Vérifiez les exigences structurelles de votre code pour pouvoir l'utiliser dans une fonction. ](/docs/openwhisk?topic=cloud-functions-prep)

    Que vous disposiez déjà d'une application ou que vous envisagiez de développer un script à utiliser, votre code a sans doute besoin de quelques ajustements pour qu'{{site.data.keyword.openwhisk}} puisse l'utiliser. Le code même doit satisfaire certaines exigences structurelles, telles que les paramètres d'entrée et les résultats en sortie. Il doit être intégré dans un seul fichier incluant l'ensemble de ses dépendances. 







## Détermination de votre source d'événement
{: #plan_source}

Envisagez votre fonction en termes d'événement que vous voulez obtenir pour déclencher le code à exécuter. Vous souhaitez peut-être que le code s'exécute sur chaque validation dans un référentiel GitHub. Ou peut-être voulez-vous que votre code s'exécute à chaque fois qu'une mise à jour est effectuée dans une base de données Cloudant.
{: shortdesc}

Après avoir choisi l'événement déclencheur de votre fonction, examinez les packages disponibles que vous pouvez utiliser. Vous pouvez en utiliser un pour simplifier le développement de votre fonction. Sinon, vous pouvez créer votre propre package pour votre source d'événement.

Il vous faudra peut-être revenir à votre code pour y effectuer quelques modifications en fonction de vos sélections d'événements.


## Organisation de déploiements sur plusieurs environnements
{: #plan_environments}

Déterminez comment vous souhaitez déployer vos fonctions dans différents environnements, par exemple les environnements de déploiement, de préproduction et de production.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} étant un service Cloud Foundry, vous pouvez gérer le déploiement de vos fonctions dans des organisations et des espaces Cloud Foundry fournis pour vous dans {{site.data.keyword.cloud_notm}}. Pour organiser vos fonctions sur ces environnements, vous pouvez choisir de créer une organisation par fonction. Créez ensuite un espace pour chaque environnement dont vous avez besoin. Vous pouvez à la place avoir une organisation pour chaque environnement et créer un espace par fonction. Peu importe comment vous organisez vos organisations et vos espaces, choisissez une structure qui vous permette de gérer efficacement vos entités fonctionnelles.

Vous pouvez également utiliser des [espaces de nom](/docs/openwhisk?topic=cloud-functions-namespaces) pour isoler des ressources. Chaque espace {{site.data.keyword.cloud_notm}} contient un espace de nom Open Whisk par défaut. Vous pouvez regrouper des entités, telles que des actions ou des déclencheurs au sein d'un espace de nom, puis créer des règles IAM (Identity and Access Management) pour gérer les droits d'accès des utilisateurs correspondant à ce groupe.




