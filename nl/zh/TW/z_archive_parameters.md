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

# 使用參數

在無伺服器動作中，資料是藉由將參數新增至動作來提供。參數宣告為主要無伺服器函數的引數。
{: shortdesc}

您可以利用以下兩種方法來提供參數的值：
* **在呼叫期間將參數傳遞給動作**：在透過 CLI 旗標或透過檔案呼叫動作時提供參數。呼叫時所提供的參數會置換任何先前設定的預設參數。
* **將參數連結至動作或套件**：在建立或更新動作或套件時設定預設參數。若要讓資料在每次執行時都保持相同（相當於其他平台上的環境變數），或是針對可能會在呼叫時間被置換的預設值，此選項十分有用。

## 在呼叫期間將參數傳遞給動作
{: #pass-params-action}

呼叫動作時，可以將參數傳遞給動作。

1. 將下列程式碼儲存至名為 `hello.js` 的檔案中。

    ```javascript
       function main(params) {
           return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. 建立 `hello` 動作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 如果您先前曾使用此動作，請予以更新，以確保清除含有任何先前設定參數的動作。
    ```
  ibmcloud fn action update hello hello.js
  ```
    {: pre}

4. 傳遞 `name` 及 `place` 參數，以呼叫動作。
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **附註**：您可以改為傳遞 JSON 格式參數的檔案：
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    輸出範例：
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}

5. 您也可以將結構化物件中的參數傳遞給動作。例如，將 `hello` 動作更新如下：
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
    }
    ```
    {: codeblock}

    現在，動作預期單一 `person` 參數具有 `name` 及 `place` 欄位。

6. 使用本身為有效 JSON 物件的單一 `person` 參數來呼叫動作。
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    輸出範例：
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}

## 將參數連結至動作
{: #default-params-action}

您可以使用多個具名參數來呼叫動作。例如，基本 `hello` 動作預期有兩個參數：人員的 `name`，以及他們來自的 `place`。

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
}
```
{: screen}

您可以將預設參數連結至動作，而非每次都將所有參數傳遞給動作。下列步驟顯示如何將 `place` 參數連結至基本 `hello` 動作，以將動作預設為 "Kansas" 這個位置。

1. 將下列程式碼儲存至名為 `hello.js` 的檔案中。

    ```javascript
       function main(params) {
           return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. 建立 `hello` 動作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 使用 `--param` 旗標及鍵值組來更新連結參數值的動作。

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **附註**：您可以改為傳遞 JSON 格式參數的檔案：
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    如果您修改非服務認證參數，則執行含新參數的 `action update` 指令，會移除目前已存在但未指定在 `action update` 指令中的所有參數。例如，如果您執行 `action update -p key1 new-value -p key2 new-value`，但省略任何其他已設定的參數，則在更新動作之後，這些參數就不再存在。任何連結至動作的服務也會一併移除，因此，在更新其他參數之後，您必須重新[將服務連結至動作](/docs/openwhisk?topic=cloud-functions-binding_services)。
  {: tip}

4. 只傳遞 `name` 參數來呼叫動作。
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy
    ```
    {: pre}

    輸出範例：
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}

    因為您未在呼叫動作時指定 `place` 參數，所以會使用連結預設參數值 `Kansas`。

5. 在呼叫時指定參數值，可以改寫連結的參數。同時傳遞 `name` 及 `place` 來呼叫動作。
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
    ```
    {: pre}

    輸出範例：
    ```
    {
                "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}

## 將參數連結至套件
{: #default-params-package}

也可以在套件層次設定預設參數。連結參數作為套件中動作的預設參數，除非：

- 動作本身具有預設參數
- 動作的參數是在呼叫時間提供

下列範例在 `MyApp` 套件上設定 `name` 的預設參數，並顯示使用它的動作。

1. 建立套件，並為其連結預設參數 `name`。
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. 將下列程式碼儲存至名為 `helloworld.js` 的檔案中。

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
       }
    ```
    {: codeblock}

3. 在 `MyApp` 套件中建立動作。
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    如果您修改非服務認證參數，則執行含新參數的 `action update` 指令，會移除目前已存在但未指定在 `action update` 指令中的所有參數。例如，如果您執行 `action update -p key1 new-value -p key2 new-value`，但省略任何其他已設定的參數，則在更新動作之後，這些參數就不再存在。任何連結至動作的服務也會一併移除，因此，在更新其他參數之後，您必須重新[將服務連結至動作](/docs/openwhisk?topic=cloud-functions-binding_services)。
  {: tip}

3. 呼叫動作，但未傳遞任何參數。
    ```
    ibmcloud fn action invoke --result MyApp/hello
    ```
    {: pre}

    輸出範例：
    ```
       {
           "payload": "Hello, World"
       }
    ```
    {: screen}

    即會使用連結至套件的預設參數。

