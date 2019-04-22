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

# 建立 .NET Core 動作
{: #dotnet-actions}

下列各節會引導您建立及呼叫單一 .NET Core 動作，並且將參數新增至該動作。

為了能夠編譯、測試及保存 .NET Core 專案，您必須在本端安裝 [.NET Core SDK](https://dotnet.microsoft.com/download)，並將環境變數 `DOTNET_HOME` 設為可找到 `dotnet` 執行檔的位置。

## 建立及呼叫 .NET Core 動作
{: #create-dotnet-action}

.NET Core 動作是具有 `Main` 方法的 .NET Core 類別庫，而此方法具有確切的簽章，如下所示。

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

建立 .NET Core 動作。

1. 建立稱為 `Apache.OpenWhisk.Example.Dotnet` 的 C# 專案。

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. 安裝 [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 套件，如下所示。

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. 將下列程式碼儲存至名為 `Hello.cs` 的檔案中。

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

4. 編譯 `Hello.cs` 及任何其他檔案，並輸出至 `out` 目錄。

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. 壓縮已發佈的檔案。

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. 建立或更新動作。

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * 使用 `--main` 引數來指定函數處理程式的名稱。`main` 的值需要使用下列格式。
    `{Assembly}::{Class Full Name}::{Method}`，例如，`Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    輸出範例。

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
