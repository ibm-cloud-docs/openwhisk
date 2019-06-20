---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用参数

在无服务器操作中，通过向操作添加参数来提供数据。参数会声明为 main 无服务器函数的自变量。
{: shortdesc}

您可以通过两种方式为参数提供值：
* **调用期间将参数传递给操作**：通过 CLI 标志或通过文件来调用操作时提供参数。调用时提供的参数会覆盖先前设置的任何缺省参数。
* **将参数绑定到操作或包**：创建或更新操作或包时设置缺省参数。对于在每次执行时保持不变的数据、等效于其他平台上的环境变量或者在调用时可能会覆盖的缺省值，此选项非常有用。

## 在调用期间将参数传递给操作
{: #pass-params-action}

调用操作时，可以将参数传递给操作。

1. 将以下代码保存到名为 `hello.js` 的文件。

    ```javascript
function main(params) {
  return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
    {: codeblock}

2. 创建 `hello` 操作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 如果先前使用了此操作，请确保通过更新此操作来清除其所有先前设置的参数。
    ```
  ibmcloud fn action update hello hello.js
  ```
    {: pre}

4. 调用操作并传递 `name` 和 `place` 参数。
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
    {: pre}

    **注**：可以改为传递包含 JSON 格式参数的文件：
    ```
      ibmcloud fn action invoke --result hello --param-file parameters.json
  ```
    {: pre}

    示例输出：
      ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
  ```
    {: screen}

5. 还可以将结构化对象中的参数传递给操作。例如，将 `hello` 操作更新为以下内容：
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
    {: codeblock}

    现在，操作需要单个 `person` 参数具有 `name` 和 `place` 字段。

6. 使用作为有效 JSON 对象的单个 `person` 参数来调用操作。
    ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
    {: pre}

    示例输出：
      ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
  ```
    {: screen}

## 将参数绑定到操作
{: #default-params-action}

可以使用多个指定参数来调用操作。例如，基本 `hello` 操作需要两个参数：人员的 `name` 及其来自的 `place`。

```javascript
function main(params) {
  return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
{: screen}

您可以将缺省参数绑定到操作，而不用每次将所有参数传递给操作。以下步骤说明如何将 `place` 参数绑定到基本 `hello` 操作，以便操作的 place 缺省为“Kansas”。

1. 将以下代码保存到名为 `hello.js` 的文件。

    ```javascript
function main(params) {
  return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
    {: codeblock}

2. 创建 `hello` 操作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 更新操作，以使用 `--param` 标志和键/值对来绑定参数值。

    ```
  ibmcloud fn action update hello --param place Kansas
  ```
    {: pre}

    **注**：可以改为传递包含 JSON 格式参数的文件：
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    如果修改了非服务凭证参数，那么运行带新参数的 `action update` 命令将除去当前存在但未在 `action update` 命令中指定的任何参数。例如，如果运行 `action update -p key1 new-value -p key2 new-value`，但省略了已设置的其他任何参数，那么更新操作后这些参数将不再存在。还会除去已绑定到操作的任何服务，因此在更新其他参数后，必须再次[将服务绑定到操作](/docs/openwhisk?topic=cloud-functions-binding_services)。
  {: tip}

4. 调用操作，但只传递 `name` 参数。
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy
  ```
    {: pre}

    示例输出：
      ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
  ```
    {: screen}

    由于在调用操作时未指定 `place` 参数，因此将使用绑定的缺省参数值 `Kansas`。

5. 绑定的参数可以通过在调用时指定该参数值来进行覆盖。调用操作并传递 `name` 和 `place`。
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
    {: pre}

    示例输出：
      ```
    {
                "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}

## 将参数绑定到包
{: #default-params-package}

还可以在包级别设置缺省参数。绑定的参数充当包中操作的缺省参数，但以下情况例外：

- 操作本身具有缺省参数
- 操作具有在调用时提供的参数

以下示例对 `MyApp` 包设置缺省参数 `name`，并显示使用该参数的操作。

1. 创建包，并将缺省参数 `name` 绑定到该包。
    ```
  ibmcloud fn package update MyApp --param name World
  ```
    {: pre}

2. 将以下代码保存在名为 `helloworld.js` 的文件中。

    ```javascript
function main(params) {
  return {payload: "Hello, " + params.name};
     }
  ```
    {: codeblock}

3. 在 `MyApp` 包中创建操作。
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    如果修改了非服务凭证参数，那么运行带新参数的 `action update` 命令将除去当前存在但未在 `action update` 命令中指定的任何参数。例如，如果运行 `action update -p key1 new-value -p key2 new-value`，但省略了已设置的其他任何参数，那么更新操作后这些参数将不再存在。还会除去已绑定到操作的任何服务，因此在更新其他参数后，必须再次[将服务绑定到操作](/docs/openwhisk?topic=cloud-functions-binding_services)。
  {: tip}

3. 在不使用任何参数的情况下调用该操作。
```
  ibmcloud fn action invoke --result MyApp/hello
  ```
    {: pre}

    示例输出：
      ```
       {
           "payload": "Hello, World"
     }
  ```
    {: screen}

    这将使用绑定到包的缺省参数。

