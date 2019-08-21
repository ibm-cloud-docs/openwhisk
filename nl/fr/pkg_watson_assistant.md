---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: openwhisk, functions

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


# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

Le service installable {{site.data.keyword.conversationfull}} combine l'apprentissage automatique, la compréhension en langage naturel et des outils de dialogue intégrés pour créer des flux de conversation entre vos applications et vos utilisateurs.
{: shortdesc}

Le package {{site.data.keyword.conversationshort}} contient les entités suivantes. Pour plus d'informations, voir la référence d'API {{site.data.keyword.conversationshort}} en cliquant sur le nom de l'entité. 

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Gestion du service {{site.data.keyword.conversationshort}}. |
| [`message`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `input`, `alternate_intents`, `context`, `entities`, `intents`, `output`, `nodes_visited_details`  | Obtention de la réponse à une entrée utilisateur. |
| [`create-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `language`, `intents`, `entities`, `dialog_nodes`, `counterexamples`, `metadata`, `learning_opt_out`  | Création d'un espace de travail. |
| [`delete-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`  | Suppression d'un espace de travail. |
| [`get-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `include_audit`  | Obtention d'informations sur un espace de travail. |
| [`list-workspaces`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des espaces de travail. |
| [`update-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `name`, `description`, `language`, `intents`, `entities`, dialog_nodes, counterexamples, `metadata`, `learning_opt_out`, `append`  | Mise à jour d'un espace de travail. |
| [`create-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `description`, `examples`  | Création d'une intention. |
| [`delete-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`  | Suppression d'une intention. |
| [`get-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `export`, `include_audit`  | Obtention d'informations sur une intention. |
| [`list-intents`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des intentions. |
| [`update-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `new_intent`, `new_description`, `new_examples`  | Mise à jour d'une intention. |
| [`create-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Création d'un exemple d'entrée utilisateur. |
| [`delete-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Suppression d'un exemple d'entrée utilisateur. |
| [`get-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `include_audit`  | Extraction des informations sur un exemple d'entrée utilisateur. |
| [`list-examples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des exemples d'entrée utilisateur. |
| [`update-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `new_text`  | Mise à jour d'un exemple d'entrée utilisateur. |
| [`create-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Création d'un exemple de compteur. |
| [`delete-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Suppression d'un exemple de compteur. |
| [`get-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `include_audit`  | Obtention d'informations sur un exemple de compteur. |
| [`list-counterexamples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des exemples de compteur. |
| [`update-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `new_text`  | Mise à jour d'un exemple de compteur. |
| [`create-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `description`, `metadata`, `values`, `fuzzy_match`  | Création d'une entité. |
| [`delete-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`  | Suppression d'une entité. |
| [`get-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `include_audit`  | Obtention d'informations sur une entité. |
| [`list-entities`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des entités. |
| [`update-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `new_entity`, `new_description`, `new_metadata`, `new_fuzzy_match`, `new_values`  | Mise à jour d'une entité. |
| [`create-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `metadata`, `synonyms`, `patterns`, `value_type`  | Ajout d'une valeur d'entité. |
| [`delete-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`  | Suppression d'une valeur d'entité. |
| [`get-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `export`, `include_audit`  | Obtention d'une valeur d'entité. |
| [`list-values`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des valeurs d'entité. |
| [`update-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `new_value`, `new_metadata`, `new_type`, `new_synonyms`, `new_patterns`  | Mise à jour d'une valeur d'entité. |
| [`create-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Ajout d'un synonyme de valeur d'entité. |
| [`delete-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Suppression d'un synonyme de valeur d'entité. |
| [`get-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `include_audit`  | Obtention d'un synonyme de valeur d'entité. |
| [`list-synonyms`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des synonymes de valeur d'entité. |
| [`update-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `new_synonym`  | Mise à jour d'un synonyme de valeur d'entité. |
| [`create-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `description`, `conditions`, `parent`, `previous_sibling`, `output`, `context`, `metadata`, `next_step`, `actions`, `title`, `node_type`, `event_name`, `variable`, `digress_in`, `digress_out`, `digress_out_slots`  | Création d'un noeud de dialogue. |
| [`delete-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, dialog_node  | Suppression d'un noeud de dialogue. |
| [`get-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `include_audit`  | Obtention d'un noeud de dialogue. |
| [`list-dialog-nodes`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Affichage de la liste des noeuds de dialogue. |
| [`update-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `new_dialog_node`, `new_description`, `new_conditions`, `new_parent`, `new_previous_sibling`, `new_output`, `new_context`, `new_metadata`, `new_next_step`, `new_title`, `new_type`, `new_event_name`, `new_variable`, `new_actions`, `new_digress_in`, `new_digress_out`, `new_digress_out_slots`  | Mise à jour d'un noeud de dialogue. |
| [`list-all-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `filter`, `sort`, `page_limit`, `cursor`  | Affichage des événements du journal de tous les espaces de travail. |
| [`list-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `sort`, `filter`, `page_limit`, `cursor`  | Affichage de la liste des événements de journal dans un espace de travail. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Suppression de données de libellé. |

## Création d'une instance de service {{site.data.keyword.conversationshort}}
{: #service_instance_conversation}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.conversationshort}} et des données d'identification du service.

1. [Créez une instance de service {{site.data.keyword.conversationshort}}](https://cloud.ibm.com/catalog/services/conversation){: external}.
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.conversationshort}}
{: #install_conversation}

Dès que vous disposez d'une instance de service {{site.data.keyword.conversationshort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.conversationshort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #conversation_cli}

**Avant de commencer**
[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.conversationshort}}, exécutez les commandes suivantes. 

1. Clonez le référentiel de package {{site.data.keyword.conversationshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
    ```
    {: pre}

3. Vérifiez que le package est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Sortie**
    ```
    packages
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.conversationshort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.conversationshort}}.
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    **Exemple de sortie**
    ```
    ok: got package assistant-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #conversation_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer](https://cloud.ibm.com/openwhisk/create){: external}.

2. Sélectionnez l'espace de nom dans lequel installer le package en utilisant le menu déroulant espace de nom. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Watson Assistant**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **assistant-v1**.

7. Pour utiliser les Actions du package **assistant-v1**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 4 et 5 des [instructions de l'interface de ligne de commande](#conversation_cli).
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. 
  
  Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
  {: note}

    1. Cliquez sur une action du package **assistant-v1** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.conversationshort}}
{: #usage_conversation}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant.

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Toutes les actions nécessitent un paramètre de version au format AAAA-MM-JJ. Lorsque l'API est modifiée avec rétrocompatibilité impossible, une nouvelle date de version est publiée. Pour plus d'informations, reportez-vous à la rubrique [Référence d'API](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning){: external}.

Les fonctions de ce package utilisent la version actuelle de Watson Assistant, `2018-07-10`. Testez l'action `list-workspaces`.
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}


