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

# Ruby-Aktionen erstellen
{: #creating-ruby-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Ruby-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben.  

Ruby-Aktionen werden in Ruby 2.5 ausgeführt. Zur Verwendung
dieser Laufzeit geben Sie den Parameter `--kind ruby:2.5` in der `ibmcloud fn`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.


## Ruby-Aktionen erstellen und aufrufen
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

Eine Aktion ist einfach eine Ruby-Funktion der höchsten Ebene. 

Erstellen Sie z. B. eine Datei `hello.rb`. 

1. Speichern Sie den folgenden Code in einer Datei namens `hello.rb`. 

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby-Aktionen verarbeiten immer einen Hash (wörterbuchähnliche Sammlung) und geben einen Hash zurück. 
    * Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch explizit beim Erstellen der Aktion über die Befehlszeilenschnittstelle `ibmcloud` mit dem Flag `--main` angegeben werden.

2. Erstellen Sie eine Aktion namens `helloRuby`. 

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Beispielausgabe:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Ruby-Aktionen packen 
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

Sie können eine PHP-Aktion und andere Dateien oder abhängige Pakete in eine ZIP-Datei packen. Sie können beispielsweise eine Aktion mit einer zweiten Datei namens `helper.rb` packen. 

1. Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. **Hinweis**: Die Quellendatei, die den Eingangspunkt enthält, muss `main.rb` heißen. 

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Erstellen Sie die Aktion.

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

Die Gems `mechanize` und `jwt` sind zusätzlich zu den standardmäßig bereitgestellten und den gebündelten Gems verfügbar.
Sie können beliebige Gems verwenden, solange Sie gezippte Aktionen verwenden, um alle Abhängigkeiten zu packen. 
