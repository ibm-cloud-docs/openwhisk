---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# Planificación de apps sin servidor
{: #plan}

Antes de empezar a crear una función, infórmese sobre las decisiones que deberá tomar en el camino.
{: shortdesc}

## Revisar el soporte del entorno de ejecución
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} proporciona [entornos de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) estándar para ejecutar el código. Pero, debido a que {{site.data.keyword.openwhisk_short}} es sin servidor, no tiene limitaciones en cuanto a los entornos de ejecución que puede utilizar. Puede crear entornos de ejecución personalizados creando su propia [imagen de Docker personalizada](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) para empaquetar el código.
{: shortdesc}



## Dar una arquitectura al código
{: #plan_architect}

Es posible que tenga que hacer algunos ajustes en el código existente para que se ejecute en {{site.data.keyword.openwhisk}}. Si aún no tiene su propio código, tenga en cuenta lo siguiente al escribirlo.
{: shortdesc}

1. Cree componentes dentro del código.

    Si ya tiene una app y quiere convertirla para que sea sin servidor, tenga en cuenta cómo podrá necesitar dividir la app en partes más pequeñas. Cada función contiene un conjunto de desencadenantes para el código que desea ejecutar. Por ejemplo, si se crea un problema en GitHub, ejecuta este código JavaScript. Si la app contiene varios de estos eventos o acciones, considere la posibilidad de separarlos en diferentes funciones.

2. Utilice los servicios de {{site.data.keyword.cloud_notm}} en lugar de infraestructuras.

    En lugar de utilizar infraestructuras para proporcionar las prestaciones a la función en tiempo de ejecución, puede utilizar servicios de {{site.data.keyword.cloud}}. Muchas tareas comunes que las infraestructuras le ayudan a completar están disponibles como servicios en {{site.data.keyword.cloud}}.
    {: shortdesc}

    Por ejemplo, en lugar de utilizar una infraestructura para la autenticación, pruebe con {{site.data.keyword.appid_full}}. Si necesita almacenamiento de archivos externos, pruebe {{site.data.keyword.cos_full}}.

    Si la prestación que desea incorporar no está disponible como servicio en {{site.data.keyword.cloud}}, siempre puede integrar esa prestación también con canales de información y API, sin necesidad de infraestructuras.

3. [Verifique que el código se ajusta a los límites del sistema.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    El código se debe ajustar a los límites del sistema para poderse ejecutar. Por ejemplo, si el código empaquetado es mayor de 48 MB, puede que tenga que seguir dividiéndolo en trozos más pequeños o empaquetarlo como una imagen de Docker.

    Las aplicaciones que contienen muchos módulos de terceros, bibliotecas nativas o herramientas externas pueden alcanzar este límite. Si crea una acción de paquete .zip o .jar mayor de 48 MB, debe ampliar la imagen del entorno de tiempo de ejecución con dependencias. A continuación, utilice un archivo de origen individual o un archivador más pequeño de 48 MB. Por ejemplo, mediante la creación de un tiempo de ejecución de Docker personalizado, que incluye bibliotecas compartidas necesarias, no es necesario que estas dependencias estén presentes en el archivo de archivado. Los archivos de origen privado se pueden empaquetar en el archivado e introducirse en el tiempo de ejecución.

4. Determine los parámetros que se deben inyectar en el código.

    En acciones sin servidor, los datos se proporcionan añadiendo parámetros a las acciones. Los parámetros se declaran como argumentos en la función principal (main) sin servidor. Generalmente, estos parámetros son credenciales para un servicio, pero pueden ser cualquier cosa que permita reutilizar su código con diferentes desencadenantes.

5. [Verifique los requisitos estructurales del código para utilizarlo en una función.](/docs/openwhisk?topic=cloud-functions-prep)

    Si ya tiene una app o tiene previsto desarrollar un script para utilizarlo, probablemente su código necesita algunos ajustes para que lo pueda consumir {{site.data.keyword.openwhisk}}. El código en sí debe cumplir ciertos requisitos estructurales, como por ejemplo los parámetros de entrada y los resultados de salida. También puede que se tenga que empaquetar en un solo archivo que incluya todas sus dependencias.







## Determinar el origen de sucesos
{: #plan_source}

Piense en su función en términos del suceso que desea que ocurra para activar la ejecución del código. Quizás desee que se ejecute el código en cada confirmación a un repositorio GitHub. O quizás desee que se ejecute el código cada vez que se realice una actualización en una base de datos Cloudant.
{: shortdesc}

Después de elegir un suceso que desencadene la función, revise los paquetes que ya están disponibles para utilizar. Quizás puede utilizar alguno y así simplificar el desarrollo de su función. Si no, puede crear su propio paquete para su origen de sucesos.

Es posible que tenga que volver revisar el código y realizar algunas adaptaciones según los sucesos que haya seleccionado.


## Organizar despliegues entre entornos
{: #plan_environments}

Decida cómo desea desplegar las funciones entre los entornos, como por ejemplo, entornos de desarrollo, de transferencia y de producción.
{: shortdesc}

Dado que {{site.data.keyword.openwhisk_short}} es un servicio basado en Cloud Foundry, puede gestionar el despliegue de sus funciones en las organizaciones y espacios de Cloud Foundry que se proporcionan en {{site.data.keyword.cloud_notm}}. Para organizar sus funciones en estos entornos, puede decidir crear una organización para cada función. A continuación, cree un espacio para cada entorno que necesite. Si no, también podría tener una organización para cada entorno y crear un espacio para cada función. No importa cómo organice las organizaciones y los espacios, elija una estructura que pueda utilizar para gestionar de forma eficaz las entidades de función.

También puede utilizar [espacios de nombres](/docs/openwhisk?topic=cloud-functions-namespaces) para aislar los recursos. Cada espacio de {{site.data.keyword.cloud_notm}} contiene de forma predeterminada un espacio de nombres Open Whisk. Puede agrupar las entidades, como por ejemplo las acciones o los desencadenantes en un espacio de nombres y, a continuación, crear políticas de Identidad y Acceso (IAM) para gestionar los permisos de usuario para ese grupo.




