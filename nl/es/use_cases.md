---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# Casos de uso comunes
{: #use_cases}

El modelo de ejecución ofrecido por {{site.data.keyword.openwhisk_short}} admite diversos casos de uso. En las secciones siguientes se incluyen ejemplos típicos. Para ver una explicación detallada de la arquitectura sin servidor, casos de uso de ejemplo, una exposición de sus pros y sus contras y prácticas recomendadas sobre su implementación, consulte el excelente [artículo de Mike Roberts en el blog de Martin Fowler](https://martinfowler.com/articles/serverless.html){: external}.
{: shortdesc}

## Microservicios
{: #use_cases_microservices}

A pesar de sus ventajas, las soluciones basadas en microservicio siguen resultando difíciles de crear mediante tecnologías de nube convencionales, que suelen requerir el control de una compleja cadena de herramientas y conductos separados de creación y de operaciones. Los equipos pequeños y ágiles pierden demasiado tiempo con las complejidades de infraestructura y operativas, como por ejemplo, tolerancia a errores, equilibrio de la carga, escalado automático y registro. Estos equipos buscan es una forma de desarrollar código ágil y de valor añadido con los lenguajes de programación que ya conocen y que mejor les van para solucionar sus problemas específicos.

La naturaleza escalable y modular de {{site.data.keyword.openwhisk_short}} lo convierte en la herramienta ideal para implementar piezas de la lógica y convertirlas en acciones. Las acciones de {{site.data.keyword.openwhisk_short}} son independientes entre sí y se pueden implementar utilizando distintos lenguajes que reciben soporte de {{site.data.keyword.openwhisk_short}} y acceden a diversos sistemas subyacentes. Cada acción se puede desplegar y gestionar de forma independiente y se escala con independencia de otras acciones. {{site.data.keyword.openwhisk_short}} proporciona la interconectividad entre acciones mediante reglas, secuencias y convenios de denominación. Este tipo de entorno constituye una buena base para aplicaciones basadas en microservicios.

Otro argumento importante en favor de {{site.data.keyword.openwhisk_short}} es el coste de un sistema en una configuración de recuperación tras desastre. Compare microservicios con PaaS o CaaS con el uso de {{site.data.keyword.openwhisk_short}} suponiendo que tiene 10 microservicios que utilizan contenedores o tiempos de ejecución de Cloud Foundry. Esta comparación equivale a 10 procesos en ejecución continua y facturables en una sola zona de disponibilidad, 20 cuando se ejecutan en 2 zonas de disponibilidad y 40 cuando se ejecutan en dos regiones con dos zonas cada una. Para conseguir el mismo objetivo con {{site.data.keyword.openwhisk_short}}, puede ejecutarlos en tantas zonas de disponibilidad o regiones como se desee sin tener que pagar nada en concepto de costes incrementales.

## Apps web
{: #use_cases_webapps}

Debido a la naturaleza controlada por sucesos de {{site.data.keyword.openwhisk_short}}, ofrece varias ventajas para aplicaciones de usuario, en las que las solicitudes HTTP procedentes del navegador del usuario prestan servicio como sucesos. Las aplicaciones {{site.data.keyword.openwhisk_short}} utilizan capacidad de cálculo y solo se facturan cuando están prestando servicio a solicitudes de usuario. No existe la modalidad de desocupado o en espera. Esta característica hace que {{site.data.keyword.openwhisk_short}} resulte mucho más asequible, en comparación con los contenedores tradicionales o con las aplicaciones Cloud Foundry. Estas dos herramientas tienen tiempo de inactividad, a la espera de que entren solicitudes de usuario, y se le factura por todo ese tiempo en que están “suspendidas”.

Se pueden crear y ejecutar aplicaciones web completas con {{site.data.keyword.openwhisk_short}}. La combinación de las API sin servidor con el alojamiento de archivos estáticos para recursos del sitio, como por ejemplo HTML, JavaScript y CSS, significa que se pueden crear aplicaciones web completamente sin servidor. La simplicidad de operar con un entorno {{site.data.keyword.openwhisk_short}} alojado es no tener que operar con nada. Ya que {{site.data.keyword.openwhisk_short}} está alojado en {{site.data.keyword.cloud_notm}}, supone una gran ventaja en comparación con tener que configurar y trabajar con Node.js Express u otro entorno de ejecución de servidor tradicional.

## IoT
{: #use_cases_iot}

De forma inherente, los escenarios de Internet de las cosas se suelen controlar mediante sensores. Por ejemplo, una acción en {{site.data.keyword.openwhisk_short}} se podría desencadenar si fuera necesario reaccionar a un sensor que supera una temperatura concreta. IoT interactions are usually stateless with the potential of high load in major spontaneous events such as natural disasters, significant weather storms, or traffic jams. Se crea la necesidad de disponer de un sistema elástico en el que la carga de trabajo normal puede ser pequeña, pero que se pueda escalar con rapidez con un tiempo de respuesta predecible. Por ello, es deseable contar con la capacidad para gestionar un gran número de sucesos sin aviso previo al sistema. Resulta muy difícil crear un sistema que se ajuste a estos requisitos con las arquitecturas de servidor tradicionales. Ya que tienden a resultar insuficientes e incapaces de gestionar una punta de tráfico o sobredimensionadas y muy caras.

Se pueden implementar aplicaciones IoT que utilizan arquitecturas de servidor tradicional. Sin embargo, en muchos casos la combinación de distintos servicios y puentes de datos requiere conductos flexibles y de alto rendimiento. Abarcan desde dispositivos IoT hasta almacén en la nube y una plataforma de análisis. A menudo, los puentes preconfigurados no ofrecen las funciones de programación para implementar y ajustar una arquitectura para una solución particular. Dada la gran variedad de conductos y la falta de estandarización en lo referente a fusión de datos en general (en IoT en particular), es común ver entornos en los que el conducto requiere una transformación de datos personalizada. Estas transformaciones de datos personalizadas se aplican a la conversión de formato, filtrado o amplificación. {{site.data.keyword.openwhisk_short}} constituye una excelente herramienta para implementar dicha transformación, en modalidad ‘sin servidor’, donde la lógica personalizada se aloja en una plataforma de nube elástica y completamente gestionada.

Consulte el siguiente ejemplo de aplicación IoT que utiliza {{site.data.keyword.openwhisk_short}}, Node-RED, Cognitive y otros servicios: [Transformación sin servidor de datos en movimiento IoT con {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}.

![Ejemplo de arquitectura de una solución IoT](images/IoT_solution_architecture_example.png)

## Programa de fondo de API
{: #use_cases_backend}

Las plataformas de cálculo sin servidor ofrecen a los programadores un método rápido de crear API sin servidores. {{site.data.keyword.openwhisk_short}} da soporte a la generación automática de API REST para acciones. La
[característica de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-apigateway) puede invocar una acción con métodos HTTP que no sean POT y sin la clave de API de autorización de la acción a través de la pasarela de API de
{{site.data.keyword.openwhisk_short}}. Esta característica resulta útil no sólo para exponer las API a consumidores externos, sino también para crear aplicaciones de microservicios.

Además, las acciones de {{site.data.keyword.openwhisk_short}} se pueden conectar a la herramienta de gestión de API que elija (como por ejemplo [IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} y otra). Al igual que en otros casos de uso, se aplican todas las consideraciones sobre escalabilidad y otras Calidades de servicio.

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} es una app de ejemplo que utiliza acciones de {{site.data.keyword.openwhisk_short}} a través de una API REST.

Consulte el siguiente ejemplo que incluye una descripción de [utilización de un sistema sin servidor como programa de fondo de API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}.

## Programa de fondo móvil
{: #use_cases_mobile}

Muchas aplicaciones de móvil precisan de lógica en el lado del servidor. Sin embargo, los desarrolladores de entornos móviles no suelen tener experiencia en gestión de la lógica de la parte del servidor y prefieren centrarse en la app que se ejecuta en el dispositivo. Este objetivo de desarrollo se consigue fácilmente utilizando {{site.data.keyword.openwhisk_short}} como programa de fondo en la parte del servidor, y es una buena solución. Además, el soporte integrado para Swift del servidor permite a los desarrolladores reutilizar sus habilidades de programación existentes en iOS. Puesto que las aplicaciones móviles suelen tener patrones de carga imprevisibles, es recomendable utilizar una solución {{site.data.keyword.openwhisk_short}} alojada como {{site.data.keyword.cloud}}. Esta solución se puede escalar para que se adapte prácticamente a cualquier demanda en la carga de trabajo sin necesidad para suministrar recursos de forma anticipada.

## Proceso de datos
{: #use_cases_data}

Con la cantidad de datos ahora disponible, el desarrollo de aplicaciones precisa de la capacidad de procesar nuevos datos, y
potencialmente reaccionar a ellos. Este requisito incluye el proceso tanto de registros de base de datos estructurado, como de documentos no estructurados, imágenes o vídeos. {{site.data.keyword.openwhisk_short}} se puede configurar mediante canales de información proporcionados por el sistema o personalizados para que reaccione ante cambios en los datos y ejecute automáticamente acciones en los canales de información de datos de entrada. Se pueden programar acciones para procesar cambios, transformar formatos de datos, enviar y recibir mensajes, invocar otras acciones y actualizar diversos almacenes de datos. Los almacenes de datos soportados incluyen bases de datos relacionales basadas en SQL, tablas de datos en memoria, base de datos NoSQL, archivos, intermediarios de mensajería y otros sistemas diversos. Las reglas y secuencias de {{site.data.keyword.openwhisk_short}} ofrecen la flexibilidad de realizar cambios en el conducto de proceso sin programación, y se realizan a través de simples actualizaciones de configuración. Las opciones de almacén de datos y el bajo mantenimiento administrativo hacen que el sistema basado en {{site.data.keyword.openwhisk_short}} resulte muy ágil y se pueda adaptar fácilmente a los cambios de requisitos.

## Aplicación cognitiva
{: #use_cases_cognitive}

Las tecnologías cognitivas pueden resultar eficientes combinadas con {{site.data.keyword.openwhisk_short}} para crear aplicaciones potentes. Por ejemplo, se puede utilizar la API IBM Alchemy y Watson Visual Recognition con {{site.data.keyword.openwhisk_short}} para extraer automáticamente información útil de vídeos sin tener que visualizarlos. Esta tecnología es una extensión “cognitiva” del caso de uso de [proceso de datos](#use_cases_data) que se ha descrito anteriormente. {{site.data.keyword.openwhisk_short}} también se ajusta perfectamente a la implementación de una función Bot combinada con servicios cognitivos.

Se proporciona una aplicación de ejemplo, [Dark Vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external}, que hace exactamente esto. En esta aplicación, el usuario carga un vídeo o una imagen utilizando la aplicación web Dark Vision, que lo almacena en una base de datos {{site.data.keyword.cloudant_short_notm}}. Una vez cargado el vídeo, {{site.data.keyword.openwhisk_short}} detecta el nuevo vídeo escuchando los cambios de {{site.data.keyword.cloudant_short_notm}} (desencadenante). Luego {{site.data.keyword.openwhisk_short}} desencadena la acción de extractor de vídeo. Durante su ejecución, el extractor genera fotogramas (imágenes) y las guarda en {{site.data.keyword.cloudant_short_notm}}. A continuación los fotogramas se procesan con Watson Visual Recognition y los resultados se guardan en la misma base de datos {{site.data.keyword.cloudant_short_notm}}. Los resultados se pueden visualizar con la aplicación web Dark Vision o bien con una aplicación iOS. Se puede utilizar {{site.data.keyword.cos_full_notm}} además de {{site.data.keyword.cloudant_short_notm}}, en cuyo caso, los metadatos de vídeo y de imágenes se guardan en {{site.data.keyword.cloudant_short_notm}} y los archivos de contenido de medios se guardan en {{site.data.keyword.cos_full_notm}}.

## Proceso de sucesos con Kafka o {{site.data.keyword.messagehub}}
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} resulta ideal para utilizar con Kafka, con {{site.data.keyword.messagehub_full}} (basado en Kafka) y con otros sistemas de mensajería. La naturaleza controlada por sucesos de estos sistemas requiere que el tiempo de ejecución controlado por sucesos procese los mensajes. El tiempo de ejecución puede aplicar la lógica empresarial a dichos mensajes, que es exactamente lo que proporciona {{site.data.keyword.openwhisk_short}} con sus canales de información, desencadenantes y acciones. Se suelen utilizar Kafka y {{site.data.keyword.messagehub}} para grandes volúmenes imprevisibles de cargas de trabajo y que requieren que los consumidores de estos mensajes se ajusten de inmediato. De nuevo, esta es una situación ideal para {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} ofrece la capacidad integrada de consumir mensajes, así como de publicar los mensajes proporcionados en el paquete [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams).



