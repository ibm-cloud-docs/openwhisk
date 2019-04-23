---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: nlc, natural language classifier, machine learning

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.nlclassifiershort}}-Paket

{{site.data.keyword.nlclassifierfull}} verwendet Algorithmen für maschinelles Lernen, um die am besten übereinstimmenden vordefinierten Klassen für die Eingabe kurzer Texte zurückzugeben. Sie erstellen und trainieren ein Klassifikationsmerkmal, um vordefinierte Klassen mit Beispieltexten zu verbinden, sodass der Service diese Klassen auf neue Eingaben anwenden kann.
{: shortdesc}

Das {{site.data.keyword.nlclassifiershort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.nlclassifiershort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html) | Paket | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Ermöglicht das Arbeiten mit dem {{site.data.keyword.nlclassifiershort}}-Service. |
| [classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    text,  | Klassifiziert eine Phrase. |
| [classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    collection,  | Klassifiziert mehrere Phrasen. |
| [create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    metadata,     training_data,  | Erstellt ein Klassifikationsmerkmal. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Löscht ein Klassifikationsmerkmal. |
| [get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Ruft Informationen zu einem Klassifikationsmerkmal ab. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers) | Aktion |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listet Klassifikationsmerkmale auf. |

## {{site.data.keyword.nlclassifiershort}}-Serviceinstanz erstellen
{: #service_instance_classifier}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.nlclassifiershort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.nlclassifiershort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/natural_language_classifier).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.nlclassifiershort}}-Paket installieren
{: #install_classifier}

Nachdem Sie über eine Instanz des {{site.data.keyword.nlclassifiershort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.nlclassifiershort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #nlclassifier_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Gehen Sie zum Installieren des {{site.data.keyword.nlclassifiershort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.nlclassifiershort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
    ```
    {: pre}

3. Stellen Sie sicher, dass das Paket zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Ausgabe:
    ```
    packages
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.nlclassifiershort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Abhängig von der Region, in der Sie die Serviceinstanz erstellt haben, wird die Serviceinstanz möglicherweise anders benannt, weil es sich um einen IAM-Service handelt. Sollte der obige Befehl fehlschlagen, verwenden Sie den folgenden Servicenamen für den Befehl 'bind':
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.nlclassifiershort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    Beispielausgabe:
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

### Installation über die Benutzerschnittstelle von {{site.data.keyword.openwhisk_short}} durchführen
{: #nlclassifier_ui}

1. Öffnen Sie die [Seite 'Erstellen' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/create) in der {{site.data.keyword.openwhisk_short}}-Konsole. 

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbereich aus, in dem die Installation des Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**. 

5. Klicken Sie auf das Paket **Natural Language Classifier**. 

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **natural-language-classifier-v1**. 

7. Um die Aktionen im Paket **natural-language-classifier-v1** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus.
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **natural-language-classifier-v1**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.nlclassifiershort}}-Paket verwenden
{: #usage_classifier}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Testen Sie versuchshalber die Aktion `list-classifiers`.
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}
