---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

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

# Creación de acciones de Ballerina
{: #ballerina-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción de Ballerina y la adición de parámetros a dicha acción. Una acción es una función de Ballerina de nivel superior que acepta y devuelve un objeto JSON. 

Las acciones de Ballerina se ejecutan en Ballerina [0.990.2](https://ballerina.io/downloads). Necesitará una versión compatible del compilador disponible de forma local para generar el ejecutable. Sin el compilador de Ballerina, no puede crear ninguna acción.

## ´Creación e invocación de una acción de Ballerina

**Antes de empezar:** cree un archivo denominado `hello.bal` con el código fuente siguiente.

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

El método de entrada de la acción es `main` de forma predeterminada. Puede especificar esta variable al crear la acción con la CLI de `wsk` utilizando `--main`. 

**Nota:** el compilador de Ballerina espera que haya una función presente denominada `main` para generar el ejecutable, por lo que el archivo de origen debe incluir un marcador denominado `main`.

Para crear una acción denominada `hello`, realice los pasos siguientes.

1. Genere el archivo .balx.
  ```
  ballerina build hello.bal
  ```
{: pre}

2. Cree la acción utilizando el archivo .balx.
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. La CLI no determina aún el tipo de la acción a partir de la extensión del archivo de origen. Debe especificar el tipo explícitamente. Para los archivos de origen `.balx`, la acción se ejecuta utilizando el entorno de tiempo de ejecución Ballerina 0.990.2.
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

Salida de ejemplo.
```json
{
  "greeting": "Hello World!"
}
```
{: screen}
