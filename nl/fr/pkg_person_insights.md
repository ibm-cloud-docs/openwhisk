---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: personality insights, cognitive, serverless, functions

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

# {{site.data.keyword.personalityinsightsshort}}
{: #pkg_person_insights}

Le service installable {{site.data.keyword.personalityinsightsfull}} permet aux applications d'obtenir des connaissances à partir de médias sociaux, de données d'entreprise ou d'autres communications numériques. Ce service utilise l'analyse linguistique pour déduire les caractéristiques de personnalité des individus (Big Five, Needs et Values) à partir de communications numériques comme les courriers électroniques, les messages de texte, les tweets, et à partir de publications sur les forums.
{: shortdesc}

Le service peut déduire automatiquement, à partir de médias sociaux potentiellement bruyants, des portraits d'individus reflétant les caractéristiques de leur personnalité. Le service peut déduire des préférences de consommation d'après les résultats de son analyse et, pour le contenu JSON qui est horodaté, il peut générer des rapports de comportement temporel.
* Pour plus d'informations sur la signification des modèles que le service utilise pour décrire les caractéristiques de personnalité, consultez la rubrique relative aux [modèles de personnalité](/docs/services/personality-insights?topic=personality-insights-models).
* Pour plus d'informations sur la signification des préférences de consommation, consultez la rubrique relative aux [préférences de consommation](/docs/services/personality-insights?topic=personality-insights-preferences).

**Remarque :** La consignation des demandes est désactivée pour le service {{site.data.keyword.personalityinsightsshort}}. Ce service ne consigne ni ne conserve les données des demandes et réponses, que l'en-tête de requête `X-Watson-Learning-Opt-Out` soit ou non défini.

Le package {{site.data.keyword.personalityinsightsshort}} contient les entités suivantes. Vous trouverez des détails supplémentaires dans la référence d'API {{site.data.keyword.personalityinsightsshort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Gestion du service {{site.data.keyword.personalityinsightsshort}} V3. |
| [`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Obtention d'un profil. |
| [`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Obtention d'un profil en tant que fichier CSV. |

## Création d'une instance de service {{site.data.keyword.personalityinsightsshort}}
{: #service_instance_insights}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.personalityinsightsshort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.personalityinsightsshort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog/services/personality_insights).
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.personalityinsightsshort}}
{: #install_insights}

Dès que vous disposez d'une instance de service {{site.data.keyword.personalityinsightsshort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.personalityinsightsshort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.personalityinsightsshort}} :

1. Clonez le référentiel de package {{site.data.keyword.personalityinsightsshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
    ```
    {: pre}

3. Vérifiez que le package est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Sortie :
    ```
    packages
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.personalityinsightsshort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    Selon la région où vous avez créé l'instance de service, celle-ci peut être nommée différemment car il s'agit d'un service IAM. Si la commande ci-dessus échoue, utilisez le nom de service suivant pour la commande bind :
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.personalityinsightsshort}}.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Personality Insights**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **personality-insights-v3**.

7. Pour utiliser les actions du package **personality-insights-v3**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **personality-insights-v3** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.personalityinsightsshort}}
{: #usage_insights}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Toutes les actions exigent un paramètre de version au format AAAA-MM-JJ. Lorsque l'API est modifiée avec rétrocompatibilité impossible, une nouvelle date de version est publiée. Pour plus de détails, consultez la [référence d'API](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

Les fonctions de ce package utilisent la version actuelle de Personality Insights, 2017-10-13. Testez l'action `profile`.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. This excerpt is just some filler text and probably won't return anything very interesting from the personality insights service. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts. The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and, for JSON content that is timestamped, can report temporal behavior."
```
{: pre}

