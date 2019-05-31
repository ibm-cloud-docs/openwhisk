---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# Gestión de espacios de nombres
{: #namespaces}

Con {{site.data.keyword.openwhisk}}, puede crear espacios de nombres gestionados por Identity and Access Management (IAM) para agrupar entidades tales como acciones o desencadenantes. A continuación, puede crear políticas de acceso de IAM para el espacio de nombres.
{: shortdesc}


**¿Qué es un espacio de nombres?**

Los espacios de nombres contienen entidades de {{site.data.keyword.openwhisk_short}} como acciones y desencadenantes, y pertenecen a un grupo de recursos. Puede dejar a los usuarios acceder a sus entidades otorgándoles acceso a su espacio de nombres.

El nombre completo de una entidad es `/namespaceName/[packageName]/entityName`.


**¿Qué ocurre cuando creo un espacio de nombres?**

Los espacios de nombres que se crean dentro de {{site.data.keyword.openwhisk_short}}, se identifican como una instancia de servicio de IAM.
Durante la creación de un espacio de nombres, puede especificar el [grupo de recursos](/docs/resources?topic=resources-rgs) al que añadir la instancia de servicio.

Cuando se crea el espacio de nombres, se crean los siguientes artefactos al mismo tiempo:

* Un ID de servicio que se puede utilizar como un ID funcional al hacer llamadas de salida. Todas las acciones que se creen en este espacio de nombres podrán utilizar este ID de servicio para acceder a los demás recursos. Para ver todos los ID de servicio, ejecute `ibmcloud iam service-ids`.

* Una clave de API para el ID de servicio que se puede utilizar para generar señales de IAM. A continuación, puede utilizar las señales para autenticar el espacio de nombres con otros servicios de {{site.data.keyword.Bluemix_notm}}. La clave de API se proporciona a las acciones como una variable de entorno.

    No suprima las claves de API.
    {: tip}

**¿Hay alguna limitación para los espacios de nombres?**

La [creación de API con la pasarela de API](/docs/openwhisk?topic=cloud-functions-apigateway) y el uso del [SDK móvil](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) no están soportados para los espacios de nombres gestionados por IAM.

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información, consulte el apartado sobre [Detalles y límites del sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov).
{: tip}



**¿Qué hago si tengo un espacio de nombres basado en Cloud Foundry?**

Los espacios de nombres basados en Cloud Foundry seguirán funcionando. No obstante, para beneficiarse de las nuevas características, debe [migrar los espacios de nombres a IAM](/docs/resources?topic=resources-migrate).

</br>


## Creación de un espacio de nombres con la CLI
{: #namespaces_create}

Puede crear un espacio de nombres gestionado por IAM como parte de un grupo de recursos y gestionar políticas de acceso a los recursos haciendo referencia al grupo de recursos cuando se crea un espacio de nombres. Si tiene otros usuarios que requieren acceso al espacio de nombres, o si desea acceder a otros recursos desde las acciones del espacio de nombres, asegúrese de establecer políticas de IAM una vez que se haya creado el espacio de nombres.
{: shortdesc}

1. Haga referencia al grupo de recursos donde desee crear el espacio de nombres. Si aún no ha creado un [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puede apuntar al grupo de recursos `default` (predeterminado).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Cree un espacio de nombres habilitado para IAM.

  ```
  ibmcloud fn namespace create <namespace_name> [-n <description>]
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
        <td>Opcional: añada una descripción al espacio de nombres, por ejemplo, indicando qué tipo de acciones o paquetes va a contener.</td>
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
  Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  También puede ver una lista de todos los espacios de nombres, incluyendo los espacios de nombres basados en IAM y los basados en Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Antes de crear entidades en el espacio de nombres, establezca el contexto de CLI en el espacio de nombres haciendo referencia al mismo.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

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
        <td>La señal de Identity and Access Management (IAM) de {{site.data.keyword.Bluemix_notm}}. Para recuperar su señal de IAM, ejecute <code>ibmcloud iam oauth-tokens</code>.</td>
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

  Salida de ejemplo:

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

  Salida de ejemplo:
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



## Pasos siguientes
{: #namespaces_next}

Ahora que ha creado un espacio de nombres, puede crear políticas de acceso de IAM para ayudarle a protegerlo. Para empezar, consulte [Gestión del acceso](/docs/openwhisk?topic=cloud-functions-iam). Para obtener más información sobre cómo gestionar los espacios de nombres basados en IAM, consulte la [Referencia de API REST de {{site.data.keyword.openwhisk_short}}](/apidocs/functions).


