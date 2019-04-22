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


# 创建 JavaScript 操作
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

以下各部分指导您使用以 JavaScript 编写的操作。首先，创建和调用简单操作。然后，向该操作添加参数，并使用参数来调用该操作。接下来，设置缺省参数并调用这些参数。最后，创建异步操作。
{: shortdesc}

## 创建并调用简单 JavaScript 操作
{: #single_action_js}
{: #openwhisk_single_action_js}

查看以下步骤和示例以创建第一个 JavaScript 操作。

1. 将以下代码保存在名为 `hello.js` 的文件中。

  ```javascript
function main() {
    return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  JavaScript 文件可能包含其他函数。但是，根据约定，名为 `main` 的函数必须存在，才能为操作提供入口点。
  {: shortdesc}

2. 使用 JavaScript 函数创建名为 `hello` 的操作。

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  示例输出：
  
  ```
  ok: created action hello
  ```
  {: screen}

  操作类型使用源文件扩展名来确定。对于 `.js` 源文件，操作会使用 Node.js 运行时运行。通过将 `--kind` 参数设置为 `nodejs:10` 或 `nodejs:8`，可以为 JavaScript 操作指定 Node.js 运行时版本。有关 Node.js 运行时的更多信息，请参阅[运行时](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)。
  {: shortdesc}

3. 验证 `hello` 操作是否在操作列表中。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  示例输出：
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. 在云中，通过运行阻塞性调用来运行操作。阻塞性调用使用请求/响应样式，并等待激活结果可用。等待时间段为 60 秒或操作的[时间限制值](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)（两者取较短的时间）。
    

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    此命令输出以下信息：
        * 激活标识 (`44794bd6aab74415b4e42a308d880e5b`)，可用于检索调用的日志或结果
        * 如果激活结果在预期的等待时间段内可用，那么输出调用结果
    

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
    
    有关激活监视的技巧，请参阅[监视操作输出](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output)。
    {: tip}

5. 如果不是立即需要操作结果，可以省略 `--blocking` 标志以运行非阻塞性调用。

    1. 运行非阻塞性调用。
        

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        示例输出：

        ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
        {: screen}

    2. 使用激活标识来获取操作结果。
        

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        将返回操作结果：
        

        ```
        {
            "payload": "Hello world"
  }
  ```
        {: screen}

6. 如果忘记记录激活标识，那么可以获取激活列表，其中激活按从最新到最旧的顺序列出。
    

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    输出：
    ```
      activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```
    {: screen}
    
### Javascript 函数原型
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript 操作在 Node.js 运行时中运行。

用 JavaScript 编写的操作必须限制为单个文件。该文件可以包含多个函数，但根据约定，必须存在名为 `main` 的函数，并且此函数是调用操作时调用的函数。例如，以下示例显示具有多个函数的操作。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

操作输入参数会以 JSON 对象形式作为参数传递到 `main` 函数。成功调用的结果也是 JSON 对象，但返回方式根据操作是同步还是异步而有所不同，如以下部分中所述。

### 同步和异步行为
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 函数即便返回之后，仍在回调函数中继续执行是很常见的情况。要允许此行为，JavaScript 操作的激活可以是*同步*或*异步*的。

如果 main 函数在下列其中一个条件下退出，那么 JavaScript 操作的激活是**同步**的：

- main 函数在不执行 `return` 语句的情况下退出。
- main 函数通过执行 `return` 语句退出，此语句返回*除* Promise 以外的任何值。

请参阅以下同步操作示例：

```javascript
// 其中每个路径生成同步激活的操作
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

如果 main 函数通过返回 Promise 退出，那么 JavaScript 操作的激活是**异步**的。在此情况下，系统假定操作仍在运行，直到履行或拒绝 Promise 为止。通过实例化新 Promise 对象并向其传递回调函数开始。回调函数采用两个自变量 resolve 和 reject，这两个自变量都是函数。所有异步代码都会进入该回调函数。

在以下示例中，可以查看如何通过调用 resolve 函数来履行 Promise。
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

以下示例显示如何通过调用 reject 函数来拒绝 Promise。
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

一个操作可以在某些输入上是同步的，而在其他输入上是异步的，如以下示例中所示。
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

不管激活是同步还是异步的，操作的调用都可以是阻塞性或非阻塞性的。

## 创建异步操作
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

通过在操作中返回 Promise，异步运行的 JavaScript 函数可在 `main` 函数返回后返回激活结果。

1. 将以下代码保存在名为 `asyncAction.js` 的文件中。
    

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

    * `main` 函数会返回 Promise。Promise 指示激活尚未完成，而是预期在未来完成。

    * `setTimeout()` JavaScript 函数会等待 2 秒再调用 Promise 的回调函数，这代表异步代码。

    * Promise 的回调函数接受自变量 `resolve` 和 `reject`，这两个自变量都是函数。

      * 对 `resolve()` 的调用会履行 Promise 并指示激活已正常完成。
      * 对 `reject()` 的调用可用于拒绝 Promise 并指示激活异常完成。

2. 创建名为 `asyncAction` 的操作。
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    示例输出：

    ```
    {
        "done": true
  }
  ```
    {: screen}

4. 通过获取激活日志来检查完成激活所用的时间。

  1. 获取激活标识。
      

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      示例输出：
    ```
        activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```
      {: screen}

  2. 获取激活标识的日志。
      

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration` 显示完成此激活所用时间略长于 2 秒：
      

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

## 使用操作来调用外部 API
{: #apicall_action}
{: #openwhisk_apicall_action}

到目前为止，提供的示例都是自包含 JavaScript 函数。您还可以创建调用外部 API 的操作。
{: shortdesc}

以下示例调用 NASA Astronomy Picture of the Day (APOD) 服务，该服务每天提供我们宇宙的独特图像。

1. 将以下代码保存在名为 `apod.js` 的文件中。
    

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

2. 将对 NASA APOD API 进行调用，并从 JSON 结果中抽取字段。有关可在操作中使用的 Node.js 包的更多信息，请参阅[运行时](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)。

3. 创建名为 `apod` 的操作。
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. 调用 `apod` 操作。
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    将返回以下示例对象：
    

    ```
    {
      "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "照片中这样的景象真的只在有蓝月亮的时候才会出现吗？...",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
    {: screen}

## 将操作包装为 Node.js 模块
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

作为在单个 JavaScript 源文件中编写所有操作码的替代方法，可以将操作编写为 `npm` 包。

例如，假设一个目录包含以下文件：

1. 将以下代码保存在名为 `package.json` 的文件中。

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

2. 将以下代码保存在名为 `index.js` 的文件中。

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

    exports.main = myAction;
    ```
    {: codeblock}

    * 操作将通过 `exports.main` 公开。
    * 操作处理程序可以具有任何名称，只要名称符合接受对象和返回对象（或对象的 `Promise`）的常规特征符即可。
    * 必须将此文件命名为 `index.js`，或者将首选文件名指定为 `package.json` 中的 `main` 属性。

3. 在本地安装所有依赖项。
    

    ```
  npm install
  ```
    {: pre}

    **注**：虽然大部分 `npm` 包会在执行 `npm install` 时安装 JavaScript 源代码，但还有些 npm 包会安装并编译二进制工件。归档文件上传只支持 JavaScript 依赖项。如果归档包含二进制依赖项，那么操作调用可能会失败。

4. 创建包含所有文件（包括所有依赖项）的 `.zip` 归档。
    

    ```
  zip -r action.zip *
  ```
    {: pre}

    使用 Windows 资源管理器操作来创建 zip 文件将导致结构不正确。{{site.data.keyword.openwhisk_short}} .zip 操作必须在 zip 的根目录中具有 `package.json`，而 Windows 资源管理器会将其放入嵌套文件夹内。最安全的选项是在命令行上使用 `zip` 命令。
  {: tip}

5. 创建操作。从 `.zip` 归档创建操作时，必须为 `--kind` 参数设置值，以指定 Node.js 运行时版本。请在 `nodejs:8` 或 `nodejs:10` 之间进行选择。

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. 调用操作。
    

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    示例输出：

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

## 将操作打包成单个捆绑软件
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

如果将操作打包为 .zip 时包含过多不必要的文件，或者您需要更快速的部署，那么可以将最少代码写入包含依赖项的单个 `.js` 文件中。
{: shortdesc}

可以使用 JavaScript 模块打包器（如 [webpack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://webpack.js.org/concepts/)）来打包操作。`webpack` 处理代码时，会以递归方式构建依赖项图，其中包含操作需要的每个模块。

1. 将以下代码保存在名为 `package.json` 的文件中。`webpack` 会添加为开发依赖项。
    

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
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

2. 将以下 webpack 配置代码保存在名为 `webpack.config.js` 的文件中。
    

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

3. 将以下代码保存在名为 `index.js` 的文件中。`global.main` 变量会设置为操作的主函数。
    

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

    global.main = myAction;
    ```
    {: codeblock}

4. 在本地安装所有依赖项。
    

    ```
  npm install
  ```
    {: pre}

5. 构建 webpack 捆绑软件。
    

    ```
  npm run build
  ```
    {: pre}

    这将创建 `dist/bundle.js` 文件，并将其部署为操作源代码。

6. 使用 `npm` 脚本或 CLI 创建操作。

    * 使用 `npm` 脚本：
        

        ```
  npm run deploy
  ```
        {: pre}

    *   使用 CLI：
  

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    由 `webpack` 构建的捆绑软件文件只支持 JavaScript 依赖项。因此，如果捆绑软件依赖于二进制文件依赖项，那么操作调用可能会失败，因为 `bundle.js` 文件中不包含此依赖项。
    {: tip}
    



