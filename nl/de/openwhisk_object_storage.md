---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (Experimentell) Objektspeicher-Ereignisquellen
{: #cloud_object_storage}

Das Paket `/whisk.system/cos-experimental` ist möglicherweise instabil, ändert sich häufig in einer Weise, die nicht mit früheren Versionen kompatibel ist, und kann mit einem kurzen Hinweis eingestellt werden. Dieses Paket wird für die Verwendung in Produktionsumgebungen nicht empfohlen. Dieses experimentelle Paket ist derzeit nur in der Region 'US-South' (USA (Süden)) verfügbar.
{: important}

In diesem Beispiel wird Folgendes gezeigt:  
* [Überwachen auf Änderungen](#listening_to_cos_bucket_changes) an einer {{site.data.keyword.cos_full}}-Instanz. 
* [Erstellen eines Auslösers](#creating_a_trigger_cos), um auf diese Änderungen zu reagieren. 
* [Erstellen von Aktionen](#creating_action_to_process_object), um die Änderungen abzurufen und zu verarbeiten. 
* [Erstellen einer Regel](#associating_action_with_change_trigger), um Ihre Aktion zuzuordnen und den Auslöser zu ändern. 
<br>

**Beispiel für einen Anwendungsfall:** Mit dem Paket `/whisk.system/cos-experimental` können Sie auf Änderungen an GPS-Daten überwachen, die in einem {{site.data.keyword.cos_full_notm}}-Bucket gespeichert sind. Wenn dann Änderungen auftreten, können Sie die automatische Neugenerierung einer GPS-Karte auslösen, sodass Benutzer auf die aktuellsten Straßendaten für ihre GPS-Anwendung zugreifen können. 

## Informationen zu IBM Cloud Object Storage
{: #cloud_object_storage_info}

**Vorbereitende Schritte:** Informationen zu {{site.data.keyword.cos_full_notm}} finden Sie unter [Informationen zu Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage). Weitere Informationen zum Einrichten der {{site.data.keyword.cos_full_notm}}-Instanz finden Sie unter [{{site.data.keyword.cos_short}}-Instanz bereitstellen](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage). 

## Auf Änderungen an einem IBM Cloud Object Storage-Bucket überwachen
{: #listening_to_cos_bucket_changes}

Mit {{site.data.keyword.openwhisk}} können Sie auf Änderungen an einem [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage)-Bucket überwachen und eine Aktion verwenden, um mindestens ein Objekt im Bucket zu verarbeiten.  

Mit dem Paket `/whisk.system/cos-experimental` können Sie Ereignisse aus einer {{site.data.keyword.cos_full_notm}}-Instanz erstellen und es enthält den folgenden Feed: 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Paket | apikey, auth_endpoint, bucket, endpoint, interval | Paket mit der Feedaktion `changes`. |
| `/whisk.system/cos-experimental/changes` | Feed | apikey, auth_endpoint, bucket, endpoint, interval | Aktivieren eines Auslöserereignisses bei Änderungen an einem {{site.data.keyword.cos_full_notm}}-Bucket. |
{: shortdesc}

Mit dem Feed `changes` können Sie den {{site.data.keyword.cos_full_notm}}-Ereignisquellenservice so konfigurieren, dass bei jeder Änderung an einem Bucket in Ihrer {{site.data.keyword.cos_full_notm}}-Instanz ein Auslöser aktiviert wird. 

In diesem Beispiel verwendete Parameter: 

**apikey**: _(Erforderlich, sofern nicht an das Paket gebunden)_. Der Parameter `apikey` ist ein IAM-API-Schlüssel für die {{site.data.keyword.cos_full_notm}}-Instanz. Normalerweise ist dieser Wert an das Paket gebunden. Wenn der Wert `apikey` jedoch bei Verwendung der Feedaktion `changes` angegeben wird, wird der angegebene Wert für die Berechtigungsnachweise statt für den gebundenen API-Schlüssel der Berechtigungsnachweise verwendet. 

**auth_endpoint**: _(Optional)_. Der Parameter `auth_endpoint` ist der Berechtigungsendpunkt, der von {{site.data.keyword.cos_full_notm}} zum Generieren eines Tokens aus dem API-Schlüssel (`apikey`) verwendet wird. Der Standardendpunkt ist der {{site.data.keyword.Bluemix}}-Endpunkt. 

**bucket**: _(Erforderlich)_. Der Parameter `bucket` ist der Name des {{site.data.keyword.cos_full_notm}}-Buckets. 

**endpoint**: _(Erforderlich)_. Der Parameter `endpoint` ist der {{site.data.keyword.cos_full_notm}}-Endpunkt, der zum Herstellen einer Verbindung mit Ihrer {{site.data.keyword.cos_full_notm}}-Instanz verwendet wird. Sie können Ihren Endpunkt in der [{{site.data.keyword.cos_full_notm}}-Dokumentation finden](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints). 

**interval**: _(Optional)_. Der Parameter `interval` ist das Abfrageintervall des Buckets in ganzen Minuten. Der Wert von `interval` muss mindestens 1 Minute betragen. Dies ist auch die Standardeinstellung. 

## Auslöser erstellen, um auf den Feed 'changes' zu reagieren
{: #creating_a_trigger_cos}

Wenn Sie den Auslöser erstellen, können Sie vermeiden, Ihre {{site.data.keyword.cos_full_notm}}-Berechtigungsnachweise an die Feedaktion `changes` weiterzugeben, indem Sie Ihre Berechtigungsnachweise direkt an das Paket `cos-experimental` binden. {: shortdesc}
 
 1. Erstellen Sie zunächst eine Paketbindung, die so geändert werden kann, dass sie Ihre Berechtigungsnachweise enthält. Im Folgenden wird eine Paketbindung, `myCosPkg`, in Ihrem Namensbereich erstellt. 
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Binden Sie Ihre {{site.data.keyword.cos_short}}-Berechtigungsnachweise an das Paket.
 Durch das Binden Ihrer {{site.data.keyword.cos_short}}-Berechtigungsnachweise an das Paket wird der Wert `apikey` an das Paket gebunden, sodass Sie den Wert `apikey` nicht angeben müssen, wenn die Feedaktion `changes` aufgerufen wird.  
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Erstellen Sie einen Trigger namens `myCosTrigger` mit dem Feed `changes` in der von Ihnen erstellten Paketbindung. Verwenden Sie die Werte für Ihren Bucketnamen und {{site.data.keyword.cos_short}}-Endpunktparameter. 
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    Beispielausgabe:
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. Starten Sie das Polling auf Aktivierungen, um einen Überblick über die aktuellen Abläufe zu erhalten.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. Erstellen Sie eine Aktion, um den Feed 'changes' zu beobachten. Beispielsweise eine Aktion mit dem Namen `showCosChange`, die den folgenden JavaScript-Code enthält: 
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
 6. Erstellen Sie eine Regel, um die Aktion `showCosChange` mit dem Auslöser `myCosTrigger` zu verbinden: 
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. Ändern Sie in Ihrem {{site.data.keyword.cos_short}}-Dashboard ein vorhandenes Bucketobjekt oder erstellen Sie eines. Informationen zum Hinzufügen eines Objekts zu Ihrem Bucket finden Sie unter [Objekte zu Ihrem Bucket hinzufügen](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects). 
 
 8. Bei jeder Änderung an einem Bucketobjekt können Sie neue Aktivierungen für den Auslöser `myCosTrigger` und die Aktion `showCosChange` beobachten. Diese Aktivierungen werden innerhalb des konfigurierten Bucket-Abfrageintervalls angezeigt. 

Wenn Sie keine neuen Aktivierungen beobachten können, stellen Sie sicher, dass die Parameterwerte `apikey`, `endpoint` und `bucket` korrekt sind. 
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Datenstruktur eines Auslöserereignisses
{: #data_structure_trigger_event}

Der Inhalt der generierten Ereignisse weist die folgenden Parameter auf:

  - `file`: Die Datei- oder Objektmetadaten. 
  - `status`: Die erkannte Änderung. Dieser Wert lautet `added`, `modified` oder `deleted`. 
  - `bucket`: Der Name des {{site.data.keyword.cos_short}}-Buckets. 
  - `endpoint`:  Der {{site.data.keyword.cos_short}}-Endpunkt für die Verbindung mit der {{site.data.keyword.cos_short}}-Instanz. 
  - `key`: Die ID des geänderten Bucketobjekts. Dieser Wert ist identisch mit `file.Key`, aber am Anfang der Auslöserereignis-JSON verfügbar. 

Beispiel für eine JSON-Darstellung des Auslöserereignisses für Bucketänderungen: 
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

## Aktion zum Verarbeiten des geänderten Objekts erstellen
{: #creating_action_to_process_object}

Sie können eine einzelne Aktion erstellen, die das Objekt abruft und verarbeitet. Oder Sie können eine Sequenz erstellen, die eine Aktion zum Abrufen des Objekts und eine andere Aktion zum Verarbeiten des Objekts verwendet. 

### Aktion zum Abrufen und Verarbeiten des Objekts erstellen
{: #creating_action_to_retrieve_object}

Dieser Beispielaktionscode ruft das Dokument mit der Benachrichtigung zur Bucketänderung ab und verarbeitet es. Sie können die Parameter `apikey` und `serviceInstanceId` direkt an die Aktion übergeben, während Sie die Aktion manuell aufrufen. Aber wenn diese Aktion von einem Auslöser aufgerufen wird, müssen die Werte aus Ihrem {{site.data.keyword.cos_short}} abgerufen werden, der mit dem Befehl `ibmcloud fn service bind` an die Aktion gebunden werden muss. 

Beispielcode: 

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
```
{: codeblock}

Da diese Aktion das NMP-Paket `ibm-cos-sdk` verwendet, muss die Aktion entweder als [Node.js-Modul](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) oder als [einzelnes Bundle](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action) gepackt sein. 

Nach dem Packen dieser Aktion in einer ZIP-Datei erstellen Sie die Aktion zum Abrufen und Verarbeiten des Objekts aus {{site.data.keyword.cos_short}}: 

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

[Binden](#cos_binding_credentials_to_action) Sie die {{site.data.keyword.cos_short}}-Berechtigungsnachweise an diese Aktion. [Erstellen Sie anschließend eine Regel](#associating_action_with_change_trigger) zum Aufrufen dieser Aktion, wenn der Auslöser aktiviert wird. 

### Aktionssequenz zum Abrufen und Verarbeiten des Objekts erstellen

Statt den Objektabrufcode in Ihre Aktion einzuschließen, können Sie die Aktion `object-read` aus dem Paket {{site.data.keyword.cos_short}} verwenden, das [manuell installiert werden muss](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation). Ihr Aktionscode müsste nur die aus `object-read` zurückgegebenen Ergebnisse verarbeiten. 

Beispielcode einer Aktion, die nur das Bucketobjekt verarbeitet: 
```javascript
function main(data) {
  if (data) {
    // Objekt verarbeiten
  }
}
```
{: codeblock}

1. Erstellen Sie die Aktion, um nur das Objekt aus {{site.data.keyword.cos_short}} zu verarbeiten: 
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. Binden Sie Ihre {{site.data.keyword.cos_short}}-Berechtigungsnachweise an Ihre `cos-experimental`-Paketbindung. 
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. Die Aktion `object-read` kann mit `myCosProcessObjectAction` zusammengesetzt werden, um eine Aktionssequenz zu erstellen. 
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

Zusätzlich zur Aktion `object-read` können Sie andere Aktionen verwenden, die in dem installierbaren Paket {{site.data.keyword.cos_short}} enthalten sind. 

[Binden](#cos_binding_credentials_to_action) Sie die {{site.data.keyword.cos_short}}-Berechtigungsnachweise an diese Aktion. [Erstellen Sie anschließend eine Regel](#associating_action_with_change_trigger) zum Aufrufen dieser Aktion, wenn der Auslöser aktiviert wird. 

 ## Berechtigungsnachweise an Ihre Aktion binden
 {: #cos_binding_credentials_to_action}
 
 Sie können vermeiden, dass vertrauliche Berechtigungsnachweise während des Aufrufs übergeben werden, indem Sie die {{site.data.keyword.cos_short}}-Berechtigungsnachweise mit dem folgenden Befehl an die Aktion binden: 
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## Regel zum Zuordnen der Aktion zum Änderungsauslöser erstellen
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

Sie können eine Aktion oder Aktionssequenz in einer [Regel](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) verwenden, um das Objekt, das einem {{site.data.keyword.cos_short}}-Änderungsereignis zugeordnet ist, abzurufen und zu verarbeiten. 

Erstellen Sie eine Regel, die die Aktion `MyCosAction` für neue {{site.data.keyword.cos_short}}-Auslöserereignisse aktiviert. 
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



