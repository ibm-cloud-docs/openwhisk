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

# .NET Core 액션 작성
{: #dotnet-actions}

다음 절에서는 단일 .NET Core 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. 

.NET Core 프로젝트를 컴파일, 테스트 및 아카이브하려면 [.NET Core SDK](https://dotnet.microsoft.com/download)가 로컬로 설치되어 있어야 하며 환경 변수 `DOTNET_HOME`이 `dotnet` 실행 파일을 찾을 수 있는 위치로 설정되어 있어야 합니다. 

## .NET Core 액션 작성 및 호출
{: #create-dotnet-action}

.NET Core 액션은 다음과 같이 정확한 서명이 있는 `Main`이라는 메소드로 .NET Core 클래스 라이브러리입니다. 

```csharp
public Newtonsoft.Json.Linq.JObject Main(Newtonsoft.Json.Linq.JObject);
```
{: screen}

.NET Core 액션을 작성하십시오. 

1. `Apache.OpenWhisk.Example.Dotnet`이라는 C# 프로젝트를 작성하십시오.

    ```bash
    dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
    cd Apache.OpenWhisk.Example.Dotnet
    ```
    {: pre}

2. [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 패키지를 다음과 같이 설치하십시오. 

    ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
    {: pre}

3. 이름이 `Hello.cs`인 파일에 다음 코드를 저장하십시오. 

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

4. `Hello.cs` 및 기타 파일과 출력을 `out` 디렉토리로 컴파일하십시오.

    ```bash
    dotnet publish -c Release -o out
    ```
    {: pre}

5. 게시된 파일을 압축하십시오. 

    ```bash
    cd out
    zip -r -0 ../helloDotNet.zip *
    cd ..
    ```
    {: pre}

6. 액션을 작성 또는 업데이트하십시오. 

    ```bash
    ibmcloud fn action update helloDotNet helloDotNet.zip --main Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main --kind dotnet:2.2
    ```
    {: pre}

    * `--main` 인수를 사용하여 함수 핸들러의 이름을 지정하십시오.
    `main`의 값은 다음 형식이어야 합니다.
    `{Assembly}::{Class Full Name}::{Method}`, e.q.,
    `Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main`
    
7. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloDotNet --param name World
    ```
    {: pre}

    예제 출력. 

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
