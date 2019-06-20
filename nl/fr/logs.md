---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# Affichage des journaux
{: #logs}

La fonction de journalisation est automatiquement activée dans {{site.data.keyword.openwhisk}} pour vous aider à identifier et résoudre les problèmes. Vous pouvez également utiliser le service {{site.data.keyword.cloudaccesstraillong}} pour suivre la façon dont les utilisateurs et les applications interagissent avec le service {{site.data.keyword.openwhisk_short}}.


## Affichage des journaux d'actions en temps réel
{: #logs_poll}

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




## Affichage des détails d'activation
{: #activation_details}

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
    "namespace": "myNamespace",
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
            "value": "myNamespace/hello"
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
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description du résultat de la commande <code>activation get</code></th>
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
<td>Annotations sur cette action. Pour obtenir une liste des annotations possibles, consultez la [rubrique de référence relative aux annotations](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indique si l'action est publiée publiquement.</td>
</tr>
</tbody></table>



## Affichage des journaux dans {{site.data.keyword.loganalysisfull_notm}}
{: #logs_view}

Vous pouvez consulter les journaux d'activation directement depuis le tableau de bord de surveillance {{site.data.keyword.openwhisk_short}}. Les journaux sont également transférés à [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) où ils sont indexés, afin de permettre une recherche en texte intégral à l'aide de tous les messages générés et d'une interrogation efficace basée sur des zones spécifiques.
{:shortdesc}

**Remarque** : la journalisation n'est pas disponible pour la région Est des Etats-Unis.

1. Ouvrez la page de surveillance [{{site.data.keyword.openwhisk_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/dashboard).

2. Facultatif : pour consulter uniquement les journaux relatifs à une action spécifique, limitez le récapitulatif de surveillance à cette action. Dans la section Options de filtrage, sélectionnez le nom de l'action dans la liste déroulante **Limiter à**.

3. Dans le volet de navigation gauche, cliquez sr **Journaux**. La page {{site.data.keyword.loganalysisshort_notm}} Kibana s'affiche.

4. Facultatif : pour afficher des journaux plus anciens, définissez la valeur de période par défaut sur 15 minutes en cliquant sur **15 dernières minutes** dans l'angle supérieur droit et en sélectionnant une période différente.

### Interrogation des journaux
{: #logs_query}

Vous pouvez trouver des journaux d'activation spécifiques dans [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) à l'aide de la syntaxe de requête de Kibana.

Les exemples de requête suivants peuvent vous aider à déboguer des erreurs :
  * Rechercher tous journaux des erreurs :
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Rechercher tous mes journaux des erreurs qui sont générés par "myAction" :
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Interrogation des résultats
{: #logs_query_results}

Outre les lignes de journaux, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) indexe également les résultats, ou les enregistrements d'activation, qui sont générés par {{site.data.keyword.openwhisk_short}}. Les résultats contiennent les métadonnées d'activation comme la durée d'activation ou le code de résultat d'activation. L'interrogation des zones de résultat peut vous aider à comprendre le comportement de vos actions {{site.data.keyword.openwhisk_short}}.

Vous pouvez trouver des journaux d'activation spécifiques à l'aide de la syntaxe de requête de Kibana. Les exemples de requête suivants peuvent vous aider à déboguer des erreurs :

* Rechercher toutes les activations ayant échoué :
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    Dans les résultats, un `0` indique une action fermée avec succès et toutes les autres valeurs indiquent une erreur.

* Rechercher toutes les activations ayant échoué avec une erreur spécifique :
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

