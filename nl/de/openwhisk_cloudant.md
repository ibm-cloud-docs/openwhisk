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

# Cloudant-Ereignisquelle
{: #openwhisk_cloudant}

Hier erfahren Sie, wie Sie Änderungen an einer {{site.data.keyword.cloudant}}-Datenbank überwachen, Datenbankänderungsereignisse filtern und eine Aktionsfolge verwenden, um ein Dokument aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank zu verarbeiten. Das Paket `/whisk.system/cloudant` ermöglicht Ihnen die Arbeit mit einer {{site.data.keyword.cloudant_short_notm}}-Datenbank und enthält die folgenden Aktionen und Feeds:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paket | dbname, host, username, password | Für die Arbeit mit einer Cloudant-Datenbank. |
| `/whisk.system/cloudant/read` | Aktion | dbname, id | Lesen eines Dokuments aus einer Datenbank. |
| `/whisk.system/cloudant/write` | Aktion | dbname, overwrite, doc | Schreiben eines Dokuments in eine Datenbank. |
| `/whisk.system/cloudant/changes` | Feed | dbname, filter, query_params, maxTriggers | Aktivieren eines Auslöserereignisses bei Änderungen an einer Datenbank. |
{: shortdesc}

In den folgenden Abschnitten wird schrittweise die Konfiguration eines zugeordneten Pakets sowie die Verwendung der Aktionen und Feeds im Paket `/whisk.system/cloudant` erläutert. Weitere Informationen zum Einrichten der {{site.data.keyword.cloudant_short_notm}}-Datenbank und zum Lesen oder Schreiben in dieser Datenbank finden Sie im Abschnitt [{{site.data.keyword.cloudant_short_notm}}-Aktionen](./cloudant_actions.html).

## Auslöser mithilfe der Filterfunktion erstellen

Mit dem Feed `changes` können Sie einen Service konfigurieren, der bei jeder Änderung an Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank einen Auslöser aktiviert. 

In diesem Beispiel werden die folgenden Parameter verwendet:

**dbname**: Der Name der {{site.data.keyword.cloudant_short_notm}}-Datenbank _(erforderlich)_.

**maxTriggers**: Stoppt die Aktivierung von Auslösern, wenn dieser Grenzwert erreicht wird _(optional)_. Standardwert: unbegrenzt.

**filter**: Die für ein Entwurfsdokument definierte Filterfunktion _(optional)_.

**query_params**: Zusätzliche Abfrageparameter für die Filterfunktion _(optional)_.

1. Erstellen Sie einen Auslöser mit dem Namen **myCloudantTrigger** mit dem Feed `changes` in der Paketbindung, die Sie zuvor erstellt haben. Schließen Sie dabei die Funktionen `filter` und `query_params` ein, um den Auslöser zu aktivieren, wenn ein Dokument hinzugefügt oder geändert wird, wenn der Status `new` ist.

  Achten Sie darauf, `/_/myCloudant` durch Ihren Paketnamen zu ersetzen.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. Starten Sie das Polling auf Aktivierungen, um einen Überblick über die aktuellen Abläufe zu erhalten.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Erstellen Sie eine Aktion, die Sie verwenden können, um die Auswirkungen des Feeds für Änderungen zu beobachten. Beispiel: Eine Aktion mit dem Namen **showCloudantChange**, die den folgenden JavaScript-Code enthält:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Erstellen Sie eine Regel, um die Aktion **showCloudantChange** mit dem zuvor erstellten Auslöser zu verbinden:
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Erstellen Sie Aktionen und eine Regel, um sie dem Auslöser **myCloudantTrigger** zuzuordnen.

6. Ändern Sie in Ihrem {{site.data.keyword.cloudant_short_notm}}-Dashboard ein vorhandenes Dokument oder erstellen Sie ein neues Dokument. Das Dokument muss über ein Feld _status_ verfügen, das auf **new** gesetzt ist.

7. Beobachten Sie die neuen Aktivierungen für den Auslöser **myCloudantTrigger** für jede Dokumentänderung mithilfe der Filterfunktion und des Abfrageparameters nur dann, wenn das Dokument den Status **new** hat.

Wenn Sie keine neuen Aktivierungen beobachten können, lesen Sie den [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html)-Thema über das Lesen und Schreiben in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank. Testen Sie die zum Lesen und Schreiben, um zu prüfen, ob Ihre {{site.data.keyword.cloudant_short_notm}}-Berechtigungsnachweise korrekt sind.
{: tip}

## Datenstruktur eines Auslöserereignisses

Der Inhalt der generierten Ereignisse weist die folgenden Parameter auf:

  - `id`: Die Dokument-ID.
  - `seq`: Die von {{site.data.keyword.cloudant_short_notm}} generierte Sequenz-ID.
  - `changes`: Ein Array von Objekten, die jeweils ein Feld `rev` haben, das die Revisions-ID des Dokuments enthält.

Die JSON-Darstellung des Auslöserereignisses sieht wie folgt aus:
```json
{
    "dbname": "testdb",
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

Sie können eine Filterfunktion definieren, um unnötige Änderungsereignisse zu verhindern, die Ihren Auslöser aktivieren.

Zum Erstellen einer neuen Filterfunktion können Sie eine Aktion verwenden.

Erstellen Sie eine JSON-Dokumentdatei `design_doc.json` mit der folgenden Filterfunktion:
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

Erstellen Sie ein Entwurfsdokument für die Datenbank mit der folgenden Filterfunktion:
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

Die Informationen für das neue Entwurfsdokument werden auf dem Bildschirm angezeigt:
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

## Aktionssequenz und Änderungsauslöser zur Verarbeitung eines Dokuments aus einer {{site.data.keyword.cloudant_short_notm}}-Datenbank verwenden
{: #openwhisk_catalog_cloudant_read_change notoc}

Sie können eine Aktionssequenz in einer Regel verwenden, um das Dokument, das einem {{site.data.keyword.cloudant_short_notm}}-Änderungsereignis zugeordnet ist, abzurufen und zu verarbeiten.

Beispielcode einer Aktion, die ein Dokument verarbeitet:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Erstellen Sie die Aktion, um das Dokument aus {{site.data.keyword.cloudant_short_notm}} zu verarbeiten:
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

Zum Lesen eines Dokuments aus der Datenbank können Sie die Aktion `read` aus dem {{site.data.keyword.cloudant_short_notm}}-Paket verwenden.
Die Aktion `read` kann mit `myAction` zusammengesetzt werden, um eine Aktionsfolge zu erstellen.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

Die Aktion `sequenceAction` kann in einer Regel verwendet werden, mit der die Aktion bei neuen {{site.data.keyword.cloudant_short_notm}}-Auslöserereignissen aktiviert wird.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Hinweis:** Der {{site.data.keyword.cloudant_short_notm}}-Auslöser `changes` hat in der Vergangenheit den Parameter `includeDoc` unterstützt, der nun nicht mehr unterstützt wird.

Sie können Auslöser, die zuvor mit `includeDoc` erstellt wurden, erneut erstellen. Führen Sie die folgenden Schritte aus, um den Auslöser erneut zu erstellen:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

Das Beispiel kann verwendet werden, um eine Aktionssequenz zu erstellen, um das geänderte Dokument zu lesen und die vorhandenen Aktionen aufzurufen. Denken Sie daran, Regeln zu inaktivieren, die nicht mehr gültig sind, und erstellen Sie neue Regeln mit dem Aktionssequenzmuster.
