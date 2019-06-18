---

copyright:
  years: 2017, [{CURRENT_<em>YEAR</em>}]
lastupdated: "2019-05-20"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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
{:gif: data-image-type='gif'}



# {{site.data.keyword.openwhisk_short}} CLI
{: #functions-cli}

執行這些指令可管理組成函數的實體。
{: shortdec}



<br />

## 動作指令
{: #cli_action}



### ibmcloud fn action create
{: #cli_action_create}

建立動作。
    

```
ibmcloud fn action create <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker docker_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>包含 <em>KEY</em> <em>VALUE</em> 格式的註釋的 JSON 檔案。這是選用旗標。</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>動作的名稱。若要在套件中包含動作，請以 <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em> 格式輸入名稱。這是必要值。</dd>

   <dt><em>APP_FILE</em></dt>
   <dd>要執行作為動作之應用程式檔案或套件的路徑。這是必要選項。</dd>

   <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
   <dd>動作的最大容器內並行啟動<em>限制</em>。預設值是一次啟動。</dd>

   <dt>--copy</dt>
   <dd>將動作視為現有動作的名稱。</dd>

   <dt>--docker <em>DOCKER_HUB_USERNAME</em>/<em>IMAGE_NAME</em></dt>
   <dd>Docker Hub 使用者名稱以及 Docker Hub 中之 Docker 映像檔的名稱，以便執行動作。這個旗標在從 Docker 映像檔建立動作時是必要的。</dd>

   <dt>--kind <em>LANGUAGE</em></dt>
   <dd>應用程式的運行環境。這是選用旗標。如果未指定 <em>VALUE</em>，會使用所偵測到之運行環境的預設版本。--kind 選項的可能 <em>VALUES</em>：
     <table>
  <tr>
    <th>語言</th>
    <th>類型 ID</th>
  </tr>
  <tr>
    <td> Node.js </td>
    <td> <code>nodejs:10</code>（預設值）、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code>（預設值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（預設值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（預設值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（預設值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（預設值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>其他語言是使用 Docker 動作支援的。</td>
  </tr>
</table>
{: caption="表 1. 受支援的運行環境" caption-side="top"}
       </dd>

   <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
   <dd>動作的日誌大小上限 (MB)。預設值是 10 MB。</dd>

   <dt>--main <em>ENTRY_METHOD_NAME</em></dt>
   <dd>如果動作的進入方法不是 `main`，請指定自訂名稱。此進入方法不是 `main` 時，這是必要的旗標。對於部分運行環境（例如 Java），名稱必須是完整的方法。</dd>

   <dt>--native</dt>
   <dd>您可以使用 `--native` 引數，作為 `--docker openwhisk/dockerskeleton` 的速記。這個引數可讓您建立及部署在標準 Docker 動作 SDK 內執行的執行檔。
       <ol><li>當您建立 Docker 映像檔時，會在位於 `/action/exec` 的容器內建立二進位執行檔。請將 `/action/exec` 檔案複製到本端檔案系統，並將它壓縮成 `exec.zip`。</li>
       <li>建立可將執行檔當作起始設定資料來接收的 Docker 動作。`--native` 引數會取代 `--docker openwhisk/dockerskeleton` 引數。
        </li></ol>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
   <dd>建立動作序列，並包含以逗點分隔的相關動作的名稱。</dd>

   <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
   <dd>逾時<em>限制</em>（以毫秒為單位）。預設值是 60000 毫秒。達到逾時值時，便會終止動作。</dd>

   <dt>--web yes|true|raw|no|false</dt>
   <dd>將動作視為 Web 動作、原始 HTTP Web 動作，或標準動作。指定 <code>yes</code> 或 <code>true</code> 表示 Web 動作，<code>raw</code> 表示原始 HTTP Web 動作，<code>no</code> 或 <code>false</code> 則表示標準動作。若要保護您的 Web 動作，也請包含 `--web-secure` 選項。</dd>

   <dt>--web-secure <em>SECRET</em></dt>
   <dd>保護 Web 動作。<em>SECRET</em> 的 <em>VALUE</em> 可以是 <em>true</em>、<em>false</em> 或任何字串。這個選項只能與 `--web` 選項搭配使用。</dd>
   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  輸出：
  ```
  ok: created hello
  ```
  {: screen}


<br />

### ibmcloud fn action delete
{: #cli_action_delete}

可以透過刪除不想再使用的動作來清理名稱空間。

```
ibmcloud fn action delete <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  輸出：
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn action get
{: #cli_action_get}

取得說明特定動作的 meta 資料。

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as <em>FILENAME</em>] [--summary] [--url]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>動作的名稱。這是必要值。</dd>

   <dt>--save</dt>
   <dd>可以取得並本端儲存與現有動作關聯的程式碼，但序列和 Docker 動作除外。<em>FILENAME</em> 與現行工作目錄中的現有動作名稱相對應，副檔名與動作類型相對應。例如，對於作為 zip 檔案的動作碼，將使用副檔名 .zip。這是選用旗標。</dd>

  <dt>--save-as <em>FILENAME</em></dt>
  <dd>透過提供檔案路徑、<em>檔名</em>和副檔名，將動作碼儲存在自訂命名的檔案中。這是選用旗標。</dd>

  <dt>--summary</dt>
  <dd>取得動作詳細資料的摘要。字首為 "*" 的參數已連結；字首為 "**" 的參數已連結並終結。這是選用旗標。</dd>

  <dt>--url</dt>
  <dd>僅取得動作的 URL。這是選用旗標。</dd>
   </dl>

<br /><strong>範例</strong>：

```
ibmcloud fn action get hello
```
{: pre}

輸出：
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
            "<em>KEY</em>": "執行程式",
            "<em>VALUE</em>": "nodejs:6"
        }
    ],
    "<em>LIMIT</em> s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />
### ibmcloud fn action invoke
{: #cli_action_invoke}

執行動作以測試。

```
ibmcloud fn action invoke <em>ACTION_NAME</em> [--blocking] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--result]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>動作的名稱。這是必要值。</dd>

   <dt>--blocking, -b</dt>
   <dd>封鎖式呼叫使用要求和回應樣式來等待啟動結果可用。等待期間為 60 秒和動作的[時間<em>限制</em><em>值</em>](/docs/openwhisk?topic=cloud-functions-limits)這兩者中較短的時間。這是選用旗標。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   <dt>--result, -r</dt>
   <dd>應用程式程式碼的結果顯示為指令的輸出。如果未指定此選項，將顯示啟動 ID。指定此選項時，呼叫將為封鎖式。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />
###     ibmcloud fn action list
    
{: #cli_action_list}

列出已建立的所有動作或列出特定數目的動作。

```
ibmcloud fn action list <em>ACTION_NAME</em> [--limit <em>NUMBER_OF_ACTIONS</em>] [--name-sort] [--skip <em>NUMBER_OF_ACTIONS</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>一系列動作的名稱。這是選用值。如果未指定，將列出所有動作。</dd>

   <dt>--limit <em>NUMBER_OF_ACTIONS</em>, -l <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>列出指定數目的動作。預設為 30 個動作。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的動作的清單排序，否則清單會依建立日期排序。</dd>

   <dt>--skip <em>NUMBER_OF_ACTIONS</em>, -s <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>從結果中排除指定數目的最近建立的動作。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />
### ibmcloud fn action update
{: #cli_action_update}

更新動作或動作中的應用程式。

```
ibmcloud fn action update <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker docker_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>動作的名稱。若要在套件中包含動作，請以 <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em> 格式輸入名稱。這是必要值。</dd>

  <dt><em>APP_FILE</em></dt>
  <dd>要執行作為動作之應用程式檔案或套件的路徑。要更新動作中的應用程式時，此選項是必要的。</dd>

  <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
  <dd>動作的最大容器內並行啟動限制。 預設值是一次啟動。</dd>

  <dt>--copy</dt>
  <dd>將動作視為現有動作的名稱。</dd>

  <dt>--docker docker_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>Docker Hub 使用者名稱以及 Docker Hub 中之 Docker 映像檔的名稱，以便執行動作。這個旗標在從 Docker 映像檔建立動作時是必要的。</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>應用程式的運行環境。這是選用旗標。如果未指定 <em>VALUE</em>，會使用所偵測到之運行環境的預設版本。--kind 選項的可能值：
     <table>
  <tr>
    <th>語言</th>
    <th>類型 ID</th>
  </tr>
  <tr>
    <td> Node.js </td>
    <td> <code>nodejs:10</code>（預設值）、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code>（預設值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（預設值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（預設值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（預設值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（預設值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>其他語言是使用 Docker 動作支援的。</td>
  </tr>
</table>
{: caption="表 1. 受支援的運行環境" caption-side="top"}
      </dd>

  <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
  <dd>動作的日誌大小上限 (MB)。預設值是 10 MB。</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>如果動作的進入方法不是 `main`，請指定自訂名稱。此進入方法不是 `main` 時，這是必要的旗標。對於部分運行環境（例如 Java），名稱必須是完整的方法。</dd>

  <dt>--native</dt>
  <dd>您可以使用 `--native` 引數，作為 `--docker openwhisk/dockerskeleton` 的速記。這個引數可讓您建立及部署在標準 Docker 動作 SDK 內執行的執行檔。
       <ol><li>當您建立 Docker 映像檔時，會在位於 `/action/exec` 的容器內建立二進位執行檔。請將 `/action/exec` 檔案複製到本端檔案系統，並將它壓縮成 `exec.zip`。</li>
      <li>建立可將執行檔當作起始設定資料來接收的 Docker 動作。`--native` 引數會取代 `--docker openwhisk/dockerskeleton` 引數。
        </li></ol>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

  <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
  <dd>透過指定相關動作的名稱來建立動作序列。</dd>

  <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
  <dd>逾時限制（以毫秒為單位）。預設值是 60000 毫秒。達到逾時值時，便會終止動作。</dd>

  <dt>--web yes|true|raw|no|false</dt>
  <dd>將動作視為 Web 動作、原始 HTTP Web 動作，或標準動作。指定 <code>yes</code> 或 <code>true</code> 表示 Web 動作，<code>raw</code> 表示原始 HTTP Web 動作，<code>no</code> 或 <code>false</code> 則表示標準動作。若要保護您的 Web 動作，也請包含 `--web-secure` 選項。</dd>

  <dt>--web-secure <em>SECRET</em></dt>
  <dd>保護 Web 動作。<em>SECRET</em> 的 <em>VALUE</em> 可以是 <em>true</em>、<em>false</em> 或任何字串。這個選項只能與 `--web` 選項搭配使用。</dd>
  </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## 啟動指令
{: #cli_activation}


### ibmcloud fn activation get
{: #cli_activation_get}

取得說明特定啟動的 meta 資料。

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<br /><strong>指令選項</strong>：

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定啟動的 ID。請使用 `ibmcloud fn activation list` 來擷取可用 ID 的清單。除非指定了 `--last` 或 `-l` 選項，否則這是必要值。</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>meta 資料中要顯示其中資訊的欄位。例如，要顯示日誌欄位，請執行 `ibmcloud fn activation get ACTIVATION_ID logs`。這是選用值。</dd>

  <dt>--last, -l</dt>
  <dd>顯示最近啟動的 meta 資料。這是選用旗標。</dd>

  <dt>--summary, -s</dt>
  <dd>僅顯示啟動詳細資料中的結果回應。這是選用旗標。</dd>
  </dl>


<br /><strong>範例</strong>：
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation list

{: #cli_activation_list}

列出套件中所有動作的所有啟動 ID。

```
ibmcloud fn activation list [--full] [--limit <em>NUMBER_OF_ACTIVATIONS</em>] [--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>] [--skip <em>NUMBER_OF_ACTIVATIONS</em>] [--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>]
```
{: pre}


<br /><strong>指令選項</strong>：

  <dl>
  <dt>--full, -f</dt>
  <dd>顯示完整啟動說明</dd>

  <dt>--limit <em>NUMBER_OF_ACTIVATIONS</em>, -l <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>列出指定數目的啟動。預設為 30 個啟動，最大為 200 個啟動。</dd>

  <dt>--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>列出自指定日期以來建立的啟動。以毫秒為單位測量。範例：Th, 01, Jan 1970</dd>

  <dt>--skip <em>NUMBER_OF_ACTIVATIONS</em>, -s <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>從結果中排除指定數目的最近啟動。</dd>

  <dt>--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>列出指定日期之前建立的啟動。以毫秒為單位測量。範例：Th, 01, Jan 1970</dd>
  </dl>

<br /><strong>範例</strong>：
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


<br />
### ibmcloud fn activation logs
{: #cli_activation_logs}

取得特定啟動的日誌。

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定啟動的 ID。請使用 `ibmcloud fn activation list` 來擷取可用 ID 的清單。這是必要值，除非指定 `--last` 或 `-l` 選項。</dd>

  <dt>--last, -l</dt>
  <dd>顯示最近啟動的日誌。這是選用旗標。</dd>

  <dt>--strip, -r</dt>
  <dd>僅顯示日誌訊息；不包含時間戳記和串流資訊。這是選用旗標。</dd>
  </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation poll

{: #cli_activation_poll}

檢視動作或名稱空間的啟動的串流式即時清單。可以按 CTRL+C 鍵結束輪詢。

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit <em>SECONDS</em>] [--since-days <em>DAYS</em>] [-since-hours <em>HOURS</em>] [--since-minutes <em>MINUTES</em>] [--since-seconds <em>SECONDS</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>名稱空間，開頭為 /。輪詢名稱空間、動作或空間的啟動。這是選用值。如果未指定名稱空間或動作，則會輪詢空間。</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>輪詢名稱空間、動作或空間的啟動。這是選用值。如果未指定名稱空間或動作，則會輪詢空間。</dd>

  <dt>--exit <em>SECONDS</em>, -e <em>SECONDS</em></dt>
  <dd>輪詢啟動持續指定的秒數，達到此時間後結束。這是選用旗標。</dd>

  <dt>--since-days <em>DAYS</em></dt>
  <dd>從指定天數之前開始輪詢啟動。這是選用旗標。</dd>

  <dt>--since-hours <em>HOURS</em></dt>
  <dd>從指定小時數之前開始輪詢啟動。這是選用旗標。</dd>

  <dt>--since-minutes <em>MINUTES</em></dt>
  <dd>從指定分鐘數之前開始輪詢啟動。這是選用旗標。</dd>

  <dt>--since-seconds <em>SECONDS</em></dt>
  <dd>從指定秒數之前開始輪詢啟動。這是選用旗標。</dd>
  </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn activation poll
```
{: pre}


<br />
### ibmcloud fn activation result
{: #cli_activation_result}

取得特定啟動的結果。

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<br /><strong>指令選項</strong>：

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定啟動的 ID。請使用 `ibmcloud fn activation list` 來擷取可用 ID 的清單。這是必要值，除非指定 `--last` 或 `-l` 選項。</dd>

  <dt>--last, -l</dt>
  <dd>顯示最近啟動的結果。這是選用旗標。</dd>

  </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## API 指令
{: #cli_api}


### ibmcloud fn api create
{: #cli_api_create}

建立 API。

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB <em>ACTION_NAME</em>] [--apiname API_NAME] [--config-file <em>FILE</em>] [--response-type TYPE]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API 的基礎路徑。</dd>

   <dt>API_NAME</dt>
   <dd>API 的名稱。API 名稱可能與基礎路徑相同。</dd>

   <dt>API_VERB</dt>
   <dd>API 的動詞，例如 `get` 或 `post`。</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>動作的名稱。</dd>

   <dt><em>--apiname API_NAME, -n API_NAME</em></dt>
   <dd>API 的名稱。指定配置檔時，將忽略此旗標。預設名稱為 BASE_PATH。這是選用旗標。</dd>

   <dt>--config-file <em>FILE</em>, -c <em>FILE</em></dt>
   <dd>包含 Swagger API 配置的 JSON 檔案。使用此旗標時，將忽略 API 名稱旗標。這是必要旗標。</dd>

   <dt>--response-type TYPE</dt>
   <dd>將 Web 動作回應類型設定為 `html`、`http`、`json`、`text` 或 `svg`。預設值是 `json`。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  輸出：
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}


<br />
### ibmcloud fn api delete
{: #cli_api_delete}

刪除 API。

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API 的基礎路徑。</dd>

   <dt>API_NAME</dt>
   <dd>API 的名稱。API 名稱可能與基礎路徑相同。</dd>

   <dt>API_PATH</dt>
   <dd>API 的路徑。</dd>

   <dt>API_VERB</dt>
   <dd>API 的動詞，例如 `get` 或 `post`。</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>將 API 輸出類型指定為 `json` 或 `yaml`。預設值是 `json`。</dd>

   <dt>--full, -f</dt>
   <dd>顯示完整 API 配置詳細資料。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />
### ibmcloud fn api get
{: #cli_api_get}

取得 API 的 meta 資料。

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API 的基礎路徑。</dd>

   <dt>API_NAME</dt>
   <dd>API 的名稱。API 名稱可能與基礎路徑相同。</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>將 API 輸出類型指定為 `json` 或 `yaml`。預設值是 `json`。</dd>

   <dt>--full, -f</dt>
   <dd>顯示完整 API 配置詳細資料。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />
###       ibmcloud fn api list
      
{: #cli_api_list}

列出已建立的所有 API 或列出特定數目的 API。如果未指定名稱或基本路徑，將列出所有 API。

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API 的基礎路徑。</dd>

   <dt>API_NAME</dt>
   <dd>API 的名稱。API 名稱可能與基礎路徑相同。</dd>

   <dt>API_PATH</dt>
   <dd>API 的路徑。</dd>

   <dt>API_VERB</dt>
   <dd>API 的動詞，例如 `get` 或 `post`。</dd>

   <dt>--full, -f</dt>
   <dd>顯示完整的 API 詳細資料。這是選用旗標。</dd>

   <dt>--limit NUMBER_OF_APIS, -l NUMBER_OF_APIS</dt>
   <dd>列出指定數目的 API。預設為 30 個 API。這是選用旗標。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的 API 清單進行排序，否則清單會依建立日期排序。這是選用旗標。</dd>

   <dt>--skip NUMBER_OF_APIS, -s NUMBER_OF_APIS</dt>
   <dd>從結果中排除指定數目的最近建立的 API。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
      ibmcloud fn api list
      ```
  {: pre}




<br /><br />
## 部署指令
{: #cli_deploy_cmds}


### ibmcloud fn deploy
{: #cli_deploy}

使用資訊清單檔部署套件、動作、觸發程式和規則的集合。

```
ibmcloud fn deploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>--apihost HOST</dt>
   <dd><code>wsk</code> API 主機。這是選用旗標。</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>配置檔。預設值是 <code>$HOME/.wskprops</code>。</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>部署檔案的路徑。</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>資訊清單檔的路徑。如果 manifest.yaml 不在現行目錄中，這是必要旗標。</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>名稱空間的名稱或 ID。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   <dt>--preview </dt>
   <dd>在部署之前顯示部署計劃。</dd>

   <dt>--project PATH</dt>
   <dd>無伺服器專案的路徑。預設為 <code>.</code>（現行目錄）。</dd>

   <dt>--strict</dt>
   <dd>容許使用者定義的運行環境版本。</dd>

   <dt>--verbose, -v</dt>
   <dd>檢視詳細輸出。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />
### ibmcloud fn undeploy
{: #cli_undeploy}

使用資訊清單檔取消部署套件、動作、觸發程式和規則的集合。

```
ibmcloud fn undeploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE <em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--apihost HOST</dt>
   <dd><code>wsk</code> API 主機。這是選用旗標。</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>配置檔。預設值是 <code>$HOME/.wskprops</code>。</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>部署檔案的路徑。</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>資訊清單檔的路徑。如果 manifest.yaml 不在現行目錄中，這是必要旗標。</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>名稱空間的名稱或 ID。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   <dt>--preview </dt>
   <dd>在部署之前顯示取消部署計劃。</dd>

   <dt>--project PATH</dt>
   <dd>無伺服器專案的路徑。預設為 <code>.</code>（現行目錄）。</dd>

   <dt>--strict</dt>
   <dd>容許使用者定義的運行環境版本。</dd>

   <dt>--verbose, -v</dt>
   <dd>檢視詳細輸出。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## 清單指令
{: #cli_list_cmd}


### ibmcloud fn list
{: #cli_list}

檢視名稱空間中的套件、動作、觸發程式和規則的分組清單。

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的每個實體群組進行排序，否則每個群組會依建立日期排序。</dd>
   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## 名稱空間指令
{: #cli_namespace}


### ibmcloud fn namespace create
{: #cli_namespace_create}

建立 IAM 名稱空間。

```
ibmcloud fn namespace create <em>NAMESPACE</em> [--description DESCRIPTION] 
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名稱空間的名稱。名稱中請不要包含連字號 (-)。這是必要值。</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>撰寫自己的唯一說明，以協助您識別名稱空間。如果您的說明是多個字組，請在說明前後加上引號 (")。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />
### ibmcloud fn namespace delete
{: #cli_namespace_delete}

刪除 IAM 名稱空間。

```
ibmcloud fn namespace delete <em>NAMESPACE</em>
```
{: pre}


<br /><strong>範例</strong>：

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />
### ibmcloud fn namespace get
{: #cli_namespace_get}

取得 Cloud Foundry 或 IAM 名稱空間的實體或其中的 meta 資料資訊。

```
ibmcloud fn namespace list <em>NAMESPACE</em> [--auth <em>KEY</em>] [--name-sort] [--properties] 
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名稱空間的名稱或 ID。這是必要值。</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱排序傳回的名稱空間清單，否則清單會依建立日期排序。這是選用旗標。</dd>

   <dt>--properties</dt>
   <dd>顯示名稱空間內容，而不是顯示名稱空間中包含的實體。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  輸出：
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />
###   ibmcloud fn namespace list
  
{: #cli_namespace_list}

列出可用的 Cloud Foundry 和 IAM 名稱空間。

```
ibmcloud fn namespace list [--auth <em>KEY</em>] [--cf] [--iam] [--limit NUMBER_OF_<em>NAMESPACE</em> S] [--name-sort] [--skip NUMBER_OF_<em>NAMESPACE</em> S] 
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--cf</dt>
   <dd>僅顯示 Cloud Foundry 名稱空間。不顯示 IAM 名稱空間。這是選用旗標。</dd>

   <dt>--iam</dt>
   <dd>僅顯示 IAM 名稱空間。不顯示 Cloud Foundry 名稱空間。這是選用旗標。</dd>

   <dt>--limit NUMBER_OF_<em>NAMESPACE</em> S, -l NUMBER_OF_<em>NAMESPACE</em> S</dt>
   <dd>列出指定數目的名稱空間。預設為 30 個名稱空間。這是選用旗標。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱排序傳回的名稱空間清單，否則清單會依建立日期排序。這是選用旗標。</dd>

   <dt>--skip NUMBER_OF_<em>NAMESPACE</em> S, -s NUMBER_OF_<em>NAMESPACE</em> S</dt>
   <dd>從結果中排除指定數目的最近建立的名稱空間。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />
### ibmcloud fn namespace update
{: #cli_namespace_update}

變更 IAM 名稱空間的名稱或說明。

```
ibmcloud fn namespace update <em>NAMESPACE</em> [NEW_<em>NAMESPACE</em>_NAME] [--description DESCRIPTION] 
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名稱空間的名稱。名稱中請不要包含連字號 (-)。這是必要值。</dd>

   <dt>NEW_<em>NAMESPACE</em>_NAME</dt>
   <dd>名稱空間的新名稱。名稱中請不要包含連字號 (-)。這是選用值。</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>撰寫自己的唯一說明，以協助您識別名稱空間。如果您的說明是多個字組，請在說明前後加上引號 (")。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## 包裝指令
{: #cli_pkg}


### ibmcloud fn package bind
{: #cli_pkg_bind}

將參數連結到套件。除非另外指定，否則套件中的所有動作都會繼承這些參數。

```
ibmcloud fn package bind <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>套件的名稱。這是必要值。</dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>
  </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />
### ibmcloud fn package create
{: #cli_pkg_create}

建立主要用來包含一個以上動作的套件。若要在套件中新增動作，請在建立或更新動作時包含套件名稱與動作名稱。

```
ibmcloud fn package create <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>套件的名稱。這是必要值。</dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>包含參數 <em>KEY</em> 和 C 的 JSON 檔案。此旗標是選用的。</dd>

  <dt>--shared yes|no</dt>
  <dd>如果沒有指定值或指定值 yes，套件將與其他使用者共用。</dd>
  </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  輸出：
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />
### ibmcloud fn package delete
{: #cli_pkg_delete}

可以透過刪除不想再使用的套件來清理名稱空間。

```
ibmcloud fn package delete <em>PACKAGE_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  輸出：
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn package get
{: #cli_pkg_get}

取得說明特定套件的 meta 資料。

```
ibmcloud fn package get <em>PACKAGE_NAME</em> [--summary]
```
{: pre}

<br /><strong>指令選項</strong>：

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>套件的名稱。這是必要值。</dd>

   <dt>--summary</dt>
   <dd>取得套件詳細資料的摘要。字首為 "*" 的參數已連結。這是選用旗標。</dd>
   </dl>

<br /><strong>範例</strong>：

```
ibmcloud fn package get hello
```
{: pre}


<br />
###     ibmcloud fn package list
    
{: #cli_pkg_list}

列出已建立的所有套件或列出特定數目的套件。

```
ibmcloud fn package list [<em>NAMESPACE</em>] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>NAMESPACE</em></dt>
   <dd>列出特定名稱空間中的套件。這是選用值。如果未指定，將列出所有套件。</dd>

   <dt>--limit NUMBER_OF_PACKAGES, -l NUMBER_OF_PACKAGES</dt>
   <dd>列出指定數目的套件。預設為 30 個套件。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的套件清單進行排序，否則清單會依建立日期排序。</dd>

   <dt>--skip NUMBER_OF_PACKAGES, -s NUMBER_OF_PACKAGES</dt>
   <dd>從結果中排除指定數目的最近建立的套件。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package list
  ```
  {: pre}

  執行 `ibmcloud fn package list /whisk.system` 可檢視預先安裝的套件的清單。
  {: tip}


<br />
###   ibmcloud fn package refresh
  
{: #cli_pkg_refresh}

重新整理特定名稱空間中所有套件的套件連結。

```
ibmcloud fn package refresh /<em>NAMESPACE</em>
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>名稱空間，開頭為 /。這是必要旗標。執行 <code>ibmcloud fn namespace list</code> 以取得可從中進行選擇的名稱空間的清單。</dd>
   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />
### ibmcloud fn package update
{: #cli_pkg_update}

更新主要用來包含一個以上動作的套件。若要在套件中新增動作，請在建立或更新動作時包含套件名稱與動作名稱。

```
ibmcloud fn package update <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>套件的名稱。這是必要值。</dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   <dt>--shared yes|no</dt>
   <dd>如果沒有指定值或指定值 <code>yes</code>，套件將與其他使用者共用。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  輸出：
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## 內容指令
{: #cli_prop}

設定 CLI 環境的廣域內容或檢視有關 <code>wsk</code> CLI 的內容；wsk CLI 作為 `ibmcloud fn` CLI 的一部分執行。

### ibmcloud fn property get
{: #cli_prop_get}

透過 <code>wsk</code> CLI 檢視內容的 meta 資料詳細資料。

```
ibmcloud fn property get [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--all</dt>
   <dd>檢視 <code>wsk</code> CLI 的所有內容。這是選用旗標。</dd>

   <dt>---apibuild</dt>
   <dd><code>wsk</code> API 建置資訊。這是選用旗標。</dd>

   <dt>--apibuildno</dt>
   <dd><code>wsk</code> API 建置號碼。這是選用旗標。</dd>

   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API 主機。這是選用旗標。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API 版本。這是選用旗標。</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端憑證。這是選用旗標。</dd>

   <dt>--cliversion</dt>
   <dd><code>wsk</code> CLI 版本。這是選用旗標。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名稱空間。無法針對 Cloud Foundry 名稱空間設定此旗標。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn property get --cliversion
  ```
  {: pre}


<br />
### ibmcloud fn property set
{: #cli_prop_set}

設定內容。至少需要一個旗標。

```
ibmcloud fn property set [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API 主機。這是選用旗標。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API 版本。這是選用旗標。</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端憑證。這是選用旗標。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名稱空間。無法針對 Cloud Foundry 名稱空間設定此旗標。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  輸出：
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### ibmcloud fn property unset
{: #cli_prop_unset}

取消設定 <code>wsk</code> CLI 的內容。至少需要一個旗標。

```
ibmcloud fn property unset [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API 主機。這是選用旗標。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API 版本。這是選用旗標。</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd><code>wsk</code> 授權 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端憑證。這是選用旗標。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> 用戶端 <em>KEY</em>。這是選用旗標。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名稱空間。無法針對 Cloud Foundry 名稱空間設定此旗標。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn property unset --key my <em>KEY</em>
  ```
  {: pre}



<br /><br />
## 規則指令
{: #cli_rule}


### ibmcloud fn rule create
{: #cli_rule_create}

建立規則以便建立觸發程式與動作的關聯。建立規則之前，必須先建立觸發程式和動作。

```
ibmcloud fn rule create <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  輸出：
  ```
  ok: created myrule
  ```
  {: screen}


<br />
### ibmcloud fn rule delete
{: #cli_rule_delete}

若要清理名稱空間，請移除不再需要的規則。

```
ibmcloud fn rule delete <em>RULE_NAME</em> [--disable]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>規則的名稱。這是必要值。</dd>

  <dt>--disable</dt>
  <dd>刪除規則之前，先停用該規則。</dd>
  </dl>


<br /><strong>範例</strong>：

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />
### ibmcloud fn rule disable
{: #cli_rule_disable}

將規則的狀態變更為非作用中，並阻止該規則在發動觸發程式時執行動作。

```
ibmcloud fn rule disable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule enable
{: #cli_rule_enable}

將規則的狀態從非作用中變更為作用中。處於作用中狀態時，動作會在發動觸發程式時執行。

```
ibmcloud fn rule enable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule get
{: #cli_rule_get}

取得說明特定規則的 meta 資料。

```
ibmcloud fn rule get <em>RULE_NAME</em> [--summary]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>規則的名稱。這是必要值。</dd>

  <dt>--summary</dt>
  <dd>取得規則詳細資料的摘要。</dd>
  </dl>

<br /><strong>範例</strong>：

```
ibmcloud fn rule get myrule
```
{: pre}


<br />
### ibmcloud fn rule list
{: #cli_rule_list}

列出已建立的所有規則或列出特定數目的規則。

```
ibmcloud fn rule list <em>RULE_NAME</em> [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>規則的名稱。這是選用值。如果未指定，將列出所有規則。</dd>

   <dt>--limit NUMBER_OF_RULES, -l NUMBER_OF_RULES</dt>
   <dd>列出指定數目的規則。預設為 30 個規則。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的規則清單進行排序，否則清單會依建立日期排序。</dd>

   <dt>--skip NUMBER_OF_RULES, -s NUMBER_OF_RULES</dt>
   <dd>從結果中排除指定數目的最近建立的規則。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />
### ibmcloud fn rule status
{: #cli_rule_status}

查看規則是處於作用中狀態還是非作用中狀態。執行 `ibmcloud fn rule disable` 或 `ibmcloud fn run enable` 指令可變更狀態。

```
ibmcloud fn rule status <em>RULE_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />
### ibmcloud fn rule update
{: #cli_rule_update}

若要變更哪些觸發程式與哪些規則相關聯，可以更新規則。

```
ibmcloud fn rule update <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## SDK 指令
{: #cli_sdk}


### ibmcloud fn sdk install
{: #cli_sdk_install}

安裝 SDK。

```
ibmcloud fn sdk install <em>COMPONENT</em> [--limit <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>COMPONENT</em></dt>
   <dd>SDK 元件，例如 `docker`、`iOS` 和 `bashauto`。這是必要值。</dd>

   <dt>--stdout, --s</dt>
   <dd>將 bash 指令結果列印到 stdout。這是選用旗標。</dd>


   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## 服務指令
{: #cli_service}


### ibmcloud fn service bind
{: #cli_service_bind}

將服務認證連結到動作或套件。

```
ibmcloud fn service bind SERVICE PACKAGE_or_<em>ACTION_NAME</em> [--instance SERVICE_INSTANCE] [--keyname SERVICE_<em>KEY</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>SERVICE</dt>
   <dd>服務的名稱。</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>要連結認證的套件或動作名稱。</dd>

   <dt>--instance SERVICE_INSTANCE</dt>
   <dd>服務實例名稱。</dd>

   <dt>--keyname SERVICE_<em>KEY</em></dt>
   <dd>要連結的服務<em>金鑰</em>認證的名稱。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />
### ibmcloud fn service unbind
{: #cli_service_unbind}

取消服務認證與動作或套件的連結。

```
ibmcloud fn service unbind SERVICE PACKAGE_or_<em>ACTION_NAME</em>
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt>SERVICE</dt>
   <dd>服務的名稱。</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>要連結認證的套件或動作名稱。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## 觸發程式指令
{: #cli_trigger}


### ibmcloud fn trigger create
{: #cli_trigger_create}

建立觸發程式。

```
ibmcloud fn trigger create <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--feed <em>ACTION_NAME</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>觸發程式的名稱。這是必要值。</dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

   <dt>--feed <em>ACTION_NAME</em>, -f <em>ACTION_NAME</em></dt>
   <dd>將觸發程式類型設定為資訊來源。這是選用旗標。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>


   </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />
### ibmcloud fn trigger delete
{: #cli_trigger_delete}

刪除觸發程式。

```
ibmcloud fn trigger delete <em>TRIGGER_NAME</em>
```
{: pre}

<br /><strong>範例</strong>：

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />
### ibmcloud fn trigger fire
{: #cli_trigger_fire}

透過發動觸發程式來對其測試，而不是等待該觸發程式自動觸發。

```
ibmcloud fn trigger fire <em>TRIGGER_NAME</em> [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>

   <dt><em>TRIGGER_NAME</em></dt>
   <dd>觸發程式的名稱。這是必要值。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數 <em>VALUES</em>。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />
### ibmcloud fn trigger get
{: #cli_trigger_get}

取得說明特定觸發程式的 meta 資料。

```
ibmcloud fn trigger get <em>TRIGGER_NAME</em> [--summary]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>觸發程式的名稱。這是必要值。</dd>

  <dt>--summary</dt>
  <dd>取得觸發程式詳細資料的摘要。</dd>
  </dl>

<br /><strong>範例</strong>：

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

###     ibmcloud fn trigger list
    
{: #cli_trigger_list}

列出已建立的所有觸發程式或列出特定數目的觸發程式。

```
ibmcloud fn trigger list <em>TRIGGER_NAME</em> [--limit <em>NUMBER_OF_TRIGGERS</em>] [--name-sort] [--skip <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>觸發程式的名稱。這是選用值。如果未指定，將列出所有觸發程式。</dd>

   <dt>--limit <em>NUMBER_OF_TRIGGERS</em>, -l <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>列出指定數目的觸發程式。預設為 30 個觸發程式。</dd>

   <dt>--name-sort, -n</dt>
   <dd>依名稱對傳回的觸發程式清單進行排序，否則清單會依建立日期排序。</dd>

   <dt>--skip <em>NUMBER_OF_TRIGGERS</em>, -s <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>從結果中排除指定數目的最近建立的觸發程式。</dd>

   </dl>

<br /><strong>範例</strong>：

  ```
    ibmcloud fn trigger list
    ```
  {: pre}


<br />

### ibmcloud fn trigger update
{: #cli_trigger_update}

更新觸發程式。

```
ibmcloud fn trigger update <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>指令選項</strong>：

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>觸發程式的名稱。這是必要值。</dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>註釋是以 <em>KEY</em> <em>VALUE</em> 格式指定。若要包含多個註釋，請為每個註釋指定這個選項。這是選用旗標。</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>包含 <em>KEY</em> VALE 格式之註釋的 JSON 檔案。這是選用旗標。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>KEY</em> <em>VALUE</em> 格式的參數值。這是選用旗標。</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>包含參數 <em>KEYS</em> 及 <em>VALUES</em> 的 JSON 檔案。這是選用旗標。</dd>
   </dl>

<br /><strong>範例</strong>：
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}



