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

# Criando ações PHP
{: #creating-php-actions}

As seções a seguir orientam você na criação e na chamada de uma ação de PHP única e na inclusão
de parâmetros nessa ação.
{: shortdesc}

## Criando e chamando uma ação PHP
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

Uma ação é simplesmente uma função PHP de nível superior. Para criar uma ação de PHP:

1. Salve o código a seguir em um arquivo chamado `hello.php`.

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

    * As ações PHP consomem uma matriz associativa e retornam uma matriz associativa.
    * O método de entrada para a ação é `main`por padrão, mas pode ser especificado quando você cria a ação com a CLI `ibmcloud fn`usando o sinalizador `-- main`.

2. Crie uma ação chamada  ` helloPHP `.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    O tipo de ação é determinado pelo uso da extensão do arquivo de origem. Para arquivos de origem `.php`, a ação é executada usando um tempo de execução PHP 7.3. Para obter mais informações sobre o tempo de execução do PHP, consulte [Tempos de Execução](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php).

3. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Exemplo de Saída:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empacotando ações PHP em arquivos .zip
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

É possível empacotar uma ação PHP e outros arquivos ou pacotes dependentes em um arquivo .zip. Por exemplo, empacote uma ação com um segundo arquivo chamado `helper.php`:

1. Crie um archive contendo os seus arquivos de origem. **Nota:** o arquivo de
origem que contém o ponto de entrada deve ser denominado `index.php`.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Crie a ação.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

