---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless, javascript, node, node.js

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
{:gif: data-image-type='gif'}


# Asociar desencadenantes y acciones mediante reglas
{: #rules}

Cada vez que se activa el desencadenante, la regla utiliza el suceso desencadenante como entrada e invoca a la acción asociada. Con el conjunto adecuado de reglas, es posible que un único suceso desencadenante invoque varias acciones, o que
una acción se invoque como respuesta a sucesos de distintos desencadenantes.
{: shortdesc}


## Creación de reglas desde la interfaz de usuario
{: #rules_ui}

Desde la interfaz de usuario, se crea automáticamente una regla para asociar una acción y un desencadenante.
{: shortdesc}

Cuando crea o accede a los detalles de una acción o un desencadenante, tiene la opción de conectar una acción o un desencadenante existente o nuevo. Cuando hace la conexión, se crea una regla automáticamente y el nombre sigue el formato `ACTION_NAME-TRIGGER_NAME`.

Desde la CLI, puede ejecutar `ibmcloud fn rule list` para verificar que se ha creado la regla.


## Creación de reglas desde la CLI
{: #rules_create}

Las reglas se utilizan para asociar un desencadenante con una acción. Cada vez que se activa un suceso desencadenante, se invoca a la acción con los parámetros del suceso desencadenante.

Antes de empezar, cree [una acción](/docs/openwhisk?topic=cloud-functions-actions) y [un desencadenante](/docs/openwhisk?topic=cloud-functions-triggers).


Cree una regla para asociar un desencadenante con una acción. Las reglas se deben crear directamente dentro de un espacio de nombres y no se pueden crear dentro de paquetes.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


Para inhabilitar la regla, puede ejecutar el mandato siguiente.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## Creación de reglas para secuencias de acciones
{: #rules_seq}

Puede utilizar reglas para asociar desencadenantes con secuencias de acciones.

Antes de empezar, cree [una secuencia de acciones](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) y [un desencadenante](/docs/openwhisk?topic=cloud-functions-triggers).

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## Asociación de varios desencadenantes y acciones
{: #rules_assoc}

Puede utilizar diferentes combinaciones de desencadenantes y acciones creando una regla para cada combinación. No es necesario que tenga una ratio de uno a uno para las acciones y los desencadenantes.

Por ejemplo, considere las acciones siguientes.
- `classifyImage` - Acción que detecta los objetos de una imagen y los clasifica.
- `thumbnailImage`- Acción que crea una versión de miniatura de una imagen.

Además, suponga que dos orígenes de sucesos activan los desencadenantes siguientes.
- `newTweet` - Desencadenante que se activa cuando se publica un nuevo tweet.
- `imageUpload` - Desencadenante que se activa cuando se sube una imagen a un sitio web.

Puede configurar reglas para que un único suceso desencadenante invoque varias acciones, y hacer que varios desencadenantes invoquen la misma acción.
- Regla `newTweet -> classifyImage`
- Regla `imageUpload -> classifyImage`
- Regla `imageUpload -> thumbnailImage`

Las tres reglas establecen el comportamiento siguiente.
- Se clasifican las imágenes de ambos tweets.
- Se clasifican las imágenes cargadas.
- Se genera una versión en miniatura.
