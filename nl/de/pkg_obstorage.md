---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

Sie können die Funktionalität Ihrer {{site.data.keyword.openwhisk}}-App erweitern, indem Sie eine {{site.data.keyword.cos_full}}-Instanz integrieren.

**Vorbereitende Schritte**
Informationen zu {{site.data.keyword.cos_full_notm}} finden Sie in [Informationen zu Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Weitere Informationen zum Einrichten der {{site.data.keyword.cos_full_notm}}-Instanz finden Sie unter [{{site.data.keyword.cos_full_notm}}-Instanz bereitstellen](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Pakete
{: #obstorage_packages}
| Paket | Verfügbarkeit | Beschreibung |
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}}-Paket](#pkg_obstorage_install)| Installierbar | Ermöglicht das Lesen, Schreiben und Löschen bei einer {{site.data.keyword.cos_full_notm}}-Instanz. |
| [(Experimentell) {{site.data.keyword.cos_full_notm}}-Ereignisquelle](#pkg_obstorage_ev) | Vorinstalliert (nur Vereinigte Staaten (Süden)) | Überwachen auf Änderungen an einer {{site.data.keyword.cos_full_notm}}-Instanz. |

## IBM Cloud Object Storage-Serviceinstanz erstellen
{: #pkg_obstorage_service}

Vor der Verwendung eines der Pakete müssen Sie eine Instanz von {{site.data.keyword.cos_full_notm}} anfordern und mindestens ein Bucket erstellen.

1. [Erstellen Sie eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision). 

2. [Erstellen Sie einen Satz HMAC-Serviceberechtigungsnachweise](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) für die {{site.data.keyword.cos_full_notm}}-Serviceinstanz. Fügen Sie im Feld **Inline-Konfigurationsparameter hinzufügen (optional)** die Zeichenfolge `{"HMAC":true}` hinzu.

3. [Erstellen Sie mindestens ein Bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## {{site.data.keyword.cos_full_notm}}-Paket über die {{site.data.keyword.openwhisk_short}}-CLI installieren
{: #pkg_obstorage_install}

Nachdem Sie über eine Instanz des {{site.data.keyword.cos_full_notm}}-Service verfügen, können Sie wahlweise die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}} oder die Benutzerschnittstelle (UI) verwenden, um das {{site.data.keyword.cos_full_notm}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}
{: #pkg_obstorage_cli}

Das installierbare {{site.data.keyword.cos_full_notm}}-Paket stellt eine Gruppe von Aktionen bereit, die Sie für die Arbeit mit Ihrer {{site.data.keyword.cos_full_notm}}-Instanz verwenden können. Diese werden entweder in Node.js oder in Python ausgeführt. Nachdem Sie das Paket installiert haben, können Sie eine Laufzeit auswählen. Eine Liste der Aktionen im Paket `cloud-object-storage` finden Sie in [Verfügbare Entitäten](#pkg_obstorage_actions).
{: note}

**Vorbereitende Schritte**

[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Gehen Sie zum Installieren des {{site.data.keyword.cos_full_notm}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.cos_full_notm}}-Paket.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis `runtimes/nodejs` oder `runtimes/python`, um eine Laufzeit für die Aktionen im Paket auszuwählen.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Stellen Sie das Paket `cloud-object-storage` bereit.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Antwort**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. Stellen Sie sicher, dass das Paket `cloud-object-storage` zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Ausgabe**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.cos_full_notm}}-Instanz an das Paket. Sie können das Flag `-- keyname` einschließen, um bestimmte Serviceberechtigungsnachweise zu binden. Weitere Informationen zum Binden von Services finden Sie in ['service'-Befehle](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service). 

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.cos_full_notm}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Parameter binden

Sie können den Befehl [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) verwenden, um die Endpunkte eines Buckets an eine bestimmte Aktion oder an das Paket `cloud-object-storage` zu binden. Ersetzen Sie `<bucket_endpoint>` durch den Endpunkt Ihres Buckets. 

Wenn Sie Parameter für ein Paket, eine Aktion oder einen Auslöser aktualisieren, müssen Sie alle zuvor erstellten Parameter angeben. Andernfalls werden die zuvor erstellten Parameter entfernt. Alle Services, die an das Paket gebunden wurden, werden ebenfalls entfernt. Daher müssen Sie die [Services erneut an Ihr Paket binden](/docs/openwhisk?topic=cloud-functions-services), nachdem Sie andere Parameter aktualisiert haben.
{: important}

**Parameter an alle Aktionen in einem Paket binden**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Parameter an bestimmte Aktionen binden**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

Sie können Parameter auch über die Registerkarte **Parameter** in der Benutzerschnittstelle an Aktionen binden. Um Parameter in der Benutzerschnittstelle hinzuzufügen, navigieren Sie zu der [Seite **Aktionen**](https://cloud.ibm.com/openwhisk/actions){: external} und klicken Sie auf eine Ihrer Aktionen. Anschließend klicken Sie auf **Parameter** > **Parameter hinzufügen**. Sie müssen Parameter in Paaren aus `<key>` und `<value>` hinzufügen.
{: tip}


## {{site.data.keyword.cos_full_notm}}-Paket über die {{site.data.keyword.openwhisk_short}}-Benutzerschnittstelle installieren
{: #pkg_obstorage_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das {{site.data.keyword.cos_full_notm}}-Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **IBM Cloud Object Storage** und anschließend auf das Paket **IBM Cloud Object Storage**.

5. Wählen Sie im Abschnitt **Verfügbare Laufzeiten** entweder `Node.JS` oder `Python` in der Dropdown-Liste aus. Klicken Sie dann auf **Installieren**.

6. Nachdem das Paket installiert wurde, werden Sie zur Seite **Aktionen** weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet `cloud-object-storage`. 

7. Um die Aktionen im Paket `cloud-object-storage` verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie Schritt 5 in den [CLI-Anweisungen](#pkg_obstorage_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. 
  
Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden, müssen Sie die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
{: note}

1. Klicken Sie auf eine Aktion aus dem Paket `cloud-object-storage`, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
2. Klicken Sie im Navigationsbereich links auf **Parameter**. 
3. Geben Sie einen neuen Parameter ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.


Weitere Informationen zum Binden von Parametern finden Sie in [Parameter an Pakete binden](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params). 


## Verfügbare Entitäten
{: #pkg_obstorage_actions}

Das {{site.data.keyword.cos_full_notm}}-Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Paket | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Ermöglicht das Arbeiten mit einer {{site.data.keyword.cos_full_notm}}-Instanz. |
| `/cloud-object-storage/object-write` | Aktion | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Schreiben eines Objekts in ein Bucket. |
| `/cloud-object-storage/object-read` | Aktion | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Lesen eines Objekts aus einem Bucket. |
| `/cloud-object-storage/object-delete` | Aktion | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Löschen eines Objekts aus einem Bucket. |
| `/cloud-object-storage/bucket-cors-put` | Aktion | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Zuordnen einer CORS-Konfiguration zu einem Bucket. |
| `/cloud-object-storage/bucket-cors-get` | Aktion | `bucket`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Lesen der Konfiguration aus einem Bucket. |
| `/cloud-object-storage/bucket-cors-delete` | Aktion | `bucket`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Löschen der CORS-Konfiguration aus einem Bucket. |
| `/cloud-object-storage/client-get-signed-url` | Aktion | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Ermöglicht das Abrufen einer signierten URL, um die Schreib-, Lese- und Löschvorgänge eines Objekts aus einem Bucket zu beschränken. |

Führen Sie den Befehl `ibmcloud fn package get cloud-object-storage` aus, um eine vollständige Liste der verfügbaren Entitäten abzurufen.
{: note}

### Paketparameter
{: #pkg_obstorage_pkgparams}

Es wird erwartet, dass die folgenden Paketparameter an das Paket gebunden werden und automatisch für alle Aktionen verfügbar sind. Es ist auch möglich, diese Parameter beim Aufruf einer der Aktionen anzugeben. 

| Paketparameter | Beschreibung |
| --- | --- |
| `apikey` | Der Parameter `apikey` ist ein IAM-API-Schlüssel für die {{site.data.keyword.cos_full_notm}}-Instanz. |
| `resource_instance_id` | Der Parameter `resource_instance_id` gibt die {{site.data.keyword.cos_full_notm}}-Instanz-ID an. |
| `cos_hmac_keys` | Der Parameter `cos_hmac_keys` gibt die HMAC-Berechtigungsnachweise der {{site.data.keyword.cos_full_notm}}-Instanz an, die die Werte für `access_key_id` und `secret_access_key` umfassen. Diese Berechtigungsnachweise werden ausschließlich von der Aktion `client-get-signed-url` verwendet.  Informationen zur Generierung von HMAC-Berechtigungsnachweisen für Ihre {{site.data.keyword.cos_full_notm}}-Instanz finden Sie in [HMAC-Berechtigungsnachweise verwenden](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials). |
 
### Aktionsparameter
{: #pkg_obstorage_actparams}

Die folgenden Aktionsparameter werden beim Aufruf der einzelnen Aktionen angegeben. Nicht alle diese Parameter werden von jeder Aktion unterstützt. Der Tabelle  [Verfügbare Entitäten](#pkg_obstorage_actions) können Sie entnehmen, welche Parameter von welcher Aktion unterstützt werden. 

| Aktionsparameter | Beschreibung |
| --- | --- |
| `bucket` | Der Parameter `bucket` gibt den Namen des {{site.data.keyword.cos_full_notm}}-Buckets an. |
| `endpoint` | Der Parameter `endpoint` gibt den {{site.data.keyword.cos_full_notm}}-Endpunkt an, der verwendet wird, um eine Verbindung zu Ihrer {{site.data.keyword.cos_full_notm}}-Instanz herzustellen. Sie finden Ihren Endpunkt in der [Dokumentation zu {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | Der Parameter `expires` gibt die Anzahl der Sekunden bis zum Ablauf der vorab signierten URL-Operation an. Der Standardwert für `expires` ist 15 Minuten. |
| `ibmAuthEndpoint` | Der Parameter `ibmAuthEndpoint` gibt den IBM Cloud-Berechtigungsendpunkt an, der von {site.data.keyword.cos_short}} verwendet wird, um ein Token aus dem API-Schlüssel (`apikey`) zu generieren. Der Standardberechtigungsendpunkt funktioniert für alle IBM Cloud-Regionen. |
| `key` | Der Parameter `key` gibt den Objektschlüssel des Buckets an. |
| `operation` | Der Parameter `operation` gibt die aufzurufende Operation der vorab signierten URL an. |
| `corsConfig` | Der Parameter `corsConfig` gibt die CORS-Konfiguration des Buckets an. |


## Objekte in ein Bucket schreiben
{: #pkg_obstorage_write}

Mit der Aktion `object-write` können Sie ein Objekt in ein {{site.data.keyword.cos_full_notm}}-Bucket schreiben.
{: shortdesc}

In den folgenden Schritten wird der Name `test-bucket` als Beispiel verwendet. Buckets müssen in {{site.data.keyword.cos_full_notm}} global eindeutig sein. Daher müssen Sie `test-bucket` durch einen eindeutigen Bucketnamen ersetzen.
{: note}

### Objekt über die Befehlszeilenschnittstelle in ein Bucket schreiben
{: #pkg_obstorage_write_cli}
Schreiben Sie mithilfe der Aktion `object-write` ein Objekt in Ihr Bucket.
{: shortdesc}


Rufen Sie die Aktion `object-write` auf, um ein Objekt in Ihr Bucket zu schreiben. Wenn Sie Ihren Bucketendpunkt an Ihr Paket oder an die Aktion `object-write` gebunden haben, müssen Sie den Endpunkt nicht als Parameter einschließen. Ersetzen Sie `<org_space>` durch den Namen Ihrer Cloud Foundry-Organisation und Ihres Cloud Foundry-Bereichs, `<test-bucket>` durch den Namen Ihres Buckets und `<test.txt>` durch den Namen des Objekts, das geschrieben werden soll. 

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Beispielausgabe**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Objekt über die Benutzerschnittstelle in ein Bucket schreiben
{: #pkg_obstorage_write_ui}


1. Rufen Sie die [Seite 'Aktionen'](https://cloud.ibm.com/openwhisk/actions){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie unter dem Paket `cloud-object-storage` auf die Aktion **object-write**.

3. Klicken Sie im Teilfenster 'Code' auf **Eingabe ändern**.

4. Geben Sie ein JSON-Objekt ein, das Ihr Bucket, Ihren Schlüssel und Ihren Hauptteil als Objektschlüssel enthält.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
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
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Objekte aus einem Bucket lesen
{: #pkg_obstorage_read}

Zum Lesen eines Objekts in einem {{site.data.keyword.cos_full_notm}}-Bucket können Sie die Aktion `object-read` verwenden.
{: shortdesc}

In den folgenden Schritten wird der Name `test-bucket` als Beispiel verwendet. Buckets müssen in {{site.data.keyword.cos_full_notm}} global eindeutig sein. Daher müssen Sie `test-bucket` durch einen eindeutigen Bucketnamen ersetzen.
{: note}

### Objekt über die Befehlszeilenschnittstelle aus einem Bucket lesen
{: #pkg_obstorage_read_cli}

Lesen Sie mithilfe der Aktion `object-read` aus einem Objekt in Ihrem Bucket.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Beispielausgabe**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Objekt über die Benutzerschnittstelle aus einem Bucket lesen
{: #pkg_obstorage_read_ui}

1. Rufen Sie die [Seite 'Aktionen'](https://cloud.ibm.com/openwhisk/actions){: external} auf. 

2. Klicken Sie unter dem Paket `cloud-object-storage` auf die Aktion `object-read`.

3. Klicken Sie im Teilfenster 'Code' auf **Eingabe ändern**.

4. Geben Sie ein JSON-Objekt ein, das Ihr Bucket und Ihren Schlüssel als Objektschlüssel enthält.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
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
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Mit der (experimentellen) Object Storage-Ereignisquelle auf Änderungen an einem Bucket überwachen
{: #pkg_obstorage_ev}

Das Paket `/whisk.system/cos-experimental` ist möglicherweise instabil, ändert sich häufig in einer Weise, die nicht mit früheren Versionen kompatibel ist, und kann mit einem kurzen Hinweis eingestellt werden. Dieses Paket wird für die Verwendung in Produktionsumgebungen nicht empfohlen. Dieses experimentelle Paket ist nur in der Region 'US-South' (USA (Süden)) verfügbar.
{: important}

Mit {{site.data.keyword.openwhisk}} können Sie auf Änderungen an einem [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations)-Bucket überwachen und eine Aktion verwenden, um mindestens ein Objekt im Bucket zu verarbeiten.

<br>

**Beispielanwendungsfall:** Mit dem Paket `/whisk.system/cos-experimental` können Sie auf Änderungen an GPS-Straßendaten überwachen, die in einem {{site.data.keyword.cos_full_notm}}-Bucket gespeichert sind. Wenn Änderungen auftreten, können Sie die automatische Neugenerierung einer GPS-Karte auslösen, sodass Benutzer auf die aktuellsten Straßendaten für ihre GPS-Anwendung zugreifen können. 

### Parameter der Object Storage-Ereignisquelle (experimentell)
{: #pkg_obstorage_ev_ch}

Mit dem Paket `/whisk.system/cos-experimental` können Sie Ereignisse aus einer {{site.data.keyword.cos_full_notm}}-Instanz konfigurieren. Es enthält den folgenden Feed: 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Paket | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Paket mit der Feedaktion `changes`. |
| `/whisk.system/cos-experimental/changes` | Feed | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Aktivieren von Auslöserereignissen bei Änderungen an einem {{site.data.keyword.cos_full_notm}}-Bucket. |
{: shortdesc}

Mit dem Feed `changes` können Sie den {{site.data.keyword.cos_full_notm}}-Ereignisquellenservice so konfigurieren, dass bei jeder Änderung an einem Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Instanz ein Auslöser aktiviert wird.

In diesem Beispiel verwendete Parameter:

| Parameter | Beschreibung |
| --- | --- |
| `apikey` | (Erforderlich, sofern nicht an das Paket gebunden). Der Parameter `apikey` ist ein IAM-API-Schlüssel für die {{site.data.keyword.cos_full_notm}}-Instanz.  Normalerweise ist dieser Wert an das Paket gebunden. Wird jedoch bei der Verwendung der Feedaktion `changes` ein Wert für `apikey` angegeben, wird der angegebene Wert und nicht der API-Schlüssel aus den gebundenen Berechtigungsnachweisen als Berechtigungsnachweis verwendet. |
| ` auth_endpoint` | (Optional). Der Parameter `auth_endpoint` gibt den Berechtigungsendpunkt an, der von {{site.data.keyword.cos_full_notm}} verwendet wird, um ein Token aus dem API-Schlüssel (`apikey`) zu generieren. Der Standardendpunkt ist der {{site.data.keyword.cloud}}-Endpunkt. |
| `bucket` | (Erforderlich). Der Parameter `bucket` gibt den Namen des {{site.data.keyword.cos_full_notm}}-Buckets an. |
| `endpoint` | (Erforderlich). Der Parameter `endpoint` gibt den {{site.data.keyword.cos_full_notm}}-Endpunkt an, der zum Herstellen einer Verbindung mit Ihrer {{site.data.keyword.cos_full_notm}}-Instanz verwendet wird. Sie finden Ihren Endpunkt in der [Dokumentation zu {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` | (Optional). Der Parameter `interval` gibt das Abfrageintervall des Buckets in ganzen Minuten an. Der Wert für `interval` muss mindestens 1 Minute betragen. Dies ist auch die Standardeinstellung. |

## Auslöser erstellen, um auf den Feed 'changes' zu reagieren
{: #pkg_obstorage_ev_trig}

Wenn Sie den Auslöser erstellen, können Sie vermeiden, Ihre {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise an die Feedaktion `changes` weiterzugeben, indem Sie Ihre Berechtigungsnachweise direkt an das Paket `cos-experimental` binden.
{: shortdesc}

 1. Erstellen Sie zunächst eine Paketbindung, die so geändert werden kann, dass sie Ihre Berechtigungsnachweise enthält. Im Folgenden wird eine Paketbindung, `myCosPkg`, in Ihrem Namensbereich erstellt.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Binden Sie Ihre {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise an das Paket. Durch das Binden Ihrer {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise an das Paket wird der Wert für `apikey` an das Paket gebunden, sodass Sie den Wert für `apikey` nicht angeben müssen, wenn die Feedaktion `changes` aufgerufen wird. 

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Erstellen Sie einen Auslöser mit dem Namen `cosTrigger` mit dem Feed `changes` in der Paketbindung, die Sie erstellt haben. Verwenden Sie die Werte für Ihren Bucketnamen und {{site.data.keyword.cos_full_notm}}-Endpunktparameter.

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Beispielausgabe**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

Erstellen Sie eine Aktion, um zu prüfen, ob der Auslöser, der Änderungsfeed und die Regel richtig konfiguriert sind und ordnungsgemäß funktionieren. 
 
  1. Speichern Sie den folgenden JavaScript-Code als `cosChange.js`.  

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

  2. Erstellen Sie mithilfe des Codes `cosChange.js` eine Aktion mit dem Namen `cosChange`. 

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. Erstellen Sie eine Regel, um die Aktion `cosChange` mit dem Auslöser `cosTrigger` zu verbinden. 

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. Starten Sie in einem eigenen Fenster das Polling auf Aktivierungen, um einen Überblick über die aktuellen Abläufe zu erhalten. Wenn der Auslöser aktiviert und die Aktion ausgeführt wird, listet dieser Befehl die Aktivierungsdatensätze für jede dieser Operationen auf, wenn sie auftreten. 

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. Ändern Sie in Ihrem {{site.data.keyword.cos_full_notm}}-Dashboard ein vorhandenes Bucketobjekt oder erstellen Sie eines. Informationen zum Hinzufügen eines Objekts zu Ihrem Bucket finden Sie unter [Objekte zu Ihrem Bucket hinzufügen](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
  6. Bei jeder Änderung an einem Bucketobjekt können Sie neue Aktivierungen für den Auslöser `cosTrigger` und die Aktion `cosChange` beobachten. Sie können diese Aktivierungen anzeigen, indem Sie den Befehl `ibmcloud fn activation poll` innerhalb des konfigurierten Bucket-Polling-Intervalls ausführen. 
  
  7. Wenn Sie keine neuen Aktivierungen beobachten können, stellen Sie sicher, dass die Parameterwerte für `apikey`, `endpoint` und `bucket` korrekt sind. 
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Datenstruktur einer Object Storage-Auslöseraktivierung
{: #pkg_obstorage_ev_data}

Der Inhalt der generierten Ereignisse weist die folgenden Parameter auf:

| Parameter | Beschreibung |
| --- | --- |
| `file` | Die Datei- oder Objektmetadaten. Diese Struktur wird unter [Objekte in spezifischem Bucket auflisten](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets) beschrieben. |
| `status` | Die erkannte Änderung.  Dieser Wert lautet `added`, `modified` oder `deleted`. |
| `bucket`| Der Name des {{site.data.keyword.cos_full_notm}}-Buckets. |
| `endpoint` | Der {{site.data.keyword.cos_full_notm}}-Endpunkt für die Herstellung einer Verbindung zu der {{site.data.keyword.cos_full_notm}}-Instanz. |
| `key` | Die ID des geänderten Bucketobjekts. Dieser Wert ist mit `file.Key` identisch; er ist jedoch am Anfang der Auslöserereignis-JSON verfügbar. |

**Beispiel für eine JSON-Darstellung der Auslöseraktivierung bei Bucketänderungen**

```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Aktion zum Verarbeiten eines geänderten Objekts erstellen
{: #pkg_obstorage_ev_act}

Sie können eine einzelne Aktion erstellen, die das Objekt abruft und verarbeitet. Oder Sie können eine Sequenz erstellen, die eine Aktion zum Abrufen des Objekts und eine andere Aktion zum Verarbeiten des Objekts verwendet.

### Aktion zum Abrufen und Verarbeiten des Objekts erstellen
{: #pkg_obstorage_ev_act_ret}

Dieser Beispielaktionscode ruft das Dokument mit der Benachrichtigung zur Bucketänderung ab und verarbeitet es. Sie können die Parameter `apikey` und `serviceInstanceId` direkt an die Aktion übergeben, wenn Sie die Aktion manuell aufrufen. Aber wenn diese Aktion von einem Auslöser aufgerufen wird, müssen die Werte aus Ihrem {{site.data.keyword.cos_full_notm}} abgerufen werden, der mit dem Befehl `ibmcloud fn service bind` an die Aktion gebunden werden muss. 

1. Speichern Sie den folgenden Code in einer .zip-Datei mit dem Namen `myCosAction.zip`. 

  ```javascript
  const COS = require('ibm-cos-sdk')

  function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

    const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

    return new Promise(function(resolve, reject) {
      client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
        if (err != null) {
          console.log(err)
        reject({ err: err })
        }  else {
          console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
      })
    });
}
exports.main = main;
  ```
  {: codeblock}

  Da diese Aktion das NPM-Paket `ibm-cos-sdk` verwendet, muss die Aktion entweder als [Node.js-Modul](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) oder als [einzelnes Bundle](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg) gepackt sein. 

2. Erstellen Sie die Aktion, um das Objekt aus {{site.data.keyword.cos_full_notm}} abzurufen und zu verarbeiten: 

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Aktionssequenz zum Abrufen und Verarbeiten des Objekts erstellen
{: #pkg_obstorage_ev_act_seq}

Statt den Objektabrufcode in Ihre Aktion einzuschließen, können Sie die Aktion `object-read` aus dem Paket `cloud-object-storage` verwenden, das [manuell installiert werden muss](#pkg_obstorage_install).  Ihr Aktionscode muss nur die von `object-read` zurückgegebenen Ergebnisse verarbeiten.
{: shortdesc}

So erstellen Sie eine Aktion, die nur das Bucketobjekt verarbeitet: 

1. Speichern Sie den folgenden Code als `myCosAction.js`. 

  ```javascript
  function main(data) {
    if (data) {
      // Objekt verarbeiten
  }
  }
  ```
  {: codeblock}

2. Erstellen Sie die Aktion, um nur das Objekt aus {{site.data.keyword.cos_full_notm}} zu verarbeiten. 

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Binden Sie Ihre {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise an Ihr manuell installiertes `cloud-object-storage`-Paket.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. Die Aktion `object-read` kann mit `myCosProcessObjectAction` zusammengesetzt werden, um eine Aktionssequenz zu erstellen.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Neben der Aktion `object-read` können Sie weitere Aktionen verwenden, die in dem installierbaren Paket `cloud-object-storage` enthalten sind.
{: tip}

Führen Sie den folgenden Befehl aus, um eine Liste der verfügbaren Entitäten abzurufen. 
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Berechtigungsnachweise an Ihre Aktion binden
{: #pkg_obstorage_ev_bind}

Sie können vermeiden, dass vertrauliche Berechtigungsnachweise während des Aufrufs übergeben werden, indem Sie Ihre {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise mit dem folgenden Befehl an die Aktion binden: 
  ```
  ibmcloud fn service bind cloud-object-storage myCosAction
  ```
  {: pre}

### Regel zum Zuordnen der Aktion zum Änderungsauslöser erstellen
{: #pkg_obstorage_ev_rule}

Sie können eine Aktion oder Aktionssequenz in einer [Regel](/docs/openwhisk?topic=cloud-functions-rules) verwenden, um das Objekt, das einem {{site.data.keyword.cos_full_notm}}-Änderungsereignis zugeordnet ist, abzurufen und zu verarbeiten. 

Erstellen Sie eine Regel, die die Aktion `MyCosAction` für neue {{site.data.keyword.cos_full_notm}}-Auslöserereignisse aktiviert.
  ```
  ibmcloud fn rule create myRule myCosTrigger myCosAction
  ```
  {: pre}



