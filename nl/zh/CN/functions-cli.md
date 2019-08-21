---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

---



{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}




# {{site.data.keyword.openwhisk_short}} CLI
{: #functions-cli}

运行这些命令可管理组成函数的实体。
{: shortdec}

<br />

## 操作命令
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

创建操作。

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>操作的名称。要将操作包含在包中，请输入格式为 `PACKAGE_NAME`/`ACTION_NAME` 的名称。此值是必需的。</dd>

   <dt>`APP_FILE`</dt>
   <dd>要作为操作运行的应用程序文件或包的路径。此选项是必需的。</dd>
   
   <dt>`--copy`</dt>
   <dd>将操作视为现有操作的名称。</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>用于运行操作的 Docker Hub 用户名和 Docker Hub 中 Docker 映像的名称。要通过 Docker 映像创建操作，此标志是必需的。</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>应用程序的运行时。此标志是可选的。如果未指定`值`，那么将使用检测到的运行时的缺省版本。
     `--kind` 选项的可能`值`：
     <table>
  <tr>
    <th>语言</th>
    <th>Kind 标识</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code>（缺省值）和 <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code> 和 <code>python:2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（缺省值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>其他语言使用 Docker 操作进行支持。</td>
  </tr>
</table>
{: caption="表 1. 支持的运行时" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>操作的最大日志大小（以 MB 为单位）。缺省值为 10 MB。</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>如果操作的条目方法不是 `main`，请指定定制名称。条目方法不是 `main` 时，此标志是必需的。对于某些运行时（如 Java），名称必须是标准方法。</dd>

   <dt>`--native`</dt>
   <dd>可以使用 `--native` 自变量作为 `--docker openwhisk/dockerskeleton` 的简写。通过此参数，可以创建和部署在标准 Docker 操作 SDK 内运行的可执行文件。
       <ol><li>创建 Docker 映像时，会在容器内的 `/action/exec` 中创建可执行文件。请将 `/action/exec` 文件复制到本地文件系统，并将其压缩成 `exec.zip`。</li>
       <li>创建将可执行文件作为初始化数据接收的 Docker 操作。`--native` 自变量会替换 `--docker openwhisk/dockerskeleton` 自变量。
        </li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>创建操作序列，并包含相关操作的名称。用逗号分隔 `ACTION_NAME`。</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>超时`限制`（以毫秒为单位）。缺省值为 60000 毫秒。达到超时后，操作将终止。</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>将操作视为 Web 操作、原始 HTTP Web 操作或标准操作。对于 Web 操作，指定 `yes` 或 `true`；对于原始 HTTP Web 操作，指定 `raw`；或者对于标准操作，指定 `no` 或 `false`。要保护 Web 操作，请同时包含 `--web-secure` 选项。</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>保护 Web 操作。`SECRET` 的 `VALUE` 可以为 `true`、`false` 或任何字符串。此选项只能与 `--web` 选项一起使用。</dd>
   </dl>

<br />**示例**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **输出**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

可以通过删除不想再使用的操作来清理名称空间。

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **输出**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

获取描述特定操作的元数据。

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>操作的名称。此值是必需的。</dd>

   <dt>`--save`</dt>
   <dd>可以获取并本地保存与现有操作关联的代码，但序列和 Docker 操作除外。`FILENAME` 与当前工作目录中的现有操作名称相对应，文件扩展名与操作类型相对应。例如，对于作为归档文件的操作码，将使用扩展名 .zip。此标志是可选的。</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>通过提供文件路径、`文件名`和扩展名将操作代码保存在定制命名的文件中。此标志是可选的。</dd>

  <dt>`--summary`</dt>
  <dd>获取操作详细信息的摘要。前缀为“*”的参数已绑定；前缀为“**”的参数已绑定并最终完成。此标志是可选的。</dd>

  <dt>`--url`</dt>
  <dd>仅获取操作的 URL。此标志是可选的。</dd>
   </dl>

<br />**示例**

```
ibmcloud fn action get hello
```
{: pre}

**输出**
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
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

运行操作以进行测试。

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>操作的名称。此值是必需的。</dd>

   <dt>`--blocking, -b`</dt>
   <dd>阻塞性调用使用请求和响应样式来等待激活结果可用。等待时间段为 60 秒或操作的[时间`限制``值`](/docs/openwhisk?topic=cloud-functions-limits)（两者取较短的时间）。此标志是可选的。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   <dt>`--result, -r`</dt>
   <dd>应用程序代码的结果显示为命令的输出。如果未指定此选项，那么将显示激活标识。指定此选项时，调用将进行阻塞。此标志是可选的。</dd>

   </dl>

<br />**示例**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

列出已创建的所有操作或列出特定数量的操作。

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>一系列操作的名称。此值是可选的。如果未指定，将列出所有操作。</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>列出指定数量的操作。缺省值为 30 个操作。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的操作的列表排序，否则列表按创建日期排序。</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>从结果中排除指定数量的最近创建的操作。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

更新操作或操作中的应用程序。

更新包、操作或触发器的参数时，必须指定所有先前创建的参数。否则，将除去先前创建的参数。对于包，还会除去已绑定到包的任何服务，因此在更新其他参数后，必须再次[绑定服务](/docs/openwhisk?topic=cloud-functions-services)到包。
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**命令选项**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>操作的名称。要将操作包含在包中，请输入格式为 `PACKAGE_NAME`/`ACTION_NAME` 的名称。此值是必需的。</dd>

  <dt>`APP_FILE`</dt>
  <dd>要作为操作运行的应用程序文件或包的路径。要更新操作中的应用程序时，此选项是必需的。</dd>

  <dt>`--copy`</dt>
  <dd>将操作视为现有操作的名称。</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>用于运行操作的 Docker Hub 用户名和 Docker Hub 中 Docker 映像的名称。要通过 Docker 映像创建操作，此标志是必需的。</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>应用程序的运行时。此标志是可选的。如果未指定值，那么将使用检测到的运行时的缺省版本。
    `--kind` 选项的可能值。
    <table>
  <tr>
    <th>语言</th>
    <th>Kind 标识</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code>（缺省值）和 <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code> 和 <code>python:2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（缺省值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>其他语言使用 Docker 操作进行支持。</td>
  </tr>
</table>
{: caption="表 1. 支持的运行时" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>操作的最大日志大小（以 MB 为单位）。缺省值为 10 MB。</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>如果操作的条目方法不是 `main`，请指定定制名称。条目方法不是 `main` 时，此标志是必需的。对于某些运行时（如 Java），名称必须是标准方法。</dd>

  <dt>`--native`</dt>
  <dd>可以使用 `--native` 自变量作为 `--docker openwhisk/dockerskeleton` 的简写。通过使用此参数，可以创建和部署在标准 Docker 操作 SDK 内运行的可执行文件。
      <ol><li>创建 Docker 映像时，会在容器内的 `/action/exec` 中创建可执行文件。请将 `/action/exec` 文件复制到本地文件系统，并将其压缩成 `exec.zip`。</li>
      <li>创建将可执行文件作为初始化数据接收的 Docker 操作。`--native` 自变量会替换 `--docker openwhisk/dockerskeleton` 自变量。
        </li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>通过指定相关操作的名称来创建操作序列。</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>超时限制（以毫秒为单位）。缺省值为 60000 毫秒。达到超时后，操作将终止。</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>将操作视为 Web 操作、原始 HTTP Web 操作或标准操作。对于 Web 操作，指定 `yes` 或 `true`；对于原始 HTTP Web 操作，指定 `raw`；或者对于标准操作，指定 `no` 或 `false`。要保护 Web 操作，请同时包含 `--web-secure` 选项。</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>保护 Web 操作。`SECRET` 的 `VALUE` 可以为 `true`、`false` 或任何字符串。此选项只能与 `--web` 选项一起使用。</dd>
  </dl>

<br />**示例**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## 激活命令
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

获取描述特定激活的元数据。

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**命令选项**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>特定激活的标识。使用 `ibmcloud fn activation list` 可检索可用标识的列表。除非指定了 `--last` 或 `-l` 选项，否则此值是必需的。</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>元数据中要显示其中信息的字段。例如，要显示日志字段，请运行 `ibmcloud fn activation get ACTIVATION_ID logs`。此值是可选的。</dd>

  <dt>`--last, -l`</dt>
  <dd>显示最近激活的元数据。此标志是可选的。</dd>

  <dt>`--summary, -s`</dt>
  <dd>仅显示激活详细信息中的结果响应。此标志是可选的。</dd>
  </dl>


<br />**示例**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

列出包中所有操作的所有激活标识。

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**命令选项**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>显示完整激活描述。</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>列出指定数量的激活。缺省值为 30 个激活，最大值为 200 个激活。</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>列出自指定日期以来创建的激活。持续时间以自 1970 年 1 月 1 日以来的毫秒数度量。示例：`1560371263` 表示 2019 年 6 月 12 日 08:27:43 UTC。</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>从结果中排除指定数量的最近激活。</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>列出指定日期之前创建的激活。持续时间以自 1970 年 1 月 1 日以来的毫秒数度量。示例：`1560371263` 表示 2019 年 6 月 12 日 08:27:43 UTC。</dd>
  </dl>

<br />**示例**
```
ibmcloud fn activation list
```
{: pre}

**输出**
```
activations
    44794bd6aab74415b4e42a308d880e5b         hello
    6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

获取特定激活的日志。

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**命令选项**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>特定激活的标识。使用 `ibmcloud fn activation list` 可检索可用标识的列表。除非指定了 `--last` 或 `-l` 选项，否则此值是必需的。</dd>

  <dt>`--last, -l`</dt>
  <dd>显示最近激活的日志。此标志是可选的。</dd>

  <dt>`--strip, -r`</dt>
  <dd>仅显示日志消息；不包含时间戳记和流信息。此标志是可选的。</dd>
  </dl>

<br />**示例**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

查看操作或名称空间的激活的流式实时列表。可以按 `CTRL+C` 键退出轮询。

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**命令选项**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>以 / 开头的名称空间。轮询名称空间、操作或空间的激活。此值是可选的。如果未指定名称空间或操作，那么将对空间进行轮询。</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>轮询名称空间、操作或空间的激活。此值是可选的。如果未指定名称空间或操作，那么将对空间进行轮询。</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>轮询激活持续指定的秒数，达到此时间后退出。此标志是可选的。</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>从指定天数之前开始轮询激活。此标志是可选的。</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>从指定小时数之前开始轮询激活。此标志是可选的。</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>从指定分钟数之前开始轮询激活。此标志是可选的。</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>从指定秒数之前开始轮询激活。此标志是可选的。</dd>
  </dl>

<br />**示例**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

获取特定激活的结果。

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**命令选项**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>特定激活的标识。使用 `ibmcloud fn activation list` 可检索可用标识的列表。除非指定了 `--last` 或 `-l` 选项，否则此值是必需的。</dd>

  <dt>`--last, -l`</dt>
  <dd>显示最近激活的结果。此标志是可选的。</dd>

  </dl>

<br />**示例**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## API 命令
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

创建 API。

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API 的基本路径。</dd>

   <dt>`API_NAME`</dt>
   <dd>API 的名称。API 名称可能与基本路径相同。</dd>

   <dt>`API_VERB`</dt>
   <dd>API 的动词，例如 `get` 或 `post`。</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>操作的名称。</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>API 的名称。指定配置文件时，将忽略此标志。缺省名称为 `BASE_PATH`。此标志是可选的。</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>包含 Swagger API 配置的 JSON 文件。使用此标志时，将忽略 API 名称标志。这个标志是必需的。</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>将 Web 操作响应类型设置为 `html`、`http`、`json`、`text` 或 `svg`。缺省值为 `json`。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **输出**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

删除 API。

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API 的基本路径。</dd>

   <dt>`API_NAME`</dt>
   <dd>API 的名称。API 名称可能与基本路径相同。</dd>

   <dt>`API_PATH`</dt>
   <dd>API 的路径。</dd>

   <dt>`API_VERB`</dt>
   <dd>API 的动词，例如 `GET` 或 `POST`。</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>将 API 输出类型指定为 `json` 或 `yaml`。缺省值为 `json`。</dd>

   <dt>`--full, -f`</dt>
   <dd>显示完整的 API 配置详细信息。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

获取 API 的元数据。

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API 的基本路径。</dd>

   <dt>`API_NAME`</dt>
   <dd>API 的名称。API 名称可能与基本路径相同。</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>将 API 输出类型指定为 `json` 或 `yaml`。缺省值为 `json`。</dd>

   <dt>`--full, -f`</dt>
   <dd>显示完整的 API 配置详细信息。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

列出已创建的所有 API 或列出特定数量的 API。如果未指定名称或基本路径，那么将列出所有 API。

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API 的基本路径。</dd>

   <dt>`API_NAME`</dt>
   <dd>API 的名称。API 名称可能与基本路径相同。</dd>

   <dt>`API_PATH`</dt>
   <dd>API 的路径。</dd>

   <dt>`API_VERB`</dt>
   <dd>API 的动词，例如 `GET` 或 `POST`。</dd>

   <dt>`--full, -f`</dt>
   <dd>显示完整的 API 详细信息。此标志是可选的。</dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>列出指定数量的 API。缺省值为 30 个 API。此标志是可选的。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的 API 列表进行排序，否则列表按创建日期排序。此标志是可选的。</dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>从结果中排除指定数量的最近创建的 API。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## 部署命令
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

使用清单文件部署包、操作、触发器和规则的集合。

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>`wsk` API 主机。此标志是可选的。</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--config` `FILE`</dt>
   <dd>配置文件。缺省值为 `$HOME/.wskprops`。</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>部署文件的路径。</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>清单文件的路径。如果 manifest.yaml 不在当前目录中，那么此标志是必需的。</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>名称空间的名称或标识。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   <dt>`--preview` </dt>
   <dd>在部署之前显示部署计划。</dd>

   <dt>`--project PATH`</dt>
   <dd>无服务器项目的路径。缺省值为 <code>.</code>（当前目录）。</dd>

   <dt>`--strict`</dt>
   <dd>允许使用用户定义的运行时版本。</dd>

   <dt>`--verbose, -v`</dt>
   <dd>查看详细输出。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

使用清单文件取消部署包、操作、触发器和规则的集合。

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>`wsk` API 主机。此标志是可选的。</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--config` `FILE`</dt>
   <dd>配置文件。缺省值为 `$HOME/.wskprops`。</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>部署文件的路径。</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>清单文件的路径。如果 manifest.yaml 不在当前目录中，那么此标志是必需的。</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>名称空间的名称或标识。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   <dt>`--preview` </dt>
   <dd>在不运行命令的情况下显示命令的结果。</dd>

   <dt>`--project PATH`</dt>
   <dd>无服务器项目的路径。缺省值为 `.`（当前目录）。</dd>

   <dt>`--strict`</dt>
   <dd>允许使用用户定义的运行时版本。</dd>

   <dt>`--verbose, -v`</dt>
   <dd>查看详细输出。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## 列表命令
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

查看名称空间中的包、操作、触发器和规则的分组列表。

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的每个实体组进行排序，否则每个组按创建日期排序。</dd>
   </dl>

<br />**示例**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## 名称空间命令
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

创建 IAM 名称空间。

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>名称空间的名称。不要在名称中包含连字符 (-)。此值是必需的。</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>编写您自己的唯一描述，以帮助您识别名称空间。如果描述有多个词，请用引号 (") 将描述括起。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

删除 IAM 名称空间。

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**示例**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

获取 Cloud Foundry 或 IAM 名称空间的实体或其中的元数据信息。

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>名称空间的名称或标识。此值是必需的。</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的名称空间列表进行排序，否则列表按创建日期排序。此标志是可选的。</dd>

   <dt>`--properties`</dt>
   <dd>显示名称空间属性，而不是显示名称空间中包含的实体。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **输出**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

列出可用的 Cloud Foundry 和 IAM 名称空间。

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--cf`</dt>
   <dd>仅显示 Cloud Foundry 名称空间。不显示 IAM 名称空间。此标志是可选的。</dd>

   <dt>`--iam`</dt>
   <dd>仅显示 IAM 名称空间。不显示 Cloud Foundry 名称空间。此标志是可选的。</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>列出指定数量的名称空间。缺省值为 30 个名称空间。此标志是可选的。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的名称空间列表进行排序，否则列表按创建日期排序。此标志是可选的。</dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>从结果中排除指定数量的最近创建的名称空间。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

更改 IAM 名称空间的名称或描述。

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>名称空间的名称。不要在名称中包含连字符 (-)。此值是必需的。</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>名称空间的新名称。不要在名称中包含连字符 (-)。此值是可选的。</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>编写您自己的唯一描述，以帮助您识别名称空间。如果描述有多个词，请用引号 (") 将描述括起。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## 打包命令
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

将参数绑定到包。除非另外指定，否则包中的所有操作都会继承这些参数。

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>包的名称。此值是必需的。</dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>
  </dl>

<br />**示例**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

创建旨在包含一个或多个操作的包。要在包中添加操作，请在创建或更新操作时，在操作名称中包含包名称。

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>包的名称。此值是必需的。</dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

  <dt>`--shared yes|no`</dt>
  <dd>如果没有指定值或指定值 yes，那么包将与其他用户共享。</dd>
  </dl>

<br />**示例**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **输出**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

可以通过删除不想再使用的包来清理名称空间。

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **输出**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

获取描述特定包的元数据。

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**命令选项**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>包的名称。此值是必需的。</dd>

   <dt>`--summary`</dt>
   <dd>获取包详细信息的摘要。前缀为“*”的参数已绑定。此标志是可选的。</dd>
   </dl>

<br />**示例**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `  ibmcloud fn package list
  `
{: #cli_pkg_list}

列出已创建的所有包或列出特定数量的包。

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>列出特定名称空间中的包。此值是可选的。如果未指定，将列出所有包。</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>列出指定数量的包。缺省值为 30 个包。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的包列表进行排序，否则列表按创建日期排序。</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>从结果中排除指定数量的最近创建的包。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  运行 `ibmcloud fn package list /whisk.system` 可查看预安装的包的列表。
  {: tip}


<br />

### `  ibmcloud fn package refresh
  `
{: #cli_pkg_refresh}

刷新特定名称空间中所有包的包绑定。

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>以 / 开头的名称空间。这个标志是必需的。运行 `ibmcloud fn namespace list` 以获取可从中进行选择的名称空间的列表。</dd>
   </dl>

<br />**示例**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

更新旨在包含一个或多个操作的包。要在包中添加操作，请在创建或更新操作时，在操作名称中包含包名称。

更新包、操作或触发器的参数时，必须指定所有先前创建的参数。否则，将除去先前创建的参数。对于包，还会除去已绑定到包的任何服务，因此在更新其他参数后，必须再次[绑定服务](/docs/openwhisk?topic=cloud-functions-services)到包。
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>包的名称。此值是必需的。</dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   <dt>`--shared yes|no`</dt>
   <dd>如果没有指定值或指定值 `yes`，那么包将与其他用户共享。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **输出**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## 属性命令
{: #cli_prop}

设置 CLI 环境的全局属性或查看有关 `wsk` CLI 的属性；wsk CLI 作为 `ibmcloud fn` CLI 的一部分运行。

### `ibmcloud fn property get`
{: #cli_prop_get}

通过 `wsk` CLI 查看属性的元数据详细信息。

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--all`</dt>
   <dd>查看 `wsk` CLI 的所有属性。此标志是可选的。</dd>

   <dt>`---apibuild`</dt>
   <dd>`wsk` API 构建信息。此标志是可选的。</dd>

   <dt>`--apibuildno`</dt>
   <dd>`wsk` API 构建号。此标志是可选的。</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 主机。此标志是可选的。</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 版本。此标志是可选的。</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 客户机证书。此标志是可选的。</dd>

   <dt>`--cliversion`</dt>
   <dd>`wsk` CLI 版本。此标志是可选的。</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 客户机`密钥`。此标志是可选的。</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 名称空间。无法为 Cloud Foundry 名称空间设置此标志。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
ibmcloud fn property get --auth
```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

设置属性。至少需要一个标志。设置属性后，该属性就会保留在工作站的 `<home_dir>/.bluemix/plugins/cloud-functions/config.json` 中。要除去属性，请运行 [`ibmcloud fn property unset --<property>`](#cli_prop_set)。 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 主机。此标志是可选的。</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 版本。此标志是可选的。</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 客户机证书。此标志是可选的。</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 客户机`密钥`。此标志是可选的。</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 名称空间。无法为 Cloud Foundry 名称空间设置此标志。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **输出**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

取消设置 `wsk` CLI 的属性。至少需要一个标志。

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 主机。此标志是可选的。</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 版本。此标志是可选的。</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>`wsk` 授权`密钥`。此标志是可选的。</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 客户机证书。此标志是可选的。</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 客户机`密钥`。此标志是可选的。</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 名称空间。无法为 Cloud Foundry 名称空间设置此标志。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## 规则命令
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

创建规则以将触发器与操作相关联。创建规则之前，必须先创建触发器和操作。

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **输出**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

要清理名称空间，请除去不再需要的规则。

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>规则的名称。此值是必需的。</dd>

  <dt>`--disable`</dt>
  <dd>删除规则之前，先禁用该规则。</dd>
  </dl>


<br />**示例**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

将规则的状态更改为不活动，并阻止该规则在触发触发器时运行操作。

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

将规则的状态从不活动更改为活动。处于活动状态时，操作会在触发触发器时运行。

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

获取描述特定规则的元数据。

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>规则的名称。此值是必需的。</dd>

  <dt>`--summary`</dt>
  <dd>获取规则详细信息的摘要。</dd>
  </dl>

<br />**示例**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

列出已创建的所有规则或列出特定数量的规则。

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>规则的名称。此值是可选的。如果未指定，将列出所有规则。</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>列出指定数量的规则。缺省值为 30 个规则。</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>按名称对返回的规则列表进行排序，否则列表按创建日期排序。</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>从结果中排除指定数量的最近创建的规则。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

查看规则是处于活动状态还是不活动状态。运行 `ibmcloud fn rule disable` 或 `ibmcloud fn run enable` 命令可更改状态。

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

要更改哪些触发器与哪些规则相关联，可以更新规则。

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**示例**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## SDK 命令
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

安装 SDK。

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>SDK 组件，例如 `docker`、`iOS` 和 `bashauto`。此值是必需的。</dd>

   <dt>`--stdout, --s`</dt>
   <dd>将 bash 命令结果打印到 `STDOUT`。此标志是可选的。</dd>


   </dl>

<br />**示例**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## 服务命令
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

将服务凭证绑定到操作或包。

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>服务的名称。</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>要将凭证绑定到的包或操作的名称。</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>服务实例名称。</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>要绑定的服务`密钥`凭证的名称。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

取消服务凭证与操作或包的绑定。

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>服务的名称。</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>要与凭证取消绑定的包或操作的名称。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## 触发器命令
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

创建触发器。

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>触发器的名称。此值是必需的。</dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>将触发器类型设置为订阅源。此标志是可选的。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>


   </dl>

<br />**示例**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

删除触发器。

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**示例**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

通过触发触发器来对其进行测试，而不是等待该触发器自动触发。

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>触发器的名称。此值是必需的。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数`值`。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>

   </dl>

<br />**示例**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

获取描述特定触发器的元数据。

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>触发器的名称。此值是必需的。</dd>

  <dt>`--summary`</dt>
  <dd>获取触发器详细信息的摘要。</dd>
  </dl>

<br />**示例**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `    ibmcloud fn trigger list
    `
{: #cli_trigger_list}

列出已创建的所有触发器或列出特定数量的触发器。

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>触发器的名称。此值是可选的。如果未指定，将列出所有触发器。</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>列出指定数量的触发器。缺省值为 30 个触发器。</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>按名称对返回的触发器列表进行排序，否则列表按创建日期排序。</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>从结果中排除指定数量的最近创建的触发器。</dd>

   </dl>

<br />**示例**

  ```
    ibmcloud fn trigger list
    ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

更新触发器。

更新包、操作或触发器的参数时，必须指定所有先前创建的参数。否则，将除去先前创建的参数。对于包，还会除去已绑定到包的任何服务，因此在更新其他参数后，必须再次[绑定服务](/docs/openwhisk?topic=cloud-functions-services)到包。
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**命令选项**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>触发器的名称。此值是必需的。</dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>注释以 `KEY` `VALUE` 格式指定。要包含多个注释，请为每个注释指定此选项。此标志是可选的。</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>包含 `KEY` `VALUE` 格式的注释的 JSON 文件。此标志是可选的。</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 格式的参数值。此标志是可选的。</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>包含参数（格式为 `KEY` 和 `VALUE`）的 JSON 文件。此标志是可选的。</dd>
   </dl>

<br />**示例**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






