---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 建立並呼叫動作
{: #openwhisk_actions}

動作是在 {{site.data.keyword.openwhisk}} 平台上執行的 Stateless 程式碼 Snippet。例如，「動作」可用來偵測影像中的臉部、回應資料庫變更、聚集一組 API 呼叫，或張貼推文。「動作」可以寫成 JavaScript、Swift、Python、PHP 函數、Java 方法或任何二進位相容執行檔，其中包括 Go 程式及包裝成 Docker 容器的自訂可執行檔。
{:shortdesc}

可以明確地呼叫動作，或為回應某事件而執行動作。在任一情況下，每次執行「動作」都會導致由唯一啟動 ID 所識別的啟動記錄。「動作」的輸入及「動作」的結果是鍵值組的字典，其中索引鍵是字串，而值是有效的 JSON 值。動作也可以由其他「動作」的呼叫或已定義的「動作」序列組成。

瞭解如何在偏好的開發環境中建立、呼叫及除錯「動作」：
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [任意執行檔](#creating-actions-arbitrary)

此外，也請瞭解：
* [監看動作輸出](#watching-action-output)
* [大型應用程式支援](#large-app-support)
* [列出動作](#listing-actions)
* [刪除動作](#deleting-actions)
* [存取動作內文內的動作 meta 資料](#accessing-action-metadata-within-the-action-body)


## 建立及呼叫 JavaScript 動作
{: #creating-and-invoking-javascript-actions}

下列各節會引導您使用 JavaScript 中的「動作」。您可以開始建立及呼叫簡單動作。然後，您可以將參數新增至動作，並使用參數來呼叫該動作。接下來是設定並呼叫預設參數。然後，您建立非同步「動作」，最後使用動作序列。


### 建立及呼叫簡式 JavaScript 動作
{: #openwhisk_single_action_js}

請檢閱下列步驟及範例，以建立您的第一個 JavaScript 動作。

1. 建立含有下列內容的 JavaScript 檔。在此範例中，檔名是 'hello.js'。

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

    JavaScript 檔可能包含其他函數。不過，依慣例，必須要有稱為 `main` 的函數，以提供動作的進入點。

2. 從下列 JavaScript 函數建立動作。在此範例中，動作稱為 'hello'。

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  CLI 會使用原始檔副檔名自動推斷動作的類型。對於 `.js` 原始檔，動作會使用 Node.js 6 執行時期來執行。您也可以明確地指定參數 `--kind nodejs:8`，來建立與 Node.js 8 一起執行的動作。如需相關資訊，請參閱 Node.js 6 與 8 [參照](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。
  
3. 列出您已建立的「動作」：

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  您可以看到您建立的 `hello` 動作。

4. 建立動作之後，即可使用 'invoke' 指令透過 OpenWhisk 在雲端執行它。在指令中指定旗標，即可透過*區塊處理* 呼叫（即要求/回應樣式）或*非區塊處理* 呼叫來呼叫「動作」。區塊處理呼叫要求會_等待_ 啟動結果可供使用。等待期間少於 60 秒，或是動作的[時間限制值](./openwhisk_reference.html#openwhisk_syslimits)。如果在等待期間內有啟動結果，則會予以傳回。否則，會在系統中繼續處理啟動，並傳回啟動 ID，以便之後可以檢查結果，如同非區塊處理要求一樣（如需監視啟動的提示，請參閱[這裡](#watching-action-output)）。

  這個範例使用區塊處理參數 `--blocking`：

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  這個指令輸出兩個重要的資訊部分：
  * 啟動 ID (`44794bd6aab74415b4e42a308d880e5b`)
  * 如果在預期的等待期間內有呼叫結果，則為呼叫結果

  在此情況下，結果是 JavaScript 函數所傳回的 `Hello world` 字串。啟動 ID 之後可以用來擷取日誌或呼叫結果。  

5. 如果您不是立即需要動作結果，則可以省略 `--blocking` 旗標，以進行非區塊處理呼叫。您之後可以透過使用啟動 ID 來取得結果。請參閱下列範例：

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. 如果您忘記記錄啟動 ID，則可以取得從最近到最舊排列的啟動清單。執行下列指令，以取得啟動的清單：

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### 將參數傳遞給動作
{: #openwhisk_pass_params}

呼叫動作時，可以將參數傳遞給動作。

1. 在動作中使用參數。例如，使用下列內容來更新 'hello.js' 檔：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  輸入參數被當作 `main` 函數的 JSON 物件參數來傳遞。請注意，在此範例中，`name` 及 `place` 參數擷取自 `params` 物件的方式。

2. 更新並呼叫 `hello` 動作，同時為其傳遞 `name` 和 `place` 參數值。請參閱下列範例：

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  如果您需要修改非服務認證參數，請注意，執行含有新參數的 `action update` 指令，會移除目前已存在但未指定在 `action update` 指令中的任何參數。例如，除了 `__bx_creds` 之外，若有兩個參數含有名為 key1 和 key2 的金鑰。如果您執行 `action update` 指令，且含有 `-p key1 new-value -p key2 new-value`，但省略 `__bx_creds` 參數，則在順利完成 `action update` 之後，`__bx_creds` 參數便不再存在。然後，您必須重新連結服務認證。這是沒有暫行解決方法的已知限制。
  {: tip}  

3.  可在指令行上明確地提供參數，或提供包含所需參數的檔案來提供參數。

  若要透過指令行直接傳遞參數，請提供一對索引鍵/值給 `--param` 旗標：
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  若要使用包含參數內容的檔案，請以 JSON 格式建立包含參數的檔案。然後，必須將檔名傳遞至 `param-file` 旗標：

  請參閱下列範例參數檔案 `parameters.json`：
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  請注意，使用 `--result` 選項：這表示 CLI 等待呼叫完成後僅顯示結果的區塊處理呼叫。為方便起見，可以在沒有自動推斷的 `--blocking` 的情況下使用此選項。

  此外，如果指令行上指定的參數值是有效的 JSON，則會對其進行剖析，並以結構化物件形式將其傳送至動作。例如，將 hello 動作更新如下：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  現在，動作預期單一 `person` 參數具有欄位 `name` 及 `place`。接下來，使用本身為有效 JSON 的單一 `person` 參數來呼叫動作，如下列範例所示：

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  結果會相同，因為 CLI 會自動將 `person` 參數值剖析為動作現在預期的結構化物件：
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### 設定預設參數
{: #openwhisk_binding_actions}

您可以使用多個具名參數來呼叫動作。請記住，前一個範例中的 `hello` 動作預期會有兩個參數：人員的名稱 (*name*) 及其所在位置 (*place*)。

您可以連結特定參數，而非每次都將所有參數傳遞給動作。下列範例會連結 *place* 參數，以將動作預設為 "Vermont" 這個位置：

1. 更新動作，方法為使用 `--param` 選項來連結參數值，或將包含參數的檔案傳遞至 `--param-file`

  若要在指令行上明確地指定預設參數，請將一對索引鍵/值提供給 `param` 旗標：

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  從檔案傳遞參數需要以 JSON 格式建立一個包含所需內容的檔案。然後，必須將檔名傳遞至 `-param-file` 旗標：

  請參閱下列範例參數檔案 `parameters.json`：
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. 呼叫動作，但這次只傳遞 `name` 參數。

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

    請注意，呼叫動作時，您不需要指定 place 參數。在呼叫時指定參數值，仍然可以改寫連結的參數。

3. 呼叫動作，並傳遞 `name` 及 `place` 值。後者會改寫連結至動作的值。

  使用 `--param` 旗標：

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  使用 `--param-file` 旗標：

  檔案 parameters.json：
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### 取得動作 URL

您可以經由 HTTPS 要求，透過 REST 介面呼叫動作。若要取得動作 URL，請執行下列指令：

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

透過 HTTPS 要求呼叫動作時，必須提供鑑別。如需使用 REST 介面來呼叫動作的相關資訊，請參閱[搭配使用 REST API 與 OpenWhisk](./openwhisk_rest_api.html#openwhisk_rest_api)。
{: tip}

### 儲存動作碼

與現有動作相關聯的程式碼會在本端提取及儲存。「儲存」可在序列及 Docker 動作以外的所有動作上執行。將動作碼儲存至檔案時，程式碼會儲存在現行工作目錄中，且會顯示已儲存的檔案路徑。

1. 將動作碼儲存至對應於現有動作名稱的檔名。會使用對應於動作類型的副檔名，或將 `.zip` 類型的副檔名用於作為 zip 檔案的動作碼。
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. 您可以使用 `--save-as` 旗標來提供自訂檔名和副檔名，而非讓 CLI 判斷已儲存程式碼的檔名和副檔名。
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### 建立非同步動作
{: #openwhisk_asynchrony_js}

在您的動作中傳回 Promise，非同步執行的 JavaScript 函數便可在 `main` 函數返回之後，傳回啟動結果。

1. 將下列內容儲存至稱為 `asyncAction.js` 的檔案中。

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

  請注意，`main` 函數會傳回 Promise，這表示啟動尚未完成，但預期未來可完成。

  在此情況下，`setTimeout()` JavaScript 函數會等待 2 秒，再呼叫回呼函數，這會呈現非同步程式碼，並進入 Promise 的回呼函數內。

  Promise 的回呼接受兩個引數（resolve 及 reject），這兩個都是函數。`resolve()` 呼叫可滿足 Promise，並指出啟動正常完成。

  `reject()` 呼叫可以用來拒絕 Promise，並發出信號指出啟動異常完成。

2. 執行下列指令，以建立並呼叫動作：

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

    請注意，您已執行非同步動作的區塊處理呼叫。

3. 提取啟動日誌，來查看啟動需要多久時間才能完成：

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  比較啟動記錄中的 `start` 與 `end` 時間戳記，您可以發現完成此啟動所需的時間略高於 2 秒。

### 使用動作來呼叫外部 API
{: #openwhisk_apicall_action}

到目前為止，這些範例自行包含 JavaScript 函數。您也可以建立呼叫外部 API 的動作。

此範例會呼叫 Yahoo Weather 服務，以取得特定位置的現行狀況。

1. 將下列內容儲存至稱為 `weather.js` 的檔案中。

  ```javascript
  var request = require('request');

    function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 此範例中的動作使用 JavaScript `request` 程式庫，對 Yahoo Weather API 發出 HTTP 要求，以及從 JSON 結果中擷取欄位。[參照](./openwhisk_reference.html#openwhisk_ref_javascript_environments)詳述可在「動作」中使用的 Node.js 套件。

  此範例也會顯示需要非同步「動作」。此動作會傳回 Promise，指出在函數返回時還無法取得這個動作的結果。而是，在 HTTP 呼叫完成之後，會在 `request` 回呼中取得結果，並且它會作為 `resolve()` 函數的引數傳遞。

2. 執行下列指令，以建立並呼叫動作：

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### 將動作包裝為 Node.js 模組
{: #openwhisk_js_packaged_action}

使用單一 JavaScript 原始檔撰寫所有動作碼的替代方案，是您可以將動作撰寫為 `npm` 套件。以具有下列檔案的目錄為例：

首先是 `package.json`：

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

然後是 `index.js`：

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

此動作透過 `exports.main` 來公開。動作處理程式本身可以有任何名稱，只要符合接受物件以及傳回物件的正常簽章（或物件的 `Promise`）。根據 Node.js 使用慣例，您必須將這個檔案命名為 `index.js`，或是將您喜好的檔名指定為 package.json 中的 `main` 內容。

若要從此套件建立 OpenWhisk 動作，請執行下列動作：

1. 先在本端安裝所有相依關係

  ```
  npm install
  ```
  {: pre}

2. 建立包含所有檔案的 `.zip` 保存檔（包括所有相依關係）：

  ```
  zip -r action.zip *
  ```
  {: pre}

  使用「Windows 檔案總管」動作來建立 zip 檔案會導致結構不正確。OpenWhisk zip 動作必須將 `package.json` 置於 zip 的根目錄，而「Windows 檔案總管」會將它放在巢狀資料夾中。最安全的選項是使用指令行 `zip` 指令。
  {: tip}

3. 建立動作：

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  使用 CLI 工具從 `.zip` 保存檔建立動作時，必須使用 `nodejs:6` 或 `nodejs:8`，明確地提供 `--kind` 旗標的值。

4. 您可以呼叫任何其他動作：

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

最後，請注意，雖然大部分 `npm` 套件都會在 `npm install` 上安裝 JavaScript 原始檔，但是有一部分也會安裝及編譯二進位構件。保存檔上傳目前不支援二進位相依關係，而只支援 JavaScript 相依關係。如果保存檔包括二進位相依關係，則動作呼叫可能會失敗。

### 將動作包裝為單一組合
{: #openwhisk_js_webpack_action}

這便於將最少程式碼併入包含相依關係的單一 `.js` 檔案。此方法可讓部署更快，在某些情況下，將動作包裝為 zip 可能太大，因為它包含不必要的檔案。

您可以使用 JavaScript 模組組合程式，例如，[Webpack](https://webpack.js.org/concepts/)。當 Webpack 處理您的程式碼時，它會遞迴地建置相依關係圖形，其中包含您的動作所需的每個模組。

以下是使用 Webpack 的快速範例：

讓先前的範例 `package.json` 將 `Webpack` 新增為開發依賴，並新增一些 npm Script 指令。
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "ic wsk action update my-action dist/bundle.js --kind nodejs:8"
  },
  "dependencies": {
    "left-pad" : "1.1.3"
  },
  "devDependencies": {
    "webpack": "^3.8.1"
  }
}
```
{: codeblock}

建立 Webpack 配置檔 `webpack.config.js`。
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

將變數 `global.main` 設定為動作的 main 函數。
在前一個範例中：
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

如果您的函數名稱是 `main`，請改用此語法：
```javascript
global.main = main;
```
{: codeblock}


若要使用 `npm` 及 `webpack` 來建置及部署「OpenWhisk 動作」：

1. 首先，在本端安裝相依關係：

  ```
  npm install
  ```
  {: pre}

2. 建置 webpack 組合：

  ```
  npm run build
  ```
  {: pre}

  已建立 `dist/bundle.js` 檔，用來部署為「動作」原始碼。

3. 使用 `npm` Script 或 CLI 來建立「動作」。使用 `npm` Script：
  ```
  npm run deploy
  ```
  {: pre}
  使用 CLI：
  ```
  ic wsk action update my-action dist/bundle.js
  ```
  {: pre}


最後，`webpack` 所建置的組合檔案不支援二進位相依關係，而是支援 JavaScript 相依關係。因此，如果組合相依於二進位相依關係，則「動作」呼叫會失敗，因為這並不包含在 `bundle.js` 檔案中。

## 建立動作序列
{: #openwhisk_create_action_sequence}

您可以建立一個動作，以將一連串的「動作」鏈結在一起。

在稱為 `/whisk.system/utils` 的套件中提供數個公用程式「動作」，可用來建立第一個序列。您可以在[套件](./openwhisk_packages.html)一節中進一步瞭解套件。

1. 顯示 `/whisk.system/utils` 套件中的「動作」。

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  在此範例中，您會使用 `split` 及 `sort` 動作。

2. 建立動作序列，以將某個動作的結果當作下一個動作的引數來傳遞。

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  此動作序列會將數行文字轉換為一個陣列，並排序這些行。

3. 呼叫動作：

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  在結果中，會排序這些行。

**附註**：序列中「動作」之間所傳遞的參數十分明確，但預設參數除外。因此，傳遞給動作序列的參數僅適用於序列中的第一個動作。序列中第一個動作的結果會變成序列中第二個動作的輸入 JSON 物件（以此類推）。此物件不會包括一開始傳遞給序列的任何參數，除非第一個動作將它們明確地包括在結果中。動作的輸入參數會與動作的預設參數合併，而前者的優先順序較高，並且會置換任何相符的預設參數。如需使用多個具名參數來呼叫動作序列的相關資訊，請參閱[設定預設參數](./openwhisk_actions.html#openwhisk_binding_actions)。

## 建立 Python 動作
{: #creating-python-actions}

建立「Python 動作」的程序，與建立「JavaScript 動作」的程序類似。下列各節會引導您建立及呼叫單一 Python 動作，以及將參數新增至該動作。

### 建立及呼叫 Python 動作
{: #openwhisk_actions_python_invoke}

動作只是最上層 Python 函數。例如，使用下列原始碼建立稱為 `hello.py` 的檔案：

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

「Python 動作」一律會使用某個字典，並產生一個字典。動作的進入方法依預設是 `main`，但可以使用 `--main` 來明確指定，以建立具有 `wsk` CLI 以及任何其他動作類型的動作。

您可以從此函數建立稱為 `helloPython` 的 OpenWhisk 動作，如下所示：
```
wsk action create helloPython hello.py
```
{: pre}

CLI 會從原始檔副檔名自動推斷動作類型。對於 `.py` 原始檔，動作會使用 Python 2 執行時期來執行。您也可以明確地指定參數 `--kind python:3`，來建立與 Python 3 一起執行的動作。此外，也有 Python 3 執行時期的類型為 `python-jessie:3`，包含適用於「IBM Cloud 服務」（例如，IBM Cloudant、IBM DB2、IBM COS 及 IBM Watson）的額外套件。
如需此 Python 3 執行時期中所含套件的相關資訊，請參閱 Python 執行時期[參照](./openwhisk_reference.html#openwhisk_ref_python_environments)。

「Python 動作」與「JavaScript 動作」的動作呼叫相同：
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### 將 Python 動作包裝在 zip 檔案中
{: #openwhisk_actions_python_zip}

您可以將 Python 動作及相依模組包裝在 zip 檔案中。
包含進入點（例如，`main`）的原始檔檔名必須是 `__main__.py`。
例如，若要建立 helper 模組稱為 `helper.py` 的動作，請先建立一個包含您原始檔的保存檔：

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

然後建立動作：

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

雖然會針對 Python 3（類型為 `python:3`）顯示這些步驟，但您可以使用替代的 Python 類型 `python:2` 或 `python-jessie:3` 來執行相同步驟。


### 使用 zip 檔案中的虛擬環境來包裝「Python 動作」
{: #openwhisk_actions_python_virtualenv}

另一種包裝 Python 相依關係的方式是使用虛擬環境 (`virtualenv`)，這可讓您鏈結可透過 [`pip`](https://packaging.python.org/installing/)（舉例說明）安裝的其他套件。



與基本 zip 檔案支援一樣，包含主要進入點的原始檔的名稱必須是 `__main__.py`。為了釐清，`__main__.py` 的內容是 main 函數，因此，在此範例中，您可以從上一節將 `hello.py` 重新命名為 `__main__.py`。此外，virtualenv 目錄必須命名為 `virtualenv`。
請參閱下列範例實務，以安裝相依關係、將它們包裝在 virtualenv 中，以及建立相容的 OpenWhisk 動作。

若要確保與 OpenWhisk 執行時期容器的相容性，必須在目標環境中使用類型的對應映像檔，進行 virtualenv 內的套件安裝。

- 對於類型 `python:2`，請使用 Docker 映像檔 `openwhisk/python2action`。
- 對於類型 `python:3`，請使用 Docker 映像檔 `openwhisk/python3action`。
- 對於類型 `python-jessie:3`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3`。

1. 如果 [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 檔案包含要安裝的 `pip` 模組及版本，請執行下列指令來安裝相依關係，並且使用相容的 Docker 映像檔來建立 virtualenv：
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. 保存 virtualenv 目錄及任何其他 Python 檔案：
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. 建立動作：
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## 建立 PHP 動作
{: #creating-php-actions}

建立「PHP 動作」的程序，與建立「JavaScript 動作」的程序類似。下列各節會引導您建立及呼叫單一 PHP 動作，以及將參數新增至該動作。

### 建立及呼叫 PHP 動作
{: #openwhisk_actions_php_invoke}

動作只是最上層 PHP 函數。例如，使用下列原始碼建立稱為 `hello.php` 的檔案：

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

「PHP 動作」一律使用聯合陣列，並傳回聯合陣列。動作的進入方法依預設是 `main`，但可以在使用 `--main` 來建立具有 `wsk` CLI 以及任何其他動作類型的動作時，明確指定。

您可以從此函數建立稱為 `helloPHP` 的 OpenWhisk 動作，如下所示：

```
wsk action create helloPHP hello.php
```
{: pre}

CLI 會從原始檔副檔名自動推斷動作類型。對於 `.php` 原始檔，動作會使用 PHP 7.1 執行時期來執行。如需相關資訊，請參閱 PHP [參照](./openwhisk_reference.html#openwhisk_ref_php)。

「PHP 動作」與「JavaScript 動作」的動作呼叫相同：

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### 將 PHP 動作包裝在 zip 檔案中
{: #openwhisk_actions_php_zip}

您可以將 PHP 動作及其他檔案與相依套件包裝在 zip 檔案中。包含進入點（例如，`main`）的原始檔檔名必須是 `index.php`。例如，若要建立包含第二個檔案（稱為 `helper.php`）的動作，請先建立包含原始檔的保存檔：

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

然後建立動作：

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## 建立 Swift 動作
{: #creating-swift-actions}

建立「Swift 動作」的程序，與建立「JavaScript 動作」的程序類似。下列各節會引導您建立及呼叫單一 Swift 動作，以及將參數新增至該動作。

您也可以使用線上 [Swift 沙盤推演](https://swiftlang.ng.bluemix.net)來測試 Swift 程式碼，而不需要在機器上安裝 Xcode。

### 建立及呼叫動作

動作只是最上層 Swift 函數。例如，建立稱為 `hello.swift` 且含有下列內容的檔案：

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

「Swift 動作」一律會使用某個字典，並產生一個字典。

您可以從此函數建立稱為 `helloSwift` 的 {{site.data.keyword.openwhisk_short}} 動作，如下所示：

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

由於不支援舊版 Swift，請一律指定 `swift:3.1.1`。
{: tip}

「Swift 動作」與「JavaScript 動作」的動作呼叫相同：

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**注意：**在 Linux 環境中執行的「Swift 動作」仍在開發中，而 {{site.data.keyword.openwhisk_short}} 通常使用最新的可用版本，但該版本不一定穩定。此外，與 {{site.data.keyword.openwhisk_short}} 搭配使用的 Swift 版本，可能與 MacOS 上穩定 XCode 版本的 Swift 版本不一致。

### 將動作包裝為 Swift 執行檔
{: #openwhisk_actions_swift_zip}

當您使用 Swift 來源檔建立 OpenWhisk Swift 動作時，必須先將其編譯成二進位檔，才能執行動作。完成之後，後續呼叫動作時就會快很多，直到清除用來保存動作的容器為止。這項延遲稱為冷啟動延遲。

若要避免冷啟動延遲，您可以將 Swift 檔案編譯成二進位檔，然後將它以 zip 檔案形式上傳至 OpenWhisk。當您需要 OpenWhisk 支撐時，建立二進位檔最簡單的方式，就是在執行檔案的相同環境中建置檔案。請參閱下列步驟：

- 使用下列指令來執行互動式 Swift 動作容器：
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- 複製原始碼，並準備建置。
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- （選用）建立 `Package.swift` 檔案，以新增相依關係。
   ```
   swift import PackageDescription
   
   let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
   ```
   {: pre}

  此範例會新增 `swift-watson-sdk` 及 `example-package-deckofplayingcards` 相依關係。請注意，標準 Swift 動作中會提供 `CCurl`、`Kitura-net` 及 `SwiftyJSON`，因此，您應該將它們包括在自己的 `Package.swift` 中。

- 將 Package.swift 複製到 spm-build 目錄
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- 切換至 spm-build 目錄
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- 編譯「Swift 動作」。
  ```
  swift build -c release
  ```
  {: pre}

- 建立 zip 保存檔。
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- 結束 Docker 容器。
  ```
  exit
  ```
  {: pre}

您可以看到已在與 hello.swift 相同的目錄中建立 hello.zip。 

- 使用動作名稱 helloSwifty，將它上傳至 OpenWhisk：
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- 若要檢查速度快多少，請執行 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

動作所需的執行時間位於 "duration" 內容中，並且與使用 hello 動作中的編譯步驟執行所需的時間進行比較。

## 建立 Java 動作
{: #creating-java-actions}

建立「Java 動作」的程序，與建立 JavaScript 及 Swift 動作的程序類似。下列各節會引導您建立及呼叫單一 Java 動作，以及將參數新增至該動作。

若要編譯、測試及保存 Java 檔案，您必須已在本端安裝 [JDK 8](http://openjdk.java.net/install)。

### 建立及呼叫動作
{: #openwhisk_actions_java_invoke}

Java 動作是一種 Java 程式，搭配稱為 `main` 的方法，其具有確切的簽章，如下所示：
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

例如，使用下列內容建立稱為 `Hello.java` 的 Java 檔案：

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

然後，將 `Hello.java` 編譯成 JAR 檔 `hello.jar`，如下所示：
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) 必須存在於 Java CLASSPATH 中，才能編譯 Java 檔。
{: tip}

您可以從這個 JAR 檔建立稱為 `helloJava` 的 OpenWhisk 動作，如下所示：

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

使用指令行及 `.jar` 原始檔時，您不需要指定您是在建立 Java 動作；工具是透過副檔名來判斷。

您需要使用 `--main` 來指定 main 類別的名稱。有資格的 main 類別是實作 static `main` 方法的類別。如果類別不在預設套件中，請使用 Java 完整類別名稱，例如，`--main com.example.MyMain`。

必要的話，您也可以自訂 Java 動作的方法名稱。這是透過指定動作的 Java 完整方法名稱來完成，例如，`--main com.example.MyMain#methodName`。

「Java 動作」與 Swift 及 JavaScript 動作的動作呼叫相同：

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## 建立 Docker 動作
{: #creating-docker-actions}

使用「{{site.data.keyword.openwhisk_short}} Docker 動作」，您可以使用任何語言來撰寫「動作」。

您的程式碼會編譯成可執行的二進位檔，並內嵌在 Docker 映像檔中。二進位程式與系統互動的方式是從 `stdin` 取得輸入，並透過 `stdout` 回覆。

先決條件是您必須具備 Docker Hub 帳戶。若要設定免費 Docker ID 及帳戶，請前往 [Docker Hub](https://hub.docker.com)。

對於下面的指示，假設 Docker 使用者 ID 是 `janesmith`，而密碼是 `janes_password`。假設已設定 CLI，則仍需執行三個步驟才能設定供 {{site.data.keyword.openwhisk_short}} 使用的自訂二進位檔。之後，上傳的 Docker 映像檔可以當作動作使用。

1. 下載 Docker 架構。您可以使用 CLI 進行下載及安裝，如下所示：

  ```
  wsk sdk install docker
  ```
  {: pre}

    Docker 架構現在安裝在現行目錄中。
  
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  架構是一個 Docker 容器範本，您可以在其中以自訂二進位檔形式來注入程式碼。

2. 在 blackbox 架構中，設定您的自訂二進位檔。此架構已包含您可以使用的 C 程式。

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  您可以視需要修改此檔案，或者將其他程式碼及相依關係新增至 Docker 映像檔。如果是後者，您可以在必要時調整 `Dockerfile` 以建置執行檔。二進位檔必須位在 `/action/exec` 的容器內。

  執行檔會從指令行收到單一引數。它是代表動作引數之 JSON 物件的字串序列化。程式可能會記載至 `stdout` 或 `stderr`。依照慣例，輸出的最後一行_必須_ 是代表動作結果的字串化 JSON 物件。

3. 建置 Docker 映像檔，並使用提供的 Script 予以上傳。您必須先執行 `docker login` 進行鑑別，然後執行具有所選擇映像檔名稱的 Script。

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  請注意，`example.c` 檔案的一部分會在 Docker 映像檔建置程序之中編譯，因此不需要在機器上編譯 C。實際上，除非您是在相容主機上編譯二進位檔，否則無法在容器內執行，因為格式不相符。

  Docker 容器現在可作為 OpenWhisk 動作使用。


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  請注意，使用 `--docker` 來建立動作。假設所有 Docker 映像檔都在 Docker Hub 上進行管理。動作會呼叫為任何其他 {{site.data.keyword.openwhisk_short}} 動作。 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  若要更新 Docker 動作，請執行 `buildAndPush.sh`，以將最新映像檔上傳至 Docker Hub。這可讓系統在下次執行您動作的程式碼時取回新的 Docker 映像檔。如果沒有暖容器，任何新呼叫會使用新的 Docker 映像檔。不過，如果有暖容器使用舊版 Docker 映像檔，則除非您執行 `wsk action update`，否則任何新呼叫都會繼續使用該映像檔。這指出針對新呼叫，系統會執行 Docekr 取回，以取得新的 Docker 映像檔。

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  您可以在[參照](./openwhisk_reference.html#openwhisk_ref_docker)一節找到建立「Docker 動作」的相關資訊。

  舊版 CLI 支援的 `--docker` 沒有參數及映像檔名稱，在以前是當成位置引數。為了讓「Docker 動作」能夠透過 (zip) 檔案接受起始設定資料，將「Docker 動作」的使用者經驗正規化，因此，現有的位置引數必須是檔案（例如，zip 檔案）。必須在 `--docker` 選項後面指定映像檔名稱。感謝使用者意見，會將 `--native` 引數併入來作為 `--docker openwhisk/dockerskeleton` 的速記，因此，在標準 Docker 動作 SDK 內執行的執行檔較方便您建立及部署。
  
  例如，本指導教學已在位於 `/action/exec` 的容器內建立二進位執行檔。如果您將此檔案複製到本端檔案系統，並將它壓縮至 `exec.zip`，則您可以使用下列指令建立一個 Docker 動作，以將執行檔接收為起始設定資料。 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  這等同於下列指令。 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## 使用任意執行檔建立動作
{: #creating-actions-arbitrary}

使用 `--native`，您可以看到任何執行檔可執行為 OpenWhisk 動作。這包括 `bash` Script 或交叉編譯的二進位檔。如果是後者，限制是二進位檔必須與 `openwhisk/dockerskeleton` 映像檔相容。

## 建立 Go 動作
{: #creating-go-actions}

`--native` 選項容許將任何執行檔包裝成動作。例如，這適用於 Go。
使用 Docker 動作，Go 執行檔會從指令行接收到單一引數。
它是代表動作引數之 JSON 物件的字串序列化。程式可能會記載至 `stdout` 或 `stderr`。依照慣例，輸出的最後一行_必須_ 是代表動作結果的字串化 JSON 物件。

以下是 Go 動作的範例。
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]
   
    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

將上述程式碼儲存至 `sample.go` 檔案，並交叉編譯它以用於 OpenWhisk。執行檔必須稱為 `exec`。
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

該動作可以執行為任何其他動作。
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

以類似的方式擷取日誌。
```bash
wsk activation logs --last --strip
my first Go action.
```

## 監視動作輸出
{: #watching-action-output}

其他使用者可能會呼叫「{{site.data.keyword.openwhisk_short}} 動作」來回應各種事件，或是作為動作序列的一部分。在這類情況下，監視呼叫可能十分有用。

您可以使用 {{site.data.keyword.openwhisk_short}} CLI 來監看所呼叫「動作」的輸出。

1. 從 Shell，發出下列指令：
  ```
  wsk activation poll
  ```
  {: pre}

  這個指令會啟動輪詢迴圈，以從啟動開始持續檢查日誌。

2. 切換至另一個視窗，然後呼叫動作：

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. 在輪詢視窗中，觀察啟動日誌：

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  同樣地，每當您執行輪詢公用程式時，都可以即時查看日誌中是否有任何代表您在 OpenWhisk 中執行的「動作」。

## 大型應用程式支援
{: #large-app-support}

「動作」的程式碼大小上限為 48 MB。包含許多協力廠商模組、原生程式庫或外部工具的應用程式可能會執行此限制。

如果您發生建立大於 48 MB 的套件「動作」（zip 或 jar），解決方案是擴充具有相依關係的執行時期映像檔，然後使用單一原始檔或小於 48 MB 的小保存檔。

例如，透過建置包含必要共用程式庫的自訂 Docker 執行時期，這些相依關係並不需要存在於保存檔中。專用原始檔仍可組合在保存檔中，並在執行時期注入。

減少保存檔大小的另一個好處是，部署時間也能得到改善。

### Python 範例

在下列 Python 範例中，opencv 可以包括程式庫 `opencv-python`，然後將 opencv 二進位檔安裝至 OS 映像檔。然後，您可以使用 `requirements.txt` 並執行 `pip install requirements.txt`，以使用更多 Python 程式庫來擴增映像檔。然後，您可以搭配使用 `action.py` 與新映像檔。

### Node.js 範例

在下列 Node.js 範例中，您可以將額外套件安裝至 OS 映像檔：

使用 `npm` 來安裝 opencv：
```
npm install opencv
```
{: pre}

同樣地，如果您有 `package.json`，請使用 `npm` 來進行安裝：
```
npm install package.json
```
{: pre}

然後，繼續搭配使用 `action.js` 與新映像檔。

## 列出動作
{: #listing-actions}

您可以列出使用下列指令所建立的所有「動作」：

```
wsk action list
```
{: pre}

隨著您撰寫愈多「動作」，此清單也會變得愈長，而其有助於將相關的「動作」分組成[套件](./openwhisk_packages.html)。若要將您的「動作」清單過濾為只有特定套件中的動作，您可以使用下列指令語法： 

```
wsk action list [PACKAGE NAME]
```
{: pre}

## 刪除動作
{: #deleting-actions}

刪除您不要使用的「動作」來進行清除。

1. 執行下列指令，以刪除動作：
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. 驗證動作不再出現於「動作」清單中。
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## 存取動作內文內的動作 meta 資料
{: #accessing-action-metadata-within-the-action-body}

動作環境包含執行中動作特有的數個內容。這些內容容許動作透過 REST API 以程式設計方式使用 OpenWhisk 資產，或設定在動作即將用完其分配時間預算時的內部警示。使用 OpenWhisk Docker 架構時，可以透過所有支援執行時期的系統環境來存取這些內容：Node.js、Python、Swift、Java 及 Docker 動作。

* `__OW_API_HOST`：執行此動作之 OpenWhisk 部署的 API 主機
* `__OW_API_KEY`：呼叫動作之主題的 API 金鑰，此金鑰可能是受限 API 金鑰
* `__OW_NAMESPACE`：_啟動_ 的名稱空間（這可能與動作的名稱空間不同）
* `__OW_ACTION_NAME`：執行中動作的完整名稱
* `__OW_ACTIVATION_ID`：此執行中動作實例的啟動 ID
* `__OW_DEADLINE`：此動作將用完其整個持續期間配額的大約時間（以新紀元毫秒測量）
