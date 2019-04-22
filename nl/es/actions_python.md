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


# Creación de acciones Python
{: #creating-python-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Python y la adición de parámetros a dicha acción.

## Creación e invocación de una acción Python
{: #openwhisk_actions_python_invoke}

Una acción es sencillamente una función Python de nivel superior. Para crear una acción de Python:

1. Guarde el código siguiente en un archivo denominado `hello.py`.
```
  python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
        return {"greeting": greeting}
  ```
{: codeblock}
    
Las acciones Python siempre consumen un
diccionario y generan un diccionario. El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar para crear la acción con la CLI de `wsk` utilizando el distintivo `--main`.
{: note}

2. Creación de una acción `helloPython`. El tipo de acción se determina utilizando la extensión del archivo de origen. Para archivos de origen `.py`, la acción se ejecuta utilizando el tiempo de ejecución Python 2.

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Salida de ejemplo:

    ```
    ok: created action helloPython
    ```
    {: screen}

3. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Salida de ejemplo:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    También puede crear una acción que se ejecute con Python 3.x. 
    * Para Python 3.6, utilice el parámetro `--kind python:3.6` (anteriormente denominado `python-jessie:3`).
    * Para Python 3.7, utilice el parámetro `--kind python:3.7`
    
    Ambos entornos de ejecución contienen paquetes adicionales para servicios de IBM Cloud como
{{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} y {{site.data.keyword.ibmwatson_notm}}.
    
Para obtener más información sobre los paquetes que se incluyen en estos entornos de ejecución de Python 3, consulte la
[referencia del entorno de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) de Python.

## Empaquetamiento de acciones de Python en archivos zip
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

Puede empaquetar una acción Python y los módulos dependientes en un archivo zip. Por ejemplo, puede crear una acción con un módulo de ayudante denominado `helper.py`.

1. Cree un archivador que contenga los archivos fuente. **Nota**: El archivo fuente que contiene el punto de entrada se debe denominar `__main__.py`.

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Cree la acción.

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## Empaquetamiento de acciones de Python con un entorno virtual en archivos zip
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

Puede empaquetar dependencias de Python utilizando un entorno virtual, `virtualenv`. El entorno virtual permite enlazar paquetes adicionales que se pueden instalar utilizando [`pip` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://packaging.python.org/installing/), por ejemplo.

Puede instalar dependencias, empaquetarlas en un entorno virtual y crear una acción de OpenWhisk compatible.

1. Cree un archivo [requirements.txt ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) que contenga las versiones y los módulos de `pip` a instalar.

2. Instale las dependencias y cree un entorno virtual. El directorio virtualenv debe denominarse `virtualenv`. Para asegurar la compatibilidad con el contenedor de tiempo de ejecución de OpenWhisk, las instalaciones de paquetes dentro de un entorno virtual deben utilizar la imagen que corresponde al tipo.

    * Para el tipo `python:3.7`, utilice la imagen de Docker `ibmfunctions/action-python-v3.7`.
    * Para el tipo `python:3.6`, utilice la imagen de Docker `ibmfunctions/action-python-v3.6`.
    * Para el tipo `python:2`, utilice la imagen de Docker `openwhisk/python2action`.

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Empaquete el directorio `virtualenv` y todos los archivos Python adicionales. El archivo fuente que contiene el punto de entrada se debe denominar `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Cree la acción `helloPython`.

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

Añada únicamente los módulos que no son parte del entorno de tiempo de ejecución seleccionado para `requirements.txt`. Esto ayuda a mantener un tamaño mínimo de `virtualenv`.
{: tip}


