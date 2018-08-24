---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Services an Aktionen binden
{: #binding_services}

Sie können das [-CLI-Plug-in für {{site.data.keyword.openwhisk}}](./bluemix_cli.html) dazu verwenden, einen Service an eine Aktion oder ein Paket zu binden. Der {{site.data.keyword.openwhisk_short}}-Befehl `ibmcloud fn service bind` bewirkt, dass Ihre Berechtigungsnachweise für den {{site.data.keyword.Bluemix_notm}}-Service für Ihren {{site.data.keyword.openwhisk_short}}-Code zur Laufzeit verfügbar gemacht werden.
{: shortdesc}

Der Befehl `ibmcloud fn service bind` darf nicht mit dem Befehl `cf bind-service` verwechselt werden, der in Cloud Foundry verfügbar ist.
{: tip}

## Service an eine Aktion oder ein Paket binden
{: #cli_bind}

Sie können einen beliebigen {{site.data.keyword.Bluemix_notm}}-Service an eine beliebige Aktion binden, die in {{site.data.keyword.openwhisk_short}} definiert ist. Beim Binden eines Service wird ein neuer Parameter für Ihre vorhandene Aktion erstellt, der Berechtigungsnachweise für die Serviceinstanz enthält.

**Hinweis**: Sie können nur einen Service für jeden Typ an eine Aktion oder ein Paket binden. Das Binden mehrerer Services desselben Typs wird nicht unterstützt.

Bevor Sie beginnen, müssen Sie für den Service, den Sie binden wollen, [Berechtigungsnachweise definieren](/docs/apps/reqnsi.html#accser_external).

1. Rufen Sie den Namen der Serviceinstanz ab, die an eine Aktion oder ein Paket gebunden werden soll.
    ```
    ibmcloud service list
    ```
    {: pre}

    Beispielausgabe:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Rufen Sie den Namen der Berechtigungsnachweise ab, die für die Serviceinstanz definiert sind, die Sie im vorherigen Schritt erhalten haben.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Beispielausgabe:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Binden Sie den Service an eine Aktion.
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Informationen zu den Komponenten des Befehls <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu ein Komponenten des Befehls <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>Der Typ des Service, den Sie binden.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>Der Name der Aktion oder des Pakets, an die/das der Service gebunden werden soll.</td>
    </tr>
    <tr>
    <td>--instance <code>instanzname</code></td>
    <td>Optional: Geben Sie einen Namen für die Serviceinstanz an. Wenn Sie keinen Serviceinstanznamen angeben, wird die erste Instanz für den Service ausgewählt.</td>
    </tr>
    <tr>
    <td>--keyname <code>berechtigungsnachweisname</code></td>
    <td>Optional: Geben Sie einen Namen für den Berechtigungsnachweissatz an. Wenn Sie keinen Namen für den Berechtigungsnachweissatz angeben, wird der erste Berechtigungsnachweissatz für die Serviceinstanz ausgewählt.</td>
    </tr>
    </tbody></table>

    Geben Sie beispielsweise Folgendes ein, um einen {{site.data.keyword.ibmwatson}}-Service 'conversation' an eine Aktion mit dem Namen `hello` zu binden:
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Stellen Sie sicher, dass die Berechtigungsnachweise erfolgreich gebunden wurden. Die Aktion, an die der Service gebunden ist, unterstützt keine angepassten Flags. Sie unterstützt die Flags 'debug' und 'verbose'.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    Beispielausgabe:
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
                    "conversation": {
                        "password": "[Servicekennwort]",
                    "url": "[Service-URL]",
                    "username": "[Servicebenutzername]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In diesem Beispiel gehören die Berechtigungsnachweise für den Service 'conversation' - zusammen mit anderen Berechtigungsnachweisen für andere Servicetypen - zu einem Parameter mit dem Namen `__bx_creds`. Die Aktion sucht nach dem gebundenen Parameter `__bx_creds` und entfernt die Referenz auf den aufgelisteten Servicetyp. Wenn dieser Servicetyp der einzige aufgelistete Typ ist, füllt die Aktion den Wert des Parameters `__bx_creds` mit Nullen. Wenn mehr als ein Service an die Aktion gebunden ist, bleibt der Parameter `__bx_creds` mit den Services bestehen, die noch gebunden sind.

Weitere Informationen zum Übergeben von Parametern an eine Aktion oder ein Paket und zu den Auswirkungen auf Berechtigungsnachweise bei `update`-Operationen finden Sie im Abschnitt [Mit Parametern arbeiten](./parameters.html#pass-params-action).


## Bindung eines Service an eine Aktion oder ein Paket aufheben
{: #cli_unbind}

Sie können eine Bindung eines Service an eine Aktion oder ein Paket aufheben. Das Aufheben einer Servicebindung entfernt vorhandene Bindungen, die durch den Befehl `service bind` erstellt wurden.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
