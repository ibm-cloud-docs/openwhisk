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

# 创建 PHP 操作
{: #creating-php-actions}

以下各部分将指导您创建并调用单个 PHP 操作，然后向该操作添加参数。
{: shortdesc}

## 创建并调用 PHP 操作
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

操作其实就是顶级 PHP 函数。要创建 PHP 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.php` 的文件中。

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

    * PHP 操作始终使用一个关联数组并返回一个关联数组。
    * 缺省情况下，操作的入口方法为 `main`，但使用 `ibmcloud fn` CLI 通过 `--main` 标志来创建操作时，可指定入口方法。

2. 创建名为 `helloPHP` 的操作。
    

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    操作类型使用源文件扩展名来确定。对于 `.php` 源文件，操作会使用 PHP 7.3 运行时运行。有关 PHP 运行时的更多信息，请参阅[运行时](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)。

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    示例输出：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 将 PHP 操作打包成 .zip 文件
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

可以将 PHP 操作以及其他文件或从属包打包成 .zip 文件。例如，打包具有名为 `helper.php` 的第二个文件的操作：

1. 创建包含源文件的归档。**注**：包含入口点的源文件必须命名为 `index.php`。
    

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. 创建操作。

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

