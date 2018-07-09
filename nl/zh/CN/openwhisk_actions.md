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

# 创建并调用操作
{: #openwhisk_actions}

操作是在 {{site.data.keyword.openwhisk}} 平台上运行的无状态代码片段。例如，操作可用于检测映像中的构面，响应数据库更改，聚集一组 API 调用或发布推文。
操作可以编写为 JavaScript、Swift、Python 和 PHP 函数，也可编写为 Java 方法或任何兼容二进制的可执行文件，包括打包为 Docker 容器的 Go 程序和定制可执行文件。
{:shortdesc}

操作可以显式调用，也可以作为对事件的响应来运行。对于其中任一情况，每次运行操作都会生成由唯一激活标识进行标识的激活记录。操作的输入和操作的结果是键/值对的字典，其中键为字符串，值为有效的 JSON 值。操作还可以由对其他操作或对定义的操作序列的调用组成。

了解如何在首选开发环境中创建、调用和调试操作：
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [任意可执行文件](#creating-actions-arbitrary)

此外，请了解：
* [监视操作输出](#watching-action-output)
* [大型应用程序支持](#large-app-support)
* [列出操作](#listing-actions)
* [删除操作](#deleting-actions)
* [访问操作体中的操作元数据](#accessing-action-metadata-within-the-action-body)


## 创建并调用 JavaScript 操作
{: #creating-and-invoking-javascript-actions}

以下各部分将逐步指导您使用 JavaScript 中的操作。您从创建和调用简单的操作开始。然后，继续向操作添加参数，并使用这些参数来调用该操作。接下来，设置缺省参数并调用这些参数。然后，创建异步操作并最终使用操作序列。


### 创建并调用简单 JavaScript 操作
{: #openwhisk_single_action_js}

查看以下步骤和示例以创建第一个 JavaScript 操作。

1. 创建具有以下内容的 JavaScript 文件。对于此示例，文件名为“hello.js”。

  ```javascript
function main() {
    return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  JavaScript 文件可能包含其他函数。但是，根据约定，名为 `main` 的函数必须存在，才能为操作提供入口点。



2. 通过以下 JavaScript 函数创建操作。对于此示例，操作名为“hello”。

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  CLI 会使用源文件扩展名自动推断操作类型。对于 `.js` 源文件，操作会使用 Node.js 6 运行时运行。还可以通过显式指定参数 `--kind nodejs:8`，创建使用 Node.js 8 运行的操作。有关更多信息，请参阅 Node.js 6 和 8 [参考](./openwhisk_reference.html#openwhisk_ref_javascript_environments)。
  
3. 列出已创建的操作：

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  您可以看到创建的 `hello` 操作。

4. 创建操作后，可以使用“invoke”命令在 OpenWhisk 的云中运行该操作。可以通过在命令中指定标志以使用*阻塞性*调用（即，请求/响应样式）或*非阻塞性*调用来调用操作。阻塞性调用请求将_等待_激活结果可用。等待时间段为 60 秒或操作的[时间限制值](./openwhisk_reference.html#openwhisk_syslimits)（两者取较短的时间）。如果激活结果在等待时间段内可用，那么会返回激活结果。否则，激活会继续在系统中处理，并返回激活标识，以便可以稍后检查结果，这与非阻塞性请求一样（请参阅[此处](#watching-action-output)，以获取有关监视激活的提示）。

  此示例使用的是阻塞参数 `--blocking`：

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

  此命令输出两条重要的信息：
  * 激活标识 (`44794bd6aab74415b4e42a308d880e5b`)
  * 如果激活结果在预期的等待时间段内可用，那么输出调用结果

  在本例中，结果为 JavaScript 函数返回的字符串 `Hello world`。激活标识可以用于在未来检索调用的日志或结果。  

5. 如果不是立即需要操作结果，可以省略 `--blocking` 标记以进行非阻塞调用。可以在以后使用激活标识来获取结果。请参阅以下示例：

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

6. 如果忘记了记录激活标识，那么可以获取激活列表，其中激活按从最新到最旧的顺序列出。运行以下命令来获取激活列表：

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### 向操作传递参数
{: #openwhisk_pass_params}

调用操作时，可以将参数传递给操作。

1. 在操作中使用参数。例如，使用以下内容来更新“hello.js”文件：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  输入参数会作为 JSON 对象参数传递给 `main` 函数。请注意此示例中的 `name` 和 `place` 参数是如何从 `params` 对象检索到的。

2. 在将 `name` 和 `place` 参数值传递给 `hello` 操作的情况下，更新并调用该操作。请参阅以下示例：

  ```
wsk action update hello hello.js
  ```
  {: pre}

  如果需要修改非服务凭证参数，请注意，执行带新参数的 `action update` 命令将除去当前存在但未在 `action update` 命令中指定的任何参数。例如，如果除了 `__bx_creds` 之外还有两个参数，其键名为 key1 和 key2。如果运行带 `-p key1 new-value -p key2 new-value` 的 `action update` 命令，但省略 `__bx_creds` 参数，那么 `__bx_creds` 参数在 `action update` 成功完成后将不再存在。您必须随后重新绑定服务凭证。这是已知的限制，没有变通方法。
  {: tip}  

3.  参数可以在命令行上显式提供，也可以通过提供包含所需参数的文件来提供。

  要直接通过命令行来传递参数，请向 `--param` 标志提供键/值对：
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  为了使用包含参数内容的文件，请创建以 JSON 格式包含参数的文件。然后，必须将文件名传递到 `param-file` 标志：

  请参阅名为 `parameters.json` 的以下示例参数文件：
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

  使用 `--result` 选项时请注意：此选项暗含 CLI 等待激活完成并随后仅显示激活结果的阻塞性调用。为了方便起见，使用此选项时可不带 `--blocking`，系统会自动推断出阻塞性。

  此外，如果在命令行上指定的参数值是有效的 JSON，那么会将它们解析为结构化对象并发送到操作。例如，将 hello 操作更新为以下内容：

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  现在，操作期望单个 `person` 参数具有 `name` 和 `place` 字段。接下来，使用作为有效 JSON 的单个 `person` 参数调用操作，如以下示例中所示：

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  结果是相同的，因为 CLI 会自动将 `person` 参数值解析为操作现在期望的结构化对象：
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### 设置缺省参数
{: #openwhisk_binding_actions}

操作可以通过多个指定参数进行调用。重新调用上面示例中的 `hello` 操作需要两个参数：*name*（人员的姓名）和 *place*（人员所在位置）。

您可以绑定特定参数，而不用每次将所有参数传递给操作。以下示例绑定 *place* 参数，以便操作的缺省位置为“Vermont”：

1. 更新该操作，方法是使用 `--param` 选项来绑定参数值，或者将包含参数的文件传递到 `--param-file`。

  要在命令行上显式指定缺省参数，请向 `param` 标志提供键/值对：

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  从文件传递参数需要创建以 JSON 格式包含所需内容的文件。然后，必须将文件名传递到 `-param-file` 标志：

  请参阅名为 `parameters.json` 的以下示例参数文件：
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

2. 调用操作，但这次只传递 `name` 参数。

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  请注意，调用操作时，无需指定 place 参数。绑定的参数仍可以通过在调用时指定该参数值来进行覆盖。



3. 调用操作，并同时传递 `name` 和 `place` 值。后者将覆盖绑定到操作的值。

  使用 `--param` 标志：

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  使用 `--param-file` 标志：

  文件 parameters.json：
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

### 获取操作 URL

可以经由 HTTPS 请求通过 REST 接口来调用操作。要获取操作 URL，请执行以下命令：

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

通过 HTTPS 请求调用操作时，必须提供认证。有关使用 REST 接口调用操作的更多信息，请参阅[将 REST API 与 OpenWhisk 一起使用](./openwhisk_rest_api.html#openwhisk_rest_api)。
{: tip}

### 保存操作码

访存与现有操作关联的代码后会将其保存在本地。将对除了序列和 Docker 操作之外的所有操作执行保存。将操作码保存到文件时，代码将保存在当前工作目录中，并且会显示保存的文件路径。

1. 将操作码保存到与现有操作名称相对应的文件名。将使用与操作类型相对应的文件扩展名，或者对于作为 zip 文件的操作码，将使用类型为 `.zip` 的扩展名。
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. 可以使用 `--save-as` 标志来提供已保存代码的定制文件名和扩展名，而不允许 CLI 来确定其文件名和扩展名。
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### 创建异步操作
{: #openwhisk_asynchrony_js}

通过在操作中返回 Promise，异步运行的 JavaScript 函数可在 `main` 函数返回后返回激活结果。

1. 将以下内容保存在名为 `asyncAction.js` 的文件中。

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

  请注意，`main` 函数返回 Promise，这指示激活尚未完成，但是预期在未来完成。

  在本例中，`setTimeout()` JavaScript 函数会等待 2 秒再调用回调函数，这代表异步代码，并且在 Promise 的回调函数内执行。

  Promise 的回调函数采用两个自变量 resolve 和 reject，这两个自变量都是函数。对 `resolve()` 的调用会履行 Promise 并指示激活已正常完成。

  对 `reject()` 的调用可用于拒绝 Promise 并指示激活异常完成。

2. 运行以下命令来创建并调用操作：

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

  请注意，您执行的是对异步操作的阻塞调用。



3. 访存激活日志以查看完成激活所用的时间：

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

  通过比较激活记录中的 `start` 和 `end` 时间戳记，可以看出完成此激活所用时间略微超过 2 秒。

### 使用操作来调用外部 API
{: #openwhisk_apicall_action}

到目前为止，示例都是自包含 JavaScript 函数。您还可以创建调用外部 API 的操作。

以下示例调用 Yahoo Weather 服务来获取特定位置的当前天气状况。

1. 将以下内容保存在名为 `weather.js` 的文件中。


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

 示例中的操作使用 JavaScript `request` 库向 Yahoo Weather API 发起 HTTP 请求，然后从 JSON 结果中抽取字段。[参考](./openwhisk_reference.html#openwhisk_ref_javascript_environments)详细描述了可以在操作中使用的 Node.js 包。

  此示例还显示了需要异步操作。此操作返回 Promise，指示函数返回时此操作的结果尚不可用。相反，结果会在 HTTP 调用完成后在 `request` 回调中提供，并且会作为自变量传递给 `resolve()` 函数。

2. 运行以下命令来创建并调用操作：

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

### 将操作打包为 Node.js 模块
{: #openwhisk_js_packaged_action}

作为在单个 JavaScript 源文件中编写所有操作码的替代方法，可以将操作编写为 `npm` 包。例如，假设一个目录包含以下文件：

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

接着是 `index.js`：

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

操作将通过 `exports.main` 公开。操作处理程序本身可以具有任何名称，只要名称符合接受对象和返回对象（或对象的 `Promise`）的通常特征符即可。根据 Node.js 约定，您必须将此文件命名为 `index.js`，或者将首选文件名指定为 package.json 中的 `main` 属性。

要通过此包创建 OpenWhisk 操作，请执行以下操作：

1. 首先在本地安装所有依赖关系

  ```
  npm install
  ```
  {: pre}

2. 创建包含所有文件（包括所有依赖关系）的 `.zip` 归档：

  ```
  zip -r action.zip *
  ```
  {: pre}

  使用 Windows 资源管理器操作来创建 zip 文件将导致结构不正确。OpenWhisk zip 操作必须在 zip 的根目录中具有 `package.json`，而 Windows 资源管理器会将其放入嵌套文件夹内。最安全的选项是在命令行上使用 `zip` 命令。
  {: tip}

3. 创建操作：

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  使用 CLI 工具从 `.zip` 归档创建操作时，必须使用 `nodejs:6` 或 `nodejs:8` 为 `--kind` 标志显式提供值。

4. 可以像调用其他任何操作一样来调用此操作：

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

最后，请注意，虽然大部分 `npm` 包会在执行 `npm install` 时安装 JavaScript 源代码，但还有些 npm 包会安装并编译二进制工件。目前，归档文件上传不支持二进制依赖关系，而只支持 JavaScript 依赖关系。如果归档包含二进制依赖关系，那么操作调用可能会失败。

### 将操作打包为单个捆绑软件
{: #openwhisk_js_webpack_action}

最简便的方法是仅将最少的代码包含在具有依赖项的单个 `.js` 文件中。通过这种方法，能更快进行部署，在某些情况下将操作打包为 zip 可能会太大，因为会包含不必要的文件。

可以使用 JavaScript 模块打包机，如 [webpack](https://webpack.js.org/concepts/)。webpack 处理代码时，会以递归方式构建依赖项图，其中包含操作需要的每个模块。

下面是使用 webpack 的快速示例：

对于上面的示例 `package.json`，将 `webpack` 添加为开发依赖项，并添加一些 npm 脚本命令。
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
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

创建 webpack 配置文件 `webpack.config.js`。
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

将 `global.main` 变量设置为操作的主函数。根据上面的示例：
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

如果函数名为 `main`，请改为使用以下语法：
```javascript
global.main = main;
```
{: codeblock}


要使用 `npm` 和 `webpack` 来构建并部署 OpenWhisk 操作，请执行以下操作：

1. 首先，在本地安装依赖项：

  ```
  npm install
  ```
  {: pre}

2. 构建 webpack 捆绑软件：

  ```
  npm run build
  ```
  {: pre}

  这将创建 `dist/bundle.js` 文件，并将其用于部署为操作源代码。

3. 使用 `npm` 脚本或 CLI 创建操作。
  使用 `npm` 脚本：
  ```
  npm run deploy
  ```
  {: pre}
  使用 CLI：
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


最后，由 `webpack` 构建的捆绑软件文件不支持二进制依赖项，而是支持 JavaScript 依赖项。因此，如果捆绑软件依赖于二进制依赖项，那么操作调用会失败，因为 `bundle.js` 文件中不包含此依赖项。

## 创建操作序列
{: #openwhisk_create_action_sequence}

可以创建用于将一序列操作链接在一起的操作。

在名为 `/whisk.system/utils` 的包中提供了多个实用程序操作，可用于创建第一个序列。您可以在[包](./openwhisk_packages.html)部分中了解有关包的更多信息。

1. 显示 `/whisk.system/utils` 包中的操作。

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils：构建用于设置数据格式和组合数据的块
   action /whisk.system/utils/head：抽取数组的前缀
   action /whisk.system/utils/split：将字符串拆分成数组
   action /whisk.system/utils/sort：对数组排序
   action /whisk.system/utils/echo：返回输入
   action /whisk.system/utils/date：当前日期和时间
   action /whisk.system/utils/cat：将输入连接成一个字符串
  ```

  您将使用此示例中的 `split` 和 `sort` 操作。

2. 创建操作序列，使一个操作的结果作为自变量传递给下一个操作。

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  此操作序列会将一些文本行转换为数组，然后对这些行排序。

3. 调用操作：

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

  在结果中，您会看到这些行已排序。

**注**：在序列中的操作之间传递的参数为显式参数，但缺省参数除外。因此，传递到操作序列的参数只可用于序列中的第一个操作。序列中第一个操作的结果成为序列中第二个操作的输入 JSON 对象（依此类推）。此对象不包含初始传递到序列的任何参数，除非第一个操作在其结果中显式包含这些参数。操作的输入参数会与操作的缺省参数合并，并且操作的输入参数优先于并覆盖任何匹配的缺省参数。有关使用多个指定参数来调用操作序列的更多信息，请参阅[设置缺省参数](./openwhisk_actions.html#openwhisk_binding_actions)。

## 创建 Python 操作
{: #creating-python-actions}

创建 Python 操作的过程与创建 JavaScript 操作的过程类似。以下各部分将指导您创建并调用单个 Python 操作，然后向该操作添加参数。

### 创建并调用 Python 操作
{: #openwhisk_actions_python_invoke}

操作其实就是顶级 Python 函数。例如，创建名为 `hello.py` 的文件并包含以下源代码：

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
        return {"greeting": greeting}
```
{: codeblock}

Python 操作始终会使用一个字典并生成一个字典。缺省情况下，操作的入口方法为 `main`，但与其他任何操作类型一样，可显式指定入口方法以使用 `wsk` CLI 通过 `--main` 创建操作。

可以通过此函数创建名为 `helloPython` 的 OpenWhisk 操作，如下所示：
```
wsk action create helloPython hello.py
```
{: pre}

CLI 会根据源文件扩展名自动推断操作类型。对于 `.py` 源文件，操作会使用 Python 2 运行时运行。还可以通过显式指定参数 `--kind python:3`，创建使用 Python 3 运行的操作。此外，还有一个类型为 `python-jessie:3` 的 Python 3 运行时，其中包含 IBM Cloud 服务（例如，IBM Cloudant、IBM Db2、IBM COS 和 IBM Watson）的其他包。
有关此 Python 3 运行时中包含的包的更多信息，请参阅 Python 运行时[参考](./openwhisk_reference.html#openwhisk_ref_python_environments)。

Python 操作的操作调用与 JavaScript 操作的操作调用相同：
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### 将 Python 操作打包成 zip 文件
{: #openwhisk_actions_python_zip}

您可以将 Python 操作和从属模块打包成 zip 文件。包含入口点（例如，`main`）的源文件的文件名必须为 `__main__.py`。例如，要创建具有名为 `helper.py` 的帮助程序模块的操作，请先创建包含源文件的归档：

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

然后，创建操作：

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

虽然说明的这些步骤是针对 Python 3（类型为 `python:3`），但对于替代 Python 类型 `python:2` 或 `python-jessie:3`，也可执行相同的步骤。


### 使用虚拟环境将 Python 操作打包成 zip 文件
{: #openwhisk_actions_python_virtualenv}

另一种打包 Python 依赖项的方法是使用虚拟环境 (`virtualenv`)，这允许您链接其他包，例如可以通过 [`pip`](https://packaging.python.org/installing/) 安装的包。


与基本 zip 文件支持一样，包含主入口点的源文件的名称必须为 `__main__.py`。需要阐明的是，`__main__.py` 的内容是主函数，因此对于此示例，可以将 `hello.py` 重命名为上一部分中的 `__main__.py`。此外，virtualenv 目录必须命名为 `virtualenv`。请参阅有关安装依赖项、将其打包在 virtualenv 中并创建兼容 OpenWhisk 操作的以下示例场景。

为了确保与 OpenWhisk 运行时容器的兼容性，virtualenv 内部的包安装必须使用与类型对应的映像在目标环境中完成。
- 对于类型 `python:2`，请使用 Docker 映像 `openwhisk/python2action`。
- 对于类型 `python:3`，请使用 Docker 映像 `openwhisk/python3action`。
- 对于类型 `python-jessie:3`，请使用 Docker 映像 `ibmfunctions/action-python-v3`。

1. 假设 [requirements.txt ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 文件中包含要安装的 `pip` 模块和版本，请运行以下命令以安装依赖项，并使用兼容 Docker 映像创建 virtualenv：
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. 归档 virtualenv 目录及其他任何 Python 文件：
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. 创建操作：
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## 创建 PHP 操作
{: #creating-php-actions}

创建 PHP 操作的过程与创建 JavaScript 操作的过程类似。以下各部分将指导您创建并调用单个 PHP 操作，然后向该操作添加参数。

### 创建并调用 PHP 操作
{: #openwhisk_actions_php_invoke}

操作其实就是顶级 PHP 函数。例如，创建名为 `hello.php` 的文件并包含以下源代码：

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

PHP 操作始终使用一个关联数组并返回一个关联数组。缺省情况下，操作的入口方法为 `main`，但与其他任何操作类型一样，在使用 `wsk` CLI 通过 `--main` 创建操作时可显式指定入口方法。

可以通过此函数创建名为 `helloPHP` 的 OpenWhisk 操作，如下所示：

```
wsk action create helloPHP hello.php
```
{: pre}

CLI 会根据源文件扩展名自动推断操作类型。对于 `.php` 源文件，操作会使用 PHP 7.1 运行时运行。有关更多信息，请参阅 PHP [参考](./openwhisk_reference.html#openwhisk_ref_php)。

PHP 操作的操作调用与 JavaScript 操作的操作调用相同：

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### 将 PHP 操作打包成 zip 文件
{: #openwhisk_actions_php_zip}

您可以将 PHP 操作以及其他文件和从属包打包成 zip 文件。包含入口点（例如，`main`）的源文件的文件名必须为 `index.php`。例如，要创建包含名为 `helper.php` 的辅助文件的操作，请先创建包含源文件的归档：

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

然后，创建操作：

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## 创建 Swift 操作
{: #creating-swift-actions}

创建 Swift 操作的过程与创建 JavaScript 操作的过程类似。以下各部分将指导您创建并调用单个 Swift 操作，然后向该操作添加参数。

您还可以使用在线 [Swift Sandbox](https://swiftlang.ng.bluemix.net) 来测试 Swift 代码，而不必在您的计算机上安装 Xcode。

### 创建并调用操作

操作仅仅是顶级 Swift 函数。例如，创建名为 `hello.swift` 的文件并包含以下内容：

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

Swift 操作始终会使用一个字典并生成一个字典。

可以通过此函数创建名为 `helloSwift` 的 {{site.data.keyword.openwhisk_short}} 操作，如下所示：

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

请始终指定 `swift:3.1.1`，因为先前的 Swift 版本不受支持。
{: tip}

Swift 操作的操作调用与 JavaScript 操作的操作调用相同：

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**注意：**在 Linux 环境中运行的 Swift 操作仍在开发中；{{site.data.keyword.openwhisk_short}} 通常会使用最新可用发行版，但此发行版不一定稳定。此外，用于 {{site.data.keyword.openwhisk_short}} 的 Swift 版本可能与 Mac OS 上 Xcode 的稳定发行版中的 Swift 版本不一致。

### 将操作打包为 Swift 可执行文件
{: #openwhisk_actions_swift_zip}

使用 Swift 源文件创建 OpenWhisk Swift 操作后，必须先将该文件编译成二进制文件，才能运行该操作。完成后，对该操作的后续调用会快得多，直到清除保存该操作的容器为止。此延迟称为冷启动延迟。

要避免冷启动延迟，可以将 Swift 文件编译为二进制文件，然后将其以 zip 文件格式上传到 OpenWhisk。由于您需要 OpenWhisk 脚手架，创建二进制文件的最简单方法是在它运行所在的环境中对其进行构建。请参阅以下步骤：

- 使用以下命令来运行交互式 Swift 操作容器：
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- 复制源代码并准备对其进行构建。
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

- （可选）创建 `Package.swift` 文件以添加依赖项。
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

  此示例添加 `swift-watson-sdk` 和 `example-package-deckofplayingards` 依赖项。
  请注意，在标准 Swift 操作中提供了 `CCurl`、`Kitura-net` 和 `SwftyJSON`，因此可以将这三项包含在您自己的 `Package.swift` 中。

- 将 Package.swift 复制到 spm-build 目录
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- 切换到 spm-build 目录
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- 编译 Swift 操作。
  ```
  swift build -c release
  ```
  {: pre}

- 创建 zip 归档。
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- 退出 Docker 容器。
  ```
  exit
  ```
  {: pre}

您可以看到在 hello.swift 所在的目录中创建了 hello.zip。 

- 使用名为 helloSwifty 的操作将其上传到 OpenWhisk：
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- 要检查其运行速度提高了多少，请运行
   
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

操作运行所用的时间会填写在“duration”属性中，并会与使用 hello 操作中的编译步骤运行所用时间进行比较。

## 创建 Java 操作
{: #creating-java-actions}

创建 Java 操作的过程与创建 JavaScript 和 Swift 操作的过程类似。以下各部分将指导您创建并调用单个 Java 操作，然后向该操作添加参数。

为了编译、测试和归档 Java 文件，您必须在本地安装 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)。

### 创建并调用操作
{: #openwhisk_actions_java_invoke}

Java 操作是包含名为 `main` 的方法的 Java 程序，该方法的特征符与以下内容完全一样：
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

例如，创建名为 `Hello.java` 的 Java 文件并包含以下内容：

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

然后，将 `Hello.java` 编译成 JAR 文件 `hello.jar`，如下所示：
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) 必须存在于 Java CLASSPATH 中，才能编译 Java 文件。
{: tip}

可以按如下所示从此 JAR 文件创建名为 `helloJava` 的 OpenWhisk 操作：

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

使用命令行和 `.jar` 源文件时，无需指定您要创建 Java 操作；该工具会根据文件扩展名来进行确定。

您需要使用 `--main` 来指定主类的名称。符合要求的主类是实现了静态 `main` 方法的主类。如果该类不在缺省包中，请使用 Java 标准类名，例如 `--main com.example.MyMain`。

如果需要，还可以定制 Java 操作的方法名称。这将通过指定操作的 Java 标准方法名称来实现，例如 `--main com.example.MyMain#methodName`。

Java 操作的操作调用与 Swift 和 JavaScript 操作的操作调用相同：

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## 创建 Docker 操作
{: #creating-docker-actions}

通过 {{site.data.keyword.openwhisk_short}} Docker 操作，可以使用任何语言编写操作。

您的代码会编译为可执行二进制文件，并嵌入到 Docker 映像中。二进制程序通过采用来自 `stdin` 的输入并通过 `stdout` 进行回复，从而与系统进行交互。

作为先决条件，您必须拥有 Docker Hub 帐户。要设置免费 Docker 标识和帐户，请转至 [Docker Hub](https://hub.docker.com)。

对于后续指示信息，假定 Docker 用户标识为 `janesmith`，密码为 `janes_password`。假定已设置 CLI，还需执行三个步骤来设置定制二进制文件以供 {{site.data.keyword.openwhisk_short}} 使用。在此之后，上传的 Docker 映像即可用作操作。



1. 下载 Docker 框架。可以使用 CLI 下载并安装 Docker 框架，如下所示：

  ```
  wsk sdk install docker
  ```
  {: pre}

    现在，Docker 框架已安装在当前目录中。
  
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  框架是一种 Docker 容器模板，可以在其中以定制二进制文件的形式插入代码。 

2. 在 Docker 框架中设置定制二进制文件。该框架已经包含可以使用的 C 程序。

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

  可以根据需要修改此文件，或者向 Docker 映像添加其他代码和依赖关系。对于后者，可以根据需要对 `Dockerfile` 进行调整，以构建可执行文件。
  二进制文件必须位于容器内的 `/action/exec` 中。

  可执行文件会从命令行接收单个自变量。该自变量是字符串序列化的 JSON 对象，表示操作的自变量。程序可能会记录到 `stdout` 或 `stderr`。根据约定，输出的最后一行_必须_是字符串化的 JSON 对象，用于表示操作结果。

3. 使用提供的脚本来构建 Docker 映像并进行上传。必须首先运行 `docker login` 以进行认证，然后使用所选映像名称来运行脚本。

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

  请注意，在 Docker 映像构建过程中，会对 `example.c` 文件中的部分内容进行编译，所以无需在您的计算机上对 C 程序进行编译。
  事实上，除非是在兼容主机上编译二进制文件，否则它无法在容器内部运行，因为格式不匹配。

  现在，Docker 容器可用作 OpenWhisk 操作。


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  请注意，创建操作时会使用 `--docker`。假定所有 Docker 映像都在 Docker Hub 上进行托管。
  该操作可以像其他任何 {{site.data.keyword.openwhisk_short}} 操作一样进行调用。 

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

  要更新 Docker 操作，请运行 `buildAndPush.sh` 以将最新的映像上传到 Docker Hub。这将允许系统在下次运行操作的代码时，拉取新的 Docker 映像。如果没有运行中的容器，那么新调用将使用新的 Docker 映像。但是，如果有使用前版 Docker 映像的运行中容器，那么除非运行 `wsk action update`，否则任何新调用都将继续使用该映像。这将指示系统对于新调用，应该执行 docker pull 来获取新的 Docker 映像。

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  您可以在[参考](./openwhisk_reference.html#openwhisk_ref_docker)部分中找到有关创建 Docker 操作的更多信息。

  先前版本的 CLI 支持不带参数的 `--docker`，并且映像名称是位置自变量。为了允许 Docker 操作通过 (zip) 文件接受初始化数据，对 Docker 操作的用户体验进行规范化，使得提供的位置自变量必须是文件（例如，zip 文件）。映像名称必须在 `--docker` 选项之后指定。此外，根据用户反馈，包含了 `--native` 自变量作为 `--docker openwhisk/dockerskeleton` 的简称，以便在标准 Docker 操作 SDK 内运行的可执行文件更方便进行创建和部署。
  
  例如，本教程在位于 `/action/exec` 的容器内部创建了二进制可执行文件。如果将此文件复制到本地文件系统并将其压缩成 `exec.zip`，那么可以使用以下命令来创建用于将可执行文件作为初始化数据接收的 Docker 操作。 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  这相当于以下命令。 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## 使用任意可执行文件创建操作
{: #creating-actions-arbitrary}

通过使用 `--native`，您会发现任何可执行文件均可作为 OpenWhisk 操作运行。这包括 `bash` 脚本或交叉编译的二进制文件。对于后者，约束是二进制文件必须与 `openwhisk/dockerskeleton` 映像相兼容。

## 创建 Go 操作
{: #creating-go-actions}

`--native` 选项支持将任何可执行文件打包为操作。例如，此选项适用于 Go。
与 Docker 操作一样，Go 可执行文件也是从命令行接收单个自变量。
该自变量是字符串序列化的 JSON 对象，表示操作的自变量。程序可能会记录到 `stdout` 或 `stderr`。根据约定，输出的最后一行_必须_是字符串化的 JSON 对象，用于表示操作结果。

下面是示例 Go 操作。
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

    // stdout 的最后一行是作为字符串的结果 JSON 对象
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

将以上代码保存到 `sample_go` 文件，然后针对 OpenWhisk 交叉编译该文件。可执行文件必须名为 `exec`。
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

该操作可以像其他任何操作一样运行。
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

日志也以类似的方式进行检索。
```bash
wsk activation logs --last --strip
my first Go action.
```

## 监视操作输出
{: #watching-action-output}

{{site.data.keyword.openwhisk_short}} 操作可能会由其他用户调用，以响应各种事件或作为操作序列的组成部分。在此类情况下，监视调用会非常有用。

可以使用 {{site.data.keyword.openwhisk_short}} CLI 在调用操作时监视其输出。

1. 在 shell 中发出以下命令：
  ```
wsk activation poll
  ```
  {: pre}

  此命令将启动轮询循环，以持续检查从激活生成的日志。

2. 切换到其他窗口，并调用操作：

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. 观察轮询窗口中的激活日志：

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  与此类似，每当运行轮询实用程序时，都会实时看到 OpenWhisk 中代表您运行的任何操作的日志。

## 大型应用程序支持
{: #large-app-support}

操作的最大代码大小为 48 MB。包含大量第三方模块、本机库或外部工具的应用程序可能会达到此限制。

如果您恰好创建了大于 48 MB 的包操作（zip 或 jar），解决方案是使用依赖项来扩展运行时映像，然后使用单个源文件或小于 48 MB 的归档。

例如，通过构建包含必要共享库的定制 Docker 运行时，无需在归档文件中提供这些依赖项。仍可在归档中捆绑专用源文件，并在运行时注入这些文件。

减小归档文件大小的另一个好处是缩短了部署时间。

### Python 示例

在以下 Python 示例中，opencv 可以包含 `opencv-python` 库，然后将 opencv 二进制文件安装到操作系统映像中。接下来，可以使用 `requirements.txt` 并运行 `pip install requirements.txt` 以通过更多 Python 库扩充映像。然后，可以将 `action.py` 用于新映像。

### Node.js 示例

在以下 Node.js 示例中，可以将额外的包安装到操作系统映像中：

使用 `npm` 安装 opencv：
```
npm install opencv
```
{: pre}

与此类似，如果有 `package.json`，请使用 `npm` 进行安装：
```
npm install package.json
```
{: pre}

接下来，继续将 `action.js` 用于新映像。

## 列表操作
{: #listing-actions}

您可以使用以下命令列出创建的所有操作：

```
wsk action list
```
{: pre}

随着您编写更多操作，此列表会变长，因此将相关操作分组成[包](./openwhisk_packages.html)会非常有用。要过滤操作列表以只列出特定包中的操作，可以使用以下命令语法： 

```
wsk action list [PACKAGE NAME]
```
{: pre}

## 删除操作
{: #deleting-actions}

可以通过删除不想使用的操作来进行清理。

1. 运行以下命令来删除操作：
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. 验证操作是否不再出现在操作列表中。
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## 访问操作体中的操作元数据
{: #accessing-action-metadata-within-the-action-body}

操作环境包含多个特定于运行中操作的属性。这些属性允许操作以编程方式通过 REST API 来使用 OpenWhisk 资产，或者允许设置在操作即将耗尽其分配的时间预算时发出内部警报。使用 OpenWhisk Docker 框架时，这些属性可通过所有受支持运行时的系统环境进行访问：Node.js、Python、Swift、Java 和 Docker 操作。

* `__OW_API_HOST`：运行此操作的 OpenWhisk 部署的 API 主机
* `__OW_API_KEY`：调用此操作的主体的 API 密钥，此密钥可能是受限制的 API 密钥
* `__OW_NAMESPACE`：_激活_的名称空间（这可能与操作的名称空间不同）
* `__OW_ACTION_NAME`：运行中操作的标准名称
* `__OW_ACTIVATION_ID`：此运行中操作实例的激活标识
* `__OW_DEADLINE`：此操作将耗尽整个持续时间配额时的近似时间（以戳记毫秒为度量单位）
