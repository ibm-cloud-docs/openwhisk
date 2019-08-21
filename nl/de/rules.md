---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# Auslöser und Aktionen einander durch Regeln zuordnen
{: #rules}

Jedes Mal, wenn der Auslöser zur Anwendung kommt, verwendet die Regel das Auslöserereignis als Eingabe und ruft die zugehörige Aktion auf. Mit dem entsprechenden Satz von Regeln kann ein einziger Auslöser mehrere Aktionen aufrufen oder eine Aktion als Reaktion auf Ereignisse aus mehreren Auslösern aufgerufen werden.
{: shortdesc}


## Regelerstellung über die UI
{: #rules_ui}

Über die Benutzerschnittstelle (UI) wird eine Regel für die Zuordnung einer Aktion und eines Auslösers für Sie erstellt.
{: shortdesc}

Wenn Sie die Details für eine Aktion oder einen Auslöser erstellen oder auf diese zugreifen, haben Sie die Möglichkeit, eine vorhandene oder neue Aktion bzw. einen vorhandenen oder neuen Auslöser zu verbinden. Wenn Sie die Verbindung herstellen, wird eine Regel für Sie erstellt und erhält einen Namen im Format `ACTION_NAME-TRIGGER_NAME`.

Über die CLI können Sie `ibmcloud fn rule list` ausführen, um zu überprüfen, ob die Regel für Sie erstellt wurde.


## Regeln über die CLI erstellen
{: #rules_create}

Regeln werden dazu verwendet, einen Auslöser einer Aktion zuzuordnen. Jedes Mal, wenn ein Auslöserereignis aktiviert wird, wird die Aktion mit den Parametern des Auslöserereignisses aufgerufen.

Bevor Sie beginnen, erstellen Sie [eine Aktion](/docs/openwhisk?topic=cloud-functions-actions) und [einen Auslöser](/docs/openwhisk?topic=cloud-functions-triggers).


Erstellen Sie eine Regel, um einem Auslöser eine Aktion zuzuordnen. Regeln müssen direkt in einem Namensbereich erstellt werden. Ihre Erstellung in einem Paket ist nicht möglich.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


Zum Inaktivieren der Regel können Sie den folgenden Befehl ausführen.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## Regeln für Aktionssequenzen erstellen
{: #rules_seq}

Anhand von Regeln können Sie Auslöser mit Aktionssequenzen zuordnen

Bevor Sie beginnen, erstellen Sie [eine Aktionssequenz](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) und [einen Auslöser](/docs/openwhisk?topic=cloud-functions-triggers).

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## Mehrere Auslöser und Aktionen zuordnen
{: #rules_assoc}

Sie können verschiedene Kombinationen von Auslösern und Aktionen verwenden, indem Sie für jede Kombination eine Regel erstellen. Aktionen und Auslöser müssen einander nicht eins zu eins zugeordnet werden.

Stellen Sie sich zum Beispiel die folgenden Aktionen vor.

| Aktion | Beschreibung |
| --- | --- |
| `classifyImage` | Eine Aktion, die die Objekte in einem Bild erkennt und klassifiziert. |
| `thumbnailImage` | Eine Aktion, die eine Piktogrammversion eines Bilds erstellt. |

Nehmen Sie außerdem an, dass zwei Ereignisquellen die folgenden Auslöser aktivieren. 

| Auslöser | Beschreibung |
| --- | --- |
| `newTweet` | Ein Auslöser, der aktiviert wird, wenn ein neuer Tweet gepostet wird. |
| `imageUpload` | Ein Auslöser, der aktiviert wird, wenn ein Bild auf eine Website hochgeladen wird. |

Sie können Regeln so einrichten, dass ein einzelnes Auslöserereignis mehrere Aktionen aufruft und dass mehrere Auslöser dieselbe Aktion aufrufen.
- Regel `newTweet -> classifyImage`
- Regel `imageUpload -> classifyImage`
- Regel `imageUpload -> thumbnailImage`

Die drei Regeln sorgen für das folgende Verhalten:
- Bilder in beiden Tweets werden klassifiziert.
- Hochgeladene Bilder werden klassifiziert
- Eine Piktogrammversion wird generiert.

