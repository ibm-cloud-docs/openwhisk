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

以下のコマンドを実行して、関数を構成するエンティティーを管理できます。
{: shortdec}



<br />

## アクションのコマンド
{: #cli_action}



### ibmcloud fn action create
{: #cli_action_create}

アクションを作成します。

```
ibmcloud fn action create <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

   <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のアノテーションが含まれた JSON ファイル。このフラグはオプションです。</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>アクションの名前。 パッケージにアクションを含めるには、<em>パッケージ名</em>/<em>アクション名</em> の形式で名前を入力します。この値は必須です。 </dd>

   <dt><em>APP_FILE</em></dt>
   <dd>アクションとして実行するアプリ・ファイルまたはパッケージのパス。このオプションは必須です。</dd>

   <dt>--concurrency <em>ACTIVATION_LIMIT</em>、-c <em>ACTIVATION_LIMIT</em></dt>
   <dd>コンテナー内でアクションを同時にアクティブ化する最大<em>限度</em>。デフォルト値は 1 回のアクティベーションです。</dd>

   <dt>--copy</dt>
   <dd>アクションを既存アクションの名前として扱います。</dd>

   <dt>--docker <em>DOCKER_HUB_USERNAME</em>/<em>IMAGE_NAME</em></dt>
   <dd>Docker Hub のユーザー名と、アクションを実行する Docker Hub の Docker イメージの名前。Docker イメージからアクションを作成する場合は、このフラグが必須です。</dd>

   <dt>--kind <em>LANGUAGE</em></dt>
   <dd>アプリのランタイム。このフラグはオプションです。 <em>値</em> を指定しない場合は、検出されたランタイムのデフォルト・バージョンが使用されます。
     --kind オプションに指定できる<em>値</em> は以下のとおりです。
     <table>
  <tr>
    <th>言語</th>
    <th>種類識別子</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (デフォルト)、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Docker アクションを使用すると、他の言語がサポートされます。</td>
  </tr>
</table>
{: caption="表 1. サポートされるランタイム" caption-side="top"}
       </dd>

   <dt>--logsize <em>LIMIT</em>、-l <em>LIMIT</em></dt>
   <dd>アクションの最大ログ・サイズ (MB 単位)。デフォルト値は 10 MB です。</dd>

   <dt>--main <em>ENTRY_METHOD_NAME</em></dt>
   <dd>アクションのエントリー・メソッドが `main` でない場合に、カスタム名を指定します。エントリー・メソッドが `main` でない場合は、このフラグが必須です。Java など、ランタイムによっては完全修飾メソッドの名前にする必要があります。</dd>

   <dt>--native</dt>
   <dd>`--docker openwhisk/dockerskeleton` の省略表現として、`--native` 引数が使用できます。 この引数を使用すると、標準的な Docker アクション SDK 内で実行される実行可能ファイルを作成してデプロイできます。
       <ol><li>Docker イメージを作成すると、`/action/exec` にあるコンテナー内にバイナリー実行可能ファイルが作成されます。`/action/exec` ファイルをローカル・ファイル・システムにコピーして、`exec.zip` に zip します。</li>
       <li>実行可能ファイルを初期化データとして受け取る Docker アクションを作成します。 `--docker openwhisk/dockerskeleton` 引数に代わって、`--native` 引数が使用されています。</li></ol>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--sequence <em>ACTION_NAME</em>、<em>ACTION_NAME</em></dt>
   <dd>アクション・シーケンスを作成し、関連するアクションの名前をコンマで区切って指定します。</dd>

   <dt>--timeout <em>LIMIT</em>、-t <em>LIMIT</em></dt>
   <dd>タイムアウト<em>限度</em> (ミリ秒単位)。デフォルト値は 60000 ミリ秒です。タイムアウトになると、アクションは強制終了されます。</dd>

   <dt>--web yes|true|raw|no|false</dt>
   <dd>アクションを Web アクション、未加工 HTTP Web アクション、または標準のアクションとして扱います。Web アクションの場合は <code>yes</code> または <code>true</code>、未加工 HTTP Web アクションの場合は <code>raw</code>、標準のアクションの場合は <code>no</code> または <code>false</code> を指定します。Web アクションを保護するには、`--web-secure` オプションも指定します。</dd>

   <dt>--web-secure <em>SECRET</em></dt>
   <dd>Web アクションを保護します。<em>SECRET</em> の<em>値</em> として、<em>true</em>、<em>false</em>、または任意のストリングを指定できます。このオプションは、`--web` オプションを指定した場合のみ使用できます。</dd>
   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  出力:
  ```
  ok: created hello
  ```
  {: screen}


<br />

### ibmcloud fn action delete
{: #cli_action_delete}

不要になったアクションを削除して、名前空間をクリーンアップできます。

```
ibmcloud fn action delete <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  出力:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn action get
{: #cli_action_get}

特定のアクションを表すメタデータを取得します。

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as <em>FILENAME</em>] [--summary] [--url]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>アクションの名前。この値は必須です。</dd>

   <dt>--save</dt>
   <dd>既存のアクションに関連付けられているコードを取得してローカルに保存できます。ただし、シーケンスと Docker アクションは例外です。現行作業ディレクトリー内の既存のアクション名に対応する<em>ファイル名</em> になり、アクションの種類に対応するファイル拡張子が付けられます。例えば、zip ファイルであるアクション・コードの場合は、拡張子 .zip が使用されます。このフラグはオプションです。</dd>

  <dt>--save-as <em>FILENAME</em></dt>
  <dd>ファイル・パス、<em>ファイル名</em>、および拡張子を指定して、アクションのコードをカスタム名のファイルに保存します。このフラグはオプションです。</dd>

  <dt>--summary</dt>
  <dd>アクションの詳細のサマリーを取得します。接頭部が「*」のパラメーターはバインドされ、接頭部が「**」のパラメーターはバインドされてファイナライズされています。このフラグはオプションです。</dd>

  <dt>--url</dt>
  <dd>アクションの URL のみ取得します。このフラグはオプションです。</dd>
   </dl>

<br /><strong>例</strong>:

```
ibmcloud fn action get hello
```
{: pre}

出力:
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
            "<em>KEY</em>": "exec",
            "<em>VALUE</em>": "nodejs:6"
        }
    ],
    "<em>LIMIT</em>s": {
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

アクションを実行してテストします。

```
ibmcloud fn action invoke <em>ACTION_NAME</em> [--blocking] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--result]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>アクションの名前。 この値は必須です。 </dd>

   <dt>--blocking、-b</dt>
   <dd>ブロッキング呼び出しは、要求と応答のスタイルを使用して、アクティベーションの結果が返されるまで待機します。待機時間は、60 秒とアクションの[時間<em>制限</em> <em>値</em>](/docs/openwhisk?topic=cloud-functions-limits) のいずれか小さいほうです。このフラグはオプションです。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--result、-r</dt>
   <dd>コマンドの出力としてアプリ・コードの結果が表示されます。このオプションを指定しない場合は、アクティベーション ID が表示されます。このオプションを指定すると、呼び出しはブロッキングされます。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />
### ibmcloud fn action list
{: #cli_action_list}

作成したすべてのアクションまたは特定の数のアクションをリストします。

```
ibmcloud fn action list <em>ACTION_NAME</em> [--limit <em>NUMBER_OF_ACTIONS</em>] [--name-sort] [--skip <em>NUMBER_OF_ACTIONS</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>アクションのパッケージの名前。この値はオプションです。 指定しない場合は、すべてのアクションがリストされます。</dd>

   <dt>--limit <em>NUMBER_OF_ACTIONS</em>、-l <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>指定した数のアクションをリストします。デフォルトは 30 個のアクションです。</dd>

   <dt>--name-sort、-n</dt>
   <dd>返されたアクションのリストを名前でソートします。指定しない場合、リストは作成日でソートされます。</dd>

   <dt>--skip <em>NUMBER_OF_ACTIONS</em>、-s <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>最近作成されたアクションを、指定した数だけ結果から除外します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />
### ibmcloud fn action update
{: #cli_action_update}

アクションまたはアクション内のアプリを更新します。

```
ibmcloud fn action update <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

  <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
  <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>アクションの名前。 パッケージにアクションを含めるには、<em>パッケージ名</em>/<em>アクション名</em> の形式で名前を入力します。この値は必須です。 </dd>

  <dt><em>APP_FILE</em></dt>
  <dd>アクションとして実行するアプリ・ファイルまたはパッケージのパス。アクション内のアプリを更新する場合は、このオプションが必須です。</dd>

  <dt>--concurrency <em>ACTIVATION_LIMIT</em>、-c <em>ACTIVATION_LIMIT</em></dt>
  <dd>コンテナー内でアクションを同時にアクティブ化する最大限度。デフォルト値は 1 回のアクティベーションです。</dd>

  <dt>--copy</dt>
  <dd>アクションを既存アクションの名前として扱います。</dd>

  <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>Docker Hub のユーザー名と、アクションを実行する Docker Hub の Docker イメージの名前。Docker イメージからアクションを作成する場合は、このフラグが必須です。</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>アプリのランタイム。このフラグはオプションです。 <em>値</em> を指定しない場合は、検出されたランタイムのデフォルト・バージョンが使用されます。
     --kind オプションに指定できる値は以下のとおりです。
    <table>
  <tr>
    <th>言語</th>
    <th>種類識別子</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (デフォルト)、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Docker アクションを使用すると、他の言語がサポートされます。</td>
  </tr>
</table>
{: caption="表 1. サポートされるランタイム" caption-side="top"}
      </dd>

  <dt>--logsize <em>LIMIT</em>、-l <em>LIMIT</em></dt>
  <dd>アクションの最大ログ・サイズ (MB 単位)。デフォルト値は 10 MB です。</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>アクションのエントリー・メソッドが `main` でない場合に、カスタム名を指定します。エントリー・メソッドが `main` でない場合は、このフラグが必須です。Java など、ランタイムによっては完全修飾メソッドの名前にする必要があります。</dd>

  <dt>--native</dt>
  <dd>`--docker openwhisk/dockerskeleton` の省略表現として、`--native` 引数が使用できます。 この引数を使用すると、標準的な Docker アクション SDK 内で実行される実行可能ファイルを作成してデプロイできます。
       <ol><li>Docker イメージを作成すると、`/action/exec` にあるコンテナー内にバイナリー実行可能ファイルが作成されます。`/action/exec` ファイルをローカル・ファイル・システムにコピーして、`exec.zip` に zip します。</li>
      <li>実行可能ファイルを初期化データとして受け取る Docker アクションを作成します。 `--docker openwhisk/dockerskeleton` 引数に代わって、`--native` 引数が使用されています。</li></ol>

  <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

  <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
  <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

  <dt>--sequence <em>ACTION_NAME</em>、<em>ACTION_NAME</em></dt>
  <dd>関連するアクションの名前を指定してアクション・シーケンスを作成します。</dd>

  <dt>--timeout <em>LIMIT</em>、-t <em>LIMIT</em></dt>
  <dd>タイムアウト限度 (ミリ秒単位)。デフォルト値は 60000 ミリ秒です。タイムアウトになると、アクションは強制終了されます。</dd>

  <dt>--web yes|true|raw|no|false</dt>
  <dd>アクションを Web アクション、未加工 HTTP Web アクション、または標準のアクションとして扱います。Web アクションの場合は <code>yes</code> または <code>true</code>、未加工 HTTP Web アクションの場合は <code>raw</code>、標準のアクションの場合は <code>no</code> または <code>false</code> を指定します。Web アクションを保護するには、`--web-secure` オプションも指定します。</dd>

  <dt>--web-secure <em>SECRET</em></dt>
  <dd>Web アクションを保護します。<em>SECRET</em> の<em>値</em> として、<em>true</em>、<em>false</em>、または任意のストリングを指定できます。このオプションは、`--web` オプションを指定した場合のみ使用できます。</dd>
  </dl>

<br /><strong>例</strong>:
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## アクティベーションのコマンド
{: #cli_activation}


### ibmcloud fn activation get
{: #cli_activation_get}

特定のアクティベーションを表すメタデータを取得します。

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定のアクティベーションの ID。存在する ID のリストを取得するには、`ibmcloud fn activation list` を使用します。`--last` オプションまたは `-l` オプションを指定しない場合は、この値が必須です。</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>情報を表示するメタデータ内のフィールド。例えば、logs フィールドを表示するには、`ibmcloud fn activation get ACTIVATION_ID logs` を実行します。この値はオプションです。</dd>

  <dt>--last、-l</dt>
  <dd>最新のアクティベーションのメタデータを表示します。このフラグはオプションです。</dd>

  <dt>--summary、-s</dt>
  <dd>アクティベーションの詳細のうち結果の応答のみを表示します。このフラグはオプションです。</dd>
  </dl>


<br /><strong>例</strong>:
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation list
{: #cli_activation_list}

パッケージ内のすべてのアクションのすべてのアクティベーション ID をリストします。

```
ibmcloud fn activation list [--full] [--limit <em>NUMBER_OF_ACTIVATIONS</em>] [--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>] [--skip <em>NUMBER_OF_ACTIVATIONS</em>] [--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>]
```
{: pre}


<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt>--full、-f</dt>
  <dd>アクティベーションの詳細な説明を表示します。</dd>

  <dt>--limit <em>NUMBER_OF_ACTIVATIONS</em>、-l <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>指定した数のアクティベーションをリストします。デフォルトは 30 個のアクティベーション、最大は 200 個のアクティベーションです。</dd>

  <dt>--since <em>DAY_OF_THE_WEEK</em>、<em>DAY</em>、<em>MONTH</em>、<em>YEAR</em></dt>
  <dd>指定した日付以降に作成されたアクティベーションをリストします。ミリ秒単位で測定されます。例: Th, 01, Jan 1970</dd>

  <dt>--skip <em>NUMBER_OF_ACTIVATIONS</em>、-s <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>最近のアクティベーションを、指定した数だけ結果から除外します。</dd>

  <dt>--upto <em>DAY_OF_THE_WEEK</em>、<em>DAY</em>、<em>MONTH</em>、<em>YEAR</em></dt>
  <dd>指定した日付より前に作成されたアクティベーションをリストします。ミリ秒単位で測定されます。例: Th, 01, Jan 1970</dd>
  </dl>

<br /><strong>例</strong>:
```
ibmcloud fn activation list
```
{: pre}

出力:
```
activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />
### ibmcloud fn activation logs
{: #cli_activation_logs}

特定のアクティベーションのログを取得します。

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定のアクティベーションの ID。存在する ID のリストを取得するには、`ibmcloud fn activation list` を使用します。`--last` オプションまたは `-l` オプションを指定しない場合は、この値が必須です。</dd>

  <dt>--last、-l</dt>
  <dd>最新のアクティベーションのログを表示します。このフラグはオプションです。</dd>

  <dt>--strip、-r</dt>
  <dd>ログ・メッセージのみ表示します。タイム・スタンプとストリーム情報は除外します。このフラグはオプションです。</dd>
  </dl>

<br /><strong>例</strong>:
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation poll
{: #cli_activation_poll}

アクションまたは名前空間のアクティベーションのストリーミング・ライブ・リストを表示します。ポーリングを終了するには CTRL+C を押します。

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit <em>SECONDS</em>] [--since-days <em>DAYS</em>] [-since-hours <em>HOURS</em>] [--since-minutes <em>MINUTES</em>] [--since-seconds <em>SECONDS</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>/ から始まる名前空間。名前空間、アクション、またはスペースのアクティベーションをポーリングします。この値はオプションです。 名前空間もアクションも指定しない場合は、スペースがポーリングされます。</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>名前空間、アクション、またはスペースのアクティベーションをポーリングします。この値はオプションです。 名前空間もアクションも指定しない場合は、スペースがポーリングされます。</dd>

  <dt>--exit <em>SECONDS</em>、-e <em>SECONDS</em></dt>
  <dd>指定した秒数だけアクティベーションをポーリングした後に終了します。このフラグはオプションです。</dd>

  <dt>--since-days <em>DAYS</em></dt>
  <dd>指定した日数前のアクティベーションのポーリングを開始します。このフラグはオプションです。</dd>

  <dt>--since-hours <em>HOURS</em></dt>
  <dd>指定した時間数前のアクティベーションのポーリングを開始します。このフラグはオプションです。</dd>

  <dt>--since-minutes <em>MINUTES</em></dt>
  <dd>指定した分数前のアクティベーションのポーリングを開始します。このフラグはオプションです。</dd>

  <dt>--since-seconds <em>SECONDS</em></dt>
  <dd>指定した秒数前のアクティベーションのポーリングを開始します。このフラグはオプションです。</dd>
  </dl>

<br /><strong>例</strong>:
```
ibmcloud fn activation poll
```
{: pre}


<br />
### ibmcloud fn activation result
{: #cli_activation_result}

特定のアクティベーションの結果を取得します。

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>特定のアクティベーションの ID。存在する ID のリストを取得するには、`ibmcloud fn activation list` を使用します。`--last` オプションまたは `-l` オプションを指定しない場合は、この値が必須です。</dd>

  <dt>--last、-l</dt>
  <dd>最新のアクティベーションの結果を表示します。このフラグはオプションです。</dd>

  </dl>

<br /><strong>例</strong>:
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## API コマンド
{: #cli_api}


### ibmcloud fn api create
{: #cli_api_create}

API を作成します。

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB <em>ACTION_NAME</em>] [--apiname API_NAME] [--config-file <em>FILE</em>] [--response-type TYPE]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API の基本パス。</dd>

   <dt>API_NAME</dt>
   <dd>API の名前。通常、API 名は基本パスと同じにします。</dd>

   <dt>API_VERB</dt>
   <dd>API の verb (`get` や `post` など)。</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>アクションの名前。</dd>

   <dt><em>--apiname API_NAME、-n API_NAME</em></dt>
   <dd>API の名前。構成ファイルを指定した場合、このフラグは無視されます。デフォルトの名前は BASE_PATH です。このフラグはオプションです。</dd>

   <dt>--config-file <em>FILE</em>、-c <em>FILE</em></dt>
   <dd>Swagger API 構成が含まれている JSON ファイル。このフラグを使用すると、API の名前のフラグは無視されます。このフラグは必須です。</dd>

   <dt>--response-type TYPE</dt>
   <dd>Web アクション応答タイプを `html`、`http`、`json`、`text`、または `svg` として設定します。デフォルト値は `json` です。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  出力:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />
### ibmcloud fn api delete
{: #cli_api_delete}

API を削除します。

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API の基本パス。</dd>

   <dt>API_NAME</dt>
   <dd>API の名前。通常、API 名は基本パスと同じにします。</dd>

   <dt>API_PATH</dt>
   <dd>API のパス</dd>

   <dt>API_VERB</dt>
   <dd>API の verb (`get` や `post` など)。</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>API 出力タイプを `json` または `yaml` として指定します。デフォルト値は `json` です。</dd>

   <dt>--full、-f</dt>
   <dd>API 構成の詳細をすべて表示します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />
### ibmcloud fn api get
{: #cli_api_get}

API のメタデータを取得します。

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API の基本パス。</dd>

   <dt>API_NAME</dt>
   <dd>API の名前。通常、API 名は基本パスと同じにします。</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>API 出力タイプを `json` または `yaml` として指定します。デフォルト値は `json` です。</dd>

   <dt>--full、-f</dt>
   <dd>API 構成の詳細をすべて表示します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />
### ibmcloud fn api list
{: #cli_api_list}

作成したすべての API または特定の数の API をリストします。名前も基本パスも指定しない場合は、API がすべてリストされます。

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>API の基本パス。</dd>

   <dt>API_NAME</dt>
   <dd>API の名前。通常、API 名は基本パスと同じにします。</dd>

   <dt>API_PATH</dt>
   <dd>API のパス</dd>

   <dt>API_VERB</dt>
   <dd>API の verb (`get` や `post` など)。</dd>

   <dt>--full、-f</dt>
   <dd>API の詳細をすべて表示します。このフラグはオプションです。 </dd>

   <dt>--limit NUMBER_OF_APIS、-l NUMBER_OF_APIS</dt>
   <dd>指定した数の API をリストします。デフォルトは 30 個の API です。このフラグはオプションです。 </dd>

   <dt>--name-sort、-n</dt>
   <dd>返された API のリストを名前でソートします。指定しない場合、リストは作成日でソートされます。このフラグはオプションです。 </dd>

   <dt>--skip NUMBER_OF_APIS、-s NUMBER_OF_APIS</dt>
   <dd>最近作成された API を、指定した数だけ結果から除外します。このフラグはオプションです。 </dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn api list
  ```
  {: pre}




<br /><br />
## デプロイメントのコマンド
{: #cli_deploy_cmds}


### ibmcloud fn deploy
{: #cli_deploy}

マニフェスト・ファイルを使用して、パッケージ、アクション、トリガー、およびルールのコレクションをデプロイします。

```
ibmcloud fn deploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>--apihost HOST</dt>
   <dd><code>wsk</code> API ホスト。このフラグはオプションです。</dd>

   <dt>--auth <em>KEY</em>、-u <em>KEY</em></dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>構成ファイル。デフォルトは <code>$HOME/.wskprops</code> です。</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>デプロイメント・ファイルのパス。</dd>

   <dt>--manifest <em>FILE</em>、-m <em>FILE</em></dt>
   <dd>マニフェスト・ファイルへのパス。 manifest.yaml が現行ディレクトリーにない場合は、このフラグが必須です。</dd>

   <dt>--namespace <em>NAMESPACE</em>、-n <em>NAMESPACE</em></dt>
   <dd>名前空間の名前または ID。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--preview </dt>
   <dd>デプロイ前にデプロイメント計画を表示します。</dd>

   <dt>--project PATH</dt>
   <dd>サーバーレス・プロジェクトのパス。デフォルトは <code>.</code> (現行ディレクトリー) です。</dd>

   <dt>--strict</dt>
   <dd>ユーザー定義のランタイム・バージョンを許可します。</dd>

   <dt>--verbose、-v</dt>
   <dd>詳細出力を表示します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />
### ibmcloud fn undeploy
{: #cli_undeploy}

マニフェスト・ファイルを使用して、パッケージ、アクション、トリガー、およびルールのコレクションをアンデプロイします。

```
ibmcloud fn undeploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE<em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--apihost HOST</dt>
   <dd><code>wsk</code> API ホスト。このフラグはオプションです。</dd>

   <dt>--auth <em>KEY</em>、-u <em>KEY</em></dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>構成ファイル。デフォルトは <code>$HOME/.wskprops</code> です。</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>デプロイメント・ファイルのパス。</dd>

   <dt>--manifest <em>FILE</em>、-m <em>FILE</em></dt>
   <dd>マニフェスト・ファイルへのパス。 manifest.yaml が現行ディレクトリーにない場合は、このフラグが必須です。</dd>

   <dt>--namespace <em>NAMESPACE</em>、-n <em>NAMESPACE</em></dt>
   <dd>名前空間の名前または ID。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--preview </dt>
   <dd>デプロイ前にアンデプロイメント計画を表示します。</dd>

   <dt>--project PATH</dt>
   <dd>サーバーレス・プロジェクトのパス。デフォルトは <code>.</code> (現行ディレクトリー) です。</dd>

   <dt>--strict</dt>
   <dd>ユーザー定義のランタイム・バージョンを許可します。</dd>

   <dt>--verbose、-v</dt>
   <dd>詳細出力を表示します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## リストのコマンド
{: #cli_list_cmd}


### ibmcloud fn list
{: #cli_list}

名前空間内のパッケージ、アクション、トリガー、およびルールのグループ化リストを表示します。

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--name-sort、-n</dt>
   <dd>返されたエンティティーの各グループを名前でソートします。指定しない場合、各グループは作成日でソートされます。</dd>
   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## 名前空間コマンド
{: #cli_namespace}


### ibmcloud fn namespace create
{: #cli_namespace_create}

IAM 名前空間を作成します。

```
ibmcloud fn namespace create <em>NAMESPACE</em> [--description DESCRIPTION] 
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名前空間の名前。名前にハイフン (-) を含めないでください。この値は必須です。</dd>

   <dt>--description DESCRIPTION、-n DESCRIPTION</dt>
   <dd>名前空間を識別しやすくするために、独自の説明を入力します。説明に複数の単語が含まれる場合は、説明を引用符 (") で囲みます。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />
### ibmcloud fn namespace delete
{: #cli_namespace_delete}

IAM 名前空間を削除します。

```
ibmcloud fn namespace delete <em>NAMESPACE</em>
```
{: pre}


<br /><strong>例</strong>:

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />
### ibmcloud fn namespace get
{: #cli_namespace_get}

Cloud Foundry 名前空間または IAM 名前空間のエンティティーまたはメタデータ情報を取得します。

```
ibmcloud fn namespace list <em>NAMESPACE</em> [--auth <em>KEY</em>] [--name-sort] [--properties] 
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名前空間の名前または ID。この値は必須です。</dd>

   <dt>--auth <em>KEY</em>、-u <em>KEY</em></dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--name-sort、-n</dt>
   <dd>返された名前空間のリストを名前でソートします。指定しない場合、リストは作成日でソートされます。このフラグはオプションです。 </dd>

   <dt>--properties</dt>
   <dd>名前空間に含まれているエンティティーではなく、名前空間のプロパティーを表示します。このフラグはオプションです。 </dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  出力:
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />
### ibmcloud fn namespace list
{: #cli_namespace_list}

存在する Cloud Foundry 名前空間と IAM 名前空間をリストします。

```
ibmcloud fn namespace list [--auth <em>KEY</em>] [--cf] [--iam] [--limit NUMBER_OF_<em>NAMESPACE</em>S] [--name-sort] [--skip NUMBER_OF_<em>NAMESPACE</em>S] 
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>--auth <em>KEY</em>、-u <em>KEY</em></dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--cf</dt>
   <dd>Cloud Foundry 名前空間のみ表示します。IAM 名前空間は表示されません。このフラグはオプションです。</dd>

   <dt>--iam</dt>
   <dd>IAM 名前空間のみ表示します。Cloud Foundry 名前空間は表示されません。このフラグはオプションです。</dd>

   <dt>--limit NUMBER_OF_<em>NAMESPACE</em>S、-l NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>指定した数の名前空間をリストします。デフォルトは 30 個の名前空間です。このフラグはオプションです。 </dd>

   <dt>--name-sort、-n</dt>
   <dd>返された名前空間のリストを名前でソートします。指定しない場合、リストは作成日でソートされます。このフラグはオプションです。 </dd>

   <dt>--skip NUMBER_OF_<em>NAMESPACE</em>S、-s NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>最近作成された名前空間を、指定した数だけ結果から除外します。このフラグはオプションです。 </dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />
### ibmcloud fn namespace update
{: #cli_namespace_update}

IAM 名前空間の名前または説明を変更します。

```
ibmcloud fn namespace update <em>NAMESPACE</em> [NEW_<em>NAMESPACE</em>_NAME] [--description DESCRIPTION] 
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>名前空間の名前。名前にハイフン (-) を含めないでください。この値は必須です。</dd>

   <dt>NEW_<em>NAMESPACE</em>_NAME</dt>
   <dd>名前空間の新しい名前。名前にハイフン (-) を含めないでください。この値はオプションです。</dd>

   <dt>--description DESCRIPTION、-n DESCRIPTION</dt>
   <dd>名前空間を識別しやすくするために、独自の説明を入力します。説明に複数の単語が含まれる場合は、説明を引用符 (") で囲みます。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## パッケージのコマンド
{: #cli_pkg}


### ibmcloud fn package bind
{: #cli_pkg_bind}

パラメーターをパッケージにバインドします。特に指定しない限り、パッケージ内のすべてのアクションがそれらのパラメーターを継承します。

```
ibmcloud fn package bind <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>パッケージの名前。この値は必須です。 </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

  <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
  <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

  <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
  <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>
  </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />
### ibmcloud fn package create
{: #cli_pkg_create}

1 つ以上のアクションを入れるためのパッケージを作成します。パッケージにアクションを追加するには、アクションの作成時または更新時に、アクション名と一緒にパッケージ名を指定します。

```
ibmcloud fn package create <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>パッケージの名前。この値は必須です。 </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

  <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
  <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
  <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

  <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
  <dd>パラメーターの<em>キー</em> と C が含まれている JSON ファイル。このフラグはオプションです。</dd>

  <dt>--shared yes|no</dt>
  <dd>値なしで指定した場合、または値 yes を使用して指定した場合、パッケージは他のユーザーと共有されます。</dd>
  </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  出力:
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />
### ibmcloud fn package delete
{: #cli_pkg_delete}

不要になったパッケージを削除して、名前空間をクリーンアップできます。

```
ibmcloud fn package delete <em>PACKAGE_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  出力:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn package get
{: #cli_pkg_get}

特定のパッケージを表すメタデータを取得します。

```
ibmcloud fn package get <em>PACKAGE_NAME</em> [--summary]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>パッケージの名前。この値は必須です。</dd>

   <dt>--summary</dt>
   <dd>パッケージの詳細のサマリーを取得します。接頭部が「*」のパラメーターはバインドされています。このフラグはオプションです。</dd>
   </dl>

<br /><strong>例</strong>:

```
ibmcloud fn package get hello
```
{: pre}


<br />
### ibmcloud fn package list
{: #cli_pkg_list}

作成したすべてのパッケージまたは特定の数のパッケージをリストします。

```
ibmcloud fn package list [<em>NAMESPACE</em>] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>NAMESPACE</em></dt>
   <dd>特定の名前空間内のパッケージをリストします。この値はオプションです。 指定しない場合は、すべてのパッケージがリストされます。</dd>

   <dt>--limit NUMBER_OF_PACKAGES、-l NUMBER_OF_PACKAGES</dt>
   <dd>指定した数のパッケージをリストします。デフォルトは 30 個のパッケージです。</dd>

   <dt>--name-sort、-n</dt>
   <dd>返されたパッケージのリストを名前でソートします。指定しない場合、リストは作成日でソートされます。</dd>

   <dt>--skip NUMBER_OF_PACKAGES、-s NUMBER_OF_PACKAGES</dt>
   <dd>最近作成されたパッケージを、指定した数だけ結果から除外します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn package list
  ```
  {: pre}

  事前インストール済みパッケージのリストを表示するには、`ibmcloud fn package list /whisk.system` を実行します。
  {: tip}


<br />
### ibmcloud fn package refresh
{: #cli_pkg_refresh}

特定の名前空間内のすべてのパッケージのパッケージ・バインディングを最新表示します。

```
ibmcloud fn package refresh /<em>NAMESPACE</em>
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>/ から始まる名前空間。このフラグは必須です。選択できる名前空間のリストを取得するには、<code>ibmcloud fn namespace list</code> を実行します。</dd>
   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />
### ibmcloud fn package update
{: #cli_pkg_update}

1 つ以上のアクションを入れるためのパッケージを更新します。パッケージにアクションを追加するには、アクションの作成時または更新時に、アクション名と一緒にパッケージ名を指定します。

```
ibmcloud fn package update <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>パッケージの名前。この値は必須です。 </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

   <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
   <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--shared yes|no</dt>
   <dd>値なしで指定した場合、または値 <code>yes</code> を使用して指定した場合、パッケージは他のユーザーと共有されます。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  出力:
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## プロパティーのコマンド
{: #cli_prop}

CLI 環境のグローバル・プロパティーを設定します。または、`ibmcloud fn` CLI の一部として実行される <code>wsk</code> CLI に関するプロパティーを表示します。

### ibmcloud fn property get
{: #cli_prop_get}

<code>wsk</code> CLI のプロパティーのメタデータの詳細を表示します。

```
ibmcloud fn property get [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--all</dt>
   <dd><code>wsk</code> CLI のすべてのプロパティーを表示します。このフラグはオプションです。</dd>

   <dt>---apibuild</dt>
   <dd><code>wsk</code> API ビルド情報。このフラグはオプションです。</dd>

   <dt>--apibuildno</dt>
   <dd><code>wsk</code> API ビルド番号。このフラグはオプションです。</dd>

   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API ホスト。このフラグはオプションです。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API のバージョン。このフラグはオプションです。</dd>

   <dt>--auth <em>KEY</em>、-u <em>KEY</em></dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント証明書。このフラグはオプションです。</dd>

   <dt>--cliversion</dt>
   <dd><code>wsk</code> CLI のバージョン。このフラグはオプションです。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント・<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名前空間。このフラグは Cloud Foundry 名前空間には設定できません。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn property get --cliversion
  ```
  {: pre}


<br />
### ibmcloud fn property set
{: #cli_prop_set}

プロパティーを設定します。少なくとも 1 つのフラグが必要です。

```
ibmcloud fn property set [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API ホスト。このフラグはオプションです。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API のバージョン。このフラグはオプションです。</dd>

   <dt>--auth <em>KEY</em>、-u</dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント証明書。このフラグはオプションです。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント・<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名前空間。このフラグは Cloud Foundry 名前空間には設定できません。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  出力:
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### ibmcloud fn property unset
{: #cli_prop_unset}

<code>wsk</code> CLI のプロパティーを設定解除します。少なくとも 1 つのフラグが必要です。

```
ibmcloud fn property unset [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd><code>wsk</code> API ホスト。このフラグはオプションです。</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd><code>wsk</code> API のバージョン。このフラグはオプションです。</dd>

   <dt>--auth <em>KEY</em>、-u</dt>
   <dd><code>wsk</code> 許可<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント証明書。このフラグはオプションです。</dd>

   <dt>--key <em>STRING</em></dt>
   <dd><code>wsk</code> クライアント・<em>キー</em>。このフラグはオプションです。</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>IAM 名前空間。このフラグは Cloud Foundry 名前空間には設定できません。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn property unset --key my<em>KEY</em>
  ```
  {: pre}



<br /><br />
## ルールのコマンド
{: #cli_rule}


### ibmcloud fn rule create
{: #cli_rule_create}

トリガーをアクションと関連付けるルールを作成します。ルールを作成する前に、トリガーとアクションを作成しておく必要があります。

```
ibmcloud fn rule create <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  出力:
  ```
  ok: created myrule
  ```
  {: screen}


<br />
### ibmcloud fn rule delete
{: #cli_rule_delete}

名前空間をクリーンアップするには、不要になったルールを削除します。

```
ibmcloud fn rule delete <em>RULE_NAME</em> [--disable]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>ルールの名前。この値は必須です。</dd>

  <dt>--disable</dt>
  <dd>ルールを無効にしてから削除します。</dd>
  </dl>


<br /><strong>例</strong>:

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />
### ibmcloud fn rule disable
{: #cli_rule_disable}

トリガーが起動されてアクションが実行されないように、ルールの状況を非アクティブに変更します。

```
ibmcloud fn rule disable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule enable
{: #cli_rule_enable}

ルールの状況を非アクティブからアクティブに変更します。アクティブのときは、トリガーが起動されるとアクションが実行されます。

```
ibmcloud fn rule enable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule get
{: #cli_rule_get}

特定のルールを表すメタデータを取得します。

```
ibmcloud fn rule get <em>RULE_NAME</em> [--summary]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>ルールの名前。この値は必須です。</dd>

  <dt>--summary</dt>
  <dd>ルールの詳細のサマリーを取得します。</dd>
  </dl>

<br /><strong>例</strong>:

```
ibmcloud fn rule get myrule
```
{: pre}


<br />
### ibmcloud fn rule list
{: #cli_rule_list}

作成したすべてのルールまたは特定の数のルールをリストします。

```
ibmcloud fn rule list <em>RULE_NAME</em> [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>ルールの名前。この値はオプションです。 指定しない場合は、すべてのルールがリストされます。</dd>

   <dt>--limit NUMBER_OF_RULES、-l NUMBER_OF_RULES</dt>
   <dd>指定した数のルールをリストします。デフォルトは 30 個のルールです。</dd>

   <dt>--name-sort、-n</dt>
   <dd>返されたルールのリストを名前でソートします。指定しない場合、リストは作成日でソートされます。</dd>

   <dt>--skip NUMBER_OF_RULES、-s NUMBER_OF_RULES</dt>
   <dd>最近作成されたルールを、指定した数だけ結果から除外します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />
### ibmcloud fn rule status
{: #cli_rule_status}

ルールがアクティブであるか非アクティブであるかを調べます。状況を変更するには、`ibmcloud fn rule disable` コマンドまたは `ibmcloud fn run enable` コマンドを実行します。

```
ibmcloud fn rule status <em>RULE_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />
### ibmcloud fn rule update
{: #cli_rule_update}

トリガーとルールの関連付けを変更するには、ルールを更新します。

```
ibmcloud fn rule update <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## SDK コマンド
{: #cli_sdk}


### ibmcloud fn sdk install
{: #cli_sdk_install}

SDK をインストールします。

```
ibmcloud fn sdk install <em>COMPONENT</em> [--limit <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>COMPONENT</em></dt>
   <dd>SDK コンポーネント (`docker`、`iOS`、`bashauto` など)。この値は必須です。</dd>

   <dt>--stdout、--s</dt>
   <dd>bash コマンドの結果を stdout に出力します。このフラグはオプションです。</dd>


   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## サービスのコマンド
{: #cli_service}


### ibmcloud fn service bind
{: #cli_service_bind}

サービス資格情報をアクションまたはパッケージにバインドします。

```
ibmcloud fn service bind SERVICE PACKAGE_or_<em>ACTION_NAME</em> [--instance SERVICE_INSTANCE] [--keyname SERVICE_<em>KEY</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>サービスの名前。</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>資格情報をバインドするパッケージまたはアクションの名前。</dd>

   <dt>--instance SERVICE_INSTANCE</dt>
   <dd>サービス・インスタンス名。</dd>

   <dt>--keyname SERVICE_<em>KEY</em></dt>
   <dd>バインドするサービス・<em>キー</em> 資格情報の名前。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />
### ibmcloud fn service unbind
{: #cli_service_unbind}

アクションまたはパッケージからサービス資格情報をアンバインドします。

```
ibmcloud fn service unbind SERVICE PACKAGE_or_<em>ACTION_NAME</em>
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>サービスの名前。</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>資格情報をアンバインドするパッケージまたはアクションの名前。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## トリガーのコマンド
{: #cli_trigger}


### ibmcloud fn trigger create
{: #cli_trigger_create}

トリガーを作成します。

```
ibmcloud fn trigger create <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--feed <em>ACTION_NAME</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>トリガーの名前。この値は必須です。 </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

   <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
   <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--feed <em>ACTION_NAME</em>、-f <em>ACTION_NAME</em></dt>
   <dd>トリガーのタイプをフィードとして設定します。このフラグはオプションです。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>


   </dl>

<br /><strong>例</strong>:
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />
### ibmcloud fn trigger delete
{: #cli_trigger_delete}

トリガーを削除します。

```
ibmcloud fn trigger delete <em>TRIGGER_NAME</em>
```
{: pre}

<br /><strong>例</strong>:

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />
### ibmcloud fn trigger fire
{: #cli_trigger_fire}

トリガーが自然に起動されるのを待つのではなく、トリガーを起動してトリガーをテストします。

```
ibmcloud fn trigger fire <em>TRIGGER_NAME</em> [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>TRIGGER_NAME</em></dt>
   <dd>トリガーの名前。この値は必須です。 </dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター<em>値</em>。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />
### ibmcloud fn trigger get
{: #cli_trigger_get}

特定のトリガーを表すメタデータを取得します。

```
ibmcloud fn trigger get <em>TRIGGER_NAME</em> [--summary]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>トリガーの名前。この値は必須です。</dd>

  <dt>--summary</dt>
  <dd>トリガーの詳細のサマリーを取得します。</dd>
  </dl>

<br /><strong>例</strong>:

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### ibmcloud fn trigger list
{: #cli_trigger_list}

作成したすべてのトリガーまたは特定の数のトリガーをリストします。

```
ibmcloud fn trigger list <em>TRIGGER_NAME</em> [--limit <em>NUMBER_OF_TRIGGERS</em>] [--name-sort] [--skip <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>トリガーの名前。この値はオプションです。 指定しない場合は、すべてのトリガーがリストされます。</dd>

   <dt>--limit <em>NUMBER_OF_TRIGGERS</em>、-l <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>指定した数のトリガーをリストします。デフォルトは 30 個のトリガーです。</dd>

   <dt>--name-sort、-n</dt>
   <dd>返されたトリガーのリストを名前でソートします。指定しない場合、リストは作成日でソートされます。</dd>

   <dt>--skip <em>NUMBER_OF_TRIGGERS</em>、-s <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>最近作成されたトリガーを、指定した数だけ結果から除外します。</dd>

   </dl>

<br /><strong>例</strong>:

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### ibmcloud fn trigger update
{: #cli_trigger_update}

トリガーを更新します。

```
ibmcloud fn trigger update <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>トリガーの名前。この値は必須です。 </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>、-a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>アノテーションは<em>キー</em> と<em>値</em> の形式で指定します。複数のアノテーションを含めるには、アノテーションごとにこのオプションを指定します。このフラグはオプションです。</dd>

   <dt>--annotation-file <em>FILE</em>、-A <em>FILE</em></dt>
   <dd><em>キー</em> と値の形式のアノテーションが含まれている JSON ファイル。このフラグはオプションです。</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>、-p <em>KEY</em> <em>VALUE</em></dt>
   <dd><em>キー</em> と<em>値</em> の形式のパラメーター値。このフラグはオプションです。</dd>

   <dt>--param-file <em>FILE</em>、-P <em>FILE</em></dt>
   <dd>パラメーターの<em>キー</em> と<em>値</em> が含まれている JSON ファイル。このフラグはオプションです。</dd>
   </dl>

<br /><strong>例</strong>:
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}



