---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

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


# Test des applications sans serveur
{: #test}

Testez chaque entité que vous créez à partir de l'interface de ligne de commande pour vérifier que votre application sans serveur fonctionne ou pour résoudre une erreur éventuelle.
{: shortdesc}


## Test des actions
{: #test-js}

Vous pouvez tester des actions en exécutant la commande `invoke`. Vous pouvez tester une action avec ou sans paramètres.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

Exemple Hello world :
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

Sortie :
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Test des paramètres stockés dans des fichiers JSON
{: #test_json_file}

Vous pouvez transmettre un fichier de paramètres au format JSON.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

Exemple de sortie :
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Test des paramètres entrés au format JSON
{: #test_json}

Vous pouvez transmettre des paramètres au format JSON avec votre appel.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

Exemple de sortie :
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Test des actions bloquantes
{: #test-block}

L'appel de l'action peut être bloquant ou non bloquant. Par défaut, les appels sont non bloquants. Si vous n'avez pas besoin du résultat de l'action immédiatement, utilisez un appel non bloquant.
{: shortdesc}

Les appels bloquants utilisent un style demande/réponse et attendent que le résultat de l'activation soit disponible. Le délai d'attente est inférieur à 60 secondes ou à la [limite de temps](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) de l'action.

Exécutez l'action dans le cloud en exécutant un appel bloquant :

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


Exemple de sortie :
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
      "status": "success",
      "success": true
  }
```
{: screen}

La commande génère les informations suivantes :
* Résultat de l'appel, s'il est disponible avant la fin du délai d'attente
* Sans l'option --result, l'ID d'activation est affiché dans le résultat. ID d'activation (`44794bd6aab74415b4e42a308d880e5b`) qui peut être utilisé pour extraire les journaux ou le résultat de l'appel.






## Test des déclencheurs
{: #test_triggers}

Les déclencheurs peuvent être exécutés, ou activés, à l'aide d'un dictionnaire de paires clé-valeur. Parfois, ce dictionnaire est
appelé événement. Les déclencheurs peuvent être exécutés explicitement par un utilisateur ou pour le compte d'un utilisateur par une source d'événements d'externe. Comme pour les actions, chaque exécution de déclencheur génère un ID d'activation.
{: shortdesc}

1. Exécutez le déclencheur.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    Un déclencheur qui n'est pas associé à une règle n'a aucun effet visible lorsqu'il est exécuté. Etant donné qu'aucune règle n'est associée à ce déclencheur, les paramètres transmis ne sont pas utilisés en entrée par une action.

    Exemple de sortie :

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Vérifiez que l'action est appelée en examinant l'enregistrement d'activation le plus récent.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Exemple de sortie :
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Obtenez davantage d'informations sur l'ID d'activation à partir du résultat de la commande précédente.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Test de délai de réalisation des activations
{: #test_time}

Vérifiez la durée de réalisation d'une activation en obtenant le journal des activations. Si cette durée est trop longue ou si vous devez ajuster le délai d'attente par défaut pour permettre une exécution plus longue de la fonction, vous pouvez mettre à jour votre action avec un délai d'attente.
{: shortdesc}

1. Obtenez l'ID d'activation.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Exemple de sortie :
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Obtenez le journal de l'ID d'activation.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    La durée (`duration`) affiche la durée en millisecondes. Cette activation prend un peu plus de 2 secondes :

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. Mettez à jour l'action avec un délai d'attente en millisecondes.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Exemple :
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Test d'utilisation de la mémoire
{: #test_memory}

Si votre application est intégrée dans une image Docker, vous pouvez utiliser des commandes Docker pour vérifier l'utilisation de mémoire de votre application.
{: shortdesc}

1. Créez un conteneur en local pour l'exécution de votre image Docker.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Obtenez une liste des conteneurs pour extraire l'ID d'un conteneur.

    ```
    docker ps
    ```
    {: pre}

3. Vérifiez les statistiques du conteneur en cours d'exécution.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Examinez l'utilisation de la mémoire du conteneur. Si la valeur n'entre pas dans les limites du système, effectuez quelques ajustements dans votre script.

5. Après avoir examiné ces informations, vous pouvez arrêter le conteneur en cours d'exécution.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Supprimez le conteneur.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}






