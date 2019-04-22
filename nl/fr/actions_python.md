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


# Création d'actions Python
{: #creating-python-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Python unique, et comment ajouter des paramètres à cette action.

## Création et appel d'une action Python
{: #openwhisk_actions_python_invoke}

Une action est simplement une fonction Python de premier niveau. Pour créer une action Python :

1. Sauvegardez le code suivant dans un fichier nommé `hello.py`. 
```
  python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
        return {"greeting": greeting}
  ```
{: codeblock}
    
Les actions Python consomment et produisent toujours un dictionnaire. Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée pour créer l'action avec l'interface de ligne de commande `wsk` à l'aide de l'indicateur `--main`.
{: note}

2. Créez une action `helloPython`. Le type d'action est déterminé à l'aide de l'extension du fichier source. Pour les fichiers source `.py`, l'action est lancée dans un environnement d'exécution Python 2.

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Exemple de sortie :

    ```
    ok: created action helloPython
    ```
    {: screen}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Exemple de sortie :

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    Vous pouvez également créer une action opérant avec Python 3.x.  
    * Pour Python 3.6, utilisez le paramètre `--kind python:3.6` (précédemment nommé `python-jessie:3`). 
    * Pour Python 3.7, utilisez le paramètre `--kind python:3.7`
    
    Les deux environnements d'exécution contiennent des packages supplémentaires pour des services IBM Cloud comme {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} et {{site.data.keyword.ibmwatson_notm}}.
    
Pour plus d'informations sur les packages qui sont inclus dans ces environnements d'exécution Python 3, consultez la [référence d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) Python. 

## Conditionnement des actions Python dans des fichiers zip
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

Vous pouvez conditionner une action Python et les modules dépendants dans un fichier zip. Par exemple, vous pouvez créer une action avec un module auxiliaire appelé `helper.py`. 

1. Créez une archive contenant vos fichiers source. **Remarque **: Le fichier source qui contient le point d'entrée doit être nommé `__main__.py`.

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Créez l'action.

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## Conditionnement d'actions Python avec un environnement virtuel dans des fichiers zip
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

Vous pouvez conditionner mes dépendances Python à l'aide d'un environnement virtuel, `virtualenv`. L'environnement virtuel vous permet de lier des packages supplémentaires qui peuvent être installés à l'aide de [`pip` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://packaging.python.org/installing/), par exemple.

Vous pouvez installer des dépendances, les conditionner dans un environnement virtuel et créer une action OpenWhisk compatible. 

1. Créez un fichier [requirements.txt ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) contenant les modules `pip` et les versions à installer.

2. Installez les dépendances et créez un environnement virtuel. Le répertoire d'environnement virtuel doit se nommer `virtualenv`. Pour assurer la compatibilité avec le conteneur d'exécution OpenWhisk, les installations de package dans un environnement virtuel doivent utiliser l'image correspondant à ce type.

    * Pour le type `python:3.7`, utilisez l'image Docker `ibmfunctions/action-python-v3.7`.
    * Pour le type `python:3.6`, utilisez l'image Docker `ibmfunctions/action-python-v3.6`.
    * Pour le type `python:2`, utilisez l'image Docker `openwhisk/python2action`.

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Conditionnez le répertoire `virtualenv` et les éventuels fichiers Python supplémentaires. Le fichier source qui contient le point d'entrée doit être nommé `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Créez l'action `helloPython`.

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

Ajoutez uniquement les modules qui ne font pas partie de l'environnement d'exécution sélectionné dans le fichier `requirements.txt`. Cela permet de conserver une taille minimale pour `virtualenv`.
{: tip}


