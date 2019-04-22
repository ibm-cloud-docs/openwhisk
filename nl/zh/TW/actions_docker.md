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

# 建立 Docker 動作
{: #creating-docker-actions}


使用 {{site.data.keyword.openwhisk_short}} Docker 動作，您可以使用任何語言來撰寫動作。
{: shortdesc}

您的程式碼會編譯成可執行的二進位檔，並內嵌在 Docker 映像檔中。二進位程式與系統互動的方式是從 `stdin` 取得輸入，並透過 `stdout` 回覆。您可以在[運行環境](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)小節中，找到建立 Docker 動作的相關資訊。

您可以使用動作，只從公用登錄中呼叫映像檔（例如，在 Docker Hub 上公然可用的映像檔）。不支援專用登錄。
{: tip}


## 建立及呼叫 Docker 動作

**開始之前：**

您必須具備 Docker Hub 帳戶。您可以在 [Docker Hub ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com) 設定免費 Docker ID 及帳戶。

**若要設定 Docker 動作，請執行下列動作：**

1. 下載並安裝 Docker 架構。架構是一個 Docker 容器範本，您可以在其中以自訂二進位檔形式來注入程式碼。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在 Blackbox 架構中，設定您的自訂二進位檔。此架構包含您可以使用的 C 程式。`example.c` 檔案的一部分會在 Docker 映像檔建置程序之中編譯，因此不需要在機器上編譯 C。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  輸出範例：
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 選用項目：修改 `Dockerfile` 以新增其他程式碼和相依關係至 Docker 映像檔，以建置您的執行檔。請注意下列需求及限制：
  * 二進位檔必須位在 `/action/exec` 的容器內。
  * 執行檔會從指令行收到單一引數。這個引數是代表動作引數之 JSON 物件的字串序列化。
  * 程式可能會記載至 `stdout` 或 `stderr`。
  * 依照慣例，輸出的最後一行必須是代表動作結果的字串化 JSON 物件。

4. 建置 Docker 映像檔，並使用提供的 Script 予以上傳。
    1. 登入 Docker。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. 前往 `dockerSkeleton` 目錄。
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. 執行 Script。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. 使用您的 Docker 容器來建立動作。
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. 呼叫動作。
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    輸出範例：
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}

7. 若要更新 Docker 動作，請將最新映像檔上傳至 Docker Hub。這可讓系統在下次執行您動作的程式碼時取回新的 Docker 映像檔。
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. 如果有暖容器使用舊版 Docker 映像檔，任何新呼叫都會繼續使用該映像檔。請更新動作，讓新的呼叫開始使用新的映像檔。
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. 選用：您可以使用 `--native` 引數，作為 `--docker openwhisk/dockerskeleton` 的速記。這個引數可讓您更輕鬆地建立及部署在標準 Docker 動作 SDK 內執行的執行檔。
    1. 上述步驟會在位於 `/action/exec` 的容器內建立二進位執行檔。請將 `/action/exec` 檔案複製到本端檔案系統，並將它壓縮成 `exec.zip`。
    2. 建立可將執行檔當作起始設定資料來接收的 Docker 動作。`--native` 引數會取代 `--docker openwhisk/dockerskeleton` 引數。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
