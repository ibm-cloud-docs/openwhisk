---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 建立及呼叫動作
{: #openwhisk_actions}

動作是在 {{site.data.keyword.openwhisk}} 平台上執行的 Stateless 程式碼 Snippet。例如，動作可用來偵測影像中的臉孔、回應資料庫變更、聚集一組 API 呼叫，或張貼推文。
{:shortdesc}

可以明確地呼叫動作，或為回應某事件而執行動作。在任一情況下，每次執行動作都會導致由唯一啟動 ID 所識別的啟動記錄。動作的輸入及動作的結果是鍵值組的字典，其中索引鍵是字串，而值是有效的 JSON 值。動作也可以由其他動作的呼叫或已定義的動作序列組成。

動作可以寫成 JavaScript、Swift、Python、PHP 函數、Java 方法或任何二進位相容執行檔，例如 Go 程式及包裝成 Docker 容器的自訂可執行檔。瞭解如何在偏好的開發環境中建立、呼叫及除錯動作。

## 建立 JavaScript 動作
{: #creating-and-invoking-javascript-actions}

下列各節會引導您透過 JavaScript 逐步執行動作。首先，請建立並呼叫簡單的動作。然後，在動作中新增參數，並使用參數來呼叫該動作。接下來，設定並呼叫預設參數。最後，建立非同步動作。

### 建立及呼叫簡單 JavaScript 動作
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

2. 使用下列 JavaScript 函數建立稱為 `hello` 的動作。
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

4. 執行區塊處理呼叫，在雲端執行動作。區塊處理呼叫使用要求/回應樣式，並會等待啟動結果可供使用。等待期間少於 60 秒，或是動作的[時間限制值](./openwhisk_reference.html#openwhisk_syslimits)。
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
    如需監視啟動的提示，請參閱[監視動作輸出](openwhisk_managing.html#monitor-action-output)。
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

### 建立非同步動作
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
    ibmcloud fn action create asyncAction asyncAction.js
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

3. 取得啟動日誌，以檢查啟動需要多久時間才能完成。

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

### 使用動作來呼叫外部 API
{: #openwhisk_apicall_action}

到目前為止，這些提供的範例自行包含 JavaScript 函數。您也可以建立呼叫外部 API 的動作。
{: shortdesc}

下列範例呼叫「NASA 每日一天文圖 (APOD)」服務，每日提供一張宇宙的獨特影像。

1. 將下列程式碼儲存至名為 `apod.js` 的檔案中。
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

    呼叫 NASA APOD API，即會從 JSON 結果擷取欄位。如需您可以在動作中使用哪些 Node.js 套件的相關資訊，請參閱[系統詳細資料及限制](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。

2. 建立稱為 `apod` 的動作。
    ```
    ibmcloud fn action create apod apod.js
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

### 將動作包裝為 Node.js 模組
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
    * 您必須將這個檔案命名為 **index.js**，或是將您喜好的檔名指定為 **package.json** 中的 `main` 內容。

3. 在本端安裝所有相依關係。
    ```
    npm install
    ```
    {: pre}
    **附註：**雖然大部分 `npm` 套件都會在 `npm install` 上安裝 JavaScript 原始檔，但是有一部分也會安裝及編譯二進位構件。保存檔上傳目前只支援 JavaScript 相依關係。如果保存檔包括二進位相依關係，則動作呼叫可能會失敗。

4. 建立包含所有檔案的 `.zip` 保存檔，包括所有相依關係。
    ```
    zip -r action.zip *
    ```
    {: pre}

    使用「Windows 檔案總管」動作來建立 zip 檔案會導致結構不正確。{{site.data.keyword.openwhisk_short}} zip 動作必須將 `package.json` 置於 zip 的根目錄，而「Windows 檔案總管」會將它放在巢狀資料夾內。最安全的選項是使用指令行 `zip` 指令。
  {: tip}

5. 建立動作。從 `.zip` 保存檔建立動作時，必須使用 `nodejs:6` 或 `nodejs:8`，明確地提供 `--kind` 旗標的值。
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### 將動作包裝為單一組合
{: #openwhisk_js_webpack_action}

如果將動作包裝成 zip 檔包含太多不必要的檔案，或您需要更快的部署，則可以將最少的程式碼寫入包含相依關係的單一 `.js` 檔案。
{: shortdesc}

您可以使用 JavaScript 模組組合程式，例如 [webpack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://webpack.js.org/concepts/) 來包裝動作。當 `webpack` 處理您的程式碼時，它會遞迴地建置相依關係圖形，其中包含您的動作所需的每個模組。

1. 將下列程式碼儲存至名為 `package.json` 的檔案中。`webpack` 新增為開發相依關係。
    ```json
    {
      "name": "my-action",
      "main": "dist/bundle.js",
      "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

    * 使用 CLI：
        ```
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **附註**：`webpack` 所建置的組合檔案僅支援 JavaScript 相依關係。如果組合相依於二進位相依關係，則動作呼叫可能會失敗，因為這並不包含在 `bundle.js` 檔案中。

## 建立 Python 動作
{: #creating-python-actions}

下列各節會引導您建立及呼叫單一 Python 動作，以及將參數新增至該動作。

### 建立及呼叫 Python 動作
{: #openwhisk_actions_python_invoke}

動作只是最上層 Python 函數。若要建立 Python 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.py` 的檔案中。
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
    ```
    {: codeblock}

  * Python 動作一律會使用某個字典，並產生一個字典。
  * 動作的進入方法依預設是 `main`，但可以使用 `--main` 旗標，以 `wsk` CLI 明確指定來建立動作。

2. 建立 `helloPython` 動作。
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    輸出範例：
    ```
    ok: created action helloPython
    ```
    {: screen}

      CLI 會使用原始檔副檔名自動推斷動作的類型。對於 `.py` 原始檔，動作會使用 Python 2 執行時期來執行。您也可以明確地指定參數 `--kind python:3`，來建立與 Python 3 一起執行的動作。您也可以使用類型為 `python-jessie:3` 的 Python 3 運行環境，它包含 IBM Cloud Services 的額外套件，例如 {{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}} 及 {{site.data.keyword.ibmwatson_notm}}。如需此 Python 3 執行時期中所含套件的相關資訊，請參閱 Python 執行時期[參照](./openwhisk_reference.html#openwhisk_ref_python_environments)。

3. 呼叫動作。
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

您可以將 Python 動作及相依模組包裝在 zip 檔案中。例如，若要建立 helper 模組稱為 `helper.py` 的動作，請執行下列動作：

1. 建立包含原始檔的保存檔。**附註**：包含進入點的原始檔必須名為 `__main__.py`。
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 建立動作。
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### 使用 zip 檔案中的虛擬環境來包裝 Python 動作
{: #openwhisk_actions_python_virtualenv}

您可以使用虛擬環境 `virtualenv` 來包裝 Python 相依關係。虛擬環境容許您鏈結其他套件，這些套件可以使用例如 [`pip` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://packaging.python.org/installing/) 來安裝。

若要安裝相依關係，請將它們包裝在虛擬環境中，然後建立相容的 OpenWhisk 動作：

1. 建立 [requirements.txt ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 檔，其中包含要安裝的 `pip` 模組和版本。

2. 安裝相依關係並建立虛擬環境。虛擬環境目錄必須命名為 `virtualenv`。若要確保與 OpenWhisk 執行時期容器的相容性，虛擬環境內的套件安裝必須使用對應於該類型的映像檔。
    - 對於類型 `python:2`，請使用 Docker 映像檔 `openwhisk/python2action`。
    - 對於類型 `python:3`，請使用 Docker 映像檔 `openwhisk/python3action`。
    - 對於類型 `python-jessie:3`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3`。
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

3. 包裝 `virtualenv` 目錄及任何其他 Python 檔案。包含進入點的原始檔必須名為 `__main__.py`。
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. 建立動作 `helloPython`。
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

只將不屬於所選取運行環境的模組新增至 `requirements.txt`。這有助於將 `virtualenv` 保持為最小。
{: tip}

## 建立 PHP 動作
{: #creating-php-actions}

下列各節會引導您建立及呼叫單一 PHP 動作，以及將參數新增至該動作。

### 建立及呼叫 PHP 動作
{: #openwhisk_actions_php_invoke}

動作只是最上層 PHP 函數。若要建立 PHP 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.php` 的檔案中。
    ```
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

    * PHP 動作一律使用聯合陣列，並傳回聯合陣列。
    * 動作的進入方法依預設是 `main`，但可以在您使用 `--main` 旗標，以 `ibmcloud fn` CLI 建立動作時明確指定。

2. 建立稱為 `helloPHP` 的動作。
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    CLI 會從原始檔副檔名自動推斷動作類型。對於 `.php` 原始檔，動作會使用 PHP 7.1 執行時期來執行。如需相關資訊，請參閱 PHP [參照](./openwhisk_reference.html#openwhisk_ref_php)。

3. 呼叫動作。
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

您可以將 PHP 動作及其他檔案或相依套件包裝在 zip 檔案中。例如，若要以第二個稱為 `helper.php` 的檔案包裝動作，請執行下列動作：

1. 建立包含原始檔的保存檔。**附註**：包含進入點的原始檔必須名為 `index.php`。
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. 建立動作。
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## 建立 Swift 動作
{: #creating-swift-actions}

下列各節會引導您建立及呼叫單一 Swift 動作，並將動作包裝在 zip 檔案中。

**附註：**Swift 動作是在 Linux 環境中執行。Linux 上的 Swift 仍在開發中，而 {{site.data.keyword.openwhisk_short}} 會使用最新的可用版本。這些版本不一定穩定。與 {{site.data.keyword.openwhisk_short}} 搭配使用的 Swift 版本，可能與 MacOS 上穩定 Xcode 版本的 Swift 版本不一致。

如需 Swift 運行環境的相關資訊，請參閱 Swift [參考資料](./openwhisk_reference.html#swift-actions)。
{: tip}

### 建立及呼叫動作
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

動作只是最上層 Swift 函數。若要建立 Swift 3 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.swift` 的檔案中。
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

2. 建立稱為 `helloSwift` 的動作。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    CLI 會從原始檔副檔名自動推斷動作類型。對於 `.php` 原始檔，動作會使用 PHP 7.1 執行時期來執行。如需相關資訊，請參閱 PHP [參照](./openwhisk_reference.html#openwhisk_ref_php)。

3. 呼叫動作。
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

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

除了 main 函數簽章之外，Swift 4 還提供另外兩種利用 [Codable ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/documentation/swift/codable) 類型的簽章。您可以在[這裡 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 進一步瞭解可編碼及可解碼的資料類型，以與外部表示法相容（例如 JSON）。

1. 將下列程式碼儲存至稱為 `hello.swift` 的檔案中。
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
    此範例接受輸入參數作為具有 `name` 欄位的 `Codable 輸入`，並傳回具有 `greetings` 欄位的 `Codable 輸出`。

2. 建立稱為 `helloSwift` 的動作。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. 呼叫動作。
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

### 將動作包裝為 Swift 執行檔
{: #packaging-an-action-as-a-swift-executable}

當您使用 Swift 來源檔建立 {{site.data.keyword.openwhisk_short}} Swift 動作時，必須先將檔案編譯成二進位檔，才能執行動作。這項延遲稱為冷啟動延遲。建立二進位檔之後，後續呼叫動作時就會快很多，直到用來保存動作的容器被清除為止。若要避免冷啟動延遲，您可以將 Swift 檔案編譯成二進位檔，然後將二進位檔以 zip 檔案形式上傳至 {{site.data.keyword.openwhisk_short}}。

您可以使用 Script 來自動化動作的包裝。

**必要條件**：下列步驟中使用的 Script 假設您有一個稱為 `actions` 的目錄，其中每一個最上層目錄都代表一個動作。
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. 將下列程式碼儲存至名為 `compile.sh` 的 Script 檔中。
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

2. 若要新增相依關係，請建立 `Package.swift` 檔。此範例會將 `example-package-deckofplayingcards` 新增為相依關係。標準 Swift 動作中會提供 `CCurl`、`Kitura-net` 及 `SwiftyJSON`，因此，您應該僅針對 Swift 3 動作將它們包括在自己的 `Package.swift` 中。
    * Swift 3 範例語法：
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

    * Swift 4 範例語法：
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

3. 若要在 `build` 中建立 `hello.zip`，請建置動作。
    * Swift 3：
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4：
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. 使用動作名稱 `helloSwiftly`，將 zip 上傳至 {{site.data.keyword.openwhisk_short}}。
    * Swift 3：
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4：
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. 呼叫動作。
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    動作所需的執行時間位於 `duration1` 內容中。

6. 您可以比較預先編譯動作呼叫的持續期間，與使用編譯步驟的指令呼叫持續期間。從前一節呼叫動作：
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Swift 4 中的錯誤處理
{: #error-handling-swift4}

藉由使用 Codable 完成處理程式，您可以傳遞錯誤來指出動作中的失敗。[Swift 中的錯誤處理 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) 類似於其他語言的異常狀況處理，使用 `try`、`catch` 及 `throw` 等關鍵字。

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

下列各節會引導您建立及呼叫單一 Java 動作，以及將參數新增至該動作。

若要編譯、測試及保存 Java 檔案，您必須已在本端安裝 [JDK 8 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://openjdk.java.net/install)。

### 建立及呼叫 Java 動作
{: #openwhisk_actions_java_invoke}

Java 動作是一種 Java 程式，搭配稱為 `main` 的方法。`main` 必須具有下列確切的簽章：
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

若要建立 Java 動作，請執行下列動作：

1. 將下列程式碼儲存至名為 `Hello.java` 的檔案中。
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

2. 將 `Hello.java` 編譯成名為 `hello.jar` 的 JAR 檔。**附註**：[google-gson ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/google/gson) 必須存在於您的 Java CLASSPATH。
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. 建立動作。
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * 您必須使用 `--main` 來指定 main 類別的名稱。有資格的 main 類別是實作 static `main` 方法的類別。如果類別不在預設套件中，請使用 Java 完整類別名稱，例如，`--main com.example.MyMain`。
  * 您可以自訂 Java 動作的方法名稱。這是透過指定動作的完整方法名稱來完成，例如，`--main com.example.MyMain#methodName`。
  * CLI 會從原始檔副檔名自動推斷動作類型。

4. 呼叫動作。
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
{: shortdesc}

您的程式碼會編譯成可執行的二進位檔，並內嵌在 Docker 映像檔中。二進位程式與系統互動的方式是從 `stdin` 取得輸入，並透過 `stdout` 回覆。您可以在[參照](./openwhisk_reference.html#openwhisk_ref_docker)小節中，找到建立 Docker 動作的相關資訊。

必要條件：您必須具備 Docker Hub 帳戶。在 [Docker Hub ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com) 設定免費 Docker ID 及帳戶。

若要設定自訂二進位檔，並使用上傳的 Docker 映像檔作為動作，請執行下列動作：

1. 下載並安裝 Docker 架構。架構是一個 Docker 容器範本，您可以在其中以自訂二進位檔形式來注入程式碼。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在 blackbox 架構中，設定您的自訂二進位檔。此架構包含您可以使用的 C 程式。`example.c` 檔案的一部分會在 Docker 映像檔建置程序之中編譯，因此不需要在機器上編譯 C。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  輸出範例：
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 選用項目：修改 `Dockerfile` 以新增其他程式碼和相依關係至 Docker 映像檔，以建置您的執行檔。請注意下列需求及限制：
  * 二進位檔必須位在 `/action/exec` 的容器內。
  * 執行檔會從指令行收到單一引數。這個引數是代表動作引數之 JSON 物件的字串序列化。
  * 程式可能會記載至 `stdout` 或 `stderr`。
  * 依照慣例，輸出的最後一行必須是代表動作結果的字串化 JSON 物件。

4. 建置 Docker 映像檔，並使用提供的 Script 予以上傳。
    1. 登入 Docker。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. 前往 `dockerSkeleton` 目錄。
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. 執行 Script。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. 使用您的 Docker 容器來建立動作。
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. 呼叫動作。
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    輸出範例：
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}

7. 若要更新 Docker 動作，請將最新映像檔上傳至 Docker Hub。這可讓系統在下次執行您動作的程式碼時取回新的 Docker 映像檔。
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. 如果有暖容器使用舊版 Docker 映像檔，任何新呼叫都會繼續使用該映像檔。請更新動作，讓新的呼叫開始使用新的映像檔。
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. 選用：您可以使用 `--native` 引數，作為 `--docker openwhisk/dockerskeleton` 的速記。這個引數可讓您更輕鬆地建立及部署在標準 Docker 動作 SDK 內執行的執行檔。
    1. 述步驟會在位於 `/action/exec` 的容器內建立二進位執行檔。請將 `/action/exec` 檔案複製到本端檔案系統，並將它壓縮成 `exec.zip`。
    2. 建立可將執行檔當作起始設定資料來接收的 Docker 動作。`--native` 引數會取代 `--docker openwhisk/dockerskeleton` 引數。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## 建立 Go 動作
{: #creating-go-actions}

`--native` 引數可讓您將任何 Go 執行檔包裝為動作。

請注意下列需求及限制。
  * Go 執行檔會從指令行收到單一引數。引數是代表動作引數之 JSON 物件的字串序列化。
  * 程式可能會記載至 `stdout` 或 `stderr`。
  * 依照慣例，輸出的最後一行必須是代表動作結果的字串化 JSON 物件。

若要建立 Go 動作，請執行下列動作：

1. 將下列程式碼儲存至名為 `sample.go` 的檔案中。
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

2. 針對 {{site.data.keyword.openwhisk_short}} 交叉編譯 `sample.go`。執行檔必須稱為 `exec`。
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. 呼叫動作。
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## 建立動作序列
{: #openwhisk_create_action_sequence}

您可以建立一個動作，將一連串的動作鏈結在一起。將某個動作的結果當作下一個動作的引數來傳遞。
{: shortdesc}

在 `/whisk.system/utils` 套件中提供了數個公用程式動作，可用來建立第一個序列。

1. 列出 `/whisk.system/utils` 套件中的動作。
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

2. 使用 `split` 和 `sort` 動作，建立動作序列，以將 `split` 的結果當成引數傳給 `sort`。此動作序列會將數行文字轉換為一個陣列，並排序這些行。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 呼叫動作。
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    在輸出中，會依字母順序來排序分割行。
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

**附註**：
* 序列中動作之間所傳遞的參數十分明確，但預設參數除外。因此，傳遞給動作序列的參數僅適用於序列中的第一個動作。序列中第一個動作的結果會變成序列中第二個動作的輸入 JSON 物件，以此類推。此物件不會包括一開始傳遞給序列的任何參數，除非第一個動作將它們明確地包括在結果中。動作的輸入參數會與動作的預設參數合併，而前者的優先順序較高，並且會置換任何相符的預設參數。如需使用多個具名參數來呼叫動作序列的相關資訊，請參閱[在動作上設定預設參數](./parameters.html#default-params-action)。
* 序列不會具有與序列中每個動作之逾時分開的整體逾時。因為序列是作業管線，所以一個動作失敗將會中斷管線。如果有一個動作逾時，則整個序列會因為該失敗而結束。

## 管理大型動作
{: #large-app-support}

動作的程式碼大小上限為 48 MB。包含許多協力廠商模組、原生程式庫或外部工具的應用程式可能會達到此限制。如果您建立大於 48 MB 的 .zip 或 .jar 套件動作，您必須擴充具有相依關係的運行環境映像檔，然後使用單一原始檔或小於 48 MB 的保存檔。

例如，透過建置包含必要共用程式庫的自訂 Docker 運行環境，相依關係並不需要存在於保存檔中。專用原始檔仍可組合在保存檔中，並在執行時期注入。

### 減少動作的大小
{: #large-app-reduce}

若要減少 Python 應用程式的程式碼大小，請執行下列動作：

1. 將程式庫 `opencv-python` 放在 `opencv` 中。
2. 將 opencv 二進位檔安裝至 OS 映像檔。
3. 執行 `pip install requirements.txt` 以更多 Python 程式庫擴增映像檔。
4. 搭配使用 `action.py` 與新映像檔。

若要減少 Node.js 應用程式的程式碼大小，請執行下列動作：

1. 安裝 `opencv`。
   ```
   npm install opencv
   ```
   {: pre}

2. 安裝 `package.json`。
   ```
   npm install package.json
   ```
   {: pre}

3. 搭配使用 `action.js` 與新映像檔。
