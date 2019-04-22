---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, python

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


# Python アクションの作成
{: #creating-python-actions}

以下のセクションでは、単一 Python アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

## Python アクションの作成と呼び出し
{: #openwhisk_actions_python_invoke}

アクションは、単にトップレベルの Python 関数です。 Python のアクションを作成するには以下のようにします。

1. 以下のコードを `hello.py` という名前のファイルに保存します。
```
  python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
    return {"greeting": greeting}
  ```
{: codeblock}
    
Python アクションは常に辞書を取り込み、辞書を生成します。 このアクションの入り口メソッドはデフォルトで `main` になっていますが、`wsk` CLI を使用して `--main` フラグを指定すれば、入り口メソッドを指定してアクションを作成することができます。
{: note}

2. `helloPython` アクションを作成します。 アクションのタイプは、ソース・ファイルの拡張子を使用して判別されます。`.py` ソース・ファイルの場合、アクションは、Python 2 ランタイムを使用して実行されます。

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    出力例:

    ```
    ok: created action helloPython
    ```
    {: screen}

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    出力例:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    Python 3.x で実行するアクションを作成することもできます。 
    * Python 3.6 の場合は、パラメーター `--kind python:3.6` (以前の名前は `python-jessie:3`) を使用します。
    * Python 3.7 の場合は、パラメーター `--kind python:3.7` を使用します
    
    どちらのランタイムにも IBM Cloud サービス ({{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}}、{{site.data.keyword.ibmwatson_notm}} など) に対応した追加のパッケージが含まれています。
    
これらの Python 3 ランタイムに含まれているパッケージについて詳しくは、Python [ランタイムのリファレンス](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)を参照してください。

## zip ファイルへの Python アクションのパッケージ化
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

Python アクションおよび依存モジュールを zip ファイルでパッケージ化することができます。 例えば、`helper.py` というヘルパー・モジュールを組み込んでアクションを作成することもできます。

1. ソース・ファイルを含めたアーカイブを作成します。 **注**: エントリー・ポイントを含むソース・ファイルは `__main__.py` という名前でなければなりません。

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. アクションを作成します。

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## zip ファイルへの、仮想環境を使用した Python アクションのパッケージ化
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

Python の依存関係は、仮想環境 `virtualenv` を使用してパッケージ化できます。 この仮想環境を使用すれば、例えば、[`pip` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン ")](https://packaging.python.org/installing/) を使用してインストールできる追加のパッケージをリンクすることができます。

依存項目をインストールして仮想環境でパッケージ化し、互換性のある OpenWhisk アクションを作成できます。

1. インストールする `pip` モジュールおよびバージョンを含む [requirements.txt ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) ファイルを作成します。

2. 依存関係をインストールし、仮想環境を作成します。 仮想環境ディレクトリーの名前は `virtualenv` でなければなりません。 OpenWhisk ランタイム・コンテナーとの互換性を確保するには、仮想環境内でパッケージをインストールする際、その kind に対応するイメージを使用する必要があります。

    * kind `python:3.7` の場合、Docker イメージ `ibmfunctions/action-python-v3.7` を使用します。
    * kind `python:3.6` の場合、Docker イメージ `ibmfunctions/action-python-v3.6` を使用します。
    * kind `python:2` の場合、Docker イメージ `openwhisk/python2action` を使用します。

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. `virtualenv` ディレクトリーと追加の Python ファイルをパッケージ化します。 エントリー・ポイントを含むソース・ファイルは `__main__.py` という名前でなければなりません。

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. アクション `helloPython` を作成します。

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

`requirements.txt` には、選択したランタイム環境にないモジュールのみを追加します。 そうすると、`virtualenv` のサイズを最低限に抑えられます。
{: tip}


