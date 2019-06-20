---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: nlc, natural language classifier, machine learning

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

# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

Le service {{site.data.keyword.nlclassifierfull}} utilise des algorithmes d'apprentissage automatique pour renvoyer les principales classes prédéfinies correspondantes pour la saisie de texte court. Vous créez et formez un discriminant pour connecter des classes prédéfinies aux exemples de texte afin que le service puisse appliquer ces classes aux nouvelles entrées.
{: shortdesc}

Le package {{site.data.keyword.nlclassifiershort}} contient les entités suivantes. Vous trouverez des détails supplémentaires dans la référence d'API {{site.data.keyword.nlclassifiershort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html) | package | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Gestion du service {{site.data.keyword.nlclassifiershort}}. |
| [classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify) | action |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    text,  | Classement d'une expression. |
| [classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection) | action |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    collection,  | Classement de plusieurs expressions. |
| [create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    metadata,     training_data,  | Création d'un discriminant. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Suppression d'un discriminant. |
| [get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Obtention d'informations sur un discriminant. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Affichage de la liste des discriminants. |

## Création d'une instance de service {{site.data.keyword.nlclassifiershort}}
{: #service_instance_classifier}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.nlclassifiershort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.nlclassifiershort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog/services/natural_language_classifier).
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.nlclassifiershort}}
{: #install_classifier}

Dès que vous disposez d'une instance de service {{site.data.keyword.nlclassifiershort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.nlclassifiershort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #nlclassifier_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.nlclassifiershort}} :

1. Clonez le référentiel de package {{site.data.keyword.nlclassifiershort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.nlclassifiershort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Selon la région où vous avez créé l'instance de service, celle-ci peut être nommée différemment car il s'agit d'un service IAM. Si la commande ci-dessus échoue, utilisez le nom de service suivant pour la commande bind :
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.nlclassifiershort}}.
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #nlclassifier_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Natural Language Classifier**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **natural-language-classifier-v1**.

7. Pour utiliser les actions du package **natural-language-classifier-v1**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **natural-language-classifier-v1** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.nlclassifiershort}}
{: #usage_classifier}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Testez l'action `list-classifiers`.
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}

