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

# .NET Core アクションの作成
{: #dotnet-actions}

以下のセクションでは、単一 .NET Core アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

.NET Core プロジェクトのコンパイル、テスト、アーカイブ作成を実行するには、[.NET Core SDK](https://dotnet.microsoft.com/download) をローカル環境にインストールしておき、環境変数 `DOTNET_HOME` を `dotnet` 実行可能ファイルの配置場所に設定しておくことが必要です。

## .NET Core アクションの作成と呼び出し
{: #create-dotnet-action}

.NET Core アクションは、以下のシグニチャーを付けた `Main` というメソッドを組み込んだ .NET Core クラス・ライブラリーです。

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

.NET Core アクションを作成します。

1. `Apache.OpenWhisk.Example.Dotnet` という C# プロジェクトを作成します。

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. 以下のようにして、[Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet パッケージをインストールします。

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. 以下のコードを `Hello.cs` という名前のファイルに保存します。

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

4. `Hello.cs` と他のファイルをコンパイルして、`out` ディレクトリーに出力します。

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. パブリッシュしたファイルを圧縮します。

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. アクションを作成するか更新します。

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * `--main` 引数を使用して関数ハンドラーの名前を指定します。
    `main` の値は、以下の形式にする必要があります。
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    出力例:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
