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

# Criando ações .NET Core
{: #dotnet-actions}

As seções a seguir orientam você na criação e na chamada de uma ação .NET Core única e na inclusão de parâmetros nessa ação.

Para compilar, testar e arquivar projetos .NET Core, deve-se ter o [.NET Core SDK](https://dotnet.microsoft.com/download) instalado localmente e a variável de ambiente `DOTNET_HOME` configurada para o local no qual o executável `dotnet` pode ser localizado.

## Criando e chamando uma ação do .NET Core
{: #create-dotnet-action}

Uma ação do .NET Core é uma biblioteca de classes .NET Core com um método chamado `Main` que tem a assinatura exata conforme a seguir.

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

Crie uma ação do .NET Core.

1. Crie um projeto C# chamado `Apache.OpenWhisk.Example.Dotnet`.

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. Instale o pacote NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) conforme a seguir.

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. Salve o código a seguir em um arquivo denominado `Hello.cs`.

    ```csharp
    usando System;
    usando Newtonsoft.Json.Linq;

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

4. Compile `Hello.cs`e quaisquer outros arquivos e saída para o diretório `out`.

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. Compacte os arquivos publicados.

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. Crie ou atualize a ação.

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * Especifique o nome do manipulador de função usando o argumento `--main`.
    O valor para `main` precisa estar no formato a seguir.
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    Saída de exemplo.

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
