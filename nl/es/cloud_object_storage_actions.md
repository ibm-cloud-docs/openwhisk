---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Paquete Object Storage
{: #cloud_object_storage_actions}

El paquete {{site.data.keyword.cos_full_notm}} proporciona un conjunto de acciones para interactuar con las instancias de {{site.data.keyword.cos_full}}. Estas acciones permiten leer, escribir y suprimir de los depósitos ("buckets") presentes en una instancia de {{site.data.keyword.cos_short}}.
{: shortdesc}

El paquete {{site.data.keyword.cos_short}} incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/cloud-object-storage` | paquete | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Trabajar con una instancia de {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | acción | bucket, key, body | Escribir un objeto en un depósito. |
| `/cloud-object-storage/object-read` | acción | bucket, key | Leer un objeto de un depósito. |
| `/cloud-object-storage/object-delete` | acción | bucket, key | Suprimir un objeto de un depósito. |
| `/cloud-object-storage/bucket-cors-put` | acción | bucket, corsConfig | Asignar una configuración de CORS a un depósito. |
| `/cloud-object-storage/bucket-cors-get` | acción | bucket | Leer la configuración de CORS de un depósito. |
| `/cloud-object-storage/bucket-cors-delete` | acción | bucket | Suprimir la configuración de CORS de un depósito. |
| `/cloud-object-storage/client-get-signed-url` | acción | bucket, key, operation | Obtener un URL firmado para restringir la lectura, escritura y supresión de un objeto en un depósito. |

## Creación de una instancia de servicio de {{site.data.keyword.cos_full_notm}}
{: #cloud_object_storage_service_instance}

Antes de instalar el paquete, debe solicitar una instancia de {{site.data.keyword.cos_short}} y crear al menos un depósito ("bucket").

1. [Creación de una instancia de servicio de {{site.data.keyword.cos_full_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance).

2. [Creación de un conjunto de credenciales de servicio HMAC ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials) para la instancia de servicio de {{site.data.keyword.cos_short}}. En el campo **Añadir parámetros de configuración en línea (opcional)**, añada `{"HMAC":true}`.

3. [Cree al menos un depósito ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage/getting-started.html#create-buckets).

## Instalación del paquete {{site.data.keyword.cos_short}}
{: #cloud_object_storage_installation}

Una vez tenga una instancia de servicio de {{site.data.keyword.cos_short}}, utilice la CLI o la interfaz de usuario de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.cos_short}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_cli}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el mandato `wskdeploy`. Consulte la [Documentación de Apache OpenWhisk ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project).

Para instalar el paquete {{site.data.keyword.cos_short}}:

1. Clone el repositorio del paquete {{site.data.keyword.cos_short}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue hasta el directorio `runtimes/nodejs` o `runtimes/python`. Las acciones en el paquete {{site.data.keyword.cos_short}} se despliegan en el tiempo de ejecución que elija.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Despliegue el paquete. Si más adelante decide ejecutar las acciones en este paquete en otro tiempo de ejecución, puede repetir el paso anterior y este paso para volver a desplegar el paquete.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verifique que el paquete `cloud-object-storage` se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Salida:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. Enlace a las credenciales de la instancia de {{site.data.keyword.cos_short}} que creó para el paquete.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.cos_short}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desea instalar el paquete {{site.data.keyword.cos_short}}. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **IBM Cloud Object Storage** y, a continuación, pulse el paquete **IBM Cloud Object Storage**.

5. En la sección de entornos de tiempo de ejecución disponibles, seleccione NodeJS o Python desde la lista desplegable y, a continuación, pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **cloud-object-storage**.

7. Para utilizar las acciones en el paquete **cloud-object-storage**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse en una Acción del paquete **cloud-object-storage** que desee utilizar. Se abre la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Escritura en un depósito de {{site.data.keyword.cos_short}}
{: #cloud_object_storage_write}

Utilice la acción `object-write` para escribir un objeto en un depósito {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: En los pasos siguientes, se utiliza el nombre `testbucket` como ejemplo. Los depósitos ("buckets") en {{site.data.keyword.cos_full_notm}} deben ser únicos globalmente, de forma que deberá sustituir `testbucket` con un nombre de depósito exclusivo.

### Escritura en un depósito desde la CLI
{: #write_bucket_cli}

Escriba un objeto en el depósito utilizando la acción `object-write`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Salida de ejemplo:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Escritura en un depósito desde la interfaz de usuario
{: #write_bucket_ui}

1. Vaya a la [página Acciones en la consola de {{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/actions).

2. Bajo el paquete `cloud-object-storage`, pulse la acción **object-write**.

3. En el panel de Código, pulse **Cambiar entrada**.

4. Especifique un objeto JSON que contenga el depósito, la clave y el cuerpo como claves de objeto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. Pulse **Guardar**.

6. Pulse **Invocar**.

7. Verifique que la salida es parecida a la siguiente:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Lectura desde un depósito de {{site.data.keyword.cos_short}}
{: #cloud_object_storage_read}

Utilice la acción `object-read` para leer un objeto de un depósito de {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: En los pasos siguientes, se utiliza el nombre `testbucket` como ejemplo. Los depósitos ("buckets") en {{site.data.keyword.cos_full_notm}} deben ser únicos globalmente, de forma que deberá sustituir `testbucket` con un nombre de depósito exclusivo.

### Lectura desde un depósito desde la CLI
{: #read_bucket_cli}

Lea un objeto en el depósito utilizando la acción `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Salida de ejemplo:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### Lectura desde un depósito desde la interfaz de usuario
{: #read_bucket_ui}

1. Vaya a la [página Acciones en la consola de {{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/actions).

2. Bajo el paquete `cloud-object-storage`, pulse la acción **object-read**.

3. En el panel de Código, pulse **Cambiar entrada**.

4. Especifique un objeto JSON que contenga el depósito y la clave como claves de objeto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. Pulse **Guardar**.

6. Pulse **Invocar**.

7. Verifique que la salida es parecida a la siguiente:   
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}
