---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: watson, translator, cognitive, translating text, language, functions

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


# Translator

## Paquete {{site.data.keyword.languagetranslatorshort}}
{: #pkg_translator}

El paquete instalable {{site.data.keyword.languagetranslatorfull}} traduce texto de un idioma a otro. El servicio ofrece varios modelos de traducción proporcionados por IBM que puede personalizar en función de su idioma y terminología exclusiva.
{: shortdesc}

El paquete {{site.data.keyword.languagetranslatorshort}} contiene las siguientes entidades. Puede encontrar información más detallada en la referencia de {{site.data.keyword.languagetranslatorshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html){: external} | Paquete | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Trabajar con el servicio {{site.data.keyword.languagetranslatorshort}}. |
| [`translate`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `model_id`, `source`, `target` | Traducir texto. |
| [`identify`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text` | Identificar el idioma del texto. |
| [`list-identifiable-languages`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listar los idiomas que se pueden identificar. |
| [`create-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `base_model_id`, `name`, `forced_glossary`, `parallel_corpus` | Crear un modelo. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Suprimir un modelo. |
| [`get-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Obtener detalles de modelo. |
| [`list-models`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models){: external} | Acción | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `source`, `target`, `default_models` | Listar modelos. |

## Creación de una instancia de servicio de {{site.data.keyword.languagetranslatorshort}}
{: #service_instance_translator}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.languagetranslatorshort}}.
{: shortdesc}

1. [Crear una instancia de servicio de {{site.data.keyword.languagetranslatorshort}}](https://cloud.ibm.com/catalog/services/language_translator){: external}.
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.languagetranslatorshort}}
{: #install_translator}

Una vez tenga una instancia de servicio de {{site.data.keyword.languagetranslatorshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.languagetranslatorshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_cli}

**Antes de empezar**

[Instale el plugin de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install) para la CLI de {{site.data.keyword.cloud_notm}}.

Instale el paquete de {{site.data.keyword.languagetranslatorshort}}.

1. Clone el repositorio del paquete {{site.data.keyword.languagetranslatorshort}}.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
    ```
    {: pre}

3. Verifique que el paquete se ha añadido a la lista de paquetes.

    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Resultado**

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

    **Resultado de ejemplo**

    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.languagetranslatorshort}}.

    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
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

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleccione el espacio de nombres en el que desea instalar el paquete utilizando el menú desplegable de espacios de nombres. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **Watson**.

5. Pulse sobre el paquete **Language Translator**.

6. Pulse **Instalar**.

7. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina `language-translator-v3`.

8. Para utilizar las acciones del paquete `language-translator-v3`, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos [5 y 6 en las instrucciones de la CLI](#languagetranslator_cli).
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario.
  
  Debe completar los pasos siguientes con cada acción que desee utilizar.
  {: note}

    1. Pulse sobre una acción del paquete `language-translator-v3` que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo `parámetro`. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.languagetranslatorshort}}
{: #usage_translator}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones requieren un parámetro de versión con el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning){: external}.

Las funciones de este paquete utilizan la versión actual de Language Translator, `2018-05-01`. Pruebe la acción `identify`.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}




