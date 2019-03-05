---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

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

# Creating .NET Core actions
{: #dotnet-actions}

The following sections guide you through creating and invoking a single .NET Core action and adding parameters to that action.

In order to compile, test and archive .NET Core projects, you must have the [.NET Core SDK](https://dotnet.microsoft.com/download) installed locally and the environment variable `DOTNET_HOME` set to the location where the `dotnet` executable can be found.

## Creating and invoking a .NET Core action
{: #create-dotnet-action}

A .NET Core action is a .NET Core class library with a method called `Main` that has the exact signature as follows.

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

Create a .NET Core action.

1. Create a C# project called `Apache.OpenWhisk.Example.Dotnet`.

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. Install the [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet package as follows.

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. Save the following code in a file named `Hello.cs`.

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

4. Compile `Hello.cs` and any other files and output to `out` directory.

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. Compress the published files.

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. Create or Update the action.

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * Specify the name of the function handler using `--main` argument.
    The value for `main` needs to be in the following format.
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`


7. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    Example output.

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
