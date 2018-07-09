---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 使用参数

了解如何对包和操作设置参数以进行部署，以及如何在调用期间将参数传递到操作。您还可以使用文件来存储参数并将文件名传递到操作，而不在命令行上单独提供每个参数。
{: shortdesc}

使用无服务器操作时，数据通过将参数添加到操作来提供，参数会声明为 main 无服务器函数的自变量。所有数据都以这种方式提供，并且值可以通过几种不同的方式来设置。第一个选项是在创建（或更新）操作或包时提供参数。对于在每次执行时保持不变的数据、等效于其他平台上的环境变量或者在调用时可能会覆盖的缺省值，此选项非常有用。第二个选项是在调用操作时提供参数，这些参数会覆盖先前设置的任何参数。

## 在调用期间将参数传递给操作
{: #pass-params-action}

调用操作时，可以将参数传递给操作。提供的示例使用的是 JavaScript，但其他所有语言的工作方式都相同。要查看详细示例，请查看以下有关 [JavaScript 操作](./openwhisk_actions.html#creating-and-invoking-javascript-actions)、[Swift 操作](./openwhisk_actions.html#creating-swift-actions)、[Python 操作](./openwhisk_actions.html#creating-python-actions)、[Java 操作](./openwhisk_actions.html#creating-java-actions)、[PHP 操作](./openwhisk_actions.html#creating-php-actions)、[Docker 操作](./openwhisk_actions.html#creating-docker-actions)或 [Go 操作](./openwhisk_actions.html#creating-go-actions)的主题。

1. 在操作中使用参数。例如，创建名为 **hello.js** 的文件并包含以下内容：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  输入参数会作为 JSON 对象参数传递给 **main** 函数。请注意此示例中的 `name` 和 `place` 参数是如何从 `params` 对象检索到的。

2. 更新 **hello** 操作以使其可供使用：
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

  如果修改了非服务凭证参数，那么运行带新参数的 `action update` 命令将除去当前存在但未在 `action update` 命令中指定的任何参数。例如，如果运行 `action update -p key1 new-value -p key2 new-value`，但省略了已设置的其他任何参数，那么更新操作后这些参数将不再存在。还会除去已绑定到操作的任何服务，因此在更新其他参数后，必须再次[将服务绑定到操作](./binding_services.html)。
  {: tip}

3. 参数可以使用命令行显式提供，也可以通过[提供文件](./parameters.html#using-parameter-files)（包含所需参数）来提供。

  要直接通过命令行来传递参数，请向 `--param` 标志提供键/值对：
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **响应：**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  使用 `--result` 选项时请注意：此选项暗含 CLI 等待激活完成并随后仅显示激活结果的阻塞性调用。为了方便起见，使用此选项时可不带 `--blocking`，系统会自动推断出阻塞性。

  此外，如果在命令行上指定的参数值是有效的 JSON，那么会将它们解析为结构化对象并发送到操作。

  例如，将 **hello** 操作更新为以下内容：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  现在，操作期望单个 `person` 参数具有 `name` 和 `place` 字段。

  接下来，使用作为有效 JSON 的单个 `person` 参数调用操作，如以下示例中所示：
  ```
  ibmcloud wsk action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **响应：**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  结果是相同的，因为 CLI 会自动将 `person` 参数值解析为操作现在期望的结构化对象。

## 对操作设置缺省参数
{: #default-params-action}

操作可以通过多个指定参数进行调用。重新调用上面示例中的 **hello** 操作需要两个参数：*name*（人员的姓名）和 *place*（人员所在位置）。

您可以绑定特定参数，而不用每次将所有参数传递给操作。以下示例绑定 *place* 参数，以便操作的缺省位置为“Kansas”：

1. 更新该操作，方法是使用 `--param` 选项来绑定参数值，或者将包含参数的文件传递到 `--param-file`。有关使用文件的示例，请参阅有关[使用参数文件](./parameters.html#using-parameter-files)的部分。

  要在命令行上显式指定缺省参数，请向 `param` 标志提供键/值对：
  ```
  ibmcloud wsk action update hello --param place Kansas
  ```
  {: pre}

2. 调用操作，但这次只传递 `name` 参数。
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  请注意，调用操作时，无需指定 place 参数。绑定的参数仍可以通过在调用时指定该参数值来进行覆盖。



3. 调用操作并传递 `name` 和 `place` 值，然后观察以下输出：

  使用 `--param` 标志调用操作：
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  示例输出：
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  创建或更新操作时对操作设置的参数始终会被直接在调用时提供的参数覆盖。
  {: tip}

## 对包设置缺省参数
{: #default-params-package}

参数可以在包级别进行设置，并充当操作的缺省参数，_除非_有以下情况：

- 操作本身具有缺省参数。
- 操作具有在调用时提供的参数，当有多个参数可用时，此参数始终“优先”。

以下示例对 **MyApp** 包设置缺省参数 `name`，并显示使用该参数的操作。

1. 创建设置了参数的包：

  ```
  ibmcloud wsk package update MyApp --param name World
  ```
  {: pre}

2. 在 **MyApp** 包中创建操作：
  ```javascript
  function main(params) {
      return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  创建操作：
  ```
  ibmcloud wsk action update MyApp/hello hello.js
  ```
  {: pre}

3. 调用操作，并观察正在使用的缺省包参数：
  ```
  ibmcloud wsk action invoke --result MyApp/hello
  ```
  {: pre}

  示例输出：
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## 使用参数文件
{: #using-parameter-files}

可以将参数放入 JSON 格式的文件中，然后通过使用 `--param-file` 标志提供文件名来传入参数。此方法可用于包和操作创建（或更新），以及在操作调用期间使用。

1. 例如，考虑早先的 **hello** 示例，但这次使用包含以下内容的 `hello.js`：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 使用更新的 `hello.js` 的内容来更新操作：

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. 创建包含 JSON 格式参数的名为 `parameters.json` 的参数文件：

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. 调用 **hello** 操作时使用 `parameters.json` 文件名，并观察输出：

  ```
  ibmcloud wsk action invoke --result hello --param-file parameters.json
  ```

  示例输出：
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
