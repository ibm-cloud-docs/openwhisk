---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

Der installierbare {{site.data.keyword.nlufull}}-Service hilft Ihnen, verschiedene Merkmale bei Textinhalten im richtigen Maß zu analysieren.
{: shortdesc}

Sie stellen Text, HTML-Rohmaterial oder eine öffentliche URL bereit und {{site.data.keyword.nlushort}} liefert die Ergebnisse für die von Ihnen angeforderten Merkmale. Der Service bereinigt den HTML-Inhalt standardmäßig vor der Analyse, sodass in den Ergebnissen ein Großteil der Werbung und sonstiger unerwünschter Inhalte ignoriert werden können. Mit Watson Knowledge Studio können Sie <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">angepasste Modelle</a> erstellen, die zum Erkennen angepasster Entitäten und Beziehungen in Natural Language Understanding verwendet werden können.

Das {{site.data.keyword.nlushort}}-Paket enthält die folgenden Entitäten. Weitere Informationen können Sie in der Referenz für die {{site.data.keyword.nlushort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} | Paket | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |  Ermöglicht das Arbeiten mit dem {{site.data.keyword.nlushort}}-Service. |
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} | Aktion |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`,  | Analysiert Text, HTML-Material oder eine öffentliche Webseite. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  | Löscht ein Modell. |
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} | Aktion |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, | Listet Modelle auf. |

## {{site.data.keyword.nlushort}}-Serviceinstanz erstellen
{: #service_instance_understanding}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.nlushort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.nlushort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.nlushort}}-Paket installieren
{: #install_understanding}

Nachdem Sie über eine Instanz des {{site.data.keyword.nlushort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.nlushort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #nlus_cli}

**Vorbereitende Schritte**
[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Gehen Sie zum Installieren des {{site.data.keyword.nlushort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.nlushort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
    ```
    {: pre}

3. Stellen Sie sicher, dass das Paket zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Ausgabe**
    ```
    packages
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.nlushort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.nlushort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #nlus_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das Paket installiert werden soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Natural Language Understanding**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert wurde, werden Sie zur Seite 'Aktionen' weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet **natural-language-understanding-v1**. 

7. Um die Aktionen im Paket **natural-language-understanding-v1** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 4 und 5 in den [CLI-Anweisungen](#nlus_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. 

  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket **natural-language-understanding-v1**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.nlushort}}-Paket verwenden
{: #usage_understanding}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}.

Die Funktionen dieses Pakets verwenden die aktuelle Version 2018-03-16 von Natural Language Understanding. Testen Sie versuchshalber die Aktion `list-models`.
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



