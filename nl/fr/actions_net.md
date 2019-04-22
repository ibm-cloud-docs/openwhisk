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

# Création d'actions .NET Core
{: #dotnet-actions}

Les sections ci-après vous expliquent comment créer et appeler une action .NET Core unique, et comment ajouter des paramètres à cette action. 

Afin de compiler, tester et archiver des projets .NET Core, le [logiciel SDK .NET Core](https://dotnet.microsoft.com/download) doit être installé localement et la variable
d'environnement `DOTNET_HOME` doit être définie sur l'emplacement où se trouve l'exécutable `dotnet`. 

## Création et appel d'une action .NET Core
{: #create-dotnet-action}

Une action .NET Core est une bibliothèque de classe .NET Core avec une méthode appelée `Main` qui comporte la signature exacte comme suit. 

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

Créez une action .NET Core. 

1. Créez un projet C# appelé `Apache.OpenWhisk.Example.Dotnet`. 

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. Installez le package NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) comme suit. 

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. Sauvegardez le code suivant dans un fichier nommé `Hello.cs`. 

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

4. Compilez `Hello.cs` ainsi que d'autres fichiers et sorties dans le répertoire `out`. 

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. Compressez les fichiers publiés. 

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. Créez ou mettez à jour l'action. 

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * Indiquez le nom du gestionnaire de fonctions à l'aide de l'argument `--main`.
    La valeur pour `main` doit être au format suivant :
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    Exemple de sortie : 

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
