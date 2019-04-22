---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

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


# 建立 JavaScript 動作
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

下列各節會引導您透過 JavaScript 逐步執行動作。首先，請建立並呼叫簡單的動作。然後，在動作中新增參數，並使用參數來呼叫該動作。接下來，設定並呼叫預設參數。最後，建立非同步動作。
{: shortdesc}

## 建立及呼叫簡單 JavaScript 動作
{: #single_action_js}
{: #openwhisk_single_action_js}

請檢閱下列步驟及範例，以建立您的第一個 JavaScript 動作。

1. 將下列程式碼儲存至名為 `hello.js` 的檔案中。

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

    JavaScript 檔可能包含其他函數。不過，依慣例，必須要有稱為 `main` 的函數，以提供動作的進入點。
  {: shortdesc}

2. 使用下列 JavaScript 函數建立稱為 `hello` 的動作。

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  輸出範例：
  
  ```
  ok: created action hello
  ```
  {: screen}

  動作的類型是使用來源副檔名來決定。對於 `.js` 原始檔，動作會使用 Node.js 運行環境來執行。您可以將 `--kind` 參數設為 `nodejs:10` 或 `nodejs:8`，以指定 JavaScript 動作的 Node.js 運行環境版本。如需 Node.js 運行環境的相關資訊，請參閱[運行環境](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)。
  {: shortdesc}

3. 驗證您的 `hello` 動作在動作清單中。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  輸出範例：
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. 執行區塊處理呼叫，在雲端執行動作。區塊處理呼叫會使用要求/回應樣式，並等待啟動結果可供使用。等待期間少於 60 秒，或是動作的[時間限制值](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)。

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    指令會輸出下列資訊：
        * 啟動 ID (`44794bd6aab74415b4e42a308d880e5b`)，這可以用來擷取日誌或呼叫結果。
        * 如果在預期的等待期間內有呼叫結果，則為呼叫結果

    ```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
        "status": "success",
        "success": true
    }
    ```
    {: screen}
    
    如需監視啟動的提示，請參閱[監視動作輸出](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output)。
    {: tip}

5. 如果您不是立即需要動作結果，則可以省略 `--blocking` 旗標，以執行非區塊處理呼叫。

    1. 執行非區塊處理呼叫。
        

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        輸出範例：

        ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
        {: screen}

    2. 使用啟動 ID 來取得動作結果。
        

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        會傳回動作結果：
        

        ```
        {
            "payload": "Hello world"
        }
        ```
        {: screen}

6. 如果您忘記記錄啟動 ID，則可以取得從最近到最舊排列的啟動清單。
    

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    輸出：
        ```
    activations
    44794bd6aab74415b4e42a308d880e5b         hello
    6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### JavaScript 函數原型
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript 動作是在 Node.js 運行環境中執行。

以 JavaScript 撰寫的動作必須限制於單一檔案。此檔案可以包含多個函數，但依慣例，必須要有一個稱為 `main` 的函數，並且在呼叫動作時呼叫此函數。例如，下列範例顯示具有多個函數的動作。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

動作輸入參數被當作 `main` 函數的 JSON 物件參數來傳遞。成功啟動的結果也是 JSON 物件，但會根據動作是同步還是非同步（如下節所述），而以不同的方式傳回。

### 同步及非同步行為
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 函數很常會在回呼函數中繼續執行，即使是函數返回之後也是一樣。為了適應此行為，JavaScript 動作的啟動可以是*同步* 或*非同步*。

如果 main 函數在下列其中一種狀況下結束，則 JavaScript 動作的啟動是**同步**：

- main 函數結束，而未執行 `return` 陳述式。
- main 函數結束的原因為執行會傳回任何值（Promise *除外*）的 `return` 陳述式。

請參閱下列同步動作範例：

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

如果 main 函數因為傳回 Promise 而結束，則 JavaScript 動作的啟動為**非同步**。在此情況下，系統會假設動作仍在執行中，直到履行或拒絕 Promise。
首先，開始實例化新 Promise 物件，並將回呼函數傳遞給它。此回呼接受兩個引數（resolve 及 reject），而這兩者同時也是函數。所有非同步程式碼都在該回呼內。

在下列範例中，您可以呼叫 resolve 函數來查看如何完成 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
      }
```
{: codeblock}

這個範例顯示如何呼叫 reject 函數來拒絕 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            reject({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

在某些輸入上，動作可能會同步，而在其他輸入上，動作可能會非同步，如下列範例所示。
```javascript
  function main(params) {
      if (params.payload) {
         // asynchronous activation
         return new Promise(function(resolve, reject) {
                setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
      } else {
// synchronous activation
         return {done: true};
      }
  }
```
{: codeblock}

不論啟動是同步還是非同步，動作的呼叫都可以是區塊處理或非區塊處理。

## 建立非同步動作
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

在您的動作中傳回 Promise，非同步執行的 JavaScript 函數便可在 `main` 函數返回之後，傳回啟動結果。

1. 將下列程式碼儲存至名為 `asyncAction.js` 的檔案中。
    

    ```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
           setTimeout(function() {
             resolve({ done: true });
           }, 2000);
        })
     }
    ```
    {: codeblock}

    * `main` 函數會傳回 Promise。Promise 表示啟動尚未完成，但預期未來可完成。

    * `setTimeout()` JavaScript 函數會等待 2 秒，再呼叫 Promise 的回呼函數，這會呈現非同步程式碼。

    * Promise 的回呼接受引數 `resolve` 和 `reject`，它們都是函數。

      * `resolve()` 呼叫可滿足 Promise，並指出啟動正常完成。
      * `reject()` 呼叫可以用來拒絕 Promise，並發出信號指出啟動異常完成。

2. 建立稱為 `asyncAction` 的動作。
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    輸出範例：

    ```
    {
        "done": true
    }
    ```
    {: screen}

4. 取得啟動日誌，以檢查啟動需要多久時間才能完成。

  1. 取得啟動 ID。
      

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      輸出範例：
      ```
      activations
      b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. 取得啟動 ID 的日誌。
      

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration` 顯示這項啟動需要稍微超過 2 秒才能完成：
      

      ```
ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## 使用動作來呼叫外部 API
{: #apicall_action}
{: #openwhisk_apicall_action}

到目前為止，這些提供的範例自行包含 JavaScript 函數。您也可以建立呼叫外部 API 的動作。
{: shortdesc}

下列範例呼叫「NASA 每日一天文圖 (APOD)」服務，每日提供一張宇宙的獨特影像。

1. 將下列程式碼儲存至名為 `apod.js` 的檔案中。
    

    ```javascript
    let rp = require('request-promise')

      function main(params) {
      const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then(res => {
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. 呼叫 NASA APOD API，即會從 JSON 結果擷取欄位。如需您可以在動作中使用之 Node.js 套件的相關資訊，請參閱[運行環境](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)。

3. 建立稱為 `apod` 的動作。
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. 呼叫 `apod` 動作。
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    會傳回下列範例物件：
    

    ```
    {
      "copyright": "Eric Houck",
      "date": "2018-03-28",
      "explanation": "Does an alignment like this occur only once in a blue moon? ...",
      "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
      "media_type": "image",
      "service_version": "v1",
      "title": "Blue Moon Tree",
      "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
    }
    ```
    {: screen}

## 將動作包裝為 Node.js 模組
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

使用單一 JavaScript 原始檔撰寫所有動作碼的替代方案，是您可以將動作撰寫為 `npm` 套件。

例如，請考慮具有下列檔案的目錄：

1. 將下列程式碼儲存至名為 `package.json` 的檔案中。

    ```json
    {
      "name": "my-action",
      "main": "index.js",
      "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. 將下列程式碼儲存至名為 `index.js` 的檔案中。

    ```javascript
function myAction(args) {
    const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * 此動作透過 `exports.main` 來公開。
    * 動作處理程式可以有任何名稱，只要符合接受物件以及傳回物件的慣用簽章（或物件的 `Promise`）即可。
    * 您必須將這個檔案命名為 `index.js`，或是將您喜好的檔名指定為 `package.json` 中的 `main` 內容。

3. 在本端安裝所有相依關係。
    

    ```
    npm install
    ```
    {: pre}

    **附註：**雖然大部分 `npm` 套件都會在 `npm install` 上安裝 JavaScript 原始檔，但是有一部分也會安裝及編譯二進位構件。保存檔上傳只支援 JavaScript 相依關係。如果保存檔包括二進位相依關係，則動作呼叫可能會失敗。

4. 建立包含所有檔案的 `.zip` 保存檔，包括所有相依關係。
    

    ```
    zip -r action.zip *
    ```
    {: pre}

    使用「Windows 檔案總管」動作來建立 zip 檔案會導致結構不正確。{{site.data.keyword.openwhisk_short}} .zip 動作必須將 `package.json` 置於 zip 的根目錄，而「Windows 檔案總管」會將它放在巢狀資料夾內。最安全的選項是使用指令行 `zip` 指令。
  {: tip}

5. 建立動作。當您從 `.zip` 保存檔建立動作時，必須設定 `--kind` 參數的值，以指定 Node.js 運行環境版本。選擇 `nodejs:8` 或 `nodejs:10`。

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    輸出範例：

    ```
    {
        "padded": [
            ".......................and now",
            "......for something completely",
            ".....................different"
        ]
    }
    ```
    {: screen}

## 將動作包裝為單一組合
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

如果將動作包裝成 .zip 包括太多不必要的檔案，或您需要更快的部署，則可以將最少的程式碼寫入包含相依關係的單一 `.js` 檔案。
{: shortdesc}

您可以使用 JavaScript 模組組合程式，例如 [webpack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://webpack.js.org/concepts/) 來包裝動作。當 `webpack` 處理您的程式碼時，它會遞迴地建置相依關係圖形，其中包含您的動作所需的每個模組。

1. 將下列程式碼儲存至名為 `package.json` 的檔案中。`webpack` 被新增為開發相依關係。

    ```json
    {
      "name": "my-action",
      "main": "dist/bundle.js",
      "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
      "dependencies": {
        "left-pad": "1.1.3"
      },
      "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. 將下列 webpack 配置程式碼儲存至名為 `webpack.config.js` 的檔案中。
    

    ```javascript
var path = require('path');
module.exports = {
  entry: './index.js',
      output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
      },
      target: 'node'
    };
    ```
    {: codeblock}

3. 將下列程式碼儲存至名為 `index.js` 的檔案中。變數 `global.main` 會設定為動作的 main 函數。
    

    ```javascript
function myAction(args) {
    const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. 在本端安裝所有相依關係。
    

    ```
    npm install
    ```
    {: pre}

5. 建置 webpack 組合。
    

    ```
    npm run build
    ```
    {: pre}

    會建立 `dist/bundle.js` 檔案，並部署為動作原始碼。

6. 使用 `npm` Script 或 CLI 來建立動作。

    * 使用 `npm` Script：
        

        ```
        npm run deploy
        ```
        {: pre}

    *   使用 CLI：
  

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    `webpack` 所建置的組合檔案僅支援 JavaScript 相依關係。如果組合相依於二進位檔相依關係，則動作呼叫可能會失敗，因為這並不包含在 `bundle.js` 檔案中。
    {: tip}
    



