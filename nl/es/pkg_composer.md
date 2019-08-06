---

copyright:
years: 2017, 2019
lastupdated: "2019-07-12"

keywords: composer, openwhisk, compositions, sequence, branch, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Composer
{: #pkg_composer}

{{site.data.keyword.openwhisk}} ahora admite Composer for Apache OpenWhisk como vista previa técnica. Composer amplía las secuencias de Apache OpenWhisk con más combinadores ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external}, [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}), que puede utilizar para crear flujos más complejos que incluyen ramificación condicional, gestión de errores y bucles. La documentación completa y las especificaciones técnicas de Composer están disponibles en el [Repositorio Git de Composer](https://github.com/apache/incubator-openwhisk-composer){: external}.

También puede utilizar el proyecto código abierto [Kui](https://github.com/ibm/kui){: external} para que sea más fácil crear, desplegar y visualizar el código fuente de Composer. Para obtener más información, consulte [Kui on GitHub](https://github.com/ibm/kui){: external}.
{: note}

## Instalación de la biblioteca Composer para JavaScript o Python 3
{: #install_composer}

Puede utilizar Composer con acciones que estén escritas en cualquier lenguaje, pero debe expresar la composición en JavaScript o en Python 3. Tras la instalación, puede utilizar los mandatos `compose/pycompose` y `deploy/pydeploy` de Composer para
[configurar y ejecutar una composición](#run).
{: shortdesc}

**Para JavaScript**
1. Instale el [paquete Node.js](https://github.com/apache/incubator-openwhisk-composer){: external} de la biblioteca Composer utilizando el gestor de paquetes de Node.

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Confirme que ha instalado la biblioteca ejecutando la ayuda de los mandatos de Composer.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    **Resultado de ejemplo**
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Para Python 3**
Instale la biblioteca [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python){: external} utilizando `pip3`.

1.  Clone el repositorio de GitHub Composer for Python 3.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}

2.  Vaya al directorio del sistema.
    ```
    cd composer-python
    ```
    {: pre}

3.  Instale la biblioteca Composer. Incluya el punto (`.`) para que el mandato busque en el directorio en el que se encuentra.
    ```
    pip3 install -e .
    ```
    {: pre}

4.  Confirme que ha instalado la biblioteca ejecutando la ayuda de los mandatos de Composer.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    **Resultado de ejemplo**
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## Configuración y ejecución de composiciones en IBM Cloud Functions
{: #run}

Puede utilizar las bibliotecas Composer de JavaScript o Python 3 para crear sus composiciones en {{site.data.keyword.openwhisk}}. Utilice
`compose` o `pycompose` para compilar el código fuente de la composición y, a continuación, utilice
`deploy` o `pydeploy` para desplegar la composición en {{site.data.keyword.openwhisk}}. Tras configurar la composición, puede ejecutarla en {{site.data.keyword.openwhisk}}.
{: shortdesc}

**Antes de empezar**
De forma predeterminada, los despliegues utilizan las variables establecidas en `~/.wskprops`. Sustituya el valor predeterminado estableciendo dos parámetros como entrada del mandato `deploy` o `pydeploy` de Composer.

1.  Establezca el host de API en el punto final de {{site.data.keyword.openwhisk}}.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Añada la clave de autenticación de CLI de `wsk`. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**Para ejecutar una composición**

1.  Cree el código fuente de Composer con las bibliotecas nodeJS o Python 3. Por ejemplo, cree un archivo `demo.js`.
2.  Compile el código fuente de Composer en un archivo JSON.
    *   **JavaScript**
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   **Python 3**
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Despliegue el código en {{site.data.keyword.openwhisk}}.
    *   En JavaScript, incluya el distintivo `-w` para sobrescribir cualquier despliegue existente denominado
`demo`.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   En Python 3, incluya el distintivo `-w` para sobrescribir cualquier despliegue existente denominado
`demo`.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Ejecute la composición de la misma manera que [invoca otras acciones](/docs/openwhisk?topic=cloud-functions-triggers) en {{site.data.keyword.openwhisk}}.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} ejecuta el código que ha desplegado como una acción especial. Para obtener más información, consulte la documentación sobre [acciones del conductor](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md){: external}.

## Ampliación de secuencias con Composer
{: #extending}

Con Apache OpenWhisk, puede encadenar funciones conjuntamente en una `secuencia`, donde la salida de una acción pasa a ser la entrada de otra acción.

### Secuencias sin Composer
{: #sequences-without-composer}
Puede encadenar juntas dos funciones denominadas `action1` y `action2` en {{site.data.keyword.openwhisk_short}}:

```
ibmcloud fn action create --sequence mysequence action1 action2
```
{: pre}

El resultado de este mandato es una función denominada `mysequence`, que es un compuesto de
`action1` y `action2`.  Puede utilizar `mysequence` de la misma manera que cualquier función de OpenWhisk.

### Secuencias con Composer
{: #sequences-with-composer}
En Composer, puede especificar secuencias utilizando el código fuente en lugar de la línea de mandatos.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}

</br>
<img src="images/composer-sequence.png" width="35%" title="Secuencia simple" alt="Secuencia con dos acciones" style="width:250px; border-style: none"/></br>
_Figura 1. Secuencia con dos acciones_

No está limitado a encadenar funciones conjuntamente en Composer. Composer incluye una familia de combinadores basados en
[JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} o
[Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md){: external} que mejoran la expresividad de las secuencias. Puede ver ejemplos comunes en las secciones siguientes.

### Manejo de errores
{: #error-handling}
Puede añadir el manejo de errores a una secuencia utilizando bloques `try-catch-finally`. En este ejemplo, englobe la secuencia en un try. El código `handleError` se ejecuta si cualquier acción devuelve un error.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}

</br>
<img src="images/composer-error.png" width="400" title="Secuencia con try" alt="Secuencia con manejo de errores" style="width:400px; border-style: none"/></br>
_Figura 2. Secuencia con manejo de errores_

### Bifurcación condicional
{: #conditional-branch}
Puede crear una secuencia ramificada utilizando `if-then-else`. Este ejemplo demuestra un bloque `if-then-else`. `action1` devuelve un booleano. Si `true`, entonces se ejecuta `action2`, de lo contrario, se ejecuta `action3`. Tenga en cuenta que `action3` es opcional y se puede omitir en `if-then`.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="Secuencia con if" alt="Secuencia con bifurcación condicional" style="width:250px; border-style: none"/></br>
_Figura 3. Secuencia con bifurcación condicional_

### Bucles
{: #loop}
Puede crear construcciones de bucles en Composer. En este ejemplo, `action2` se ejecuta siempre que
`action1` devuelva `true`. Composer limita el número total de pasos que puede ejecutar en una secuencia compuesta. El límite actual es 20.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="Secuencia con while" alt="Secuencia con bucle while" style="width:250px; border-style: none"/></br>
_Figura 4. Secuencia con bucle `while`_

### Definición de acciones en línea
{: #inline-def}
Puede definir acciones dentro del propio código de composición. En este ejemplo, puede crear la definición de acción en línea con la composición denominada `hello` utilizando `composer.action()`.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}

</br>
<img src="images/composer-inline.png" width="250" title="Secuencia con while" alt="Secuencia con definición de acción en línea" style="width:250px; border-style: none"/></br>
_Figura 5. Secuencia con definición de acción en línea_

## Utilización de otras definiciones de combinador
{: #combinator-def}
Consulte la documentación de Composer en Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} o
[Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}) para ver la lista completa de definiciones de combinadores.






