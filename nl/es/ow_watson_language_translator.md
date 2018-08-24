---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Paquete {{site.data.keyword.languagetranslatorshort}}

{{site.data.keyword.languagetranslatorfull}} traduce texto de un idioma a otro. El servicio ofrece varios modelos de traducción proporcionados por IBM que puede personalizar en función de su idioma y terminología exclusiva.
{: shortdesc}

El paquete {{site.data.keyword.languagetranslatorshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.languagetranslatorshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.languagetranslatorshort}}. |
| [translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text, model_id, source, target,  | Traducir texto. |
| [identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,  | Identificar el idioma del texto. |
| [list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listar los idiomas que se pueden identificar. |
| [create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    base_model_id,     name,     forced_glossary,     parallel_corpus,  | Crear un modelo. |
| [delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Suprimir un modelo. |
| [get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Obtener detalles de modelo. |
| [list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    source,     target,     default_models,  | Listar modelos. |

## Creación de una instancia de servicio de {{site.data.keyword.languagetranslatorshort}}
{: #service_instance}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.languagetranslatorshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.languagetranslatorshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/language_translator).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.languagetranslatorshort}}
{: #install}

Una vez tenga una instancia de servicio de {{site.data.keyword.languagetranslatorshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.languagetranslatorshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_cli}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el [mandato `wskdeploy` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) y añada el binario descargado a su PATH.

Para instalar el paquete {{site.data.keyword.languagetranslatorshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.languagetranslatorshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    wskdeploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.languagetranslatorshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.languagetranslatorshort}}.
    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desea instalar el paquete {{site.data.keyword.cos_short}}. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse en el paquete **Language Translator**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones y puede buscar el nuevo paquete, que se denomina **language-translator-v3**.

7. Para utilizar las acciones en el paquete **language-translator-v3**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba. 
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse en una Acción del paquete **language-translator-v3** que desee utilizar. Se abre la página de detalles de dicha acción. 
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**. 
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.languagetranslatorshort}}
{: #usage}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning).

Las funciones de este paquete utilizan la versión actual de Language Translator, 2018-05-01. Pruebe la acción `identify`.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}
