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

# Creación de acciones PHP
{: #creating-php-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción PHP y la adición de parámetros a dicha acción.
{: shortdesc}

## Creación e invocación de una acción PHP
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

Una acción es sencillamente una función PHP de nivel superior. Para crear una acción PHP:

1. Guarde el código siguiente en un archivo denominado `hello.php`.

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

    * Las acciones PHP siempre consumen una matriz asociativa y devuelven una matriz asociativa.
    * El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar cuando crea la acción con la CLI de `ibmcloud` utilizando el distintivo `--main`.

2. Cree una acción denominada `helloPHP`.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    El tipo de acción se determina utilizando la extensión del archivo de origen. Para archivos de origen `.php`, la acción se ejecuta utilizando el tiempo de ejecución PHP 7.3. Para obtener más información sobre el entorno de ejecución PHP, consulte
[Entornos de ejecución](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php).

3. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Salida de ejemplo:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empaquetamiento de acciones PHP en archivos .zip
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

Puede empaquetar una acción PHP y otros archivos o paquetes dependientes en un archivo .zip. Por ejemplo, empaquete una acción con un segundo archivo denominado `helper.php`:

1. Cree un archivador que contenga los archivos fuente. **Nota**: El archivo fuente que contiene el punto de entrada se debe denominar `index.php`.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Cree la acción.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

