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

# PHP-Aktionen erstellen
{: #creating-php-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen PHP-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben.
{: shortdesc}

## PHP-Aktion erstellen und aufrufen
{: #actions_php_invoke}
{: #openwhisk_actions_php_invoke}

Eine Aktion ist einfach eine PHP-Funktion der höchsten Ebene. So erstellen Sie eine PHP-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.php`. 

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

    * PHP-Aktionen verarbeiten stets eine assoziative Feldgruppe und geben auch eine assoziative Feldgruppe zurück.
    * Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch explizit beim Erstellen der Aktion über die Befehlszeilenschnittstelle `ibmcloud` mit dem Flag `--main` angegeben werden. 

2. Erstellen Sie eine Aktion namens `helloPHP`.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    Der Typ der Aktion wird durch die Verwendung der Erweiterung der Quellendatei bestimmt. Für `.php`-Quellendateien wird die Aktion in einer Laufzeit mit PHP 7.3 ausgeführt. Weitere Informationen zur PHP-Laufzeit finden Sie unter [Laufzeiten](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php).
  

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Beispielausgabe:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## PHP-Aktionen in ZIP-Dateien packen
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

Sie können eine PHP-Aktion und andere Dateien oder abhängige Pakete in eine ZIP-Datei packen. So erstellen Sie zum Beispiel eine Aktion mit einer zweiten Datei namens `helper.php`: 

1. Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. **Hinweis**: Die Quellendatei, die den Eingangspunkt enthält, muss `index.php` heißen.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Erstellen Sie die Aktion.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

