---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Services über Aktionen verwenden
{: #binding_services}

Sie können das [CLI-Plug-in für {{site.data.keyword.openwhisk}}](./bluemix_cli.html) dazu verwenden, einen Service an eine Aktion zu binden. {{site.data.keyword.openwhisk_short}} stellt den Befehl `service bind` bereit, um Ihre {{site.data.keyword.Bluemix}}-Serviceberechtigungsnachweise für Ihren Cloud Functions-Code zur Laufzeit verfügbar zu machen. Der Befehl `service bind` darf nicht mit dem Befehl `cf bind-service` verwechselt werden, der in Cloud Foundry verfügbar ist. Dieser Befehl ist lediglich eine automatisierte Methode, einen neuen Parameter für Ihre vorhandene Aktion zu erstellen, der Serviceberechtigungsnachweise enthält. Der {{site.data.keyword.openwhisk_short}}-Befehl `service bind` ist flexibler und ermöglicht Ihnen, einen beliebigen {{site.data.keyword.Bluemix_notm}}-Service an eine beliebige Aktion zu binden, die in {{site.data.keyword.openwhisk_short}} definiert ist. Der einzige Vorbehalt besteht darin, dass Sie über Berechtigungsnachweise für den Service verfügen müssen, den Sie binden möchten.
{: shortdesc}

## Service an eine Aktion binden
{: #cli_bind}

Binden Sie einen Service an eine Aktion mithilfe des Befehls `ic wsk service bind`, der von dem [{{site.data.keyword.openwhisk_short}}-CLI-Plug-in](./bluemix_cli.html) bereitgestellt wird. Weitere Informationen finden Sie im Abschnitt [Einschränkungen](./binding_services.html#limitations).

Syntax für die Verwendung von `bind`:
```
ic wsk service bind SERVICENAME AKTIONSNAME [--instance instanzname] [--keyname name]
```
{: pre}

Der Befehl `service bind` erfordert einen Servicetyp und einen Aktionsnamen, an die die Bindung erfolgt. Wenn Sie zum Beispiel einen Watson-Service 'conversation' an eine Aktion mit dem Namen `hello` binden wollen, könnte Ihr Aufruf ähnlich wie im folgenden Befehl aussehen:
```
ic wsk service bind conversation hello
```
{: pre}

Dadurch wird die folgende Ausgabe erzeugt:
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

Dieser Befehl durchsucht Ihren aktuellen Bereich nach vorhandenen Watson-Services 'conversation', nimmt den ersten gefundenen Service 'conversation' und ruft dann alle Berechtigungsnachweise ab, die zu diesem Service gehören. Dann bindet er die erste Gruppe von Berechtigungsnachweisen, die zu diesem Services gehören, als Parameter an die angegebene Aktion `hello`. Die Ausgabe zeigt genau, an welchen Service die Aktion gebunden wurde und welche Gruppe von Berechtigungsnachweisen aus diesem Service für die Bindung verwendet wurden.

Zur Überprüfung, ob die Berechtigungsnachweise erfolgreich gebunden wurden, führen Sie den folgenden Befehl aus:
```
ic wsk action get hello parameters
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

In dieser Ausgabe können Sie sehen, dass die Berechtigungsnachweise für diesen Service 'conversation' (zusammen mit anderen Berechtigungsnachweisen für andere Servicetypen) zu einem Parameter mit dem Namen `__bx_creds` gehören, der jetzt innerhalb des Aktionscodes wie jeder andere gebundene Parameter verwendet werden kann. Die Aktion wählt den ersten verfügbaren Service 'conversation' aus, der die erste Gruppe von Berechtigungsnachweisen in diesem Service enthält. 

Weitere Informationen zur Übergabe von Parametern an eine Aktion sowie dazu, wie sich die Ausführung einer Operation `action update` auf Berechtigungsnachweise auswirkt, finden Sie im Dokument [Aktionen erstellen und aufrufen](openwhisk_actions.html#openwhisk_pass_params).

Der Befehl `wsk service` unterstützt die folgenden beiden Flags:

<dl>
    <dt>--instance</dt>
    <dd>Der Name des bestimmten Service des Typs, den Sie verwenden möchten.</dd>
    <dt>--keyname</dt>
    <dd>Der Name der bestimmten Berechtigungsnachweise in dem Service, den Sie verwenden möchten.</dd>
</dl>

Betrachten Sie das folgende Beispiel, um sich mit der Verwendung dieser Flags vertraut zu machen. Nehmen Sie an, dass nach dem vorherigen Befehl `ic wsk service bind` tatsächlich zwei Services 'conversation' vorhanden sind und dass die Standardoperation für die Aktion den falschen Service und die falschen Berechtigungsnachweise gebunden hat. Sie könnten den Befehl erneut mit den Flags `--instance` und `--keyname` ausführen, um sicherzustellen, dass der richtige Service an die richtige Aktion gebunden wird. Sehen Sie sich zuerst an, welche Services verfügbar sind und welche Berechtigungsnachweise an sie gebunden sind. Eine Auflistung der vorhandenen Services könnte zum Beispiel wie die folgende aussehen:

```
ic service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

Dieser Ausgabe ist zu entnehmen, dass der Service **Conversation-qp** der erste von zwei aufgeführten Services ist und dass er derjenige ist, an den der erste Befehl `ic wsk service bind conversation hello` die Bindung durchgeführt hat. Sie möchten jetzt lieber die Bindung an den Service **Conversation-uc** durchführen. Um nun absolut sicher zu gehen, können Sie prüfen, welche Berechtigungsnachweise der Service **Conversation-uc** enthält, um sich zu vergewissern, dass Sie die richtige Gruppe von Berechtigungsnachweisen binden.

```
ic service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [Ihre_ID]...

name
Credentials-1
Credentials-2
```

Sie wollen die Bindung an die Berechtigungsnachweise mit dem Namen "Credentials-2" aus diesem Server durchführen. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass die Aktion das gewünschte Verhalten ausführt:
```
ic wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Dadurch wird die folgende Ausgabe erzeugt:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

In der Ausgabe können Sie sehen, dass die richtige Gruppe von Berechtigungsnachweisen an die Aktion gebunden wurden. Zur Prüfung können Sie wiederum den folgenden Befehl `ic wsk action get` ausführen.
```
ic wsk action get hello parameters
```
{: pre}

Dieser Befehl erzeugt die folgenden Ergebnisse:
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

Die normalen Debug-Flags werden unterstützt und geben Antwortheader von Aufrufen aus.

## Bindung eines Service an eine Aktion aufheben
{: #cli_unbind}

Die Bindung eines Service an eine Aktion wird mit dem Befehl `ic wsk service unbind` aufgehoben. Der Befehl `service unbind` entfernt vorhandene Bindungen, die durch den Befehl `service bind` erstellt wurden.

Syntax für die Verwendung von `unbind`:
```
ic wsk service unbind SERVICENAME AKTIONSNAME
```
{: pre}

## Einschränkungen
{: #limitations}

Die Aktion `service` unterstützt keine angepassten Flags. Sie unterstützt die üblichen Flags 'debug' und 'verbose'. Die Aktion sucht nach dem gebundenen Parameter `__bx_creds` und entfernt die Referenz auf den aufgelisteten Servicetyp. Wenn dieser Servicetyp der einzige aufgelistete Typ ist, füllt die Aktion den Wert des Parameters `__bx_creds` mit Nullen. Wenn mehr als ein Service an die Aktion gebunden ist, bleibt der Parameter `__bx_creds` mit den Services bestehen, die noch gebunden sind.

Sie können nur einen Service für jeden Typ an eine Aktion binden. Das Binden mehrerer Services desselben Typs innerhalb einer Aktion wird nicht unterstützt.
{: tip}

