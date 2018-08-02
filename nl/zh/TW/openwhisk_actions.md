---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 建立及呼叫動作
{: #openwhisk_actions}

動作是在 {{site.data.keyword.openwhisk}} 平台上執行的 Stateless 程式碼 Snippet。例如，動作可用來偵測影像中的臉孔、回應資料庫變更、聚集一組 API 呼叫，或張貼推文。動作可以寫成 JavaScript、Swift、Python、PHP 函數、Java 方法或任何二進位相容執行檔，其中包括 Go 程式及包裝成 Docker 容器的自訂可執行檔。
{:shortdesc}

可以明確地呼叫動作，或為回應某事件而執行動作。在任一情況下，每次執行動作都會導致由唯一啟動 ID 所識別的啟動記錄。動作的輸入及動作的結果是鍵值組的字典，其中索引鍵是字串，而值是有效的 JSON 值。動作也可以由其他動作的呼叫或已定義的動作序列組成。

瞭解如何在偏好的開發環境中建立、呼叫及除錯動作：
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [任意執行檔](#creating-actions-arbitrary)

此外，也請瞭解：
* [監視動作輸出](#monitor-action-output)
* [取得動作](#getting-actions)
* [列出動作](#listing-actions)
* [刪除動作](#deleting-actions)
* [大型應用程式支援](#large-app-support)
* [存取動作內文內的動作 meta 資料](#accessing-action-metadata-within-the-action-body)

## 建立及呼叫 JavaScript 動作
{: #creating-and-invoking-javascript-actions}

下列各節會引導您透過 JavaScript 逐步執行動作。您可以開始建立及呼叫簡單動作。然後，您可以將參數新增至動作，並使用參數來呼叫該動作。接下來是設定並呼叫預設參數。然後，您建立非同步動作，最後使用動作序列。

### 建立及呼叫簡單 JavaScript 動作
{: #openwhisk_single_action_js}

請檢閱下列步驟及範例，以建立您的第一個 JavaScript 動作。

1. 建立含有下列內容的 JavaScript 檔。在此範例中，將檔案命名為 **hello.js**。
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

    JavaScript 檔可能包含其他函數。不過，依慣例，必須要有稱為 **main** 的函數，以提供動作的進入點。

2. 從下列 JavaScript 函數建立動作。在此範例中，動作稱為 **hello**。
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  輸出範例：
  ```
  ok: created action hello
  ```
  {: screen}

    CLI 會使用原始檔副檔名自動推斷動作的類型。對於 `.js` 原始檔，動作會使用 Node.js 6 執行時期來執行。您也可以明確地指定參數 `--kind nodejs:8`，來建立與 Node.js 8 一起執行的動作。如需相關資訊，請參閱 Node.js 6 與 8 [參照](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。

3. 列出您已建立的動作：
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

  您可以看到您建立的 **hello** 動作。

4. 建立動作之後，即可使用 **invoke** 指令在雲端中執行它。在指令中指定旗標，即可透過*區塊處理* 呼叫（即要求/回應樣式）或*非區塊處理* 呼叫來呼叫動作。區塊處理呼叫要求會_等待_ 啟動結果可供使用。等待期間少於 60 秒，或是動作的[時間限制值](./openwhisk_reference.html#openwhisk_syslimits)。如果在等待期間內有啟動結果，則會予以傳回。否則，會在系統中繼續處理啟動，並傳回啟動 ID，以便之後可以檢查結果，如同非區塊處理要求一樣（如需監視啟動的提示，請參閱[這裡](#monitor-action-output)）。

  這個範例使用區塊處理參數 `--blocking`：
  ```
  ibmcloud fn action invoke --blocking hello
  ```
  {: pre}

  這個指令輸出兩個重要的資訊部分：
  * 啟動 ID (`44794bd6aab74415b4e42a308d880e5b`)
  * 如果在預期的等待期間內有呼叫結果，則為呼叫結果

  **輸出顯示啟動 ID：**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **呼叫結果：**
  ```
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  在此情況下，結果是 JavaScript 函數所傳回的 `Hello world` 字串。啟動 ID 之後可以用來擷取日誌或呼叫結果。

5. 如果您不是立即需要動作結果，則可以省略 `--blocking` 旗標，以進行非區塊處理呼叫。您之後可以透過使用啟動 ID 來取得結果。

  請參閱下列範例：
  ```
  ibmcloud fn action invoke hello
  ```
  {: pre}

  **指令輸出：**
  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  既然您已知道啟動 ID，即可指定它來取得動作結果：
  ```
  ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **動作結果：**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. 如果您忘記記錄啟動 ID，則可以取得從最近到最舊排列的啟動清單。執行下列指令，以取得啟動的清單：

  **列出啟動：**
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

### 建立非同步動作
{: #openwhisk_asynchrony_js}

在您的動作中傳回 Promise，非同步執行的 JavaScript 函數便可在 `main` 函數返回之後，傳回啟動結果。

1. 將下列內容儲存至稱為 **asyncAction.js** 的檔案中。
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

2. 執行下列指令，以建立並呼叫動作。

  建立稱為 **asyncAction** 的動作：
  ```
  ibmcloud fn action create asyncAction asyncAction.js
  ```
  {: pre}

  呼叫動作：
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

    請注意，您已執行非同步動作的區塊處理呼叫。

3. 提取啟動日誌，來查看啟動需要多久時間才能完成。

  若要這麼做，請先列出可取得啟動 ID 的動作：
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

  現在使用啟動 ID 來取得啟動日誌資訊：
  ```
  ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  比較啟動記錄中的 `start` 與 `end` 時間戳記，您可以發現完成此啟動所需的時間略高於 2 秒。

### 使用動作來呼叫外部 API
{: #openwhisk_apicall_action}

到目前為止，這些提供的範例自行包含 JavaScript 函數。您也可以建立呼叫外部 API 的動作。

下列範例呼叫「NASA 每日一天文圖 (APOD)」服務，每日提供一張宇宙的獨特影像。

1. 將下列內容儲存至名為 **apod.js** 的檔案中。
  ```javascript
  var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

  $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
      $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
      $("#copyright").text("Image Credits: " + "Public Domain");
    }

    if(result.media_type == "video") {
      $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
      $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
    $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
  });
  ```
  {: codeblock}

  呼叫 NASA APOD API，並擷取 JSON 結果中的欄位。[參照](./openwhisk_reference.html#openwhisk_ref_javascript_environments)主題詳述可在動作中使用的 Node.js 套件。

2. 執行下列指令，以建立並呼叫動作。

  建立名為 **apod** 的動作：
  ```
  ibmcloud fn action create apod apod.js
  ```
  {: pre}

  呼叫 **apod** 動作：
  ```
  ibmcloud fn action invoke --result apod
  ```
  {: pre}

  **傳回物件：**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken.  During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree.  Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence.  The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA.  Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time.  Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th).  Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
  {: screen}

### 將動作包裝為 Node.js 模組
{: #openwhisk_js_packaged_action}

使用單一 JavaScript 原始檔撰寫所有動作碼的替代方案，是您可以將動作撰寫為 `npm` 套件。以具有下列檔案的目錄為例：

**package.json：**
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

**index.js：**
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

若要從此套件建立 {{site.data.keyword.openwhisk_short}} 動作，請執行下列動作：

1. 在本端安裝所有相依關係：
  ```
  npm install
  ```
  {: pre}

2. 建立包含所有檔案的 `.zip` 保存檔（包括所有相依關係）：
  ```
  zip -r action.zip *
  ```
  {: pre}

  使用「Windows 檔案總管」動作來建立 zip 檔案會導致結構不正確。{{site.data.keyword.openwhisk_short}} zip 動作必須將 `package.json` 置於 zip 的根目錄，而「Windows 檔案總管」會將它放在巢狀資料夾內。最安全的選項是使用指令行 `zip` 指令。
  {: tip}

3. 建立動作：
  ```
  ibmcloud fn action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  使用 CLI 工具從 `.zip` 保存檔建立動作時，必須使用 `nodejs:6` 或 `nodejs:8`，明確地提供 `--kind` 旗標的值。

4. 您可以呼叫任何其他動作：
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
    "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

如果您的函數名稱是 `main`，請改用下列語法：
```javascript
global.main = main;
```
{: codeblock}

若要使用 `npm` 及 `webpack` 來建置並部署 OpenWhisk 動作，請執行下列動作：

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

  已建立 `dist/bundle.js` 檔案，用來部署為動作原始碼。

3. 使用 `npm` Script 或 CLI 來建立動作。

  使用 `npm` Script：
  ```
  npm run deploy
  ```
  {: pre}

    使用 CLI：
  
  ```
  ibmcloud fn action update my-action dist/bundle.js
  ```
  {: pre}

最後，`webpack` 所建置的組合檔案不支援二進位相依關係，而是支援 JavaScript 相依關係。因此，如果組合相依於二進位相依關係，則動作呼叫會失敗，因為這並不內含在 `bundle.js` 檔案中。

## 建立動作序列
{: #openwhisk_create_action_sequence}

您可以建立一個動作，將一連串的動作鏈結在一起。

在稱為 `/whisk.system/utils` 的套件中提供數個公用程式動作，可用來建立第一個序列。您可以在[套件](./openwhisk_packages.html)一節中進一步瞭解套件。

1. 顯示 `/whisk.system/utils` 套件中的動作。
  ```
  ibmcloud fn package get --summary /whisk.system/utils
  ```
  {: pre}

  輸出範例：
  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```
  {: screen}

  在此範例中，您將使用 `split` 及 `sort` 動作。

2. 建立動作序列，以將某個動作的結果當作下一個動作的引數來傳遞。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  此動作序列會將數行文字轉換為一個陣列，並排序這些行。

3. 呼叫動作：
  ```
  ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  輸出範例：
  ```
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```
  {: screen}

  在結果中，會排序這些行。

**附註**：序列中動作之間所傳遞的參數十分明確，但預設參數除外。
因此，傳遞給動作序列的參數僅適用於序列中的第一個動作。序列中第一個動作的結果會變成序列中第二個動作的輸入 JSON 物件（以此類推）。此物件不會包括一開始傳遞給序列的任何參數，除非第一個動作將它們明確地包括在結果中。動作的輸入參數會與動作的預設參數合併，而前者的優先順序較高，並且會置換任何相符的預設參數。如需使用多個具名參數來呼叫動作序列的相關資訊，請參閱[在動作上設定預設參數](./parameters.html#default-params-action)。

## 建立 Python 動作
{: #creating-python-actions}

建立 Python 動作的程序，與建立 JavaScript 動作的程序類似。下列各節會引導您建立及呼叫單一 Python 動作，以及將參數新增至該動作。

### 建立及呼叫 Python 動作
{: #openwhisk_actions_python_invoke}

動作只是最上層 Python 函數。例如，使用下列原始碼建立稱為 **hello.py** 的檔案：
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python 動作一律會使用某個字典，並產生一個字典。動作的進入方法依預設是 `main`，但可以使用 `--main` 來明確指定，以建立具有 `wsk` CLI 以及任何其他動作類型的動作。

您可以從此函數建立稱為 **helloPython** 的 {{site.data.keyword.openwhisk_short}} 動作，如下所示：
```
ibmcloud fn action create helloPython hello.py
```
{: pre}

CLI 會從原始檔副檔名自動推斷動作類型。對於 `.py` 原始檔，動作會使用 Python 2 執行時期來執行。您也可以明確地指定參數 `--kind python:3`，來建立與 Python 3 一起執行的動作。此外，也有 Python 3 執行時期的類型為 `python-jessie:3`，包含適用於「IBM Cloud 服務」（例如，IBM Cloudant、IBM DB2、IBM COS 及 IBM Watson）的額外套件。
如需此 Python 3 執行時期中所含套件的相關資訊，請參閱 Python 執行時期[參照](./openwhisk_reference.html#openwhisk_ref_python_environments)。

Python 動作與 JavaScript 動作的動作呼叫相同：
```
ibmcloud fn action invoke --result helloPython --param name World
```
{: pre}

輸出範例：
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

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
ibmcloud fn action create helloPython --kind python:3 helloPython.zip
```
{: pre}

雖然會針對 Python 3（類型為 `python:3`）顯示這些步驟，但您可以使用替代的 Python 類型 `python:2` 或 `python-jessie:3` 來執行相同步驟。

### 使用 zip 檔案中的虛擬環境來包裝 Python 動作
{: #openwhisk_actions_python_virtualenv}

另一種包裝 Python 相依關係的方式是使用虛擬環境 (`virtualenv`)，這容許您鏈結可使用 [`pip`](https://packaging.python.org/installing/)（舉例說明）安裝的其他套件。

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

3. 建立動作 **helloPython**：
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

請只將那些不屬於所選取運行環境的模組新增至 `requirements.txt`。這有助於將 virtualenv 保持為最小。
{: tip}

## 建立 PHP 動作
{: #creating-php-actions}

建立 PHP 動作的處理程序，與建立 JavaScript 動作的處理程序類似。下列各節會引導您建立及呼叫單一 PHP 動作，以及將參數新增至該動作。

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
{: codeblock}

PHP 動作一律使用聯合陣列，並傳回聯合陣列。動作的進入方法依預設是 `main`，但可以在使用 `--main` 來建立具有 `ibmcloud fn` CLI 以及任何其他動作類型的動作時明確指定。

您可以從此函數建立稱為 **helloPHP** 的 {{site.data.keyword.openwhisk_short}} 動作，如下所示：
```
ibmcloud fn action create helloPHP hello.php
```
{: pre}

CLI 會從原始檔副檔名自動推斷動作類型。對於 `.php` 原始檔，動作會使用 PHP 7.1 執行時期來執行。如需相關資訊，請參閱 PHP [參照](./openwhisk_reference.html#openwhisk_ref_php)。

PHP 動作與 JavaScript 動作的動作呼叫相同：
```
ibmcloud fn action invoke --result helloPHP --param name World
```
{: pre}

輸出範例：
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### 將 PHP 動作包裝在 zip 檔案中
{: #openwhisk_actions_php_zip}

您可以將 PHP 動作及其他檔案與相依套件包裝在 zip 檔案中。包含進入點（例如，`main`）的原始檔檔名必須是 `index.php`。

例如，若要建立包含第二個檔案（稱為 `helper.php`）的動作，請先建立包含原始檔的保存檔：
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

然後，建立 **helloPHP** 動作：
```bash
ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## 建立 Swift 動作
{: #creating-swift-actions}

建立 Swift 動作的程序，與建立 JavaScript 動作的程序類似。下列各節會引導您建立及呼叫單一 Swift 動作，並將動作包裝在 zip 檔案中。

您也可以使用線上[線上 Swift 遊樂場](http://online.swiftplayground.run)來測試 Swift 程式碼，而不需要在機器上安裝 Xcode。

**注意：**Swift 動作是在 Linux 環境中執行。Linux 上的 Swift 仍在開發中，而 OpenWhisk 一般會使用最新的可用版本，但該版本不一定穩定。此外，與 OpenWhisk 搭配使用的 Swift 版本，可能與 MacOS 上穩定 Xcode 版本的 Swift 版本不一致。

### 建立及呼叫動作

#### Swift 3
動作只是最上層 Swift 函數。例如，建立稱為 **hello.swift** 且含有下列內容的檔案：

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

在此範例中，Swift 動作會取用字典，並產生字典。

您可以從此函數建立稱為 **helloSwift** 的 OpenWhisk 動作，如下所示：
```
ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

除了上述 main 函數簽章之外，Swift 4 中的新功能還有兩個充分運用 [Codable](https://developer.apple.com/documentation/swift/codable) 類型的額外現成可用簽章。您可以在[這裡](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)進一步瞭解可編碼及可解碼的資料類型，以與外部表示法相容（例如 JSON）。

下列範例接受輸入參數作為具有 `name` 欄位的 **Codable 輸入**，並傳回具有 `greetings` 欄位的 **Codable 輸出**。
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

在此範例中，Swift 動作會取用 Codable，並產生 Codable 類型。如果您不需要處理任何輸入，則可以使用不接受任何輸入但只接受 Codable 輸出的函數簽章。
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

您可以從此函數建立稱為 `helloSwift` 的 OpenWhisk 動作，如下所示：
```
ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

如需 Swift 運行環境的相關資訊，請參閱 Swift [參照](./openwhisk_reference.html#swift-actions)。

Swift 動作與 JavaScript 動作的動作呼叫相同：
```
ibmcloud fn action invoke --result helloSwift --param name World
```
{: pre}

輸出範例：
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

在[使用參數](./parameters.html)主題中，找出參數的相關資訊。

### 將動作包裝為 Swift 執行檔
{: #packaging-an-action-as-a-swift-executable}

當您使用 Swift 來源檔建立 OpenWhisk Swift 動作時，必須先將其編譯成二進位檔，才能執行動作。完成之後，後續呼叫動作時就會快很多，直到用來保存動作的容器被清除為止。這項延遲稱為冷啟動延遲。

若要避免冷啟動延遲，您可以將 Swift 檔案編譯成二進位檔，然後將它以 zip 檔案形式上傳至 OpenWhisk。在您需要 OpenWhisk 支撐時，建立二進位檔的最簡單方式就是在執行檔案的相同環境內建置檔案。

### 使用 Script 來建置 Swift 包裝的動作

您可以使用 Script 來自動化動作的包裝。使用下列範例程式碼，建立名為 `compile.sh` 的 Script 檔。
```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
    exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="ibmfunctions/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
    BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

Script 假設您有一個稱為 `actions` 的目錄，其中每一個最上層目錄都代表一個動作。
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- 建立 `Package.swift` 檔案，以新增相依關係。**附註：**Swift 3 與 Swift 4 工具的語法不同。

  Swift 3 範例語法：
  ```swift
    import PackageDescription

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
  {: codeblock}

  Swift 4 範例語法：
  ```swift
  // swift-tools-version:4.0
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
        .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
        .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
      ],
      targets: [
        .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
      ]
  )
  ```
  {: codeblock}

  如您所見，此範例會將 `example-package-deckofplayingcards` 新增為相依關係。請注意，標準 Swift 動作中會提供 `CCurl`、`Kitura-net` 及 `SwiftyJSON`，因此，您應該僅針對 Swift 3 動作將它們包括在自己的 `Package.swift` 中。

- 針對 Swift 3 動作，執行下列指令來建置動作：
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  若要針對 Swift 4 編譯，請使用 `swift:4.1`，而非 `swift:3.1.1`：
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  這已在 `build` 中建立 `hello.zip`。

- 使用動作名稱 **helloSwifty**，將它上傳至 OpenWhisk。針對 Swift 3，使用 `swift:3.1.1` 類型
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  針對 Swift 4，使用 `swift:3.1.1` 類型：
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- 若要檢查速度快多少，請執行下列指令：
  ```
  ibmcloud fn action invoke helloSwiftly --blocking
  ```
  {: pre}

  動作所需的執行時間位於 "duration" 內容中，並且與使用 **hello** 動作中的編譯步驟執行所需的時間進行比較。

### Swift 4 中的錯誤處理

使用新的 Codable 完成處理程式，您可以傳遞 Error 來指出動作中的失敗。
[Swift 中的錯誤處理](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)類似其他語言的異常狀況處理，並使用 `try、catch` 及 `throw` 關鍵字。

下列 Snippet 顯示處理錯誤的範例：
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
   completion(nil, error)
    }
}
```
{: codeblock}

## 建立 Java 動作
{: #creating-java-actions}

建立 Java 動作的程序，與建立 JavaScript 及 Swift 動作的程序類似。下列各節會引導您建立及呼叫單一 Java 動作，以及將參數新增至該動作。

若要編譯、測試及保存 Java 檔案，您必須已在本端安裝 [JDK 8](http://openjdk.java.net/install)。

### 建立及呼叫 Java 動作
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

您可以從此 JAR 檔建立稱為 **helloJava** 的 {{site.data.keyword.openwhisk_short}} 動作，如下所示：
```
ibmcloud fn action create helloJava hello.jar --main Hello
```
{: pre}

使用指令行及 `.jar` 原始檔時，您不需要指定您是在建立 Java 動作；工具是透過副檔名來判斷。

您需要使用 `--main` 來指定 main 類別的名稱。有資格的 main 類別是實作 static `main` 方法的類別。如果類別不在預設套件中，請使用 Java 完整類別名稱，例如，`--main com.example.MyMain`。

必要的話，您也可以自訂 Java 動作的方法名稱。這是透過指定動作的 Java 完整方法名稱來完成，例如，`--main com.example.MyMain#methodName`。

對於 Java 動作，動作呼叫是相同的，因為其適用於 Swift 及 JavaScript 動作：
```
ibmcloud fn action invoke --result helloJava --param name World
```
{: pre}

輸出範例：
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

## 建立 Docker 動作
{: #creating-docker-actions}

使用 {{site.data.keyword.openwhisk_short}} Docker 動作，您可以使用任何語言來撰寫動作。

您的程式碼會編譯成可執行的二進位檔，並內嵌在 Docker 映像檔中。二進位程式與系統互動的方式是從 `stdin` 取得輸入，並透過 `stdout` 回覆。

先決條件是您必須具備 Docker Hub 帳戶。若要設定免費 Docker ID 及帳戶，請前往 [Docker Hub](https://hub.docker.com)。

對於下面的指示，假設 Docker 使用者 ID 是 `janesmith`，而密碼是 `janes_password`。假設已設定 CLI，則仍需執行三個步驟才能設定供 {{site.data.keyword.openwhisk_short}} 使用的自訂二進位檔。之後，上傳的 Docker 映像檔可以當作動作使用。

1. 下載 Docker 架構。您可以使用 CLI 進行下載及安裝，如下所示：
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

    Docker 架構現在安裝在現行目錄中。
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  輸出範例：
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

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

  Docker 容器現在可以作為 {{site.data.keyword.openwhisk_short}} 動作使用：
  ```
  ibmcloud fn action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  請注意，使用 `--docker` 來建立動作。假設所有 Docker 映像檔都在 Docker Hub 上進行管理。動作會呼叫為任何其他 {{site.data.keyword.openwhisk_short}} 動作。
  ```
  ibmcloud fn action invoke --result example --param payload Rey
  ```
  {: pre}

  **呼叫輸出：**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  若要更新 Docker 動作，請執行 `buildAndPush.sh`，以將最新映像檔上傳至 Docker Hub。這可讓系統在下次執行您動作的程式碼時取回新的 Docker 映像檔。如果沒有暖容器，任何新呼叫會使用新的 Docker 映像檔。不過，如果有暖容器使用舊版 Docker 映像檔，則除非您執行 `ibmcloud fn action update`，否則任何新呼叫都會繼續使用該映像檔。這指出針對新呼叫，系統會執行 Docekr 取回，以取得新的 Docker 映像檔。

  **將最新映像檔上傳至 Docker Hub：**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **更新動作，讓新的呼叫開始使用新的映像檔：***
  ```
  ibmcloud fn action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  您可以在[參照](./openwhisk_reference.html#openwhisk_ref_docker)小節中，找到建立 Docker 動作的相關資訊。

  舊版 CLI 支援的 `--docker` 沒有參數及映像檔名稱，在以前是當成位置引數。為了容許 Docker 動作能夠使用 (zip) 檔案接受起始設定資料，將 Docker 動作的使用者經驗正規化，因此，相反地，現有的位置引數必須是檔案（例如，zip 檔案）。必須在 `--docker` 選項後面指定映像檔名稱。感謝使用者意見，會將 `--native` 引數併入來作為 `--docker openwhisk/dockerskeleton` 的速記，因此，在標準 Docker 動作 SDK 內執行的執行檔較方便您建立及部署。

  例如，本指導教學已在位於 `/action/exec` 的容器內建立二進位執行檔。如果您將此檔案複製到本端檔案系統，並將它壓縮至 `exec.zip`，則您可以使用下列指令建立一個 Docker 動作，以將執行檔接收為起始設定資料。

  **從 zip 檔案建立動作：**
  ```
  ibmcloud fn action create example exec.zip --native
  ```
  {: pre}

  這等同於下列指令。
  ```
  ibmcloud fn action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

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
{: codeblock}

將上述程式碼儲存至 `sample.go` 檔案，並交叉編譯它以用於 OpenWhisk。執行檔必須稱為 `exec`。
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
ibmcloud fn action create helloGo --native exec.zip
```
{: codeblock}

該動作可以執行為任何其他動作。
```bash
ibmcloud fn action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

以類似的方式擷取日誌。
```bash
ibmcloud fn activation logs --last --strip
my first Go action.
```

## 使用任意執行檔建立動作
{: #creating-actions-arbitrary}

使用 `--native`，您可以看到_任何_ 執行檔可執行為 OpenWhisk 動作。這包括 `bash` Script 或交叉編譯的二進位檔。如果是後者，限制是二進位檔必須與 `openwhisk/dockerskeleton` 映像檔相容。

## 監視動作輸出
{: #monitor-action-output}

其他使用者可能會呼叫 {{site.data.keyword.openwhisk_short}} 動作來回應各種事件，或是作為動作序列的一部分。在這類情況下，監視呼叫可能十分有用。

您可以使用 {{site.data.keyword.openwhisk_short}} CLI 來監看所呼叫動作的輸出。

1. 從 Shell，發出下列指令：
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  這個指令會啟動輪詢迴圈，以從啟動開始持續檢查日誌。

2. 切換至另一個視窗，然後呼叫動作：
  ```
  ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  輸出範例：
  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. 在輪詢視窗中，觀察啟動日誌：
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  同樣地，每當您執行輪詢公用程式時，都可以即時查看日誌，以尋找代表您在 OpenWhisk 中執行的所有動作。

## 取得動作
{: #getting-actions}

使用 `ibmcloud fn action` get 指令，可以擷取說明現有動作的 meta 資料。

**指令：**
```
ibmcloud fn action get hello
```

***結果：**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### 取得動作 URL
{: #get-action-url}

您可以透過 HTTPS 要求，使用 REST 介面來呼叫動作。若要取得動作 URL，請執行下列指令：
```
ibmcloud fn action get actionName --url
```
{: pre}

針對標準動作，會傳回具有下列格式的 URL：
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

針對 [Web 動作](./openwhisk_webactions.html)，會以下列格式傳回 URL：
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**附註：**針對標準動作，透過 HTTPS 要求呼叫時，必須提供鑑別。如需使用 REST 介面來呼叫動作的相關資訊，請參閱 [REST API 參照](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction)。

### 儲存動作碼
{: #save-action}

與現有動作相關聯的程式碼會在本端擷取及儲存。「儲存」可以在序列及 Docker 動作以外的所有動作上執行。

1. 將動作碼儲存至對應於現行工作目錄中現有動作名稱的檔名。會使用對應於動作類型的副檔名，或將 .zip 副檔名用於作為 zip 檔案的動作碼。
  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

  輸出範例：
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. 您可以使用 `--save-as` 旗標來提供自訂檔案路徑、檔名及副檔名，而非讓 CLI 判斷要儲存之程式碼的目的地。
  ```
  ibmcloud fn action get actionName --save-as codeFile.js
  ```
  {: pre}

  輸出範例：
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## 列出動作
{: #listing-actions}

您可以列出使用下列指令所建立的所有動作：
```
ibmcloud fn action list
```
{: pre}

隨著您撰寫愈多動作，此清單也會變得愈長，而其有助於將相關的動作分組成[套件](./openwhisk_packages.html)。若要將您的動作清單過濾為只有特定套件中的動作，您可以使用下列指令語法：
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

## 刪除動作
{: #deleting-actions}

您可以藉由刪除不要使用的動作來進行清除。

1. 執行下列指令，以刪除動作：
  ```
  ibmcloud fn action delete hello
  ```
  {: pre}

  輸出範例：
  ```
  ok: deleted hello
  ```
  {: screen}

2. 驗證動作不再出現於動作清單中。
  
  ```
  ibmcloud fn action list
  ```
  {: pre}

  輸出範例：
  ```
  actions
  ```
  {: screen}

## 大型應用程式支援
{: #large-app-support}

動作的程式碼大小上限為 48 MB。包含許多協力廠商模組、原生程式庫或外部工具的應用程式可能會執行此限制。

如果您要建立大於 48 MB 的套件動作（zip 或 jar），則解決方案是擴充具有相依關係的運行環境映像檔，然後使用單一原始檔或小於 48 MB 的保存檔。

例如，透過建置包含必要共用程式庫的自訂 Docker 執行時期，這些相依關係並不需要存在於保存檔中。專用原始檔仍可組合在保存檔中，並在執行時期注入。

減少保存檔大小的另一個好處是，部署時間也能得到改善。下列各節提供兩個運行環境範例，以示範如何運用這項技術來減少應用程式大小。

### Python 範例

針對 Python 應用程式，請參閱下列步驟來減少其程式碼大小。

1. 將程式庫 `opencv-python` 放在 opencv 中。
2. 然後，將 opencv 二進位檔安裝至 OS 映像檔。
3. 然後，您可以使用 `requirements.txt` 並執行 `pip install requirements.txt`，以使用更多 Python 程式庫來擴增映像檔。
4. 然後，您可以搭配使用 `action.py` 與新映像檔。

### Node.js 範例

若要減少 Node.js 應用程式的應用程式大小，請參閱下列步驟，以在 OS 映像檔中安裝額外的套件：

1. 使用 `npm` 來安裝 opencv：
   ```
npm install opencv
```
   {: pre}

2. 同樣地，如果您有 `package.json`，請使用 `npm` 來進行安裝：
   ```
npm install package.json
```
   {: pre}

3. 然後，繼續搭配使用 `action.js` 與新映像檔。

## 存取動作內文內的動作 meta 資料
{: #accessing-action-metadata-within-the-action-body}

動作環境包含執行中動作特有的數個內容。這些內容容許動作透過 REST API 以程式設計方式使用 OpenWhisk 資產，或設定在動作即將用完其分配時間預算時的內部警示。使用 OpenWhisk Docker 架構時，可以在所有支援運行環境的系統環境中存取這些內容：Node.js、Python、Swift、Java 及 Docker 動作。

* `__OW_API_HOST`：執行此動作之 OpenWhisk 部署的 API 主機
* `__OW_API_KEY`：呼叫動作之主題的 API 金鑰，此金鑰可能是受限 API 金鑰
* `__OW_NAMESPACE`：_啟動_ 的名稱空間（這可能與動作的名稱空間不同）
* `__OW_ACTION_NAME`：執行中動作的完整名稱
* `__OW_ACTIVATION_ID`：此執行中動作實例的啟動 ID
* `__OW_DEADLINE`：此動作將用完其整個持續期間配額的大約時間（以新紀元毫秒測量）
