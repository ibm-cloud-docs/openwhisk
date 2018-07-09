---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Conceptos generales
{: #openwhisk_triggers}

Los desencadenantes y reglas de {{site.data.keyword.openwhisk_short}} aportan prestaciones dirigidas por sucesos a la plataforma. Los sucesos de orígenes de sucesos externos e internos se ponen en el canal por medio de un desencadenante, y las reglas permiten sus acciones de respuesta para dichos sucesos.
{: shortdesc}

## ¿Qué es un desencadenante?
{: #openwhisk_triggers_create}

Los desencadenantes son un canal con nombre para una clase de sucesos. A continuación se muestran ejemplos de desencadenantes:
- Un desencadenante de sucesos de actualización de ubicación.
- Un desencadenante de subidas de documento a un sitio web.
- Un desencadenante de correos electrónicos de entrada.

Los desencadenantes se pueden activar (*fired*) mediante un diccionario de pares de clave/valor. Este diccionario se denomina
a veces *event*. Como con las acciones, cada activación de un desencadenante tiene como resultado un **ID de activación**.

Los desencadenantes se pueden activar de forma explícita mediante un usuario o de parte de un usuario por parte de un origen de suceso externo.
Un *canal de información* es una forma cómoda de configurar un origen de sucesos externo para activar sucesos desencadenantes que {{site.data.keyword.openwhisk_short}} pueda consumir. Consulte los siguientes canales de información de ejemplo:
- Canal de información de cambio de datos de {{site.data.keyword.cloudant}} que activa un suceso desencadenante cada vez que se añade o modifica un documento en una base de datos.
- Un canal de información Git que desencadena un suceso por cada confirmación contra un repositorio Git.

## ¿Cómo afectan las reglas a los desencadenantes?
{: #openwhisk_rules_use}

Una regla asocia un desencadenante a una acción, para cada activación del desencadenante que provoca la invocación de la acción correspondiente con un suceso de desencadenante como entrada.

Con el conjunto adecuado de reglas, es posible que un único suceso desencadenante invoque varias acciones, o que
una acción se invoque como respuesta a sucesos de distintos desencadenantes.

Por ejemplo, pensemos en un sistema con las acciones siguientes:
- `classifyImage` - Acción que detecta los objetos de una imagen y los clasifica.
- `thumbnailImage`- Acción que crea una versión de miniatura de una imagen.

Además, suponga que dos orígenes de sucesos activan los desencadenantes siguientes:
- `newTweet` - Desencadenante que se activa cuando se publica un nuevo tweet.
- `imageUpload` - Desencadenante que se activa cuando se sube una imagen a un sitio web.

Puede configurar reglas para que un único suceso desencadenante invoque varias acciones, y hacer que varios desencadenantes
invoquen la misma acción:
- Regla `newTweet -> classifyImage`.
- Regla `imageUpload -> classifyImage`.
- Regla `imageUpload -> thumbnailImage`.

Las tres reglas establecen el comportamiento siguiente:
- Se clasifican las imágenes de ambos tweets.
- Se clasifican las cargadas.
- Se genera una versión en miniatura.

## Creación y activación de desencadenantes
{: #openwhisk_triggers_fire}

Los desencadenantes se pueden activar cuando se producen determinados sucesos, o bien se pueden activar manualmente.

Como ejemplo, cree un desencadenante para enviar actualizaciones de ubicación de usuario y activar manualmente el desencadenante.
1. Especifique el mandato siguiente para crear el desencadenante:
  ```
  ibmcloud wsk trigger create locationUpdate
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. Compruebe que ha creado el desencadenante mostrando una lista del conjunto de desencadenantes.
  ```
  ibmcloud wsk trigger list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  Ahora se crea un "canal" con nombre, para el que se pueden activar sucesos.

3. A continuación, active el suceso desencadenante especificando el nombre de desencadenante y los parámetros:
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

Un desencadenante que se activa sin una regla que lo acompañe con la que cotejarlo, no tendrá ningún efecto visible.
Los desencadenantes no se pueden crear dentro de un paquete; deben crearse directamente bajo un **espacio de nombres**.

## Uso de reglas para asociar desencadenantes con acciones
{: #openwhisk_rules_assoc}

Las reglas se utilizan para asociar un desencadenante con una acción. Cada vez que se activa un suceso desencadenante, la acción
se invoca con parámetros de suceso.

Como ejemplo, cree una regla que invoque la acción `hello` siempre que se publique una actualización de ubicación.
1. Cree un archivo denominado 'hello.js' con el código de acción siguiente:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Asegúrese de que la acción de desencadenante exista:
  ```
  ibmcloud wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. El siguiente paso consiste en crear la regla. La regla se habilita después de crearse; es decir, está disponible de forma inmediata para responder a las activaciones del desencadenante. Los tres parámetros son: el _nombre de la regla_, el _desencadenante_ y la _acción_.
  ```
  ibmcloud wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  Puede inhabilitar una regla en cualquier momento:
  ```
  ibmcloud wsk rule disable myRule
  ```
  {: pre}

4. Active el desencadenante **locationUpdate**. Cada vez que activa un suceso, la acción **hello** se invoca con los parámetros de suceso.
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Compruebe que la acción **hello** se ha invocado, revisando la activación más reciente.
  ```
  ibmcloud wsk activation list --limit 1 hello
  ```
  {: pre}

  Salida de ejemplo:
  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  Ahora consulte el ID de activación que aparece en la lista de la salida del mandato anterior:
  ```
  ibmcloud wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  Verá que la acción **hello** ha recibido la carga del suceso y ha devuelto la serie prevista.

Puede crear varias reglas que se asocien con el mismo desencadenante con distintas acciones.
Los desencadenantes y reglas no pueden pertenecer a un paquete. La regla puede estar asociada a una acción
que pertenece a un paquete; sin embargo, por ejemplo:
  ```
  ibmcloud wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

También puede utilizar reglas con secuencias. Por ejemplo, se puede crear una secuencia de acción
`recordLocationAndHello` que se active mediante la regla `anotherRule`.
  ```
  ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
