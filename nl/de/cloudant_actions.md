---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant-Paket
{: #cloudant_actions}

Das Paket `/whisk.system/cloudant` ermöglicht Ihnen die Arbeit mit einer [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)-Datenbank und enthält die folgenden Aktionen und Feeds:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paket | dbname, host, username, password | Für die Arbeit mit einer Cloudant-Datenbank. |
| `/whisk.system/cloudant/read` | Aktion | dbname, id | Lesen eines Dokuments aus einer Datenbank. |
| `/whisk.system/cloudant/write` | Aktion | dbname, overwrite, doc | Schreiben eines Dokuments in eine Datenbank. |
| `/whisk.system/cloudant/changes` | Feed | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Aktivieren eines Auslöserereignisses bei Änderungen an einer Datenbank. |
{: shortdesc}

In den folgenden Abschnitten werden Sie schrittweise durch die Einrichtung einer {{site.data.keyword.cloudant_short_notm}}-Datenbank geführt und es wird beschrieben, wie der Lese- und Schreibzugriff erfolgt.
Weitere Informationen zur Verwendung von Feeds mit dem Paket `/whisk.system/cloudant` enthält der Abschnitt zur [{{site.data.keyword.cloudant_short_notm}}-Ereignisquelle](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

## {{site.data.keyword.cloudant_short_notm}}-Datenbank in {{site.data.keyword.Bluemix_notm}} einrichten
{: #cloudantdb_cloud}

Wenn Sie {{site.data.keyword.openwhisk}} in {{site.data.keyword.Bluemix_notm}} verwenden, können Sie mit dem [{{site.data.keyword.openwhisk}}-CLI-Plug-in](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) einen Service an eine Aktion oder ein Paket binden. 

Zunächst müssen Sie manuell eine Paketbindung für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto erstellen. 

1. Erstellen Sie eine Paketbindung, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Prüfen Sie, ob die Paketbindung vorhanden ist.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}
  
3. Rufen Sie den Namen der Serviceinstanz ab, die an eine Aktion oder ein Paket gebunden werden soll.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Beispielausgabe:
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Rufen Sie den Namen der Berechtigungsnachweise ab, die für die Serviceinstanz definiert sind, die Sie im vorherigen Schritt erhalten haben.
    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    Beispielausgabe:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Binden Sie den Service an das Paket, das Sie in Schritt 1 erstellt haben.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. Stellen Sie sicher, dass die Berechtigungsnachweise erfolgreich gebunden wurden.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

    In diesem Beispiel gehören die Berechtigungsnachweise für den Service 'Cloudant' zu einem Parameter mit dem Namen `__bx_creds`. 
  

## Aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank lesen
{: #cloudant_read}

Sie können eine Aktion verwenden, um ein Dokument aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank mit dem Namen **testdb** abzurufen. Stellen Sie sicher, dass diese Datenbank in Ihrem {{site.data.keyword.cloudant_short_notm}}-Konto vorhanden ist.

- Rufen Sie ein Dokument mit der Aktion **read** in der Paketbindung ab, die Sie zuvor erstellt haben. Achten Sie darauf, `/_/myCloudant` durch Ihren Paketnamen zu ersetzen.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## In eine {{site.data.keyword.cloudant_short_notm}}-Datenbank schreiben
{: #cloudant_write}

Sie können eine Aktion verwenden, um ein Dokument in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank mit dem Namen **testdb** zu speichern. Stellen Sie sicher, dass diese Datenbank in Ihrem {{site.data.keyword.cloudant_short_notm}}-Konto vorhanden ist.

1. Speichern Sie ein Dokument mit der Aktion **write** in der Paketbindung, die Sie zuvor erstellt haben. Achten Sie darauf, `/_/myCloudant` durch Ihren Paketnamen zu ersetzen.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Prüfen Sie, ob das Dokument vorhanden ist, indem Sie in Ihrem {{site.data.keyword.cloudant_short_notm}}-Dashboard danach suchen.

  Die Dashboard-URL für die Datenbank **testdb** sieht in etwa wie folgt aus: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.
