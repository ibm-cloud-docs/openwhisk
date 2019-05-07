---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-07"

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

# Creating PHP actions
{: #creating-php-actions}

The following sections guide you through creating and invoking a single PHP action and adding parameters to that action.
{: shortdesc}

## Creating and invoking a PHP action
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

An action is simply a top-level PHP function. To create a PHP action:

1. Save the following code in a file called `hello.php`.

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

    * PHP actions always consume an associative array and return an associative array.
    * The entry method for the action is `main` by default but can be specified when you create the action with the `ibmcloud fn` CLI by using the `--main` flag.

2. Create an action called `helloPHP`.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    The type of action is determined by using the source file extension. For `.php` source files, the action runs by using a PHP 7.3 runtime. For more information about the PHP runtime, see [Runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php).

3. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Example output:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Packaging PHP actions in .zip files
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

You can package a PHP action and other files or dependent packages in a .zip file. For example, package an action with a second file called `helper.php`:

1. Create an archive containing your source files. **Note**: The source file that contains the entry point must be named `index.php`.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Create the action.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}


