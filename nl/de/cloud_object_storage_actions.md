---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: object storage, bucket, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloud Object Storage-Paket
{: #cloud_object_storage_actions}

Das {{site.data.keyword.cos_full}}-Paket bietet eine Gruppe von Aktionen für die Interaktion mit {{site.data.keyword.cos_full_notm}}-Instanzen. Diese Aktionen ermöglichen Ihnen, aus den in einer {{site.data.keyword.cos_short}}-Instanz vorhandenen Buckets zu lesen, schreiben und löschen.
{: shortdesc}

Das {{site.data.keyword.cos_short}}-Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Paket | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Ermöglicht das Arbeiten mit einer {{site.data.keyword.cos_short}}-Instanz. |
| `/cloud-object-storage/object-write` | Aktion | bucket, key, body, endpoint, ibmAuthEndpoint | Ermöglicht das Schreiben eines Objekts in ein Bucket. |
| `/cloud-object-storage/object-read` | Aktion | bucket, key, endpoint, ibmAuthEndpoint | Ermöglicht das Lesen eines Objekts aus einem Bucket. |
| `/cloud-object-storage/object-delete` | Aktion | bucket, key, endpoint, ibmAuthEndpoint | Ermöglicht das Löschen eines Objekts aus einem Bucket. |
| `/cloud-object-storage/bucket-cors-put` | Aktion | bucket, corsConfig, endpoint, ibmAuthEndpoint | Ermöglicht das Zuordnen einer CORS-Konfiguration zu einem Bucket. |
| `/cloud-object-storage/bucket-cors-get` | Aktion | bucket, endpoint, ibmAuthEndpoint | Ermöglicht das Lesen der Konfiguration aus einem Bucket. |
| `/cloud-object-storage/bucket-cors-delete` | Aktion | bucket, endpoint, ibmAuthEndpoint | Ermöglicht das Löschen der CORS-Konfiguration aus einem Bucket. |
| `/cloud-object-storage/client-get-signed-url` | Aktion | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Ermöglicht das Abrufen einer signierten URL, um die Schreib-, Lese- und Löschvorgänge eines Objekts aus einem Bucket zu beschränken. |

## Paket- und Aktionsparameter

#### Paketparameter

Die folgenden Parameter sollen an das Paket gebunden werden; dadurch stehen sie automatisch allen Aktionen zur Verfügung. Es ist auch möglich, diese Parameter beim Aufruf der Aktionen anzugeben. 

**apikey**: Der Parameter `apikey` ist ein IAM-API-Schlüssel für die {{site.data.keyword.cos_short}}-Instanz. 

**resource_instance_id**: Der Parameter `resource_instance_id` ist die {{site.data.keyword.cos_short}}-Instanz-ID. 

**cos_hmac_keys**: Der Parameter `cos_hmac_keys` bezeichnet die HMAC-Berechtigungsnachweise der {{site.data.keyword.cos_short}}-Instanz, darunter die Werte `access_key_id` und `secret_access_key`. Diese Berechtigungsnachweise werden ausschließlich von der Aktion `client-get-signed-url` verwendet. Anweisungen zum Generieren von HMAC-Berechtigungsnachweisen für Ihre {{site.data.keyword.cos_short}}-Instanz finden Sie unter [HMAC-Berechtigungsnachweise verwenden](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials). 

#### Aktionsparameter

Die folgenden Parameter werden beim Aufruf der einzelnen Aktionen angegeben. Nicht alle diese Parameter werden von allen Aktionen unterstützt. In der folgenden Tabelle ist aufgeführt, welche Parameter von welcher Aktion unterstützt werden. 

**bucket**: Der Parameter `bucket` ist der Name des {{site.data.keyword.cloud_object_storage_short_notm}}-Buckets. 

**endpoint**: Der Parameter `endpoint` ist der {{site.data.keyword.cos_short}}-Endpunkt, der zum Herstellen einer Verbindung mit Ihrer {{site.data.keyword.cos_short}}-Instanz verwendet wird. Sie können Ihren Endpunkt in der [{{site.data.keyword.cos_short}}-Dokumentation finden](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints). 

**expires**: Der Parameter `expires` ist die Anzahl von Sekunden bis zum Ablauf der vorab signierten URL-Operation. Der Standardwert von `expires` ist 15 Minuten. 

**ibmAuthEndpoint**: Der Parameter `ibmAuthEndpoint` ist der IBM Cloud-Berechtigungsendpunkt, der von {site.data.keyword.cos_short}} zum Generieren eines Tokens aus dem API-Schlüssel (`apikey`) verwendet wird. Der Standardberechtigungsendpunkt sollte für alle IBM Cloud-Regionen funktionieren. 

**key**: Der Parameter `key` ist der Schlüssel des Bucketobjekts. 

**operation**: Der Parameter `operation` ist die aufzurufende Operation der vorab signierten URL. 

**corsConfig**: Der Parameter `corsConfig` ist die CORS-Konfiguration eines Buckets. 


