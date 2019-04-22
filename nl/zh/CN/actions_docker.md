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

# 创建 Docker 操作
{: #creating-docker-actions}


通过 {{site.data.keyword.openwhisk_short}} Docker 操作，可以使用任何语言编写操作。
{: shortdesc}

您的代码会编译为可执行二进制文件，并嵌入到 Docker 映像中。二进制程序通过采用来自 `stdin` 的输入并通过 `stdout` 进行回复，从而与系统进行交互。您可以在[运行时](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)部分中找到有关创建 Docker 操作的更多信息。

使用操作只能从公共注册表中调用映像，例如 Docker Hub 上公开可用的映像。不支持专用注册表。
{: tip}


## 创建并调用 Docker 操作

**开始之前：**

您必须拥有 Docker Hub 帐户。可以在 [Docker Hub ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com) 上设置免费 Docker 标识和帐户。

**要设置 Docker 操作，请执行以下操作：**

1. 下载并安装 Docker 框架。框架是一种 Docker 容器模板，可以在其中以定制二进制文件的形式插入代码。 
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在黑匣框架中设置定制二进制文件。该框架包含可以使用的 C 程序。在 Docker 映像构建过程中，会对 `example.c` 文件中的部分内容进行编译，所以无需在您的计算机上进行 C 程序编译。
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
    1. 以上步骤在容器内的 `/action/exec` 中创建了二进制可执行文件。请将 `/action/exec` 文件复制到本地文件系统，并将其压缩成 `exec.zip`。
    2. 创建将可执行文件作为初始化数据接收的 Docker 操作。`--native` 自变量会替换 `--docker openwhisk/dockerskeleton` 自变量。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
