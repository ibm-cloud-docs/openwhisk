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

# 建立 PHP 動作
{: #creating-php-actions}

下列各節會引導您建立及呼叫單一 PHP 動作，以及將參數新增至該動作。
{: shortdesc}

## 建立及呼叫 PHP 動作
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

動作只是最上層 PHP 函數。若要建立 PHP 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.php` 的檔案中。

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

    * PHP 動作一律使用聯合陣列，並傳回聯合陣列。
    * 動作的進入方法依預設是 `main`，但可以在您使用 `--main` 旗標，以 `ibmcloud fn` CLI 建立動作時指定。

2. 建立稱為 `helloPHP` 的動作。
    

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    動作的類型是使用來源副檔名來決定。對於 `.php` 原始檔，動作會使用 PHP 7.3 運行環境來執行。如需 PHP 運行環境的相關資訊，請參閱[運行環境](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)。

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    輸出範例：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 將 PHP 動作包裝在 .zip 檔案中
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

您可以將 PHP 動作及其他檔案或相依套件包裝在 .zip 檔案中。例如，使用第二個稱為 `helper.php` 的檔案來包裝動作：

1. 建立包含原始檔的保存檔。**附註**：包含進入點的原始檔必須名為 `index.php`。
    

    ```bash
zip -r helloPHP.zip index.php helper.php
```
    {: pre}

2. 建立動作。
    

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

