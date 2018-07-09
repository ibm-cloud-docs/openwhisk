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

# 使用參數

瞭解如何在套件和動作上設定參數以進行部署，以及如何在呼叫期間將參數傳遞給動作。您也可以使用檔案來儲存參數，並將檔名傳遞給動作，而不是在指令行上個別提供每一個參數。
{: shortdesc}

使用無伺服器動作，資料是藉由將參數新增至動作來提供，而這些動作宣告為主要無伺服器函數的引數。所有資料都是透過這種方式取得，而值可能透過數種不同的方式設定。第一個選項是在建立（或更新）動作或套件時提供參數。若要讓資料在每次執行時都保持相同（相當於其他平台上的環境變數），或是針對可能會在呼叫時間被置換的預設值，此選項十分有用。第二個選項是在呼叫動作時提供參數，而此動作會置換先前設定的所有參數。

## 在呼叫期間將參數傳遞給動作
{: #pass-params-action}

呼叫動作時，可以將參數傳遞給動作。所提供的範例使用 JavaScript，但所有其他語言的運作方式都相同。若要查看詳細範例，請參閱下列主題：[Javascript 動作](./openwhisk_actions.html#creating-and-invoking-javascript-actions)、[Swift 動作](./openwhisk_actions.html#creating-swift-actions)、[Python 動作](./openwhisk_actions.html#creating-python-actions)、[Java 動作](./openwhisk_actions.html#creating-java-actions)、[PHP 動作](./openwhisk_actions.html#creating-php-actions)、[Docker 動作](./openwhisk_actions.html#creating-docker-actions)或 [Go 動作](./openwhisk_actions.html#creating-go-actions)。

1. 在動作中使用參數。例如，建立名為 **hello.js** 且含有下列內容的檔案：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  輸入參數被當作 **main** 函數的 JSON 物件參數來傳遞。請注意，在此範例中，`name` 及 `place` 參數擷取自 `params` 物件的方式。

2. 更新 **hello** 動作，以供使用：
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

  如果您修改非服務認證參數，則執行含新參數的 `action update` 指令，會移除目前已存在但未指定在 `action update` 指令中的所有參數。例如，如果您執行 `action update -p key1 new-value -p key2 new-value`，但省略任何其他已設定的參數，則在更新動作之後，這些參數就不再存在。任何連結至動作的服務也會一併移除，因此，在更新其他參數之後，您必須重新[將服務連結至動作](./binding_services.html)。
  {: tip}

3. 使用指令行或[提供檔案](./parameters.html#using-parameter-files)（內含所需參數)，即可明確地提供參數。

  若要透過指令行直接傳遞參數，請提供一對索引鍵/值給 `--param` 旗標：
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **回應：**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  請注意，使用 `--result` 選項：這表示 CLI 等待呼叫完成後僅顯示結果的區塊處理呼叫。為方便起見，可以在沒有自動推斷的 `--blocking` 的情況下使用此選項。

  此外，如果指令行上指定的參數值是有效的 JSON，則會對其進行剖析，並以結構化物件形式將其傳送至動作。

  例如，將 **hello** 動作更新如下：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  現在，動作預期單一 `person` 參數具有欄位 `name` 及 `place`。

  接下來，使用本身為有效 JSON 的單一 `person` 參數來呼叫動作，如下列範例所示：
  ```
  ibmcloud wsk action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **回應：**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  結果會相同，因為 CLI 會自動將 `person` 參數值剖析為動作現在預期的結構化物件。

## 設定動作的預設參數
{: #default-params-action}

您可以使用多個具名參數來呼叫動作。請記住，前一個範例中的 **hello** 動作預期會有兩個參數：人員的名稱 (*name*) 及其來源位置 (*place*)。

您可以連結特定參數，而非每次都將所有參數傳遞給動作。下列範例會連結 *place* 參數，以將動作預設為 "Kansas" 這個位置：

1. 更新動作，方法為使用 `--param` 選項來連結參數值，或將包含參數的檔案傳遞至 `--param-file`（如需使用檔案的範例，請參閱有關[使用參數檔案](./parameters.html#using-parameter-files)的小節）。

  若要在指令行上明確地指定預設參數，請將鍵值組提供給 `param` 旗標：
  ```
  ibmcloud wsk action update hello --param place Kansas
  ```
  {: pre}

2. 這次只傳遞 `name` 參數，來呼叫動作。
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy
  ```
  {: pre}

  輸出範例：
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

    請注意，呼叫動作時，您不需要指定 place 參數。在呼叫時指定參數值，仍然可以改寫連結的參數。

3. 同時傳遞 `name` 和 `place` 值來呼叫動作，並觀察輸出：

  使用 `--param` 旗標來呼叫動作：
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  輸出範例：
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  在建立或更新動作時於動作上所設定的參數，一律會置換為在呼叫時直接提供的參數。
  {: tip}

## 設定套件的預設參數
{: #default-params-package}

您可以在套件層次設定參數，並將其作為動作 _unless_ 的預設參數：

- 動作本身具有預設參數。
- 動作的參數是在呼叫時間所提供，當有多個參數可用時，一律會「優先」使用此參數。

下列範例在 **MyApp** 套件上設定 `name` 的預設參數，並顯示使用它的動作。

1. 建立已設定參數的套件：

  ```
  ibmcloud wsk package update MyApp --param name World
  ```
  {: pre}

2. 在 **MyApp** 套件中建立動作：
  ```javascript
  function main(params) {
      return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  建立動作：
  ```
  ibmcloud wsk action update MyApp/hello hello.js
  ```
  {: pre}

3. 呼叫動作，並觀察使用中的預設 package 參數：
  ```
  ibmcloud wsk action invoke --result MyApp/hello
  ```
  {: pre}

  輸出範例：
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## 使用參數檔案
{: #using-parameter-files}

您可以將參數放入 JSON 格式的檔案中，然後使用 `--param-file` 旗標提供檔名來傳入參數。此方法可以用於建立（或更新）套件和動作，以及呼叫動作期間。

1. 舉例來說，請考慮使用先前使用具有下列內容之 `hello.js` 的 **hello** 範例：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 使用更新過的 `hello.js` 內容來更新動作：

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. 建立稱為 `parameters.json` 且包含 JSON 格式化參數的參數檔案：

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. 在呼叫 **hello** 動作時使用 `parameters.json` 檔名，並觀察輸出：

  ```
  ibmcloud wsk action invoke --result hello --param-file parameters.json
  ```

  輸出範例：
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
