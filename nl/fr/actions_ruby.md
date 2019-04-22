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

# Création d'actions Ruby
{: #creating-ruby-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Ruby unique, et comment ajouter des paramètres à cette action.  

Les actions Ruby sont exécutées dans Ruby 2.5. Pour utiliser cet environnement d'exécution, spécifiez le paramètre d'interface de ligne de commande `ibmcloud fn` `--kind ruby:2.5` lors de
la création ou de la mise à jour d'une action. 

## Création et appel d'actions Ruby
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

Une action est simplement une méthode Ruby de niveau supérieur. 

Par exemple, créez un fichier appelé `hello.rb`. 

1. Sauvegardez le code suivant dans un fichier nommé `hello.rb`. 

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Les actions Ruby utilisent et renvoient toujours un hachage (collection semblable à un dictionnaire). 
    * Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée lorsque vous créez l'action avec l'interface de ligne de commande `ibmcloud fn` en utilisant l'indicateur `--main`.

2. Créez une action nommée `helloRuby`.

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Exemple de sortie :

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Conditionnement d'actions Ruby
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

Vous pouvez conditionner une action PHP, ainsi que pour d'autres fichiers et packages dépendants, dans un fichier .zip. Par exemple, vous pouvez conditionner une action avec un second fichier appelé `helper.rb`.

1. Créez une archive contenant vos fichiers source. **Remarque **: Le fichier source qui contient le point d'entrée doit être nommé `main.rb`.

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Créez l'action.

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

Les modules GEM `mechanize` et `jwt` sont disponibles en plus des modules GEM regroupés et par défaut.
Vous pouvez utiliser des modules GEM arbitraires tant que vous utilisez des actions compressées pour conditionner toutes les dépendances. 
