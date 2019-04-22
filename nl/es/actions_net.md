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

# Creación de acciones .NET Core
{: #dotnet-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción .NET Core y la adición de parámetros a dicha acción.

Para poder compilar, probar y archivar proyectos .NET Core, debe tener el
[SDK de .NET Core](https://dotnet.microsoft.com/download) instalado de manera local y la variable de entorno
`DOTNET_HOME` establecida en la ubicación donde se encuentre el ejecutable `dotnet`.

## Creación e invocación de una acción .NET Core
{: #create-dotnet-action}

Una acción .NET Core es una biblioteca de clases de .NET Core con un método denominado `Main` que tiene la firma exacta que se indica a continuación.

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

Cree una acción .NET Core.

1. Cree un proyecto C# denominado `Apache.OpenWhisk.Example.Dotnet`.

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. Instale el paquete NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) tal como se indica a continuación.

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. Guarde el código siguiente en un archivo denominado `Hello.cs`.

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

4. Compile `Hello.cs` y cualquier otro archivo con salida en el directorio `out`.

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. Comprima los archivos publicados.

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. Cree o actualice la acción.

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * Especifique el nombre del manejador de función utilizando el argumento `--main`.
    El valor de `main` debe tener el formato siguiente.
    `{Assembly}::{Class Full Name}::{Method}`, por ejemplo,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    Salida de ejemplo.

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
