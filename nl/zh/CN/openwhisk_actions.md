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

# 创建并调用操作
{: #openwhisk_actions}

操作是在 {{site.data.keyword.openwhisk}} 平台上运行的无状态代码片段。例如，操作可用于检测映像中的构面，响应数据库更改，聚集一组 API 调用或发布推文。
{:shortdesc}

操作可以显式调用，也可以作为对事件的响应来运行。对于其中任一情况，每次运行操作都会生成由唯一激活标识进行标识的激活记录。操作的输入和操作的结果是键/值对的字典，其中键为字符串，值为有效的 JSON 值。操作还可以由对其他操作或对定义的操作序列的调用组成。

操作可以编写为 JavaScript、Swift、Python 和 PHP 函数，也可编写为 Java 方法或任何兼容二进制的可执行文件，包括打包成 Docker 容器的 Go 程序和定制可执行文件。了解如何在首选开发环境中创建、调用和调试操作。

## 创建 JavaScript 操作
{: #creating-and-invoking-javascript-actions}

以下各部分指导您使用以 JavaScript 编写的操作。首先，创建和调用简单操作。然后，向该操作添加参数，并使用参数来调用该操作。接下来，设置缺省参数并调用这些参数。最后，创建异步操作。

### 创建并调用简单 JavaScript 操作
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



2. 使用 JavaScript 函数创建名为 `hello` 的操作：
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  示例输出：
  ```
  ok: created action hello
  ```
  {: screen}

    CLI 会使用源文件扩展名自动推断操作类型。对于 `.js` 源文件，操作会使用 Node.js 6 运行时运行。还可以通过显式指定参数 `--kind nodejs:8`，创建使用 Node.js 8 运行的操作。有关更多信息，请参阅 Node.js 6 和 8 [参考](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。

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

4. 在云中，通过运行阻塞性调用来运行操作。阻塞性调用请求将使用请求/响应样式，并等待激活结果可用。等待时间段为 60 秒或操作的[时间限制值](./openwhisk_reference.html#openwhisk_syslimits)（两者取较短的时间）。
    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    此命令输出以下信息：
        * 激活标识 (`44794bd6aab74415b4e42a308d880e5b`)，可用于在未来检索调用的日志或结果。
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
    有关激活监视的技巧，请参阅[监视操作输出](openwhisk_managing.html#monitor-action-output)。
    {: tip}

5. 如果不是立即需要操作结果，可以省略 `--blocking` 标记以运行非阻塞性调用。
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

6. 如果忘记了记录激活标识，那么可以获取激活列表，其中激活按从最新到最旧的顺序列出。
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

### 创建异步操作
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

    * `main` 函数会返回 Promise。Promise 指示激活尚未完成，但是预期在未来完成。
    * `setTimeout()` JavaScript 函数会等待 2 秒再调用 Promise 的回调函数，这代表异步代码。
    * Promise 的回调函数接受自变量 `resolve` 和 `reject`，这两个自变量都是函数。
      * 对 `resolve()` 的调用会履行 Promise 并指示激活已正常完成。
      * 对 `reject()` 的调用可用于拒绝 Promise 并指示激活异常完成。

2. 创建名为 `asyncAction` 的操作。
    ```
    ibmcloud fn action create asyncAction asyncAction.js
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

3. 通过获取激活日志来检查完成激活所需的时间。

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

      `duration` 显示此激活需要略长于 2 秒的时间才能完成：
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

### 使用操作来调用外部 API
{: #openwhisk_apicall_action}

到目前为止，提供的示例都是自包含 JavaScript 函数。您还可以创建调用外部 API 的操作。
{: shortdesc}

以下示例调用 NASA Astronomy Picture of the Day (APOD) 服务，该服务每天提供我们宇宙的独特图像。

1. 将以下代码保存在名为 `apod.js` 的文件中。
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

    将对 NASA APOD API 进行调用，并从 JSON 结果中抽取字段。有关可在操作中使用的 Node.js 包的更多信息，请参阅[系统详细信息和限制](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。

2. 创建名为 `apod` 的操作。
    ```
    ibmcloud fn action create apod apod.js
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

### 将操作包装为 Node.js 模块
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
    * 必须将此文件命名为 **index.js**，或者将首选文件名指定为 **package.json** 中的 `main` 属性。

3. 在本地安装所有依赖项。
    ```
  npm install
  ```
    {: pre}
    **注**：虽然大部分 `npm` 包会在执行 `npm install` 时安装 JavaScript 源代码，但还有些 npm 包会安装并编译二进制工件。目前，归档文件上传只支持 JavaScript 依赖项。如果归档包含二进制依赖项，那么操作调用可能会失败。

4. 创建包含所有文件（包括所有依赖项）的 `.zip` 归档。
    ```
  zip -r action.zip *
  ```
    {: pre}

    使用 Windows 资源管理器操作来创建 zip 文件将导致结构不正确。{{site.data.keyword.openwhisk_short}} zip 操作必须在 zip 的根目录中具有 `package.json`，而 Windows 资源管理器会将其放入嵌套文件夹内。最安全的选项是在命令行上使用 `zip` 命令。
  {: tip}

5. 创建操作。从 `.zip` 归档创建操作时，必须使用 `nodejs:6` 或 `nodejs:8` 为 `--kind` 标志显式提供值。
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### 将操作打包成单个捆绑软件
{: #openwhisk_js_webpack_action}

如果将操作打包为 zip 时，包含过多不必要的文件，或者您需要更快速的部署，那么可以将最少代码写入包含依赖项的单个 `.js` 文件中。
{: shortdesc}

可以使用 JavaScript 模块打包器（如 [webpack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://webpack.js.org/concepts/)）来打包操作。`webpack` 处理代码时，会以递归方式构建依赖项图，其中包含操作需要的每个模块。

1. 将以下代码保存在名为 `package.json` 的文件中。`webpack` 会添加为开发依赖项。
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
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **注**：由 `webpack` 构建的捆绑软件文件只支持 JavaScript 依赖项。因此，如果捆绑软件依赖于二进制依赖项，那么操作调用会失败，因为 `bundle.js` 文件中不包含此依赖项。

## 创建 Python 操作
{: #creating-python-actions}

以下各部分将指导您创建并调用单个 Python 操作，然后向该操作添加参数。

### 创建并调用 Python 操作
{: #openwhisk_actions_python_invoke}

操作其实就是顶级 Python 函数。要创建 Python 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.py` 的文件中。
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
```
    {: codeblock}

  * Python 操作始终会使用一个字典并生成一个字典。
  * 缺省情况下，操作的入口方法为 `main`，但可显式指定入口方法以使用 `wsk` CLI 通过 `--main` 标志来创建操作。

2. 创建 `helloPython` 操作。
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    示例输出：
    ```
    ok: created action helloPython
    ```
    {: screen}

      CLI 会使用源文件扩展名自动推断操作类型。对于 `.py` 源文件，操作会使用 Python 2 运行时运行。还可以通过显式指定参数 `--kind python:3`，创建使用 Python 3 运行的操作。您还可以通过类型 `python-jessie:3` 来使用 Python 3 运行时，其中包含 {{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}} 和 {{site.data.keyword.ibmwatson_notm}} 等 IBM Cloud 服务的其他包。有关此 Python 3 运行时中包含的包的更多信息，请参阅 Python 运行时[参考](./openwhisk_reference.html#openwhisk_ref_python_environments)。

3. 调用操作。
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    示例输出：
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### 将 Python 操作打包成 zip 文件
{: #openwhisk_actions_python_zip}

您可以将 Python 操作和从属模块打包成 zip 文件。例如，要创建具有名为 `helper.py` 的帮助程序模块的操作，请执行以下操作：

1. 创建包含源文件的归档。**注**：包含入口点的源文件必须命名为 `__main__.py`。
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 创建操作。
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### 使用虚拟环境将 Python 操作打包成 zip 文件
{: #openwhisk_actions_python_virtualenv}

可以使用虚拟环境 `virtualenv` 来打包 Python 依赖项。虚拟环境允许您链接其他包，例如可以使用 [`pip` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://packaging.python.org/installing/) 安装的包。

要安装依赖项，请在虚拟环境中对其打包，然后创建兼容的 OpenWhisk 操作：

1. 创建包含要安装的 `pip` 模块和版本的 [requirements.txt ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 文件。

2. 安装依赖项并创建虚拟环境。虚拟环境目录必须命名为 `virtualenv`。为了确保与 OpenWhisk 运行时容器的兼容性，虚拟环境内部的包安装必须使用与类型对应的映像。
    - 对于类型 `python:2`，请使用 Docker 映像 `openwhisk/python2action`。
    - 对于类型 `python:3`，请使用 Docker 映像 `openwhisk/python3action`。
    - 对于类型 `python-jessie:3`，请使用 Docker 映像 `ibmfunctions/action-python-v3`。
    ```
docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. 打包 `virtualenv` 目录和任何其他 Python 文件。包含入口点的源文件必须命名为 `__main__.py`。
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. 创建 `helloPython` 操作。
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

仅将不属于所选运行时环境的模块添加到 `requirements.txt` 中。这有助于使 `virtualenv` 的大小保持在最小。
{: tip}

## 创建 PHP 操作
{: #creating-php-actions}

以下各部分将指导您创建并调用单个 PHP 操作，然后向该操作添加参数。

### 创建并调用 PHP 操作
{: #openwhisk_actions_php_invoke}

操作其实就是顶级 PHP 函数。要创建 PHP 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.php` 的文件中。
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

    * PHP 操作始终使用一个关联数组并返回一个关联数组。
    * 缺省情况下，操作的入口方法为 `main`，但使用 `ibmcloud fn` CLI 通过 `--main` 标志来创建操作时，可显式指定入口方法。

2. 创建名为 `helloPHP` 的操作。
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    CLI 会根据源文件扩展名自动推断操作类型。对于 `.php` 源文件，操作会使用 PHP 7.1 运行时运行。有关更多信息，请参阅 PHP [参考](./openwhisk_reference.html#openwhisk_ref_php)。

3. 调用操作。
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    示例输出：
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### 将 PHP 操作打包成 zip 文件
{: #openwhisk_actions_php_zip}

可以将 PHP 操作以及其他文件或从属包打包成 zip 文件。例如，要打包具有名为 `helper.php` 的第二个文件的操作，请执行以下操作：

1. 创建包含源文件的归档。**注**：包含入口点的源文件必须命名为 `index.php`。
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. 创建操作。
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## 创建 Swift 操作
{: #creating-swift-actions}

以下各部分将指导您创建并调用单个 Swift 操作，以及将该操作打包成 zip 文件。

**注：**Swift 操作在 Linux 环境中运行。Linux 上的 Swift 仍在开发中；{{site.data.keyword.openwhisk_short}} 会使用最新可用的发行版。但这些发行版可能不稳定。此外，用于 {{site.data.keyword.openwhisk_short}} 的 Swift 版本可能与 Mac OS 上 Xcode 的稳定发行版中的 Swift 版本不一致。

有关 Swift 运行时的更多信息，请参阅 Swift [参考](./openwhisk_reference.html#swift-actions)。
{: tip}

### 创建并调用操作
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

操作仅仅是顶级 Swift 函数。要创建 Swift 3 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.swift` 的文件中。
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

    在此示例中，Swift 操作会使用一个字典并生成一个字典。

2. 创建名为 `helloSwift` 的操作。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    CLI 会根据源文件扩展名自动推断操作类型。对于 `.php` 源文件，操作会使用 PHP 7.1 运行时运行。有关更多信息，请参阅 PHP [参考](./openwhisk_reference.html#openwhisk_ref_php)。

3. 调用操作。
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    示例输出：
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

除了 main 函数特征符外，Swift 4 中还新增了两个开箱即用的特征符，这两个特征符利用的是 [Codable ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/swift/codable) 类型。您可以在[此处 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 了解有关用于与外部表示（例如 JSON）兼容的可编码和可解码的数据类型的更多信息。

1. 将以下代码保存在名为 `hello.swift` 的文件中。
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
    此示例采用带有 `name` 字段的 `Codable Input` 作为输入参数，并返回带有 `greeting` 字段的 `Codable Output`。

2. 创建名为 `helloSwift` 的操作。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. 调用操作。
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    示例输出：
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### 将操作打包成 Swift 可执行文件
{: #packaging-an-action-as-a-swift-executable}

使用 Swift 源文件创建 {{site.data.keyword.openwhisk_short}} Swift 操作后，必须先将该文件编译成二进制文件，才能运行该操作。此延迟称为冷启动延迟。创建二进制文件后，对该操作的后续调用会快得多，直到清除保存该操作的容器为止。要避免冷启动延迟，可以将 Swift 文件编译为二进制文件，然后将该二进制文件以 zip 文件格式上传到 {{site.data.keyword.openwhisk_short}}。

可以使用脚本来自动对操作打包。

**先决条件**：以下步骤中使用的脚本假定您有名为 `actions` 的目录，其中每个顶级目录表示一个操作。
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. 将以下代码保存在名为 `compile.sh` 的脚本文件中。
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

2. 要添加依赖项，请创建 `Package.swift` 文件。以下示例将 `example-package-deckofplayingcards` 添加为依赖项。在标准 Swift 操作中提供了 `CCurl`、`Kitura-net` 和 `SwftyJSON`，因此应该将这三项包含在您自己的 `Package.swift` 中以仅用于 Swift 3 操作。
    * Swift 3 示例语法：
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

    * Swift 4 示例语法：
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

3. 要在 `build` 中创建 `hello.zip`，请构建操作。
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

4. 使用操作名称 `helloSwiftly` 将该 zip 上传到 {{site.data.keyword.openwhisk_short}}。
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

5. 调用操作。
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    操作运行所用的时间会填写在“duration1”属性中。

6. 可以将预编译操作调用的持续时间与带有编译步骤的命令调用的持续时间进行比较。调用上一部分中的操作：
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Swift 4 中的错误处理
{: #error-handling-swift4}

通过新的 Codable 完成处理程序，可以传递错误以指示操作中的故障。[Swift 中的错误处理 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) 与其他语言中使用 `try`、`catch` 和 `throw` 关键字的异常处理类似。

以下片段显示错误处理的示例：
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // 返回实际错误
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
    completion(nil, error)
    }
}
```
{: codeblock}

## 创建 Java 操作
{: #creating-java-actions}

以下各部分将指导您创建并调用单个 Java 操作，然后向该操作添加参数。

为了编译、测试和归档 Java 文件，您必须在本地安装 [JDK 8 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://openjdk.java.net/install)。

### 创建并调用 Java 操作
{: #openwhisk_actions_java_invoke}

Java 操作是包含名为 `main` 的方法的 Java 程序。`main` 必须具有以下确切的特征符：
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

要创建 Java 操作，请执行以下操作：

1. 将以下代码保存在名为 `Hello.java` 的文件中。
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

2. 将 `Hello.java` 编译成名为 `hello.jar` 的 JAR 文件。**注**：[google-gson ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/gson) 必须存在于 Java CLASSPATH 中。
    ```
javac Hello.java
```
    {: pre}
    ```
jar cvf hello.jar Hello.class
```
    {: pre}

3. 创建操作。
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * 必须使用 `--main` 来指定主类的名称。符合要求的主类是实现了静态 `main` 方法的主类。如果该类不在缺省包中，请使用 Java 标准类名，例如 `--main com.example.MyMain`。
  * 可以定制 Java 操作的方法名称。这将通过指定操作的标准方法名称来实现，例如 `--main com.example.MyMain#methodName`。
  * CLI 会根据源文件扩展名自动推断操作类型。

4. 调用操作。
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    示例输出：
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 创建 Docker 操作
{: #creating-docker-actions}

通过 {{site.data.keyword.openwhisk_short}} Docker 操作，可以使用任何语言编写操作。
{: shortdesc}

您的代码会编译为可执行二进制文件，并嵌入到 Docker 映像中。二进制程序通过采用来自 `stdin` 的输入并通过 `stdout` 进行回复，从而与系统进行交互。您可以在[参考](./openwhisk_reference.html#openwhisk_ref_docker)部分中找到有关创建 Docker 操作的更多信息。

先决条件：您必须拥有 Docker Hub 帐户。在 [Docker Hub ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com) 上设置免费 Docker 标识和帐户。

要设置定制二进制文件并将上传的 Docker 映像用作操作，请执行以下操作：

1. 下载并安装 Docker 框架。框架是一种 Docker 容器模板，可以在其中以定制二进制文件的形式插入代码。 
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在 Docker 框架中设置定制二进制文件。该框架包含可以使用的 C 程序。在 Docker 映像构建过程中，会对 `example.c` 文件中的部分内容进行编译，所以无需在您的计算机上对 C 程序进行编译。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  示例输出：
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
            printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 可选：通过修改 `Dockerfile` 来构建可执行文件，从而向 Docker 映像添加其他代码和依赖项。请注意以下需求和限制：
  * 二进制文件必须位于容器内的 `/action/exec` 中。
  * 可执行文件会从命令行接收单个自变量。此自变量是字符串序列化的 JSON 对象，表示操作的自变量。
  * 程序可能会记录到 `stdout` 或 `stderr`。
  * 根据约定，输出的最后一行必须是字符串化的 JSON 对象，用于表示操作结果。

4. 使用提供的脚本来构建 Docker 映像并进行上传。
    1. 登录到 Docker。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. 转至 `dockerSkeleton` 目录。
        ```
  cd dockerSkeleton
  ```
        {: pre}

    3. 运行脚本。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. 使用 Docker 容器来创建操作。
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. 调用操作。
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    示例输出：
    ```
    {
        "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
    {: screen}

7. 要更新 Docker 操作，请将最新的映像上传到 Docker Hub。这将允许系统在下次运行操作的代码时，拉取新的 Docker 映像。
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. 如果有使用前版 Docker 映像的运行中容器，那么任何新调用都将继续使用该映像。更新操作，以便新的调用开始使用新映像。
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. 可选：可以使用 `--native` 自变量作为 `--docker openwhisk/dockerskeleton` 的简写。通过此参数，可以更轻松地创建和部署在标准 Docker 操作 SDK 内运行的可执行文件。
    1. 以上步骤在 `/action/exec` 的容器内部创建了二进制可执行文件。将 `/action/exec` 文件复制到本地文件系统，并将其压缩成 `exec.zip`。
    2. 创建将可执行文件作为初始化数据接收的 Docker 操作。`--native` 自变量会替换 `--docker openwhisk/dockerskeleton` 自变量。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## 创建 Go 操作
{: #creating-go-actions}

`--native` 自变量允许您将任何 Go 可执行文件打包为操作。

请注意以下需求和限制。
  * Go 可执行文件会从命令行接收单个自变量。该自变量是字符串序列化的 JSON 对象，表示操作的自变量。
  * 程序可能会记录到 `stdout` 或 `stderr`。
  * 根据约定，输出的最后一行必须是字符串化的 JSON 对象，用于表示操作结果。

要创建 Go 操作，请执行以下操作：

1. 将以下代码保存在名为 `sample.go` 的文件中。
    ```go
    package main

    import "encoding/json"
    import "fmt"
    import "os"

    func main() {
        //程序接收一个自变量：JSON 对象作为字符串
    arg := os.Args[1]

    

        // 将字符串反序列化为 JSON 对象
        var obj map[string]interface{}
        json.Unmarshal([]byte(arg), &obj)

        // 可以选择记录到 stdout（或 stderr）
        fmt.Println("hello Go action")

        name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    

        // stdout 的最后一行是作为字符串的结果 JSON 对
        msg := map[string]string{"msg": ("Hello, " + name + "!")}
        res, _ := json.Marshal(msg)
        fmt.Println(string(res))
    }
    ```
    {: codeblock}

2. 针对 {{site.data.keyword.openwhisk_short}} 交叉编译 `sample.go`。可执行文件必须名为 `exec`。
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. 调用操作。
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## 创建操作序列
{: #openwhisk_create_action_sequence}

可以创建用于将一序列操作链接在一起的操作。将一个操作的结果作为自变量传递给下一个操作。
{: shortdesc}

在 `/whisk.system/utils` 包中提供了多个实用程序操作，可用于创建第一个序列。

1. 列出 `/whisk.system/utils` 包中的操作。
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    示例输出：
    ```
      package /whisk.system/utils：构建用于设置数据格式和组合数据的块
   action /whisk.system/utils/head：抽取数组的前缀
   action /whisk.system/utils/split：将字符串拆分成数组
   action /whisk.system/utils/sort：对数组排序
   action /whisk.system/utils/echo：返回输入
   action /whisk.system/utils/date：当前日期和时间
   action /whisk.system/utils/cat：将输入连接成一个字符串
  ```
    {: screen}

2. 使用 `split` 和 `sort` 操作来创建操作序列，以便 `split` 的结果可作为自变量传递给 `sort`。此操作序列会将一些文本行转换为数组，然后对这些行排序。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 调用操作。
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    在输出中，拆分的行按字母顺序排序。
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

**注**：
* 在序列中的操作之间传递的参数为显式参数，但缺省参数除外。因此，传递到操作序列的参数只可用于序列中的第一个操作。序列中第一个操作的结果成为序列中第二个操作的输入 JSON 对象，依此类推。此对象不包含初始传递到序列的任何参数，除非第一个操作在其结果中显式包含这些参数。操作的输入参数会与操作的缺省参数合并，并且操作的输入参数优先于并覆盖任何匹配的缺省参数。有关使用多个指定参数来调用操作序列的更多信息，请参阅[在操作上设置缺省参数](./parameters.html#default-params-action)。
* 序列的总体超时会受该序列中各个操作的超时的影响。因为序列是操作的管道，所以一个操作发生故障会使管道中断。如果一个操作超时，那么将退出整个序列，并返回该故障。

## 管理大型操作
{: #large-app-support}

操作的最大代码大小为 48 MB。包含大量第三方模块、本机库或外部工具的应用程序可能会达到此限制。如果您创建了大于 48 MB 的 .zip 或 .jar 包操作，那么必须使用依赖项来扩展运行时映像，然后使用单个源文件或小于 48 MB 的归档。

例如，通过构建包含必要共享库的定制 Docker 运行时，无需在归档文件中提供这些依赖项。仍可在归档中捆绑专用源文件，并在运行时注入这些文件。

### 减小操作的大小
{: #large-app-reduce}

要减小 Python 应用程序的代码大小，请执行以下操作：

1. 将 `opencv-python` 库放入到 `opencv` 中。
2. 将 opencv 二进制文件安装到操作系统映像中。
3. 通过运行 `pip install requirements.txt` 以使用更多 Python 库来扩充映像。
4. 将 `action.py` 用于新映像。

要减小 Node.js 应用程序的代码大小，请执行以下操作：

1. 安装 `opencv`。
   ```
npm install opencv
```
   {: pre}

2. 安装 `package.json`。
   ```
npm install package.json
```
   {: pre}

3. 将 `action.js` 用于新映像。
