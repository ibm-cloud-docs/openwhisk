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

# Creación de acciones Ruby
{: #creating-ruby-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Ruby y la adición de parámetros a dicha acción. 

Las acciones Ruby se ejecutan en Ruby 2.5. Para utilizar este entorno de ejecución, especifique el parámetro `--kind ruby:2.5` de la CLI de `ibmcloud fn` al crear o actualizar una acción.

## Creación e invocación de acciones Ruby
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

Una acción es sencillamente un método Ruby de nivel superior.

Por ejemplo, cree un archivo denominado `hello.rb`.

1. Guarde el código siguiente en un archivo denominado `hello.rb`.

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Las acciones Ruby siempre consumen un hash (recopilación de tipo diccionario) y devuelven un hash.
    * El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar cuando crea la acción con la CLI de `ibmcloud` utilizando el distintivo `--main`.

2. Cree una acción denominada `helloRuby`.

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Salida de ejemplo:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empaquetado de acciones Ruby
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

Puede empaquetar una acción PHP y otros archivos o paquetes dependientes en un archivo .zip. Por ejemplo, puede empaquetar una acción con un segundo archivo denominado `helper.rb`.

1. Cree un archivador que contenga los archivos fuente. **Nota**: el archivo fuente que contiene el punto de entrada se debe denominar `main.rb`.

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Cree la acción.

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

Las gemas `mechanize` y `jwt` están disponibles además de las gemas predeterminadas y empaquetadas.
Puede utilizar gemas arbitrarias siempre que utilice acciones comprimidas para empaquetar todas las dependencias.
