---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: actions, serverless, .NET, net

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

# .NET-Core-Aktionen erstellen
{: #dotnet-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen .NET Core-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben. 

Um .NET Core-Projekte zu kompilieren, zu testen und zu archivieren, muss das [.NET Core-SDK](https://dotnet.microsoft.com/download) lokal installiert und die Umgebungsvariable `DOTNET_HOME` auf den Speicherort festgelegt sein, an dem sich die ausführbare `dotnet`-Datei befindet. 

## .NET-Core-Aktion erstellen und aufrufen
{: #create-dotnet-action}

Eine .NET Core-Aktion ist eine .NET Core-Klassenbibliothek mit einer Methode namens `Main`, die exakt die folgende Signatur aufweist. 

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

Erstellen Sie eine .NET Core-Aktion. 

1. Erstellen Sie ein C#-Projekt namens `Apache.OpenWhisk.Example.Dotnet`. 

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. Installieren Sie das [Newtonsoft.Json](https://www.newtonsoft.com/json)-NuGet-Paket wie folgt. 

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. Speichern Sie den folgenden Code in einer Datei namens `Hello.cs`. 

    ```csharp
    using System;
    using Newtonsoft.Json.Linq;

    namespace Apache.OpenWhisk.Example.Dotnet
    {
        public class Hello
        {
            public JObject Main(JObject args)
            {
                string name = "stranger";
                if (args.ContainsKey("name")) {
                    name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
        }
    }
    ```
    {: codeblock}

4. Kompilieren Sie `Hello.cs` und alle anderen Dateien und die Ausgabe im Verzeichnis `out`. 

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. Komprimieren Sie die veröffentlichten Dateien. 

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. Erstellen oder aktualisieren Sie die Aktion. 

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * Geben Sie den Namen des Funktionshandlers mit dem Argument `--main` an.
    Der Wert für `main` muss das folgende Format haben.
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    Beispielausgabe. 

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
