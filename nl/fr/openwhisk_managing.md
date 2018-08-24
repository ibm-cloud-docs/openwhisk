---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gestion d'actions
{: #openwhisk_managing}

Vous pouvez gérer des actions en surveillant le résultat d'une action, en obtenant des informations spécifiques sur une action, ou encore en supprimant des actions.
{: shortdec}

## Extractions d'actions
{: #getting-actions}

Après que vous avez créé une action, vous pouvez obtenir des informations supplémentaires sur celle-ci et répertorier les actions de votre espace de nom.
{: shortdesc}

Pour répertorier toutes les actions que vous avez créées :
```
ibmcloud fn action list
```
{: pre}

A mesure que vous créez d'autres actions, il peut être opportun de regrouper les actions associées dans des [packages](./openwhisk_packages.html). Pour filtrer votre liste d'actions de manière à afficher uniquement celles qui appartiennent à un package spécifique, utilisez la syntaxe de commande suivante :
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

Pour obtenir les métadonnées qui décrivent des actions spécifiques :

```
ibmcloud fn action get hello
```
{: pre}

Exemple de sortie :
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>Description du résultat de la commande <code>action get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idea"/> Description du résultat de la commande <code>action get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Espace de nom contenant cette action.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Nom de l'action.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>Version sémantique de l'action.</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code> : type d'action. Les valeurs possibles sont nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox et sequence.</li>
<li><code>code</code> : code Javascript ou Swift à exécuter lorsque kind est défini sur nodejs ou swift.</li>
<li><code>components</code> : actions de la séquence lorsque kind est défini sur sequence. Les actions sont répertoriées dans l'ordre.</li>
<li><code>image</code> : nom de l'image de conteneur lorsque kind est défini sur blackbox.</li>
<li><code>init</code> : référence de fichier zip en option lorsque kind est défini sur nodejs.</li>
<li><code>binary</code> : indique si l'action est compilée dans un exécutable binaire.</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotations sur cette action. Pour obtenir une liste des annotations possibles, consultez les rubriques de référence relatives aux [annotations d'action](openwhisk_annotations.html#action) et aux [annotations d'action Web](openwhisk_annotations.html#annotations-specific-to-web-actions).</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code> : délai d'attente, en millisecondes, défini pour l'action au terme duquel l'action est terminée. Valeur par défaut : 6000</li>
<li><code>memory</code> : limite de mémoire maximum, en Mo, définie pour l'action. Valeur par défaut : 256</li>
<li><code>logs</code> : limite de taille de journal maximum, en Mo, définie pour l'action. Valeur par défaut : 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indique si l'action est publiée publiquement.</td>
</tr>
</tbody></table>

## Affichage des détails d'activation
{: #activation}

Des actions {{site.data.keyword.openwhisk_short}} peuvent être appelées par d'autres utilisateurs en réponse à divers événements, ou dans le cadre d'une séquence d'actions. Chaque fois qu'une action est appelée, un enregistrement d'activation est créé pour cet appel. Pour obtenir des informations sur le résultat de l'action d'appel, vous pouvez obtenir des détails sur l'activation.

Pour extraire tous les ID d'enregistrement d'activation dans un espace de nom :
```
ibmcloud fn activation list
```
{: pre}

Pour obtenir des détails sur un enregistrement d'activation spécifique à un appel d'action :
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Exemple de sortie :
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Description du résultat de la commande <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idea"/> Description du résultat de la commande <code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Espace de nom contenant cette activation. Il peut s'agir d'un espace de nom différent de celui contenant l'action.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Nom de l'action.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>Version sémantique de l'action.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>Compte utilisateur qui a activé l'élément.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID de cet enregistrement d'activation.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>Heure à laquelle a débuté l'activation.</td>
</tr>
<tr>
<td><code>end</code></td>
<td>Heure à laquelle s'est terminée l'activation.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Durée, en millisecondes, d'exécution de l'activation.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code> : statut de sortie de l'activation.</li>
<li><code>statusCode</code> : code de statut. Si l'action était en erreur, code d'erreur HTTP.</li>
<li><code>success</code> : indique si l'action a abouti.</li>
<li><code>result</code> : valeur de retour de l'activation.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Journaux de cette activation.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotations sur cette action. Pour obtenir une liste des annotations possibles, consultez la [rubrique de référence relative aux annotations](openwhisk_annotations.html#activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indique si l'action est publiée publiquement.</td>
</tr>
</tbody></table>

## Accès aux métadonnées d'action dans le corps de l'action
{: #accessing-action-metadata-within-the-action-body}

L'environnement de l'action contient plusieurs propriétés spécifiques à l'action en cours d'exécution. Ces propriétés permettent à l'action de fonctionner à l'aide d'un programme avec des actifs OpenWhisk au moyen de l'API REST, ou de définir une alarme interne lorsque l'action est sur le point d'utiliser la totalité du budget de temps qui lui a été alloué. Les propriétés sont accessibles dans l'environnement système de tous les contextes d'exécution pris en charge : Node.js, Python, Swift, Java et Docker si le squelette OpenWhisk Docker est utilisé.

| Propriété | Description |
| -------- | ----------- |
| `__OW_API_HOST` | Hôte d'API pour le déploiement OpenWhisk qui exécute cette action. |
| `__OW_API_KEY` |Clé d'API pour l'objet qui appelle l'action ; peut être une clé d'API restreinte.|
| `__OW_NAMESPACE` | Espace de nom pour l'activation. Cet espace de nom peut être différent de l'espace de nom pour l'action. |
| `__OW_ACTION_NAME` |Nom qualifié complet de l'action en cours d'exécution.|
| `__OW_ACTIVATION_ID` |ID d'activation pour cette instance d'action en cours d'exécution.|
| `__OW_DEADLINE` | Durée approximative, en millisecondes époque, lorsque cette action consomme l'intégralité du quota de durée. |

## Obtention d'une adresse URL d'action
{: #get-action-url}

Une action peut être appelée en utilisant l'interface REST par le biais d'une demande HTTP.
{: shortdesc}

Pour obtenir une URL d'action :
```
ibmcloud fn action get actionName --url
```
{: pre}

Exemple de résultat pour des actions standard :
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Exemple de sortie pour des [actions Web](./openwhisk_webactions.html) :
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Remarque :** pour les actions standard, l'authentification doit être fournie lorsque ces actions sont appelées par le biais d'une demande HTTPS. Pour plus d'informations concernant l'appel d'actions à l'aide de l'interface REST, voir le [document de référence de l'API REST](https://console.bluemix.net/apidocs/openwhisk).

## Sauvegarde de code d'action
{: #save-action}

Vous pouvez obtenir et sauvegarder en local le code associé à une action existante. Vous pouvez sauvegarder le code de toutes les actions, à l'exception des actions sequences et Docker.
{: shortdesc}

Sauvegardez le code d'action dans un nom de fichier qui correspond à un nom d'action existant dans le répertoire de travail en cours.
```
ibmcloud fn action get actionName --save
```
{: pre}

Une extension de fichier correspondant au type d'action est utilisée. Pour le code d'action dans un fichier zip, une extension .zip est utilisée. Exemple de sortie :
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

Vous pouvez aussi indiquer un chemin d'accès au fichier, un nom de fichier et une extension personnalisés au moyen de l'indicateur `--save-as`.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Exemple de sortie :
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Surveillance des journaux des actions
{: #monitor-action-output}

Des actions {{site.data.keyword.openwhisk_short}} peuvent être appelées par d'autres utilisateurs en réponse à divers événements, ou dans le cadre d'une séquence d'actions. Pour obtenir des informations sur la date et l'heure d'appel des actions et la nature de la sortie, il peut être utile de surveiller les journaux des actions.

Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour surveiller la sortie des actions, au fur et à mesure qu'elles sont appelées.

1. Démarrez une boucle d'interrogation qui consulte en permanence les journaux des activations.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Passez dans une autre fenêtre et appelez une action.
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Dans la fenêtre d'interrogation, vous pouvez voir le journal d'activation.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    Vous pouvez aussi voir les journaux de n'importe quelle action exécutée pour votre compte dans {{site.data.keyword.openwhisk_short}} en temps réel.

## Suppression d'actions
{: #deleting-actions}

Vous pouvez procéder à un nettoyage en supprimant les actions que vous ne voulez pas utiliser.

1. Supprimez une action.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: deleted hello
    ```
    {: screen}

2. Vérifiez que l'action n'apparaît plus dans la liste des actions.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Exemple de sortie :
    ```
    actions
    ```
    {: screen}
