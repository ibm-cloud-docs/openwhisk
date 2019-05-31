---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: watson assistant, openwhisk, functions

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

# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

El servicio instalable {{site.data.keyword.conversationfull}} combina el aprendizaje máquina, la compresión del lenguaje natural y herramientas de diálogo integradas para crear flujos de conversación entre sus apps y los usuarios.
{: shortdesc}

El paquete {{site.data.keyword.conversationshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.conversationshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.conversationshort}}. |
| [message](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    input, alternate_intents, context, entities, intents, output,     nodes_visited_details,  | Obtener la respuesta a la entrada del usuario. |
| [create-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, description, language, intents, entities, dialog_nodes, counterexamples, metadata, learning_opt_out,  | Crear un espacio de trabajo. |
| [delete-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,  | Suprimir un espacio de trabajo. |
| [get-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     export,     include_audit,  | Obtener información sobre un espacio de trabajo. |
| [list-workspaces](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar espacios de trabajo. |
| [update-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    name, description, language, intents, entities, dialog_nodes, counterexamples, metadata, learning_opt_out,     append,  | Actualizar un espacio de trabajo. |
| [create-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    intent, description, examples,  | Crear una intención. |
| [delete-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,  | Suprimir una intención. |
| [get-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,     export,     include_audit,  | Obtener información sobre una intención. |
| [list-intents](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     export,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Lista intenciones. |
| [update-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,    new_intent, new_description, new_examples,  | Actualizar una intención. |
| [create-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,    text,  | Crear un ejemplo de entrada de usuario. |
| [delete-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,     text,  | Suprimir un ejemplo de entrada de usuario. |
| [get-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,     text,     include_audit,  | Obtener información acerca de un ejemplo de entrada de usuario. |
| [list-examples](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar ejemplos de entrada de usuario. |
| [update-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     intent,     text,    new_text,  | Actualizar un ejemplo de entrada de usuario. |
| [create-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    text,  | Crear un contraejemplo. |
| [delete-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     text,  | Suprimir un contraejemplo. |
| [get-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     text,     include_audit,  | Obtener información sobre un contraejemplo. |
| [list-counterexamples](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar contraejemplos. |
| [update-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     text,    new_text,  | Actualizar un contraejemplo. |
| [create-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    entity, description, metadata, values, fuzzy_match,  | Crear una entidad. |
| [delete-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,  | Suprimir una entidad. |
| [get-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     export,     include_audit,  | Obtener información sobre una entidad. |
| [list-entities](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     export,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar entidades. |
| [update-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,    new_entity, new_description, new_metadata, new_fuzzy_match, new_values,  | Actualizar una entidad. |
| [create-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,    value, metadata, synonyms, patterns, value_type,  | Añadir un valor de entidad. |
| [delete-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,  | Suprimir un valor de entidad. |
| [get-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,     export,     include_audit,  | Obtener un valor de entidad. |
| [list-values](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     export,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar valores de entidad. |
| [update-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,    new_value, new_metadata, new_type, new_synonyms, new_patterns,  | Actualizar un valor de entidad. |
| [create-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,    synonym,  | Añadir un sinónimo de valor de entidad. |
| [delete-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,     synonym,  | Suprimir un sinónimo de valor de entidad. |
| [get-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,     synonym,     include_audit,  | Obtener un sinónimo de valor de entidad. |
| [list-synonyms](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar sinónimos de valores de entidad. |
| [update-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     entity,     value,     synonym,    new_synonym,  | Actualizar un sinónimo de valor de entidad. |
| [create-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,    dialog_node, description, conditions, parent, previous_sibling, output, context, metadata, next_step, actions, title, node_type, event_name, variable, digress_in, digress_out, digress_out_slots,  | Crear un nodo de diálogo. |
| [delete-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     dialog_node,  | Suprimir un nodo de diálogo. |
| [get-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     dialog_node,     include_audit,  | Obtener un nodo de diálogo. |
| [list-dialog-nodes](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     page_limit,     include_count,     sort,     cursor,     include_audit,  | Listar nodos de diálogo. |
| [update-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     dialog_node,    new_dialog_node, new_description, new_conditions, new_parent, new_previous_sibling, new_output, new_context, new_metadata, new_next_step, new_title, new_type, new_event_name, new_variable, new_actions, new_digress_in, new_digress_out, new_digress_out_slots,  | Actualizar un nodo de diálogo. |
| [list-all-logs](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    filter,     sort,     page_limit,     cursor,  | Listar sucesos de registro en todos los espacios de trabajo. |
| [list-logs](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    workspace_id,     sort,     filter,     page_limit,     cursor,  | Listar sucesos de registro en un espacio de trabajo. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suprimir datos etiquetados. |

## Creación de una instancia de servicio de {{site.data.keyword.conversationshort}}
{: #service_instance_conversation}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.conversationshort}}.

1. [Cree una instancia de servicio de {{site.data.keyword.conversationshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/conversation).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.conversationshort}}
{: #install_conversation}

Una vez tenga una instancia de servicio de {{site.data.keyword.conversationshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.conversationshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #conversation_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar el paquete {{site.data.keyword.conversationshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.conversationshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
    ```
    {: pre}

3. Verifique que el paquete se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Salida:
    ```
    packages
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.conversationshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.conversationshort}}.
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package assistant-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #conversation_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desee instalar el paquete. 

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse el paquete **Watson Assistant**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **assistant-v1**.

7. Para utilizar las acciones en el paquete **assistant-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **assistant-v1** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.conversationshort}}
{: #usage_conversation}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning).

Las funciones de este paquete utilizan la versión actual de Watson Assistant, 2018-07-10. Pruebe la acción `list-workspaces`.
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}

