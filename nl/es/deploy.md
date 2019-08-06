---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploying actions, manifest, manifest file, functions

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


# Despliegue de entidades con un archivo de manifiesto
{: #deploy}

Puede utilizar {{site.data.keyword.openwhisk_short}} para describir y desplegar todas las entidades del espacio de nombres utilizando un archivo de manifiesto que esté escrito en YAML. Puede utilizar este archivo para desplegar todos los [Paquetes](/docs/openwhisk?topic=cloud-functions-pkg_ov), [Acciones](/docs/openwhisk?topic=cloud-functions-actions), [Desencadenantes](/docs/openwhisk?topic=cloud-functions-triggers) y [Reglas](/docs/openwhisk?topic=cloud-functions-rules) de Functions con un solo mandato.

El archivo de manifiesto describe el conjunto de entidades que desea desplegar o cuyo despliegue desea anular como un grupo. El contenido del archivo de manifiesto debe ajustarse a la
[Especificación YAML de despliegue de OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification){: external}. Una vez definido, puede utilizar el archivo de manifiesto para desplegar o volver a desplegar un grupo de entidades de Functions dentro del mismo espacio de nombres de Functions o de otro distinto. Puede utilizar los mandatos `ibmcloud fn deploy` e `ibmcloud fn undeploy` del plugin de Functions para desplegar o anular el despliegue de entidades de Functions que están definidas en el archivo de manifiesto.

## Creación del ejemplo de API Hello World
{: #deploy_helloworld_example}

Este ejemplo utiliza un código Node.js simple `helloworld.js`, crea una acción web `hello_world` dentro de un paquete `hello_world_package` y define una API REST para esta acción.
{: shortdesc}

1. Cree un archivo `helloworld.js` con el código siguiente.

    ```javascript
    function main() {
       return {body: 'Hello world'};
}
    ```
    {: codeblock}

    El archivo de manifiesto del despliegue define las variables siguientes.
    * El nombre del paquete.
    * El nombre de la acción.
    * La anotación de acción que indica que va a ser una acción web.
    * El nombre de archivo de código de acción.
    * La API con una vía de acceso base de `/hello`.
    * La vía de acceso de punto final de `/world`.

2. Cree el archivo `hello_world_manifest.yml`.

    ```yaml
    packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
    ```
    {: codeblock}

3. Utilice el mandato `deploy` para desplegar el paquete, la acción y la API.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. Puede ver una lista de acciones, paquetes y API para confirmar que las tres entidades esperadas se han creado correctamente.

    1. Muestre una lista de las acciones utilizando el mandato siguiente.

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. Muestre una lista de los paquetes utilizando el mandato siguiente.

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. Muestre una lista de las API utilizando el mandato siguiente.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. Invoque la API.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

Opcional: Puede anular el despliegue de algunas entidades utilizando el mandato `undeploy`.

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## Más ejemplos de despliegue de OpenWhisk
{: more_deploy_examples}

El despliegue de Functions se basa en el proyecto de despliegue de OpenWhisk, que tiene
[varios ejemplos de manifiesto de despliegue](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples){: external} que se pueden utilizar dentro de Functions.  Puede utilizar el mandato `ibmcloud fn deploy` en lugar de
`wskdeploy`.

## Especificación del manifiesto de despliegue
{: manifest_specification}

Los manifiestos de despliegue de Functions deben ajustarse a la especificación de manifiestos de despliegue de OpenWhisk. Consulte la [Documentación de la especificación de manifiestos de despliegue de OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification){: external} para obtener detalles.




