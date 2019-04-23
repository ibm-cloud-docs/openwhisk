---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, python

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


# Python-Aktionen erstellen
{: #creating-python-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Python-Aktion sowie zum Hinzufügen von Parametern an diese Aktion beschrieben.

## Python-Aktion erstellen und aufrufen
{: #openwhisk_actions_python_invoke}

Eine Aktion ist einfach eine Python-Funktion der höchsten Ebene. So erstellen Sie eine Python-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.py`. 
```
  python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
    return {"greeting": greeting}
  ```
{: codeblock}
    
Python-Aktionen lesen stets ein Wörterverzeichnis (Dictionary) und generieren ein Wörterverzeichnis. Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch explizit zum Erstellen der Aktion über die Befehlszeilenschnittstelle `wsk` mit dem Flag `--main` angegeben werden.
{: note}

2. Erstellen Sie eine Aktion namens `helloPython`. Der Typ der Aktion wird durch die Verwendung der Erweiterung der Quellendatei bestimmt. Für `.py`-Quellendateien wird die Aktion in einer Laufzeit mit Python 2 ausgeführt.

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Beispielausgabe:

    ```
    ok: created action helloPython
    ```
    {: screen}

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Beispielausgabe:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    Sie können auch eine Aktion erstellen, die mit Python 3.x ausgeführt wird.  
    * Verwenden Sie für Python 3.6 den Parameter `--kind python:3.6` (zuvor `python-jessie:3`). 
    * Verwenden Sie für Python 3.7 den Parameter `--kind python:3.7`. 
    
    Beide Laufzeiten enthalten zusätzliche Pakete für IBM Cloud-Services wie {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} und {{site.data.keyword.ibmwatson_notm}}. 
    
Weitere Informationen zu den Paketen, die in diesem Python 3-Laufzeiten enthalten sind, finden Sie in der Python-[Laufzeitreferenz](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments). 

## Python-Aktionen in ZIP-Dateien packen
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

Sie können eine Python-Aktion und abhängige Module in eine ZIP-Datei packen. Sie können beispielsweise eine Aktion mit einem Helpermodul namens `helper.py` erstellen. 

1. Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. **Hinweis**: Die Quellendatei, die den Eingangspunkt enthält, muss `__main__.py` heißen.

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Erstellen Sie die Aktion.

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## Python-Aktionen mit einer virtuellen Umgebung in ZIP-Dateien packen
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

Zum Packen von Python-Abhängigkeiten können Sie eine virtuelle Umgebung (`virtualenv`) verwenden. Die virtuelle Umgebung ermöglicht Ihnen, zusätzliche Pakete zu verknüpfen, die zum Beispiel mit [`pip` ![Symbl für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://packaging.python.org/installing/) installiert werden können.

Sie können Abhängigkeiten installieren, sie in einer virtuellen Umgebung packen und eine kompatible OpenWhisk-Aktion erstellen. 

1. Erstellen Sie eine Datei namens [requirements.txt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://pip.pypa.io/en/latest/user_guide/#requirements-files), die die `pip`-Module und Versionen enthält, die installiert werden sollen.

2. Installieren Sie die Abhängigkeiten und erstellen Sie eine virtuelle Umgebung. Das Verzeichnis für die virtuelle Umgebung muss den Namen `virtualenv` aufweisen. Um die Kompatibilität mit dem OpenWhisk-Laufzeitcontainer sicherzustellen, müssen Paketinstallationen in einer virtuellen Umgebung das Image verwenden, das der Art entspricht.

    * Für `python:3.7` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3.7`. 
    * Für `python:3.6` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3.6`. 
    * Für `python:2` verwenden Sie das Docker-Image `openwhisk/python2action`. 

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Packen Sie das Verzeichnis `virtualenv` und alle weiteren Python-Dateien. Die Quellendatei, die den Eingangspunkt enthält, muss `__main__.py` heißen.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Erstellen Sie die Aktion `helloPython`.

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

Fügen Sie nur solche Module zu der Datei `requirements.txt` hinzu, die nicht Teil der ausgewählten Laufzeitumgebung sind. Auf diese Weise kann die virtuelle Umgebung (`virtualenv`) möglichst klein gehalten werden.
{: tip}


