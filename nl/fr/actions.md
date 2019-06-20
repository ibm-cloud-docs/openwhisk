---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: actions, serverless, javascript, node, node.js

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


# Création d'actions
{: #actions}

Créez une action, qui correspond à une fonction de niveau supérieur renvoyant un objet JSON. Vous pouvez regrouper des actions dans un package pour simplifier la gestion de vos actions.
{: shortdesc}

Avant de commencer :
Pour créer une action, votre code source doit respecter un certain nombre de critères. Par exemple, si vous voulez créer une action à partir d'un code inclus dans plusieurs fichiers, intégrez votre code dans un seul fichier avant de créer l'action. Voir la section sur la [préparation de votre code d'application pour un environnement sans serveur](/docs/openwhisk?topic=cloud-functions-prep) pour obtenir des détails sur les exigences relatives à chaque contexte d'exécution.


## Création d'actions à partir de l'interface de ligne de commande
{: #actions_cli}

1. Créez une action.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  Exemple :
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Exemple de sortie :

  ```
  ok: created action hello
  ```
  {: screen}

  Astuces :
  - Pour réduire les coûts, vous pouvez définir des limites.
      - Pour définir une limite d'utilisation de la mémoire, incluez `--memory VALUE` dans la commande create, avec une valeur en mégaoctets.
      - Pour définir un délai d'attente, incluez `--timeout VALUE` dans la commande create, avec une valeur en millisecondes.
  - Si vous avez intégré votre code en tant qu'image Docker, incluez `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` dans la commande create au lieu du chemin d'accès local à votre application et de l'indicateur --kind. Gérez bien vos images en évitant, dans la mesure du possible, d'utiliser la balise `latest`. Lorsque la balise `latest` est utilisée, l'image associée à cette balise est utilisée, alors qu'il ne s'agit pas forcément de l'image la plus récente qui a été créée.  
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  


2. Vérifiez que l'action figure dans votre liste d'actions.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemple de sortie :

  ```
  actions
  hello       private
  ```
  {: screen}


## Mise à jour d'applications ou de contextes d'exécution dans les actions
{: #actions_update}

Vous pouvez exécuter la commande update dès que vous devez mettre à jour le code dans votre application ou pour effectuer la migration vers une version de contexte d'exécution plus récente. Par exemple, comme Node.js version 8 est en mode maintenance, vous souhaiterez peut-être faire basculer le contexte d'exécution sur Node.js 10.

Lorsque vous effectuez la migration vers une nouvelle version de contexte d'exécution, vous aurez peut-être à modifier le code de votre application pour vous conformer à la nouvelle version du contexte d'exécution. Dans la plupart des cas, les versions de contexte d'exécution sont compatibles.
{: tip}

1. Mettez à jour votre application localement.

2. Si vous avez intégré votre application en tant qu'image Docker, téléchargez l'image la plus récente dans Docker Hub. Ainsi, le système pourra extraire votre nouvelle image Docker à la prochaine exécution du code pour votre action. Si un conteneur en cours d'exécution utilise une version précédente de votre image Docker, les nouveaux appels continuent d'utiliser cette image. Vous devez exécuter la commande update pour que les nouveaux appels commencent à utiliser la nouvelle image.

3. Mettez à jour une action et incluez le chemin d'accès local à votre application ou à l'image Docker.
    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    Exemple :
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    Exemple de sortie :

    ```
    ok: updated action hello
    ```
    {: screen}

    Si vous avez intégré votre code en tant qu'image Docker, incluez `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` dans votre commande create au lieu du chemin d'accès à votre application locale et de l'indicateur --kind. Gérez bien vos images en évitant, dans la mesure du possible, d'utiliser la balise `latest`. Lorsque la balise `latest` est utilisée, l'image associée à cette balise est utilisée, alors qu'il ne s'agit pas forcément de l'image la plus récente qui a été créée.
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## Liaison de paramètres à des actions
{: #actions_params}

Vous pouvez lier des paramètres à des actions pour définir des paramètres par défaut. Les paramètres liés servent de paramètres par défaut aux actions sauf s'ils sont fournis au moment de l'appel.
{: shortdesc}

Avant de commencer, [créez l'action](#actions_cli).

Pour lier les paramètres :

1. Mettez à jour une action et liez-la aux paramètres par défaut.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    Exemple :
    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: updated action MyApp
    ```
    {: screen}

    Si vous modifiez vos paramètres de données d'identification qui ne concernent pas un service, l'exécution d'une commande `action update` avec de nouveaux paramètres entraîne la suppression de tous les paramètres existants qui ne sont pas spécifiés dans la commande `action update`. Par exemple, si vous exécutez la commande `action update -p key1 new-value -p key2 new-value` en omettant d'autres paramètres qui étaient définis, ces paramètres n'existeront plus une fois l'action mise à jour. Tous les services liés à l'action sont également supprimés. Si vous avez lié un service, vous devez à nouveau [lier ce service à votre action](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Vérifiez que les paramètres ont été liés à l'action.
    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

Facultatif : pour effacer les paramètres qui étaient déjà liés, mettez à jour l'action sans inclure de paramètre.
```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## Chaînage d'actions sous forme de séquences d'actions
{: #actions_seq}

Vous pouvez créer une action qui assemble des actions dans une séquence. Le résultat d'une action est transmis sous forme d'argument à l'action suivante.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

Les paramètres transmis entre les actions de la séquence sont explicites, sauf les paramètres par défaut. Par conséquent, les paramètres qui sont transmis à la séquence d'actions ne sont disponibles que pour la première action de la séquence. Le résultat de la première action dans la séquence devient l'objet JSON d'entrée de la deuxième action de la séquence, et ainsi de suite. Cet objet ne comporte pas les paramètres qui sont transmis initialement à la séquence sauf si la première action les inclut dans son résultat. Les paramètres d'entrée d'une action sont fusionnés avec ses paramètres par défaut, les premiers étant prioritaires et remplaçant tout paramètre par défaut correspondant.

Une séquence n'a pas de délai d'attente global distinct des délais de chaque action au sein de la séquence. Etant donné qu'une séquence est un pipeline d'opérations, une action qui échoue rompt le pipeline. Si une action dépasse le délai d'attente, la séquence entière est sortie avec cet échec.

Ensuite, lorsque vous créez une règle ou appelez les actions, utilisez le nom de la séquence.




## Package d'actions
{: #actions_pkgs}

Dans {{site.data.keyword.openwhisk}}, vous pouvez utiliser des packages afin de regrouper des actions et des flux associés et les partager. Les packages permettent également le partage des paramètres entre toutes les entités au sein d'un package.
{: shortdesc}

Un package peut inclure des *actions* et des *flux*.
- Une action est un élément de code qui s'exécute sur {{site.data.keyword.openwhisk_short}}. Par exemple, le package {{site.data.keyword.cloudant}} contient des actions permettant de lire et d'écrire des enregistrements dans une base de données {{site.data.keyword.cloudant_short_notm}}.
- Un flux est utilisé pour configurer une source d'événements externe afin d'exécuter des événements déclencheurs. Par exemple, le package Alarm inclut un flux qui peut exécuter un déclencheur à la fréquence spécifiée.



1. Créez un package.
  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. Obtenez un récapitulatif du package. Remarquez que le package est vide.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/custom
  ```
  {: screen}

4. Créez une action et incluez-la dans le package. La création d'une action dans un package requiert que vous ajoutiez le nom de package comme préfixe au nom d'action. L'imbrication de packages n'est pas autorisée. Un package ne peut contenir que des actions et ne peut pas comporter d'autres packages.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. Obtenez un récapitulatif du package.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## Liaison de paramètres à des packages
{: #actions_pkgs_params}

Vous pouvez configurer des paramètres par défaut pour toutes les entités d'un package en définissant des paramètres au niveau du package qui sont hérités par toutes les actions du package.

Les paramètres liés servent de paramètres par défaut pour les actions dans le package, sauf si :

- L'action elle-même comporte un paramètre par défaut.
- L'action comporte un paramètre fourni au moment de l'appel.

Avant de commencer, créez un package comprenant au moins une action.

1. Mettez à jour le package et liez-le avec le paramètre par défaut.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    Exemple :
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: updated package MyApp
    ```
    {: screen}

    Si vous modifiez vos paramètres de données d'identification qui ne concernent pas un service, l'exécution d'une commande `package update` avec de nouveaux paramètres entraîne la suppression de tous les paramètres existants qui ne sont pas spécifiés dans la commande `package update`. Par exemple, si vous exécutez la commande `package update -p key1 new-value -p key2 new-value` en omettant d'autres paramètres qui étaient définis, ces paramètres n'existeront plus une fois le package mis à jour. Tous les services qui étaient liés au package sont également supprimés. Par conséquent, après avoir mis à jour d'autres paramètres, vous devez à nouveau [lier les services à votre package](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Vérifiez que les paramètres ont été liés au package.
    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. Vérifiez que le package a hérité des paramètres.
    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## Partage des packages d'actions
{: #actions_pkgs_share}

Une fois que les actions et les flux qui constituent un package ont été débogués et testés, le package peut être partagé avec tous les utilisateurs de {{site.data.keyword.openwhisk_short}}. Le partage du package permet aux utilisateurs de lier le package, d'appeler des actions du package et de créer des actions de séquence et des règles {{site.data.keyword.openwhisk_short}}. Les actions et les flux qui se trouvent dans un package partagé sont _publics_. Si le package est privé, son contenu est également privé.
{: shortdesc}

1. Partagez le package avec tous les utilisateurs :
  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. Affichez la propriété `publish` du package pour vérifier qu'elle est désormais associée à la valeur true.
  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. Obtenez une description du package pour fournir à d'autres utilisateurs le nom qualifié complet du package pour qu'ils puissent le lier ou appeler des actions dedans. Le nom qualifié complet comprend l'espace de nom. Dans cet exemple, il s'agit de l'espace de nom `myNamespace`.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## Variables d'environnement pour les actions
{: #actions_envvars}

L'environnement d'une action contient plusieurs variables d'environnement spécifiques à l'action en cours d'exécution. Les propriétés sont accessibles dans l'environnement système de tous les contextes d'exécution pris en charge. Ces propriétés permettent aux actions de fonctionner à l'aide d'un programme avec des actifs au moyen de l'API REST, ou de définir une alarme interne lorsque l'action est sur le point d'utiliser la totalité du budget de temps qui lui a été alloué.
{: shortdesc}

| Propriété | Description |
| -------- | ----------- |
| `__OW_API_HOST` | Hôte d'API pour le déploiement qui exécute cette action. |
| `__OW_API_KEY` | Clé d'API pour l'objet qui appelle l'action. Il peut s'agir d'une clé d'API restreinte. Elle n'est pas indiquée, sauf si elle est explicitement demandée. Voir [Annotations](/docs/openwhisk?topic=cloud-functions-annotations). |
| `__OW_NAMESPACE` | Espace de nom pour l'activation. Cet espace de nom peut être différent de l'espace de nom pour l'action. |
| `__OW_ACTION_NAME` | Nom qualifié complet de l'action en cours d'exécution. |
| `__OW_ACTIVATION_ID` | ID d'activation pour cette instance d'action en cours d'exécution. |
| `__OW_DEADLINE` | Durée approximative, en millisecondes époque, lorsque cette action consomme l'intégralité du quota de durée. |

### Incorporation de variables d'environnement dans votre application
{: #actions_envvars_app}

Pour visualiser les valeurs d'une action, incluez leur affichage dans votre code d'application et faites-les apparaître dans les résultats.

Exemple pour Python :
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

Après avoir mis à jour et activé le code dans une action, le résultat comprend le nom qualifié complet de l'action.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
