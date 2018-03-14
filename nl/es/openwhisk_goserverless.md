---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Integración de OpenWhisk con Serverless Framework
{: #openwhisk_goserverless}

[Serverless Framework](https://serverless.com/) es una infraestructura de código abierto para crear aplicaciones sin servidor. Mediante un archivo de manifiesto simple, los desarrolladores pueden definir funciones sin servidor, conectarlas con orígenes de sucesos y declarar servicios en la nube que necesita su aplicación. La infraestructura gestiona el despliegue de estas aplicaciones sin servidor en los proveedores de la nube. También permite a los desarrolladores supervisar los servicios en producción, las actualizaciones de despliegue y ayudar a depurar problemas. También ofrece un interesante ecosistema de plugins de otros proveedores para ampliar la funcionalidad de la infraestructura. OpenWhisk es la tecnología en la que se basa Serverless Framework.
{:shortdesc}

OpenWhisk tiene [su propio plugin de proveedor para Serverless Framework](https://github.com/serverless/serverless-openwhisk). Los desarrolladores que utilizan Serverless Framework pueden optar por desplegar sus aplicaciones en cualquier instancia de plataforma de OpenWhisk (alojada en {{site.data.keyword.Bluemix_notm}}, o en otra nube o red privada). Se admiten varios proveedores, lo que significa que mover una aplicación entre plataformas es mucho más fácil, permitiendo así a los desarrolladores crear aplicaciones sin servidor en varias nubes.

## Iniciación
{: #openwhisk_goserverless_starting}

[Guía de iniciación a OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/) oficial de Serverless Framework.
* Una guía que abarca la instalación, el flujo de desarrollo, las prácticas recomendadas, una guía paso a paso para crear y desplegar una aplicación de OpenWhisk en funcionamiento, y más temas.

Vea el vídeo sobre [Serverless Framework y OpenWhisk](https://youtu.be/GJY10W98Itc), que explica cómo utilizar Serverless Framework con el plugin de proveedor de OpenWhisk.

## Documentación
{: #openwhisk_goserverless_docs}

[Aquí encontrará](https://serverless.com/framework/docs/providers/openwhisk/) documentación actualizada sobre cómo utilizar OpenWhisk con Serverless Framework.

## Ejemplos
{: #openwhisk_goserverless_samples}
[Repositorio GitHub de ejemplos de Serverless Framework](https://github.com/serverless/examples) contiene ejemplos de OpenWhisk que muestran cómo crear API HTTP, planificadores, funciones de encadenamiento y más.
