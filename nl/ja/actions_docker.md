---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: docker, actions, serverless

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

# Docker アクションの作成
{: #creating-docker-actions}


{{site.data.keyword.openwhisk_short}} Docker アクションでは、任意の言語でアクションを作成できます。
{: shortdesc}

コードはコンパイルされて実行可能バイナリーになり、Docker イメージに組み込まれます。 バイナリー・プログラムとシステムの対話は、`stdin` から入力を受け取り、`stdout` を通して応答することによって行われます。   『[ランタイム](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)』セクションに、Docker アクションの作成に関する詳細情報があります。

アクションを使用して呼び出せるイメージは、パブリック・レジストリーにあるイメージだけです (Docker Hub でパブリックに公開されているイメージなど)。プライベート・レジストリーはサポートされません。
{: tip}


## Docker アクションの作成と呼び出し

**開始前に、以下のことを行います。**

ユーザーは Docker Hub アカウントを持っている必要があります。 [Docker Hub ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com) で、無料の Docker ID とアカウントをセットアップできます。

**Docker アクションをセットアップするには、以下のようにします。**

1. Docker スケルトンをダウンロードしてインストールします。 このスケルトンは、Docker コンテナー・テンプレートであり、そこにカスタム・バイナリーの形でコードを注入できます。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. ブラック・ボックス・スケルトン内にカスタム・バイナリーをセットアップします。 スケルトンには C プログラムが含まれているので、それを使用できます。 `example.c` ファイルの部分は Docker イメージのビルド・プロセスの一環でコンパイルされるので、C を自分のマシンでコンパイルする必要はありません。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  出力例:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. オプション: 実行可能ファイルをビルドするように `Dockerfile` を変更することで、さらなるコードと依存関係を Docker イメージに追加します。 以下の要件と制限に注意してください。
  * そのバイナリーはコンテナー内部の `/action/exec` に置く必要があります。
  * 実行可能ファイルはコマンド・ラインから単一の引数を受け取ります。 この引数は、アクションへの引数を表す JSON オブジェクトのストリング・シリアライゼーションです。
  * プログラムは `stdout` または `stderr` にログを記録することがあります。
  * 規則により、出力の最終行は、アクションの結果を表す、stringify によって文字列化された JSON オブジェクトでなければなりません。

4. Docker イメージをビルドし、提供されているスクリプトを使用してアップロードします。
    1. Docker にログインします。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. `dockerSkeleton` ディレクトリーに移動します。
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. スクリプトを実行します。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Docker コンテナーを使用してアクションを作成します。
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    出力例:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Docker アクションを更新するため、最新のイメージを Docker Hub にアップロードします。 これにより、システムは、アクション用のコードの次回実行時に新規 Docker イメージをプルできるようになります。
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Docker イメージの前のバージョンを使用しているウォーム・コンテナーがあると、新しい呼び出しではすべて、引き続きそのイメージが使用されます。 新規の呼び出しが新規イメージを使用し始めるように、アクションを更新します。
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. オプション: `--docker openwhisk/dockerskeleton` の省略表現として、`--native` 引数が使用できます。 この引数を使用すると、標準的な Docker アクション SDK 内で実行される実行可能ファイルの作成とデプロイがさらに容易になります。
    1. 上記のステップで、`/action/exec` にあるコンテナー内にバイナリーの実行可能ファイルが作成されます。`/action/exec` ファイルをローカルのファイル・システムにコピーし、それを zip して `exec.zip` を作成します。
    2. 実行可能ファイルを初期化データとして受け取る Docker アクションを作成します。 `--docker openwhisk/dockerskeleton` 引数に代わって、`--native` 引数が使用されています。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
