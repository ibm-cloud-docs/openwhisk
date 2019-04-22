---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: actions, serverless, ruby

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

# Criando ações do Ruby
{: #creating-ruby-actions}

As seções a seguir orientam você na criação e na chamada de uma ação do Ruby única e na inclusão de parâmetros nessa ação. 

As ações do Ruby são executadas no Ruby 2.5. Para usar esse tempo de execução, especifique o parâmetro da CLI `ibmcloud fn` `--kind ruby:2.5` ao criar ou atualizar uma ação.

## Criando e chamando ações do Ruby
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

Uma ação é simplesmente um método Ruby de nível superior.

Por exemplo, crie um arquivo chamado `hello.rb`.

1. Salve o código a seguir em um arquivo chamado `hello.rb`.

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * As ações do Ruby sempre consomem um Hash (coleta como de dicionário) e retornam um Hash.
    * O método de entrada para a ação é `main`por padrão, mas pode ser especificado quando você cria a ação com a CLI `ibmcloud fn`usando o sinalizador `-- main`.

2. Crie uma ação chamada `helloRuby`.

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Exemplo de Saída:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empacotando ações do Ruby.
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

É possível empacotar uma ação PHP e outros arquivos ou pacotes dependentes em um arquivo .zip. Por exemplo, é possível empacotar uma ação com um segundo arquivo chamado `helper.rb`.

1. Crie um archive contendo os seus arquivos de origem. **Nota**: o arquivo de origem que contém o ponto de entrada deve ser denominado `main.rb`.

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Crie a ação.

    ```bash
    A ação ibmcloud fn cria helloRuby -- tipo ruby: 2.5 helloRuby.zip
    ```
    {: pre}

Os gems `mechanize` e `jwt` estão disponíveis além dos gems padrão e empacotados. É possível usar gemas arbitrárias, desde que você use ações compactadas para empacotar todas as dependências.
