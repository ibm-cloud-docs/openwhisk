---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# Gestión de espacios de nombres
{: #namespaces}

Con {{site.data.keyword.openwhisk}}, puede crear espacios de nombres gestionados por Identity and Access Management (IAM) para agrupar entidades tales como acciones o desencadenantes. A continuación, puede crear políticas de acceso de IAM para el espacio de nombres. Para obtener una visión general de IAM, consulte el [Blog de anuncio de habilitación de IAM de {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## ¿Qué es un espacio de nombres?

Los espacios de nombres contienen entidades de {{site.data.keyword.openwhisk_short}} como acciones y desencadenantes, y pertenecen a un grupo de recursos. Puede dejar a los usuarios acceder a sus entidades otorgándoles acceso a su espacio de nombres.

El nombre completo de una entidad es `/namespaceName/packageName/entityName`.

### ¿Qué ocurre cuando creo un espacio de nombres?

Los espacios de nombres que se crean dentro de {{site.data.keyword.openwhisk_short}}, se identifican como una instancia de servicio de IAM.
Durante la creación de un espacio de nombres, puede especificar el [grupo de recursos](/docs/resources?topic=resources-rgs) al que añadir la instancia de servicio.

Cuando se crea un espacio de nombres, se crean los componentes siguientes:

| Componente | Descripción |
| --- | --- | 
| Un ID de servicio | Puede utilizar el ID de servicio como un ID funcional al hacer llamadas salientes. Todas las acciones que se creen en este espacio de nombres podrán utilizar este ID de servicio para acceder a los demás recursos. De forma predeterminada, el usuario funcional obtiene el rol Lector. Un acceso de lector significa que puede leer entidades de espacio de nombres e invocar acciones. El rol de Lector es utilizado por los desencadenantes para invocar acciones. Para controlar el tráfico de entrada, es posible que desee otorgar acceso a otros usuarios como, por ejemplo, asignar el rol de lector para que puedan invocar acciones. |
| Una clave de API | Una clave de API para el ID de servicio que se puede utilizar para generar señales de IAM. Puede utilizar las señales para autenticar el espacio de nombres con otros servicios de {{site.data.keyword.cloud_notm}}. La clave de API se proporciona a las acciones como la variable de entorno `__OW_IAM_NAMESPACE_API_KEY`. |

Ver todos los ID de servicio.
```
ibmcloud iam service-ids
```
{: pre}

Ver las claves de API que están asociadas a un ID de servicio. 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

No suprima las claves de API.
{: tip}

### ¿Hay alguna limitación para los espacios de nombres?

La [creación de API con la pasarela de API](/docs/openwhisk?topic=cloud-functions-apigateway) y el [SDK móvil](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) no están soportados para los espacios de nombres gestionados por IAM. 

Los nombres de todas las entidades incluidas las acciones, desencadenantes, reglas, paquetes y los espacios de nombres están en una secuencia de caracteres que cumplen el formato siguiente:
* El primer carácter debe ser un carácter alfanumérico o un signo de subrayado.
* Los caracteres posteriores pueden ser alfanuméricos, espacios o cualquiera de los siguientes valores: `_`, `@`, `.`, `-`.
* El último carácter no puede ser un espacio.

### ¿Qué hago si tengo un espacio de nombres basado en Cloud Foundry?

Los espacios de nombres basados en Cloud Foundry seguirán funcionando. No obstante, para beneficiarse de las nuevas características, debe crear un espacio de nombres habilitado para IAM.


## Creación de un espacio de nombres basado en IAM en la interfaz de usuario
{: #create_iam_ui}

1. En la [consola de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}, pulse en el menú desplegable de espacios de nombres.

2. Pulse en **Crear espacio de nombres**.

3. Especifique un nombre de visualización para el espacio de nombres y una breve descripción, como por ejemplo los tipos de acciones o paquetes que tiene previsto crear en este espacio de nombres.

4. Elija el grupo de recursos en el que desea crear el espacio de nombres y una ubicación en la que desplegar el recurso de espacio de nombres.

5. Pulse **Crear**.

6. Para ver la instancia de servicio para el recurso de espacio de nombres, vaya al [panel de control de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: external} y busque el nombre del espacio de nombres en el panel **Espacios de nombres de funciones**.

Si lo necesita, puede actualizar el nombre o la descripción del espacio de nombres en la página **Valores de espacio de nombres** en la consola de {{site.data.keyword.openwhisk_short}}.

## Creación de un espacio de nombres basado en IAM con la CLI
{: #namespaces_create}

Puede crear un espacio de nombres gestionado por IAM como parte de un grupo de recursos y gestionar políticas de acceso a los recursos haciendo referencia al grupo de recursos cuando se crea un espacio de nombres. Si tiene otros usuarios que requieren acceso al espacio de nombres, o si desea acceder a otros recursos desde las acciones del espacio de nombres, asegúrese de establecer políticas de IAM una vez que se haya creado el espacio de nombres.
{: shortdesc}

1. Haga referencia al grupo de recursos donde desee crear el espacio de nombres. Si aún no ha creado un [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puede apuntar al grupo de recursos `default` (predeterminado).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Cree un espacio de nombres habilitado para IAM. Opcional: Incluya una descripción para el espacio de nombres utilizando el distintivo `-n` o `--description`. Si la descripción es más larga que una palabra, debe ir entre comillas.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Descripción de los componentes de este mandato</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>El nombre de visualización del espacio de nombres basado en IAM.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Opcional: añada una descripción al espacio de nombres, por ejemplo, indicando el tipo de acciones o paquetes que tiene previsto crear. Si la descripción es más larga que una palabra, debe ir entre comillas.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Opcional: añada una descripción al espacio de nombres, por ejemplo, indicando el tipo de acciones o paquetes que tiene previsto crear. Si la descripción es más larga que una palabra, debe ir entre comillas.</td>
      </tr>
    </tbody>
  </table>

  Salida de ejemplo:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verifique que se haya creado el nuevo espacio de nombres.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Salida de ejemplo:

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  También puede ver una lista de todos los espacios de nombres, incluyendo los espacios de nombres basados en IAM y los basados en Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Para poder crear entidades en el espacio de nombres, debe establecer el contexto de CLI al espacio de nombres estableciéndolo como destino.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

Después de establecer una propiedad, como la propiedad `--namespace`, se retiene hasta que se desconfigura manualmente. Si desea conmutar entre los espacios de nombres IAM o entre Cloud Foundry e IAM, debe anular la configuración de la propiedad de espacio de nombres y restablecerla. Para obtener más información, consulte [`ibmcloud fn property set`]
{: note}

## Creación de un espacio de nombres con la API
{: #namespaces_create_api}

Puede crear un espacio de nombres gestionado por IAM como parte de un grupo de recursos y gestionar políticas de acceso a los recursos haciendo referencia al grupo de recursos cuando se crea un espacio de nombres. Si tiene otros usuarios que requieren acceso al espacio de nombres, o si desea acceder a otros recursos desde las acciones del espacio de nombres, asegúrese de establecer políticas de IAM una vez que se haya creado el espacio de nombres.
{: shortdesc}

1. Haga referencia al grupo de recursos donde desee crear el espacio de nombres. Si aún no ha creado un [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puede apuntar al grupo de recursos `default` (predeterminado).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Cree un espacio de nombres habilitado para IAM.

  ```
  curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Descripción de los componentes de este mandato</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>La señal de Identity and Access Management (IAM) de {{site.data.keyword.cloud_notm}}. Para recuperar su señal de IAM, ejecute <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>El nombre del espacio de nombres.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>El ID del grupo de recursos en el que desea crear el espacio de nombres. Para ver los ID de los grupos de recursos, ejecute <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>El ID del plan de recursos, como functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Opcional: añada una descripción al espacio de nombres, por ejemplo, indicando qué tipo de acciones o paquetes va a contener.</td>
      </tr>
    </tbody>
  </table>

  **Resultado de ejemplo**

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. Verifique que se haya creado el nuevo espacio de nombres.

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  También puede ver una lista de todos los espacios de nombres, incluyendo los espacios de nombres basados en IAM y los basados en Cloud Foundry:
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}

Para obtener más información sobre cómo trabajar con HTTP REST, consulte los [Documentos de la API de {{site.data.keyword.openwhisk_short}}](/apidocs/functions).
{: tip}

### Acceso a otros recursos desde un espacio de nombres
{: #namespace-access}

Generalmente las acciones llaman a otros {{site.data.keyword.cloud_notm}} recursos y servicios que requieren una autenticación adecuada. Si estos servicios están habilitados para IAM y aceptan las señales IAM, puede aprovechar el ID funcional del espacio de nombres para la comunicación de salida.
{: shortdesc}

Como se describe en [Gestión de acceso de IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), para cada espacio de nombres, se crea in ID de servicio que representa el espacio de nombres. Puede otorgar acceso a otros servicios y recursos para este ID de servicio, asignando los roles adecuados utilizando la gestión de políticas de IAM. Para obtener más información sobre cómo crear ID de servicio para acceder a otros servicios habilitados para IAM, consulte [Creación y trabajo con los ID de servicio](/docs/iam?topic=iam-serviceids#serviceids).

En tiempo de ejecución, {{site.data.keyword.openwhisk_short}} pasa una clave de API del ID de servicio del espacio de nombres al código de acción como la variable de entorno `__OW_IAM_NAMESPACE_API_KEY`. El código de acción puede utilizar esta clave de API para generar una señal de IAM. La mayoría de los SDK de {{site.data.keyword.openwhisk_short}} soportados, como Cloudant, {{site.data.keyword.watson}}, y {{site.data.keyword.cos_full_notm}} se autentican con la propia clave de API de IAM. Para otros recursos o servicios gestionados por IAM que utilizan una API REST, puede autenticarse con la señal que se deriva de la clave de API de IAM. Para obtener más información, consulte [Creación de una señal de acceso de IAM para un ID de usuario o de servicio](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

¿No está seguro de cómo se relacionan las claves de API y las señales? Obtenga más información en
[los documentos de IAM](/docs/iam?topic=iam-iamapikeysforservices).

## Pasos siguientes
{: #namespaces_next}

Ahora que ha creado un espacio de nombres, puede crear políticas de acceso de IAM para ayudar a protegerlo. Para empezar, consulte [Gestión del acceso](/docs/openwhisk?topic=cloud-functions-iam). 

Para obtener más información sobre cómo gestionar los espacios de nombres basados en IAM, consulte la [Referencia de API REST de {{site.data.keyword.openwhisk_short}}](/apidocs/functions).








