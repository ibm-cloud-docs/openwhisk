---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Entornos de ejecución

## Entornos de ejecución de acciones
Las acciones se pueden codificar y ejecutar en varios lenguajes de programación (por ejemplo, Javascript, Python, etc.). Los entornos de ejecución disponibles se muestran en las secciones siguientes.

Los enlaces siguientes devuelven una respuesta JSON que muestra los entornos de ejecución disponibles para IBM Cloud Functions en cada región.

La sección `runtimes` de la respuesta contiene el conjunto de entornos de ejecución disponibles.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

La sección `image` contiene el nombre de la imagen de entornos de ejecución en [DockerHub](https://hub.docker.com/) y la etiqueta que se utiliza. 

Los ejemplos siguientes hacen referencia a las imágenes `ibmfunctions/action-nodejs-v10` y `openwhisk/nodejs6action`.
Las etiquetas pueden ser números de versión como `1.9.0` o la forma abreviada de un código hash de confirmación de git, como
`b99d71e`.

Campos de imagen de ejemplo.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- Los entornos de ejecución de acciones se actualizan de forma regular. Estas actualizaciones incluyen arreglos de seguridad y actualizaciones de versión menores a los paquetes dentro de los entornos de ejecución. Es posible que las actualizaciones de versión menores hagan que se pierda la compatibilidad con versiones anteriores. Las actualizaciones del entorno de ejecución pueden afectar a las acciones. No hay migración automática a un entorno de ejecución más reciente del mismo tipo.
- Las acciones que estén en entornos de ejecución en desuso no se podrán completar correctamente hasta que se actualice el entorno de ejecución a una versión con soporte. Al resolver problemas con una acción anómala, para identificar si un entorno de ejecución está en desuso, compruebe `deprecated=true` en la respuesta de consulta. Para actualizar el entorno de ejecución,
consulte [Cambio del entorno de ejecución de acciones](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)

## Entornos de ejecución JavaScript
{: #openwhisk_ref_javascript_environments}

Las acciones JavaScript se pueden ejecutar en Node.js versión 8 o 10. 

Node.js versión 6 es la versión predeterminada, pero quedó en desuso desde el 6 de diciembre de 2018. Para seguir utilizando una acción JavaScript, actualice a Node.js versión 8 o 10.
{: deprecated}

### Entorno Node.js versión 10 con SDK de IBM
{: #openwhisk_ref_javascript_environments_10}
Se utiliza el entorno Node.js versión 10 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:10` al crear o actualizar una acción.

#### Migración de `nodejs:8` a `nodejs:10`
- El paquete npm `ibm_db` no está disponible en `nodejs:10`. El paquete `ibm_db` no admite Node.js 10. Puede realizar el seguimiento del progreso de este problema consultando
[issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- El paquete npm `cloudant` no está disponible en `nodejs:10`; el paquete ha quedado en desuso, por lo que necesita utilizar el paquete npm oficial [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 al importar el módulo nodejs (por ejemplo, `require('@cloudant/cloudant')`); además,
[v3.x solo devuelve Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- El paquete npm `cradle` no está disponible en `nodejs:10`.
- El paquete npm `log4js` no está disponible en `nodejs:10`. Puede realizar el seguimiento del problema en
[log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- El paquete npm `watson-developer-cloud` no está disponible en `nodejs:10`. Puede realizar el seguimiento del progreso de la nueva versión en este problema: [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Encontrará información detallada sobre el entorno de ejecución nodejs versión 10 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

### Entorno Node.js versión 8 con SDK de IBM
{: #openwhisk_ref_javascript_environments_8}
Se utiliza el entorno Node.js versión 8 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:8` al crear o actualizar una acción.

Node.js versión 8 está en modalidad de mantenimiento y estará disponible hasta diciembre de 2019. Consulte la
[Planificación de releases de Node.js](https://github.com/nodejs/Release).
{: deprecated}
 
Encontrará información detallada sobre el entorno de ejecución Node.js versión 8 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).

### Entorno Node.js versión 6 (en desuso)
{: #openwhisk_ref_javascript_environments_6}
Node.js versión 6 es la versión predeterminada, pero ha quedado en desuso. Para seguir utilizando una acción JavaScript, actualice a Node.js versión 8 o 10.
{: deprecated}

Encontrará información detallada sobre el entorno de ejecución nodejs versión 6 en [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md).

## Entornos de tiempo de ejecución Python
{: #openwhisk_ref_python_environments}

OpenWhisk da soporte a la ejecución de acciones de Python utilizando dos versiones diferentes de tiempo de ejecución.

### Acciones Python 3.7 (basadas en Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Las acciones Python 3.7 se ejecutan con Python 3.7.x. Para utilizar este entorno de ejecución, especifique el parámetro de la CLI de `wsk` `--kind python:3.7` cuando cree o actualice una acción.

El entorno de ejecución contiene paquetes SDK para los servicios de IBM Cloud disponibles que las acciones de Python pueden utilizar, además de las bibliotecas estándares de Python 3.7.

Encontrará información detallada sobre el entorno de ejecución Python 3.7 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Acciones Python 3.6 (basadas en Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Las acciones Python 3 se ejecutan con Python 3.6.x. Para utilizar este entorno de ejecución, especifique el parámetro de la CLI de `wsk` `--kind python:3.6` cuando cree o actualice una acción.

El tiempo de ejecución contiene paquetes SDK para los servicios de IBM Cloud disponibles que las acciones de Python pueden utilizar, además de las bibliotecas estándares de Python 3.6.

Encontrará información detallada sobre el entorno de ejecución Python 3.6 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Acciones de Python 2

A menos que se especifique el distintivo `--kind` al crear o actualizar una acción, las acciones Python 2 se ejecutarán con Python 2.7.15. Para seleccionar este tiempo de ejecución de forma explícita, utilice `--kind python:2`.

Al crear acciones de python mediante virtualenv, utilice la imagen de docker `openwhisk/python2action`.
Las acciones de Python 2 pueden utilizar los paquetes siguientes, además de las bibliotecas estándares de Python 2.7.

Encontrará información detallada sobre el entorno de ejecución Python 2 en [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).

## Acciones Swift
{: #swift-actions}

Los entornos de ejecución Swift 3.1.1 y 4.1 están en desuso y están disponibles hasta el 28 de febrero de 2019.
Inicie nuevas acciones o migre las acciones existentes al entorno de ejecución Swift 4.2 utilizando el tipo
`swift:4.2` y el nuevo proceso de compilación.
{: tip}

### Swift 3
Las acciones Swift 3 se ejecutan con Swift 3.1.1 `--kind swift:3.1.1`. Especifique siempre el tipo `swift:3.1.1`, ya que las versiones previas de Swift no están admitidas.

Debe migrar todas las acciones de Swift para utilizar el tipo `swift:3.1.1`. Como práctica recomendada, proporcione siempre el tipo específico al crear o actualizar acciones.
{: tip}

Las acciones Swift 3.1.1 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- KituraNet versión 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON versión 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK versión 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Las acciones Swift 4 se pueden ejecutar utilizando Swift 4.1 o 4.2 usando `--kind swift:4.1` o `--kind swift:4.2` respectivamente.
El valor `--kind swift:default` predeterminado es Swift 4.2.

Los entornos de ejecución de acciones Swift 4.x no incluyen ningún paquete; siga las instrucciones correspondientes a [acciones de swift empaquetadas](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable) para incluir dependencias mediante Package.swift.

Las acciones Swift 4.1 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- Watson Developer Cloud SDK versión 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Las acciones Swift 4.2 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- Watson Developer Cloud SDK versión 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Migración de Swift 3.1.1 a Swift 4.1

#### SwiftyJSON con un solo archivo de acción de origen
Si tiene una acción `swift:3.1.1` que no está compilada, y utiliza el paquete **SwiftyJSON**, debe precompilar la acción y especificar la versión de SwiftyJSON que desea utilizar para la acción de tipo `swift:4.2`. Tenga en cuenta que, a partir de Swift 4.1, hay mejoras en la gestión de los datos JSON.

## Acciones PHP
{: #openwhisk_ref_php}

PHP 7.1 y 7.2 están en desuso desde el 11 de enero de 2019. Para seguir utilizando una acción PHP, actualice a PHP 7.3.
{: deprecated}

Las acciones PHP se ejecutan con PHP 7.3.0. Para utilizar este entorno de ejecución, especifique el parámetro de la CLI de `wsk` `--kind php:7.3` cuando cree o actualice una acción. Este comportamiento es el predeterminado al crear una acción con un archivo que tiene una extensión `.php`.

Los entornos de ejecución PHP 7.1 y 7.2 están en desuso. Migre todas las acciones a PHP 7.3 para obtener una mejor latencia y una ejecución más rápida de extremo a extremo.

Las siguientes extensiones PHP están disponibles, además de las estándares:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Acciones de Docker
{: #openwhisk_ref_docker}

Las acciones Docker ejecutan un binario proporcionado por el usuario en un contenedor Docker. El binario se ejecuta en una imagen Docker basada en [python:3.6-alpine](https://hub.docker.com/r/library/python), por lo que el binario debe ser compatible con dicha distribución.

El esqueleto de Docker es una forma cómoda de crear imágenes Docker compatibles con OpenWhisk. Puede instalar el esqueleto con el mandato de plugin de CLI `ibmcloud fn sdk install docker`.

El programa binario principal debe estar en `/action/exec` dentro del contenedor. El ejecutable recibe los argumentos de entrada desde una serie de argumento de línea de mandatos, que se puede deserializar como un objeto `JSON`. Debe devolver un resultado utilizando `stdout` como una serie de una línea de `JSON` serializado.

Puede incluir los pasos de compilación o dependencias modificando el `archivo de Docker` incluido en `dockerSkeleton`.
