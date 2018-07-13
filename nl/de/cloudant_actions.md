---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant
{: #cloudant_actions}

Das Paket `/whisk.system/cloudant` ermöglicht Ihnen die Arbeit mit einer [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) -Datenbank und enthält die folgenden Aktionen und Feeds:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paket | dbname, host, username, password |Für die Arbeit mit einer Cloudant-Datenbank.|
| `/whisk.system/cloudant/read` | Aktion | dbname, id |Lesen eines Dokuments aus einer Datenbank.|
| `/whisk.system/cloudant/write` | Aktion | dbname, overwrite, doc |Schreiben eines Dokuments in eine Datenbank.|
| `/whisk.system/cloudant/changes` | Feed | dbname, filter, query_params, maxTriggers |Aktivieren eines Auslöserereignisses bei Änderungen an einer Datenbank.|
{: shortdesc}

In den folgenden Abschnitten werden Sie schrittweise durch die Einrichtung einer {{site.data.keyword.cloudant_short_notm}}-Datenbank geführt und es wird beschrieben, wie der Lese- und Schreibzugriff erfolgt.
Weitere Informationen zur Verwendung von Feeds mit dem Paket `/whisk.system/cloudant` finden Sie im Abschnitt [{{site.data.keyword.cloudant_short_notm}}-Ereignisquelle](./openwhisk_cloudant.html).

## {{site.data.keyword.cloudant_short_notm}}-Datenbank in {{site.data.keyword.Bluemix_notm}} einrichten
{: #cloudantdb_cloud}

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.Bluemix_notm}} verwenden, erstellt {{site.data.keyword.openwhisk_short}} automatisch Paketbindungen für Ihre {{site.data.keyword.cloudant_short_notm}}-Serviceinstanzen. Wenn Sie {{site.data.keyword.openwhisk_short}} und {{site.data.keyword.cloudant_short_notm}} von {{site.data.keyword.Bluemix_notm}} nicht verwenden, fahren Sie mit dem nächsten Abschnitt fort.

1. Erstellen Sie eine {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz in Ihrem [{{site.data.keyword.Bluemix_notm}}-Dashboard](http://console.bluemix.net).

  Stellen Sie sicher, dass Sie für jede neue Serviceinstanz einen Berechtigungsnachweisschlüssel erstellen.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für jede {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz mit einem definierten Berechtigungsnachweisschlüssel.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Ihre Paketbindung enthält nun die Ihrer {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz zugeordneten Berechtigungsnachweise.

3. Prüfen Sie, ob die zuvor erstellte Paketbindung mit dem Host und den Berechtigungsnachweisen für Ihre {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}}-Serviceinstanz konfiguriert ist.

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```
  {: screen}

## {{site.data.keyword.cloudant_short_notm}}-Datenbank außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten
{: #cloudantdb_nocloud}

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie Ihre {{site.data.keyword.cloudant_short_notm}}-Datenbank außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto erstellen. Sie benötigen den Hostnamen, den Benutzernamen und das Kennwort des {{site.data.keyword.cloudant_short_notm}}-Kontos.

1. Erstellen Sie eine Paketbindung, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Prüfen Sie, ob die Paketbindung vorhanden ist.
  ```
  wsk package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## Aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank lesen
{: #cloudant_read}

Sie können eine Aktion verwenden, um ein Dokument aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank mit dem Namen **testdb** abzurufen. Stellen Sie sicher, dass diese Datenbank in Ihrem {{site.data.keyword.cloudant_short_notm}}-Konto vorhanden ist.

- Rufen Sie ein Dokument mit der Aktion **read** in der Paketbindung ab, die Sie zuvor erstellt haben. Achten Sie darauf, `/_/myCloudant` durch Ihren Paketnamen zu ersetzen.
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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
