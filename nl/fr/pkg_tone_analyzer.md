---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: functions, serverless, watson

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


# {{site.data.keyword.toneanalyzershort}}
{: #pkg_tone_analyzer}

Le service installable {{site.data.keyword.toneanalyzerfull}} utilise l'analyse linguistique pour détecter des tonalités émotionnelles et de langue dans du texte écrit.
{:shortdesc}

Le service peut analyser la tonalité au niveau du document et des phrases. Vous pouvez utiliser ce service pour comprendre comment vos communications écrites sont perçues et améliorer ensuite la tonalité de vos communications. Les entreprises peuvent utiliser ce service pour l'apprentissage de la tonalité des communications avec leurs clients et pour répondre à chaque client, ou encore pour comprendre et améliorer leurs conversations clients.

La consignation des demandes est désactivée pour le service Tone Analyzer. Ce service ne consigne ni ne conserve les données des demandes et réponses, que l'en-tête de requête `X-Watson-Learning-Opt-Out` soit ou non défini.
{: note}

Le package {{site.data.keyword.toneanalyzershort}} contient les entités suivantes. Vous trouverez des détails supplémentaires dans la référence d'API {{site.data.keyword.toneanalyzershort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Gestion du service {{site.data.keyword.toneanalyzershort}}. |
| [`tone`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`,`iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,    `tone_input`, `content_type`, `sentences`, `tones`, `content_language`, `accept_language` | Analyse de la tonalité générale. |
| [`tone-chat`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `utterances`, `content_language`, `accept_language` | Analyse de la tonalité d'engagement client. |


## Création d'une instance de service {{site.data.keyword.toneanalyzershort}}
{: #service_instance_tone}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.toneanalyzershort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.toneanalyzershort}}](https://cloud.ibm.com/catalog/services/tone_analyzer){: external}.
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.toneanalyzershort}}
{: #install_tone}

Dès que vous disposez d'une instance de service {{site.data.keyword.toneanalyzershort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.toneanalyzershort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #toneanalyzer_cli}

**Avant de commencer**
[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.toneanalyzershort}} :

1. Clonez le référentiel de package {{site.data.keyword.toneanalyzershort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.toneanalyzershort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    Selon la région où vous avez créé l'instance de service, celle-ci peut être nommée différemment car il s'agit d'un service IAM. Si la commande échoue, utilisez le nom de service suivant pour la commande bind :
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.toneanalyzershort}}.
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    **Exemple de sortie**
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
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
{: #toneanalyzer_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer](https://cloud.ibm.com/openwhisk/create){: external}.

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Tone Analyzer**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page des actions et vous pouvez rechercher votre nouveau package, nommé **`tone-analyzer-v3`**.

7. Pour utiliser les actions du package **tone-analyzer-v3**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 4 et 5 des [instructions de l'interface de ligne de commande](#toneanalyzer_cli).
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. 
  
  Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
  {: note}
  
    1. Cliquez sur une action du package **tone-analyzer-v3** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.toneanalyzershort}}
{: #usage_tone}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Toutes les actions nécessitent un paramètre de version au format AAAA-MM-JJ. Lorsque l'API est modifiée avec rétrocompatibilité impossible, une nouvelle date de version est publiée. Pour plus de détails, consultez la [référence d'API](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning){: external}.

Les fonctions de ce package utilisent la version actuelle de Tone Analyzer, 2017-09-21. Testez l'action `tone`.
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}

