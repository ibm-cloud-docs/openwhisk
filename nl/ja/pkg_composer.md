---

copyright:
years: 2017, 2019
lastupdated: "2019-05-15"

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
{:gif: data-image-type='gif'}

# Composer
{: #pkg_composer}

{{site.data.keyword.openwhisk}} で、テクニカル・プレビューとして Apache OpenWhisk 用の Composer がサポートされるようになりました。 Composer は Apache OpenWhisk シーケンスを、追加のコンビネーター ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md)、[Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)) で拡張します。これにより、条件付き分岐、エラー処理、およびループを含む、より複雑なフローを作成できます。 Composer に関する詳しい資料と技術仕様は、[Composer Git リポジトリー](https://github.com/apache/incubator-openwhisk-composer)から入手できます。

また、オープン・ソース・プロジェクト [Kui](https://github.com/ibm/kui) を使用して、Composer ソース・コードを簡単に作成、デプロイ、および視覚化することもできます。 Composer で Kui を使用する方法について詳しくは、[Github の Kui](https://github.com/ibm/kui) を参照してください。
{: note}

## JavaScript または Python 3 用の Composer ライブラリーのインストール
{: #install_composer}

Composer で使用するアクションの作成はどの言語でも行えますが、コンポジションの表現は JavaScript または Python 3 のいずれかで行います。インストール後、Composer の `compose/pycompose` コマンドと `deploy/pydeploy` コマンドを使用して、[コンポジションを構成および実行します](#run)。
{: shortdesc}

**JavaScript の場合**:
1. ノード・パッケージ・マネージャーを使用して、Composer ライブラリーの [Node.js パッケージ](https://github.com/apache/incubator-openwhisk-composer)をインストールします。

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Composer コマンドの help を実行して、ライブラリーをインストールしたことを確認します。

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    出力例:
    ```
    使用法:
        compose composition.js [flags]

    使用法:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Python 3 の場合**:
`pip3` を使用して [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python) ライブラリーをインストールします。

1.  Composer for Python 3 GitHub リポジトリーを複製します。
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  composer ディレクトリーに移動します。
    ```
    cd composer-python
    ```
    {: pre}
3.  Composer ライブラリーをインストールします。 コマンドが現行ディレクトリー内を検索するよう、ピリオド (`.`) を含めます。
    ```
    pip3 install -e .
    ```
    {: pre}
4.  Composer コマンドの help を実行して、ライブラリーをインストールしたことを確認します。
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    出力例:
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## IBM Cloud Functions でのコンポジションの構成と実行
{: #run}

JavaScript または Python 3 の Composer ライブラリーを使用して {{site.data.keyword.openwhisk}} でコンポジションを作成できます。 `compose` または `pycompose` を使用してコンポジションのソース・コードをコンパイルし、その後、`deploy` または `pydeploy` を使用してコンポジションを {{site.data.keyword.openwhisk}} にデプロイします。 コンポジションを構成した後、{{site.data.keyword.openwhisk}} で実行できます。
{: shortdesc}

**始める前に**:
デフォルトで、`~/.wskprops` で設定される値をデプロイメントで使用します。 Composer の `deploy` コマンドまたは `pydeploy` コマンドの入力として 2 つのパラメーターを設定することにより、デフォルトをオーバーライドします。

1.  API ホストを {{site.data.keyword.openwhisk}} エンドポイントに設定します。
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  `wsk` CLI 認証鍵を追加します。 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**コンポジションを実行するには、以下のようにします**。

1.  NodeJS ライブラリーまたは Python 3 ライブラリーを使用して Composer のソース・コードを作成します。 例えば、`demo.js` ファイルを作成します。
2.  Composer のソース・コードを JSON ファイルにコンパイルします。
    *   JavaScript の場合:
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   Python 3 の場合:
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  コードを {{site.data.keyword.openwhisk}} にデプロイします。
    *   JavaScript の場合: `-w` フラグを使用して、`demo` という名前の既存のデプロイメントを上書きします。
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   Python 3 の場合: `-w` フラグを使用して、`demo` という名前の既存のデプロイメントを上書きします。
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  {{site.data.keyword.openwhisk}} で[他のアクションを呼び出す](/docs/openwhisk?topic=cloud-functions-triggers)場合と同じ方法でコンポジションを実行します。
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

デプロイしたコードは、特別な種類のアクションとして {{site.data.keyword.openwhisk}} で実行されます。 詳しくは、[コンダクター・アクション](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md)に関する資料を参照してください。

## Composer でのシーケンスの拡張
{: #extending}

Apache OpenWhisk を使用すると、`sequence` で複数の関数をチェーニングすることができます。この場合、あるアクションの出力が別のアクションの入力になります。

### Composer を使用しないシーケンス
{: #sequences-without-composer}
{{site.data.keyword.openwhisk_short}} で `action1` および `action2` という 2 つの関数をチェーニングすることができます。

`ibmcloud fn action create --sequence mysequence action1 action2`.

このコマンドの結果として、`mysequence` という名前の関数が生成されます。これは、`action1` と `action2` の複合物です。  OpenWhisk の他の関数と同じ方法で `mysequence` を使用できます。

### Composer を使用したシーケンス
{: #sequences-with-composer}
Composer では、コマンド・ラインではなくソース・コードを使用して、より豊富なシーケンスを指定できます。

JavaScript の場合:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Python 3 の場合:
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="単純なシーケンス" alt="2 つのアクションで構成されるシーケンス" style="width:250px; border-style: none"/></br>
_図 1. 2 つのアクションで構成されるシーケンス_

Composer の機能は、関数のチェーニングに限定されているわけではありません。 Composer には、シーケンスの表現力を向上させる [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) または [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) ベースのコンビネーターのファミリーが含まれています。 以下のセクションで一般的な例を紹介します。

### エラー処理
{: #error-handling}
`try-catch-finally` ブロックを使用して、シーケンスにエラー処理を追加できます。 この例では、シーケンスを try で囲んでいます。 いずれかのアクションでエラーが返されると、`handleError` コードが実行されます。

JavaScript の場合:
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Python 3 の場合:
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Try シーケンス" alt=" エラー処理を含むシーケンス" style="width:400px; border-style: none"/></br>
_図 2. エラー処理を含むシーケンス_

### 条件付き分岐
{: #conditional-branch}
`if-then-else` を使用して、分岐シーケンスを作成できます。 この例では、`if-then-else` について説明しています。 `action1` はブール値を返すはずです。 `true` の場合は `action2` が実行され、その他の場合は `action3` が実行されます。 `action3` はオプションであり、`if-then` では省略可能です。

JavaScript の場合:
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Python 3 の場合:
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If シーケンス" alt="条件分岐を含むシーケンス" style="width:250px; border-style: none"/></br>
_図 3. 条件分岐を含むシーケンス_

### ループ
{: #loop}
Composer では、ループ構造を作成できます。 この例では、`action1` によって `true` が返された場合のみ `action2` が実行されます。 Composer は、組み合わせられたシーケンスで実行できるステップの総数を制限します。 現在の上限は 20 個です。

JavaScript の場合:
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Python 3 の場合:
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While シーケンス" alt="while ループを含むシーケンス" style="width:250px; border-style: none"/></br>
_図 4. `while` ループを含むシーケンス_

### アクションのインライン定義
{: #inline-def}
コンポジション・コード自体の中でアクションを定義できます。 この例では、`composer.action()` を使用して、`hello` という名前のコンポジションでインライン化したアクション定義を作成します。

JavaScript の場合:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Python 3 の場合:
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="While シーケンス" alt="インライン・アクション定義を含むシーケンス" style="width:250px; border-style: none"/></br>
_図 5. インライン・アクション定義を含むシーケンス_

## その他のコンビネーター定義の使用
{: #combinator-def}
コンビネーター定義の詳細なリストについては、Apache OpenWhisk 用の Composer ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) または [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)) の資料を参照してください。