## IBM Cloud Object Storage-Serviceinstanz erstellen
{: #cloud_object_storage_service_instance}

Vor dem Installieren des Pakets müssen Sie eine Instanz von {{site.data.keyword.cos_short}} anfordern und mindestens ein Bucket erstellen.

1. [Erstellen Sie eine {{site.data.keyword.cos_short}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-order-storage#creating-a-new-service-instance).

2. [Erstellen Sie eine Gruppe von HMAC-Serviceberechtigungsnachweisen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) für die Instanz des {{site.data.keyword.cos_short}}-Service. Fügen Sie im Feld **Inline-Konfigurationsparameter hinzufügen (optional)** die Zeichenfolge `{"HMAC":true}` hinzu.

3. [Erstellen Sie mindestens ein Bucket ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).

## {{site.data.keyword.cos_short}}-Paket installieren
{: #cloud_object_storage_installation}

Nachdem Sie über eine Instanz des {{site.data.keyword.cos_short}}-Service verfügen, können Sie wahlweise die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}} oder die Benutzerschnittstelle (UI) verwenden, um das {{site.data.keyword.cos_short}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #cloud_object_storage_cli}

Vorbereitende Schritte:

[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Gehen Sie zum Installieren des {{site.data.keyword.cos_short}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.cos_short}}-Paket.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigieren Sie zum Verzeichnis `runtimes/nodejs` oder zum Verzeichnis `runtimes/python`. Die Aktionen im {{site.data.keyword.cos_short}}-Paket werden in der von Ihnen ausgewählten Laufzeit bereitgestellt.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie das Paket bereit. Sie können die vorherigen Schritte wiederholen, um das Paket in einer anderen Laufzeit erneut zu implementieren.
```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Stellen Sie sicher, dass das Paket `cloud-object-storage` zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Ausgabe:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.cos_short}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.cos_short}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #cloud_object_storage_ui}

1. Öffnen Sie die [Seite 'Erstellen' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/create) in der {{site.data.keyword.openwhisk_short}}-Konsole. 

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbereich aus, in dem die Installation des {{site.data.keyword.cos_short}}-Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- (`org`) und des Bereichsnamens (`space`) gebildet. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **IBM Cloud Object Storage** und anschließend auf das Paket **IBM Cloud Object Storage**. 

5. Wählen Sie im Abschnitt **Verfügbare Laufzeiten** entweder `Node.JS` oder `Python` in der Dropdown-Liste aus. Klicken Sie dann auf **Installieren**. 

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **cloud-object-storage**. 

7. Um die Aktionen im Paket **cloud-object-storage** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus.
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **cloud-object-storage**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## In ein {{site.data.keyword.cos_short}}-Bucket schreiben
{: #cloud_object_storage_write}

Mit der Aktion `object-write` können Sie ein Objekt in ein {{site.data.keyword.cos_short}}-Bucket schreiben.
{: shortdesc}

**Hinweis**: In den folgenden Schritten wird die Bezeichnung `testbucket` als Beispielname verwendet. In {{site.data.keyword.cos_full_notm}} müssen Buckets auf globaler Ebene eindeutig sein. Daher müssen Sie den Beispielnamen `testbucket` durch einen eindeutigen Bucketnamen ersetzen.

### Über die CLI in ein Bucket schreiben
{: #write_bucket_cli}

Schreiben Sie mithilfe der Aktion `object-write` ein Objekt in Ihr Bucket.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Beispielausgabe:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Über die UI in ein Bucket schreiben
{: #write_bucket_ui}

1. Öffnen Sie die [Seite 'Aktionen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/actions). 

2. Klicken Sie unter dem Paket `cloud-object-storage` auf die Aktion **object-write**.

3. Klicken Sie im Teilfenster 'Code' auf **Eingabe ändern**.

4. Geben Sie ein JSON-Objekt ein, das Ihr Bucket, Ihren Schlüssel und Ihren Hauptteil als Objektschlüssel enthält.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. Klicken Sie auf **Speichern**.

6. Klicken Sie auf **Aufrufen**.

7. Überprüfen Sie, ob die Ausgabe der folgenden ähnelt:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Aus einem {{site.data.keyword.cos_short}}-Bucket lesen
{: #cloud_object_storage_read}

Zum Lesen eines Objekts in einem {{site.data.keyword.cos_short}}-Bucket können Sie die Aktion `object-read` verwenden.
{: shortdesc}

**Hinweis**: In den folgenden Schritten wird die Bezeichnung `testbucket` als Beispielname verwendet. In {{site.data.keyword.cos_full_notm}} müssen Buckets auf globaler Ebene eindeutig sein. Daher müssen Sie den Beispielnamen `testbucket` durch einen eindeutigen Bucketnamen ersetzen.

### Über die CLI aus einem Bucket lesen
{: #read_bucket_cli}

Lesen Sie mithilfe der Aktion `object-read` aus einem Objekt in Ihrem Bucket.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Beispielausgabe:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### Über die UI aus einem Bucket lesen
{: #read_bucket_ui}

1. Öffnen Sie die [Seite 'Aktionen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/actions). 

2. Klicken Sie unter dem Paket `cloud-object-storage` auf die Aktion **object-read**.

3. Klicken Sie im Teilfenster 'Code' auf **Eingabe ändern**.

4. Geben Sie ein JSON-Objekt ein, das Ihr Bucket und Ihren Schlüssel als Objektschlüssel enthält.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. Klicken Sie auf **Speichern**.

6. Klicken Sie auf **Aufrufen**.

7. Überprüfen Sie, ob die Ausgabe der folgenden ähnelt:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}
