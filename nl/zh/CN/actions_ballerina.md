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

# 创建 Ballerina 操作
{: #ballerina-actions}

以下各部分将指导您创建并调用单个 Ballerina 操作，然后向该操作添加参数。操作是顶级 Ballerina 函数，可接受并返回 JSON 对象。 

Ballerina 操作使用 Ballerina [0.990.2](https://ballerina.io/downloads) 执行。您需要本地可用的兼容版本编译器，才能生成可执行文件。如果没有 Ballerina 编译器，那么无法创建操作。

## 创建并调用 Ballerina 操作

**开始之前：**使用以下源代码创建名为 `hello.bal` 的文件。

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

缺省情况下，操作的入口方法为 `main`。使用 `wsk` CLI 通过 `--main` 创建操作时，可以指定此变量。 

**注：**Ballerina 编译器需要有名为 `main` 的函数才能生成可执行文件，因此源文件必须包含名为 `main` 的占位符。

要创建名为 `hello` 的操作，请完成以下步骤。

1. 生成 .balx 文件。
  ```
  ballerina build hello.bal
  ```
{: pre}

2. 使用 .balx 文件来创建操作。
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. CLI 尚无法根据源文件扩展名来确定操作的类型。必须显式指定类型。对于 `.balx` 源文件，操作会使用 Ballerina 0.990.2 运行时来运行。
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

示例输出。
```json
{
  "greeting": "Hello World!"
      }
    ```
{: screen}
