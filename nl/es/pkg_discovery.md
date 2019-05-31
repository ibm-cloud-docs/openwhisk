---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: watson discovery, functions, watson, cognitive

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

# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

El servicio instalable {{site.data.keyword.discoveryfull}} corresponde a una búsqueda cognitiva y un motor de analíticas de contenido que puede añadir a sus aplicaciones para identificar patrones, tendencias y conocimientos prácticos para lograr una mejor toma de decisiones. Unifique con seguridad datos estructurados y no estructurados con contenido preenriquecido y utilice un lenguaje de consulta simplificado para eliminar la necesidad de filtrar manualmente los resultados.
{: shortdesc}

El paquete {{site.data.keyword.discoveryshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.discoveryshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.discoveryshort}}. |
| [create-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, description, size,  | Crear un entorno. |
| [delete-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  | Suprimir un entorno. |
| [get-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  | Obtener información sobre un entorno. |
| [list-environments](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,  | Listar entornos. |
| [list-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,  | Listar campos de todas las recopilaciones. |
| [update-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description,  | Actualizar un entorno. |
| [create-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, conversions, enrichments, normalizations,  | Añadir una configuración. |
| [delete-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  | Suprimir una configuración. |
| [get-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  | Obtener detalles de configuración. |
| [list-configurations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  | Listar configuraciones. |
| [update-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,    name, description, conversions, enrichments, normalizations,  | Actualizar una configuración. |
| [test-configuration-in-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration,     step,     configuration_id,     file,     metadata,     file_content_type,  | Probar una configuración. |
| [create-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, configuration_id, language,  | Crear una recopilación. |
| [delete-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Suprimir una recopilación. |
| [get-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Obtener detalles de recopilación. |
| [list-collection-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Listar campos de recopilación. |
| [list-collections](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  | Listar recopilaciones. |
| [update-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    name, description, configuration_id,  | Actualizar una recopilación. |
| [create-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    expansions,  | Crear o actualizar la lista de expansión. |
| [delete-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Suprimir la lista de expansión. |
| [list-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Obtener la lista de expansión. |
| [add-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     file,     metadata,     file_content_type,  | Añadir un documento. |
| [delete-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  | Suprimir un documento. |
| [get-document-status](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  | Obtener detalles de documento. |
| [update-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,     file,     metadata,     file_content_type,  | Actualizar un documento. |
| [federated-query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar documentos en varias recopilaciones. |
| [federated-query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar varios avisos del sistema de recopilación. |
| [query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar su recopilación. |
| [query-entities](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    feature, entity, context, count, evidence_count,  | Consultar una entidad de Knowledge Graph. |
| [query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar avisos del sistema. |
| [query-relations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    entities, context, sort, filter, count, evidence_count,  | Consultar una relación de Knowledge Graph. |
| [add-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    natural_language_query, filter, examples,  | Añadir una consulta a los datos de entrenamiento. |
| [create-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,    document_id, cross_reference, relevance,  | Añadir un ejemplo a la consulta de datos de entrenamiento. |
| [delete-all-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Suprimir todos los datos de entrenamiento. |
| [delete-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Suprimir una consulta de datos de entrenamiento. |
| [delete-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  | Suprimir un ejemplo de la consulta de datos de entrenamiento. |
| [get-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Obtener detalles sobre una consulta. |
| [get-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  | Obtener detalles de un ejemplo de datos de entrenamiento. |
| [list-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Listar datos de entrenamiento. |
| [list-training-examples](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Listar ejemplos para una consulta de datos de entrenamiento. |
| [update-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,    cross_reference, relevance,  | Cambiar una etiqueta o referencia cruzada de un ejemplo. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suprimir datos etiquetados. |

## Creación de una instancia de servicio de {{site.data.keyword.discoveryshort}}
{: #service_instance_discovery}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.discoveryshort}}.

1. [Cree una instancia de servicio de {{site.data.keyword.discoveryshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/discovery).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.discoveryshort}}
{: #install_discovery}

Una vez tenga una instancia de servicio de {{site.data.keyword.discoveryshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.discoveryshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #discovery_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar el paquete {{site.data.keyword.discoveryshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.discoveryshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.discoveryshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.discoveryshort}}.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #discovery_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desee instalar el paquete. 

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse el paquete **Discovery**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **discovery-v1**.

7. Para utilizar las acciones del paquete **discovery-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, realice el paso 4 de las instrucciones de la CLI que aparecen más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **discovery-v1** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.discoveryshort}}
{: #usage_discovery}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning).

Las funciones de este paquete utilizan la versión actual de Discovery, 2018-03-05. Pruebe la acción `list-environments`.
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}

