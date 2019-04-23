---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}

# Ballerina-Aktionen erstellen
{: #ballerina-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Ballerina-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben. Eine Aktion ist eine Ballerina-Funktion der höchsten Ebene, die ein JSON-Objekt akzeptiert und zurückgibt.  

Ballerina-Aktionen werden in Ballerina [0.990.2](https://ballerina.io/downloads) ausgeführt. Sie benötigen eine kompatible Version des Compilers, die lokal verfügbar ist, um die ausführbare Datei zu generieren. Ohne den Ballerina-Compiler können Sie keine Aktion erstellen. 

## Ballerina-Aktion erstellen und aufrufen

**Vorbereitende Schritte:** Erstellen Sie eine Datei namens `hello.bal` mit dem folgenden Quellcode. 

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie können diese Variable angeben, wenn Sie die Aktion über die Befehlszeilenschnittstelle `wsk` mit dem Flag `--main` erstellen.  

**Hinweis:** Der Ballerina-Compiler setzt voraus, dass eine Funktion namens `main` vorhanden ist, um die ausführbare Datei zu generieren, deshalb muss Ihre Quellendatei einen Platzhalter namens `main` enthalten. 

Führen Sie die folgenden Schritte aus, um eine Aktion namens `hello` zu erstellen. 

1. Generieren Sie die .balx-Datei. 
  ```
  ballerina build hello.bal
  ```
{: pre}

2. Erstellen Sie die Aktion mithilfe der .balx-Datei. 
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. Die Befehlszeilenschnittstelle leitet noch nicht den Typ der Aktion aus der Erweiterung der Quellendatei ab. Sie müssen den Typ explizit angeben. Für `.balx`-Quellendateien wird die Aktion mithilfe der Laufzeit von Ballerina 0.990.2 ausgeführt. 
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

Beispielausgabe. 
```json
{
  "greeting": "Hello World!"
      }
```
{: screen}
