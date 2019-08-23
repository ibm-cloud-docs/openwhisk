---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"

keywords: services, serverless, functions

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


# {{site.data.keyword.cloud_notm}}-Services an {{site.data.keyword.openwhisk_short}}-Entitäten binden
{: #services}

Sie können Funktionen von IBM Cloud-Services in Ihre {{site.data.keyword.openwhisk_short}}-App integrieren.
{: shortdesc}

**Wie füge ich IBM Cloud-Services zu meiner App hinzu?**

1. Sie können REST-API-Aufrufe in Ihrer App fest codieren. Diese Option ist möglicherweise der schnellste Weg zur Kommunikation mit einem IBM Cloud-Service.
2. Sie können ein bereits installiertes oder installierbares Paket verwenden, um die Funktionalität zu integrieren. Sie können die Aktionen und Feeds verwenden, die in den Paketen in Ihrem App-Code gespeichert sind. Diese Option könnte Ihren Code verschlanken, wenn Ihre App nahe an den Systemgrenzen liegt.


**Vorgehensweise zum Festlegen von Parametern, auf die meine App zugreifen muss?**

Diese Parameter können Werte enthalten, die bewirken, dass Ihre App mit anderen Daten wiederverwendbar ist, oder Werte, die für den Service erforderlich sind (wie z. B. Berechtigungsnachweise).  
1. Sie können Parameter in Ihrer App fest codieren. Diese Option ist aber keine sichere Vorgehensweise, was das Speichern vertraulicher Informationen (z. B. Berechtigungsnachweise) betrifft.
2. Sie können die Parameter an Ihre App binden, indem Sie sie an eine Aktion oder ein Paket binden.


## Service an eine Aktion oder ein Paket binden
{: #services_bind}

Sie können einen beliebigen {{site.data.keyword.cloud_notm}}-Service an eine beliebige Aktion binden. Wenn ein Service gebunden wird, wird ein neuer Parameter für Ihre vorhandene Aktion erstellt, der Berechtigungsnachweise für die Serviceinstanz enthält.

Es ist nicht möglich, mehrere Instanzen desselben Service an eine Aktion oder ein Paket zu binden. Sie können nur eine Instanz eines Service binden.
{: note}

Bevor Sie beginnen, müssen Sie [eine Aktion erstellen](/docs/openwhisk?topic=cloud-functions-actions) und für den Service, den Sie an die Aktion binden wollen, [Berechtigungsnachweise definieren](/docs/resources?topic=resources-externalapp#externalapp).

1. Rufen Sie den Namen des Service und der Serviceinstanz ab, der/die an eine Aktion oder ein Paket gebunden werden soll. In der Beispielausgabe ist `composer` der Service und `Composer-qp` der Name der Serviceinstanz.
    ```
    ibmcloud service list
    ```
    {: pre}

    **Beispielausgabe**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Rufen Sie den Namen der Berechtigungsnachweise ab, die für eine Serviceinstanz definiert sind.
    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **Beispiel**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Binden Sie den Service an eine Aktion. Der Befehl `ibmcloud fn service bind` macht Ihrem {{site.data.keyword.openwhisk_short}}-Code zur Laufzeit die {{site.data.keyword.cloud_notm}}-Serviceberechtigungsnachweise verfügbar. Die folgenden Befehlsparameter sind für den Befehl `ibmcloud fn service bind` verfügbar. 

    <table>
    <thead>
        <tr>
        <th>Parameter</th>
        <th>Beschreibung</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>Der Name des Service, den Sie binden.</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>Der Name der Aktion oder des Pakets, an die/das der Service gebunden werden soll.</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>(Optional) Geben Sie einen Serviceinstanznamen an. Wenn Sie keinen Serviceinstanznamen angeben, wird die erste Instanz für den Service ausgewählt.</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>(Optional) Geben Sie den Berechtigungsnachweisnamen an. Wenn Sie keinen Berechtigungsnachweisnamen angeben, werden die ersten Berechtigungsnachweise für die Serviceinstanz ausgewählt. </td>
        </tr>
    </tbody>
    </table>

    **Beispielsyntax**
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME][--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    Beispiel: Führen Sie den folgenden Befehl aus, um einen {{site.data.keyword.ibmwatson}} Composer-Service an eine Aktion mit dem Namen `hello` zu binden.
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **Ausgabe**
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Stellen Sie sicher, dass die Berechtigungsnachweise erfolgreich gebunden wurden. Die Aktion, an die der Service gebunden ist, unterstützt keine angepassten Flags. Sie unterstützt die Flags 'debug' und 'verbose'.

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got action Hello World
{
        "parameters": [
        {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In diesem Beispiel gehören die Berechtigungsnachweise für den Service 'Composer' - zusammen mit anderen Berechtigungsnachweisen für andere Servicetypen - zu einem Parameter mit dem Namen `__bx_creds`. Die Aktion sucht nach dem gebundenen Parameter `__bx_creds` und entfernt die Referenz auf den aufgelisteten Servicetyp. Wenn dieser Servicetyp der einzige aufgelistete Typ ist, füllt die Aktion den Wert des Parameters `__bx_creds` mit Nullen. Wenn mehr als ein Service an die Aktion gebunden ist, bleibt der Parameter `__bx_creds` mit den Services bestehen, die noch gebunden sind.

Weitere Informationen zum Übergeben von Parametern an eine Aktion oder ein Paket finden Sie unter [Parameter an Aktionen binden](/docs/openwhisk?topic=cloud-functions-actions#actions_params).

## Bindung von Services an Aktionen aufheben
{: #services_unbind}

Beim Aufheben der Bindung eines Service an eine Aktion oder ein Paket werden die bestehenden Servicebindungen entfernt.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

