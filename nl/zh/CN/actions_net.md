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

# 创建 .NET Core 操作
{: #dotnet-actions}

以下各部分将指导您创建并调用单个 .NET Core 操作，然后向该操作添加参数。

要编译、测试和归档 .NET Core 项目，必须在本地安装 [.NET Core SDK](https://dotnet.microsoft.com/download)，并且将环境变量 `DOTNET_HOME` 设置为可以找到 `dotnet` 可执行文件的位置。

## 创建并调用 .NET Core 操作
{: #create-dotnet-action}

.NET Core 操作是一个 .NET Core 类库，使用名为 `Main` 的方法，该方法的确切特征符如下所示。

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

创建 .NET Core 操作。

1. 创建名为 `Apache.OpenWhisk.Example.Dotnet` 的 C# 项目。

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. 安装 [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 包，如下所示。

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. 将以下代码保存在名为 `Hello.cs` 的文件中。

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

4. 编译 `Hello.cs` 和其他任何文件，然后输出到 `out` 目录。

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. 压缩发布的文件。

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. 创建或更新操作。

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * 使用 `--main` 自变量指定函数处理程序的名称。
    `main` 的值需要采用以下格式。
    `{Assembly}::{Class Full Name}::{Method}`，例如：`Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    示例输出。

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
