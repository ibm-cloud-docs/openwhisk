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

# Docker 액션 작성
{: #creating-docker-actions}


{{site.data.keyword.openwhisk_short}} Docker 액션을 사용하면 사용자의 액션을 어떤 언어로든 작성할 수 있습니다.
{: shortdesc}

사용자 코드는 실행 가능 2진으로 컴파일되며 Docker 이미지에 임베드됩니다. 2진 프로그램은 `stdin`에서 입력을 가져오고, `stdout`을 통해 응답하여 시스템과 상호작용합니다.   [런타임](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker) 절에서 Docker 액션 작성에 대한 자세한 정보를 찾을 수 있습니다.

Docker Hub에서 공개적으로 사용할 수 있는 이미지와 같이 공용 레지스트리에서만 이미지를 호출하도록 액션을 사용할 수 있습니다. 개인용 레지스트리는 지원되지 않습니다.
{: tip}


## Docker 액션 작성 및 호출

**시작하기 전에 다음을 수행하십시오. **

Docker 허브 계정이 있어야 합니다. [Docker 허브 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com)에서 무료 Docker ID 및 계정을 설정할 수 있습니다.  

**Docker 액션을 설정하려면 다음을 수행하십시오.**

1. Docker 스켈레톤을 다운로드하고 설치하십시오. 스켈레톤은 사용자 정의 2진 양식으로 코드가 삽입될 수 있는 Docker 컨테이너 템플리트입니다.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 블랙박스 스켈레톤에서 사용자 정의 2진을 설정하십시오. 스켈레톤에는 사용 가능한 C 프로그램이 포함되어 있습니다. `example.c` 파일의 일부가 Docker 이미지 빌드 프로세스의 일부로서 컴파일되므로, 시스템에서 컴파일된 C 프로그램은 필요하지 않습니다.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  출력 예:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 선택사항: 실행 파일 빌드를 위해 `Dockerfile`을 수정하여 Docker 이미지에 추가로 코드 및 종속 항목을 추가하십시오. 다음 요구사항과 제한사항을 참고하십시오.
  * 2진은 `/action/exec`의 컨테이너 내부에 위치해야 합니다.
  * 실행 파일은 명령행에서 단일 인수를 수신합니다. 이 인수는 액션에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다.
  * 프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다.
  * 관례상 출력의 마지막 행은 액션의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다.

4. Docker 이미지를 빌드하고 제공된 스크립트를 사용하여 이를 업로드하십시오.
    1. Docker에 로그인하십시오.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. `dockerSkeleton` 디렉토리로 이동하십시오.
        ```
  cd dockerSkeleton
        ```
        {: pre}

    3. 스크립트를 실행하십시오.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Docker 컨테이너를 사용하여 액션을 작성하십시오.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    출력 예:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Docker 액션을 업데이트하려면 최신 이미지를 Docker 허브로 업로드하십시오. 그러면 다음 번에 액션에 대한 코드를 실행할 때 시스템이 새 Docker 이미지를 가져올 수 있습니다.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. 이전 버전의 Docker 이미지를 사용하는 웜 컨테이너가 있는 경우, 새 호출은 해당 이미지를 계속 사용합니다. 새 호출이 새 이미지 사용을 시작하도록 액션을 업데이트하십시오.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. 선택사항: `--native` 인수를 `--docker openwhisk/dockerskeleton`에 대한 속기로서 사용할 수 있습니다. 이 인수를 사용하면 표준 Docker 액션 SDK 내에서 실행되는 실행 파일을 보다 손쉽게 작성하고 배치할 수 있습니다. 
    1. 위의 단계는 `/action/exec`에 있는 컨테이너 내의 2진 실행 파일을 작성합니다. `/action/exec` 파일을 로컬 파일 시스템으로 복사하고 이를 `exec.zip`으로 압축하십시오. 
    2. 초기화 데이터로서 실행 파일을 수신하는 Docker 액션을 작성하십시오. `--native` 인수는 `--docker openwhisk/dockerskeleton` 인수를 대체합니다.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
