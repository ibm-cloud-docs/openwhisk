---

copyright:
years: 2017, 2019
lastupdated: "2019-03-08"

keywords: composer, openwhisk, compositions, sequence, branch

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

# 使用 Composer 建立動作組合
{: #openwhisk_composer}

{{site.data.keyword.openwhisk}} 現在支援 Composer for Apache OpenWhisk 作為技術預覽。Composer 會使用其他組合器（[JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md)、[Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)）來擴充 Apache OpenWhisk 序列，以讓您建立包括條件式分支、錯誤處理及迴圈的較複雜流程。您可以在 [Composer Git 儲存庫](https://github.com/apache/incubator-openwhisk-composer)上取得 Composer 的完整文件及技術規格。

您也可以使用開放程式碼專案 [Kui](https://github.com/ibm/kui) 輕鬆地建立、部署及視覺化 Composer 原始碼。如需如何搭配使用 Kui 與 Composer 的相關資訊，請參閱 [Kui on Github](https://github.com/ibm/kui)。
{: note}

## 安裝 JavaScript 或 Python 3 的 Composer 程式庫
{: #install_composer}

您可以搭配使用 Composer 與以任何語言撰寫的動作，但以 JavaScript 或 Python 3 表達組合。安裝之後，您可以使用 Composer `compose/pycompose` 及 `deploy/pydeploy` 指令來[配置及執行組合](#run)。
{: shortdesc}

**JavaScript**：
1. 使用 Node Package Manager 來安裝 Composer 程式庫 [Node.js 套件](https://github.com/apache/incubator-openwhisk-composer)。

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  確認您已執行 Composer 指令的說明來安裝程式庫。

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    輸出範例：
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Python 3**：使用 `pip3` 來安裝 [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python) 程式庫。

1.  複製 Composer for Python 3 GitHub 儲存庫。
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  導覽至 composer 目錄。
    ```
    cd composer-python
    ```
    {: pre}
3.  安裝 Composer 程式庫。請包括句點 (`.`)，讓指令在您所在的目錄內尋找。
    ```
    pip3 install -e .
    ```
    {: pre}
4.  確認您已執行 Composer 指令的說明來安裝程式庫。
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    輸出範例：
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## 在 IBM Cloud Functions 中配置及執行組合
{: #run}

您可以使用 JavaScript 或 Python 3 Composer 程式庫，在 {{site.data.keyword.openwhisk}} 中建立組合。使用 `compose` 或 `pycompose` 來編譯組合原始碼，然後使用 `deploy` 或 `pydeploy` 將組合部署至 {{site.data.keyword.openwhisk}}。配置組合之後，您可以在 {{site.data.keyword.openwhisk}} 中執行它。
{: shortdesc}

**開始之前**：依預設，部署會使用 `~/.wskprops` 中設定的值。將兩個參數設為 Composer `deploy` 或 `pydeploy` 指令的輸入，以置換預設值。

1.  將 API 主機設為 {{site.data.keyword.openwhisk}} 端點。
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  新增 `wsk` CLI 鑑別金鑰。
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**若要執行組合**，請執行下列動作：

1.  使用 NodeJS 或 Python 3 程式庫來建立 Composer 原始碼。例如，建立 `demo.js` 檔案。
2.  將 Composer 原始碼編譯為 JSON 檔案。
    *   在 JavaScript 中：
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   在 Python 3 中：
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  將程式碼部署至 {{site.data.keyword.openwhisk}}。
    *   在 JavaScript 中：包括 `-w` 旗標，以改寫任何名為 `demo` 的現有部署。
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   在 Python 3 中：包括 `-w` 旗標，以改寫任何名為 `demo` 的現有部署。
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  執行組合的方式，與在 {{site.data.keyword.openwhisk}} 中[呼叫其他動作](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers)的方式相同。
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} 會執行您部署為特殊動作類型的程式碼。如需詳細資料，請參閱有關 [Conductor 動作](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md)的文件。

## 使用 Composer 擴充序列
{: #extending}

Apache OpenWhisk 可讓您在 `sequence` 中將函數鏈結在一起，而其中一個動作的輸出會成為另一個動作的輸入。

### 未使用 Composer 的序列
{: #sequences-without-composer}
您可以在 {{site.data.keyword.openwhisk_short}} 中將名為 `action1` 和 `action2` 的兩個函數鏈結在一起：

`ibmcloud fn action create --sequence mysequence action1 action2`。

此指令的結果是稱為 `mysequence` 的函數，其為 `action1` 和 `action2` 的複合項。您可以使用與 OpenWhisk 中任何函數相同的方式來使用 `mysequence`。

### 使用 Composer 的序列
{: #sequences-with-composer}
在 Composer 中，您可以使用原始碼而非指令行來指定更豐富的序列。

JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Python 3：
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="簡單序列" alt="具有兩個動作的序列" style="width:250px; border-style: none"/></br>
_圖 1. 具有兩個動作的序列_

您不受限於使用 Composer 將函數鏈結在一起。Composer 包括一系列的 [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 或 [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) 型組合器，可加強序列的表達。您可以在下列各節中查看一般範例。

### 錯誤處理
{: #error-handling}
您可以使用 `try-catch-finally` 區塊來新增序列的錯誤處理。在此範例中，使用 try 來括住序列。如果任一動作傳回錯誤，則會執行 `handleError` 程式碼。

JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Python 3：
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Try 序列" alt="具有錯誤處理的序列" style="width:400px; border-style: none"/></br>
_圖 2. 具有錯誤處理的序列_

### 條件式分支
{: #conditional-branch}
您可以使用 `if-then-else` 來建立分支序列。此範例示範 `if-then-else`。`action1` 應該傳回布林值。如果 `true`，則會執行 `action2`，否則會執行 `action3`。請注意，`action3` 是選用項目，可對 `if-then` 予以省略。

JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Python 3：
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If 序列" alt="具有條件式分支的序列" style="width:250px; border-style: none"/></br>
_圖 3. 具有條件式分支的序列_

### 迴圈
{: #loop}
您可以在 Composer 中建立迴圈建構。在此範例中，只要 `action1` 傳回 `true`，就會執行 `action2`。Composer 會限制您可依編製序列執行的步驟總數。現行限制是 20。

JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Python 3：
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While 序列" alt="具有 while 迴圈的序列" style="width:250px; border-style: none"/></br>
_圖 4. 具有 `while` 迴圈的序列_

### 動作的行內定義
{: #inline-def}
您可以在組合程式碼本身內定義動作。在此範例中，您使用 `composer.action()` 來建立動作定義，且行內具有名為 `hello` 的組合。

JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Python 3：
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="While 序列" alt="具有行內動作定義的序列" style="width:250px; border-style: none"/></br>
_圖 5. 具有行內動作定義的序列_

## 使用其他組合器定義
{: #combinator-def}
如需完整的組合器定義清單，請參閱 Composer on Apache OpenWhisk（[JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 或 [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)）文件。
