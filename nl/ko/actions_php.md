---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: actions, serverless, php

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

# PHP 액션 작성
{: #creating-php-actions}

다음 절에서는 단일 PHP 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다.
{: shortdesc}

## PHP 액션 작성 및 호출
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

액션은 단순히 최상위 레벨의 PHP 함수입니다. PHP 액션을 작성하려면 다음을 수행하십시오.

1. `hello.php`라고 하는 파일에 다음 코드를 저장하십시오.
    

    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * PHP 액션은 항상 연관 배열을 이용하고 연관 배열을 리턴합니다.
    * 액션에 대한 진입 메소드는 기본적으로 `main`이지만, `--main` 플래그를 사용하여 `ibmcloud fn` CLI에서 액션을 작성할 때 지정될 수 있습니다. 

2. `helloPHP`라고 하는 액션을 작성하십시오.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    소스 파일 확장자를 사용하여 액션의 유형이 판별됩니다. `.php` 소스 파일의 경우, 액션은 PHP 7.3 런타임을 사용하여 실행됩니다. PHP 런타임에 대한 자세한 정보는 [런타임](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)을 참조하십시오.

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    출력 예:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## PHP 액션을 .zip 파일에 패키징
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

PHP 액션 및 기타 파일 또는 종속 패키지를 Zip 파일에 패키징할 수 있습니다. 예를 들어, `helper.php`라고 하는 두 번째 파일로 액션을 패키징하십시오. 

1. 소스 파일이 포함된 아카이브를 작성하십시오. **참고**: 시작점이 포함된 소스 파일의 이름은 `index.php`여야 합니다.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. 액션을 작성하십시오.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

