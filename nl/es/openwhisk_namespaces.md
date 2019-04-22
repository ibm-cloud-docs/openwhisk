---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Creación de espacios de nombres
{: #openwhisk_namespaces}

En la región de Tokio, {{site.data.keyword.openwhisk_short}} utiliza espacios de nombres gestionados por Identity and Access Management (IAM) para agrupar entidades, como acciones o desencadenantes, de manera conjunta. A continuación, puede crear políticas de acceso para el espacio de nombres.
{: shortdesc}

Al crear un espacio de nombres de {{site.data.keyword.openwhisk_short}}, se identifica como una instancia de servicio de IAM. Las instancias de servicio gestionadas por IAM deben crearse dentro de un [grupo de recursos](/docs/resources?topic=resources-rgs). Puede crear su propio grupo de recursos o hacer referencia al predeterminado. Para ver las instancias de servicio de IAM que tiene en la cuenta, puede ejecutar `ibmcloud resource service-instances`.

Los artefactos siguientes se crean junto con el espacio de nombres. No los suprima.

* Se crea un ID de servicio que se puede utilizar como ID funcional al realizar llamadas de salida. Todas las acciones que se creen en este espacio de nombres podrán utilizar este ID de servicio para acceder a los demás recursos. Para ver todos los ID de servicio, ejecute `ibmcloud iam service-ids`.

* Se crea una clave de API para el ID de servicio anterior, que se puede utilizar para generar señales de IAM. A continuación, puede utilizar las señales para autenticar el espacio de nombres con otros servicios de IBM Cloud. La clave de API se proporciona a las acciones como una variable de entorno.


## Limitaciones
{: #limitations}

Actualmente no hay soporte para la [creación de API con la pasarela de API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) ni para el uso del [SDK móvil](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk) en espacios de nombres gestionados por IAM.

</br>

Para definir como objetivo el servicio de fondo de {{site.data.keyword.openwhisk_short}} en la ubicación de Tokio, debe añadir el `apihost` a todas las llamadas de CLI, como `ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com`. Esto es temporal hasta que se pueda hacer referencia a la ubicación mediante `ibmcloud target -r jp-tok`.
{: tip}



</br>
</br>


## Creación de un espacio de nombres con la CLI
{: #create_iam_cli}

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
          <td>El nombre de visualización del nuevo espacio de nombres basado en IAM.</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
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
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. Antes de crear entidades en el nuevo espacio de nombres, establezca el contexto de CLI en el espacio de nombres haciendo referencia a él.
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## Creación de un espacio de nombres con la API
{: #create_iam_api}

Puede crear un espacio de nombres gestionado por IAM como parte de un grupo de recursos y gestionar políticas de acceso a los recursos haciendo referencia al grupo de recursos cuando se crea un espacio de nombres. Si tiene otros usuarios que requieren acceso al espacio de nombres, o si desea acceder a otros recursos desde las acciones del espacio de nombres, asegúrese de establecer políticas de IAM una vez que se haya creado el espacio de nombres.
{: shortdesc}



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
          <td>La señal de IBM Cloud Identity and Access Management (IAM). Para recuperar su señal de IAM, ejecute <code>ibmcloud iam oauth-tokens</code>.</td>
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


Para obtener más información sobre cómo trabajar con HTTP REST, consulte los [Documentos de la API de Cloud Functions](https://cloud.ibm.com/apidocs/functions).
{: tip}

</br>
</br>


## Pasos siguientes
{: #next}

Ahora que ha creado un espacio de nombres, puede crear políticas de acceso de IAM para ayudarle a protegerlo. Para empezar, consulte [Gestión del acceso](/docs/openwhisk?topic=cloud-functions-iam#iam). Para obtener más información sobre cómo gestionar los espacios de nombres basados en IAM, consulte la [Referencia de API REST de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/apidocs/functions).
