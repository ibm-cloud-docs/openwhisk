---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

Das vorinstallierte Paket `/whisk.system/cloudant` ermöglicht Ihnen die Arbeit mit einer [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)-Datenbank. Für die Verwendung dieses Pakets ist keine Servicebindung erforderlich.
{: shortdesc}


## Verfügbare Entitäten
{: #cloudant_available}
Die folgende Tabelle zeigt eine Auswahl der Entitäten, die im Paket `whisk.system/cloudant` verfügbar sind. Sie können das Paket `whisk.system/cloudant` verwenden, um Dokumente zu lesen, zu schreiben, zu aktualisieren oder zu löschen. Sie können auch den Feed `changes` verwenden, um auf Änderungen an {{site.data.keyword.cloudant_short_notm}}-Datenbanken zu überwachen.
{: shortdesc}

Führen Sie den Befehl `ibmcloud fn package get /whisk.system/cloudant` aus, um eine vollständige Liste der Entitäten abzurufen, die im Paket `/whisk.system/cloudant` verfügbar sind.
{: note}

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paket | `dbname`, `host`, `username`, `password` | Für die Arbeit mit einer Cloudant-Datenbank. |
| `/whisk.system/cloudant/read` | Aktion | `dbname`, `id` | Lesen eines Dokuments aus einer Datenbank. |
| `/whisk.system/cloudant/write` | Aktion | `dbname`, `overwrite`, `doc` | Schreiben eines Dokuments in eine Datenbank. |
| `/whisk.system/cloudant/update-document` | Aktion | `dbname`, `doc` | Aktualisieren eines Dokuments in einer Datenbank. |
| `/whisk.system/cloudant/changes` | Feed | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Aktivieren eines Auslöserereignisses bei Änderungen an einer Datenbank. |

Der Parameter `includeDoc` wird für die Verwendung mit dem Feed `/whisk.system/cloudant/changes` nicht mehr unterstützt. Wenn Sie Auslöser erstellt haben, die diesen Parameter verwenden, müssen Sie sie ohne den Parameter `includeDoc` erneut erstellen.
{: deprecated}

## Binden Sie das Paket `/whisk.system/cloudant` an Ihre {{site.data.keyword.cloudant_short_notm}}-Datenbank. 
Wenn Sie {{site.data.keyword.openwhisk}} in {{site.data.keyword.cloud_notm}} verwenden, können Sie mit dem {{site.data.keyword.openwhisk}}-CLI-Plug-in einen Service an eine Aktion oder ein Paket binden.
{: #cloudant_db}

**Vorbereitende Schritte**
Sie müssen über eine Instanz von {{site.data.keyword.cloudant_short_notm}} verfügen. Informationen zum Erstellen einer Instanz finden Sie in [Einführung in {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). 

1. Erstellen Sie eine Bindung für das Paket `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist. In diesem Beispiel lautet der Paketname `myCloudant`. 

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Prüfen Sie, ob die Paketbindung vorhanden ist.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Beispielausgabe**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Rufen Sie den Namen der Serviceinstanz ab, die an eine Aktion oder ein Paket gebunden werden soll.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Beispielausgabe**
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

    **Beispielausgabe**

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

    **Beispielausgabe**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
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

In diesem Beispiel gehören die Berechtigungsnachweise für den Service {{site.data.keyword.cloudant_short_notm}} zu einem Parameter mit dem Namen `__bx_creds`. 

## Mit Dokumenten in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank arbeiten
{: #cloudant_read}

Sie können eine Aktion verwenden, um ein Dokument in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank zu lesen, zu schreiben, zu aktualisieren oder zu löschen.
{: shortdesc}

### Dokument lesen
Sie können die Aktion `/whisk.system/cloudant/read` verwenden, um ein Dokument in Ihrer {{site.data.keyword.cloudant_short_notm}} Datenbank zu lesen. 

**Vorbereitende Schritte**
Wenn Ihre {{site.data.keyword.cloudant_short_notm}}-Datenbank keine Dokumente enthält, können Sie über das [{{site.data.keyword.cloudant_short_notm}}-Dashboard ein Dokument erstellen. Die URL für das Dashboard lautet `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`. 

Rufen Sie ein Dokument mit der Aktion `read` ab. Ersetzen Sie `/_/myCloudant` durch Ihren Paketnamen, `<database_name>` durch Ihren Datenbanknamen und `<document_id>` durch die Datei-ID. Rufen Sie die Aktion auf, um das Abrufen eines Dokuments zu testen. 

**Befehlssyntax**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Beispiel für Aktion 'read' in einer Datenbank mit dem Namen `test`**
Rufen Sie die Aktion auf, um das Lesen einer Datei zu testen. In diesem Beispiel wird eine Datei mit der ID (`id`) `9f86f4955e7a38ab0169462e6ac0f476`, eine leere Datei, gelesen. 

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Beispielausgabe**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Dokument in eine {{site.data.keyword.cloudant_short_notm}}-Datenbank schreiben
{: #cloudant_write}

Sie können eine Aktion verwenden, um Dokumente in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank zu erstellen oder zu aktualisieren.
{: shortdesc}

**Vorbereitende Schritte**
Erstellen Sie eine [Paketbindung](#cloudant_db) für `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist. 

1. Speichern Sie ein Dokument mit der Aktion `write` in der Paketbindung, die Sie erstellt haben. Ersetzen Sie `/_/myCloudant` durch Ihren Paketnamen, `<database_name>` durch den Namen Ihrer Datenbank, `<document_id>` durch Ihre Dokument-ID und `<test_name>` durch einen Namen. 

  **Befehlssyntax**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Beispiel für Aktion zum Schreiben in eine Datenbank mit dem Namen `test`**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Beispielausgabe**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Überprüfen Sie, ob das Dokument im {{site.data.keyword.cloudant_short_notm}}-Dashboard vorhanden ist. Die Dashboard-URL für die Datenbank `test` weist das folgende Format auf: `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`. 

  **Beispieldokument im {{site.data.keyword.cloudant_short_notm}}-Dashboard**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Dokument aktualisieren
Sie können die Aktion `/update-document` verwenden, um ein Dokument in Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank zu aktualisieren.
{: short desc}

**Vorbereitende Schritte**
Erstellen Sie eine [Paketbindung](#cloudant_db) für `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist. 

Im folgenden Beispiel wird das Dokument aktualisiert, das in [Dokument in eine {{site.data.keyword.cloudant_short_notm}}-Datenbank schreiben](#cloudant_write) erstellt wurde.
{: note}

Sie können ein Dokument in Ihrer Datenbank aktualisieren, indem Sie `<test>` durch Ihren Datenbanknamen ersetzen und das Flag `-- param doc` durch die ID (`id`) und den Inhalt des Dokuments in Ihrer Datenbank ersetzen, das Sie aktualisieren möchten. 


1. Sie können ein Dokument in der Datenbank `test` aktualisieren, indem Sie den folgenden Befehl ausführen. In diesem Beispiel wird der Wert `shade` zum Dokument `color` hinzugefügt.  

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Ausgabe**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. Um die Aktualisierung anzuzeigen, rufen Sie das Dokument erneut ab. 

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Beispieldokument**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Auslöser mithilfe der Filterfunktion erstellen
{: #cloudant_trigger}

Mit dem Feed `changes` können Sie einen Service konfigurieren, der bei jeder Änderung an Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank einen Auslöser aktiviert.

**Vorbereitende Schritte**
Erstellen Sie eine [Paketbindung](#cloudant_db) für `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist. 

In diesem Beispiel verwendete Parameter. 

| Parameter | Beschreibung |
| --- | --- |
| `dbname` | (Erforderlich) der Name der {{site.data.keyword.cloudant_short_notm}}-Datenbank. |
| `iamApiKey` | (Optional) Der IAM-API-Schlüssel für die Cloudant-Datenbank. Falls angegeben, wird dieser Wert anstelle des Benutzernamens und Kennworts als Berechtigungsnachweis verwendet. |
| `iamUrl` | (Optional) Die URL des IAM-Token-Service, die verwendet wird, wenn `iamApiKey` angegeben ist. Der Standardwert ist `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Optional) Stoppt die Aktivierung von Auslösern, wenn dieser Grenzwert erreicht wird. Standardwert: unbegrenzt. |
| `filter` | (Optional) Die Filterfunktion, die in einem Entwurfsdokument definiert ist. |
| `query_params` | (Optional) Zusätzliche Abfrageparameter, die möglicherweise für die Filterfunktion erforderlich sind. |
| `includeDoc` | (Veraltet) Der Parameter `includeDoc` wird für die Verwendung mit dem Feed `/whisk.system/cloudant/changes` nicht mehr unterstützt. |

</br>

1. Erstellen Sie einen Auslöser mit dem Namen `cloudantTrigger` mit dem Feed `changes` in der Paketbindung, die Sie zuvor erstellt haben. Schließen Sie dabei die Funktionen `filter` und `query_params` ein, um den Auslöser zu aktivieren, wenn ein Dokument hinzugefügt oder geändert wird, wenn der Status `new` ist.

  Ersetzen Sie `/_/myCloudant` durch den Namen Ihres Pakets. In diesem Beispiel wird eine Datenbank mit dem Namen `test` verwendet. 
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Beispielausgabe**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Speichern Sie den folgenden JavaScript-Code als `cloudantChange.js`. 

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Erstellen Sie eine Aktion mit dem Namen `cloudantChange`, die Sie zum Beobachten des Feeds 'changes' verwenden können. Ersetzen Sie `<file_path>` durch den Dateipfad zu Ihrer Datei `cloudantChange.js` auf Ihrem Computer. 

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Erstellen Sie eine Regel mit dem Namen `cloudantRule`, um die Aktion `cloudantChange` mit dem Auslöser `cloudantTrigger` zu verbinden, den Sie zuvor erstellt haben. 

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. Starten Sie in einem anderen Terminalfenster das Polling, damit Sie sehen können, wann Aktivierungen auftreten. 

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. Verwenden Sie das {{site.data.keyword.cloudant_short_notm}}-Dashboard, um ein vorhandenes Dokument zu ändern oder ein neues Dokument zu erstellen. 

7. Beobachten Sie die Aktivierungen für den Auslöser `cloudantTrigger` für jede Dokumentänderung. 

**Beispiel für Aktivierung des Auslösers `cloudantTrigger`**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### Datenstruktur einer Auslöseraktivierung
{: #cloudant_struct}

Der Inhalt des generierten Ereignisses weist die folgenden Parameter auf. 

| Parameter | Beschreibung |
| --- | --- |
| `id` | Die Dokument-ID. |
| `seq` | Die von {{site.data.keyword.cloudant_short_notm}} generierte Sequenz-ID. |
| `changes` | Ein Array von Objekten, die jeweils ein Feld `rev` mit der Revisions-ID des Dokuments enthalten. |

**JSON-Darstellung der Auslöseraktivierung**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## Datenbankänderungsereignisse filtern
{: #cloudant_filter}

Sie können eine Filterfunktion definieren, um unnötige Änderungsereignisse zu verhindern, die Ihren Auslöser aktivieren. 

**Vorbereitende Schritte**
Erstellen Sie eine [Paketbindung](#cloudant_db) für `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist. 

Zum Erstellen einer Filterfunktion können Sie eine Aktion verwenden. 

1. Speichern Sie den folgenden JSON-Filter in einer Datei mit dem Namen `design_doc.json`. 

  ```json
  {
    "doc": {
      "_id": "_design/mailbox",
    "filters": {
        "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
    }
  }
  ```
  {: codeblock}

2. Erstellen Sie ein Entwurfsdokument in der Datenbank mit der folgenden Filterfunktion. Ersetzen Sie `<database_name>` durch den Namen Ihrer Datenbank und `<file_path>` durch den Dateipfad Ihrer Datei `design_doc.json`. Rufen Sie die Aktion `write` auf, um die Erstellung eines Entwurfsdokuments zu testen. 

**Befehlssyntax**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Beispielbefehl zum Schreiben einer Datei `design_doc.json` in eine Datenbank `test`**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Beispielausgabe**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


Weitere Informationen zu {{site.data.keyword.cloudant_short_notm}}-Entwurfsdokumenten finden Sie in [Entwurfsdokumente](/docs/services/Cloudant?topic=cloudant-design-documents). 

## Einzelnes Dokument mithilfe einer Aktionssequenz verarbeiten
{: #cloudant_seq}

Sie können eine Aktionssequenz in einer Regel verwenden, um das Dokument, das einem {{site.data.keyword.cloudant_short_notm}}-Änderungsereignis zugeordnet ist, abzurufen und zu verarbeiten.

**Vorbereitende Schritte**
Erstellen Sie eine [Paketbindung](#cloudant_db) für `/whisk.system/cloudant`, die für Ihr {{site.data.keyword.cloudant_short_notm}}-Konto konfiguriert ist.  

In diesem Beispiel wird das Dokument aktualisiert, das in [Dokument in eine {{site.data.keyword.cloudant_short_notm}}-Datenbank schreiben](#cloudant_write) erstellt wurde.
{: note}

### Aktion zum Verarbeiten eines einzelnen Dokuments erstellen

Führen Sie die folgenden Befehle aus, um eine Aktion zu erstellen, die Änderungen an einem einzelnen Dokument verarbeitet.
{: shortdesc}

1. Speichern Sie den folgenden Code als `docChange.js`. 

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Erstellen Sie eine Aktion mit dem Namen `docChange`, um das Dokument mit dem Namen `blue` zu verarbeiten, das Sie zuvor erstellt haben. Ersetzen Sie `<file_path>` durch den Dateipfad Ihrer Datei `docChange.js`. 

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created action docChange
  ```
  {: screen}

### Sequenz mit der Aktion `read` erstellen 

Die Aktion `read` kann mit Ihrer Aktion `docChange` kombiniert werden, um eine Aktionssequenz zu erstellen.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Auslöser mit dem Feed `changes` erstellen

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Regel zum Zuordnen des Auslösers zu der Sequenz erstellen

Die Aktion `docSequence` kann in einer Regel verwendet werden, mit der die Aktion bei neuen {{site.data.keyword.cloudant_short_notm}}-Auslöserereignissen aktiviert wird. 

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created rule docRule
  ```

  **Beispielaktivierung**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Sequenz testen

1. Testen Sie die Aktionssequenz `docSequence`, indem Sie eine Änderung an der Datei `blue` vornehmen, die Sie zuvor erstellt haben. In diesem Beispiel wird der Wert für `shade` in `indigo` geändert. 

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Beispielaktivierung**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. Überprüfen Sie, ob die Datei mit dem neuen Wert für `shade` aktualisiert wurde, indem Sie die Aktion `read` aufrufen. Ersetzen Sie `<database>`_name durch den Namen Ihrer Datenbank. 

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Ausgabe**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Nächste Schritte
Da Sie nun auf Änderungen an einem Dokument in Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank überwachen, können Sie Slack-Benachrichtigungen für die Änderungen mithilfe des [Pakets `/whisk.system/slack`](/docs/openwhisk?topic=cloud-functions-pkg_slack) auslösen. 


