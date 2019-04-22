---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: use cases, microservices, web apps, iot, serverless, cognitive

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Casos de uso comunes
{: #openwhisk_common_use_cases}

El modelo de ejecución ofrecido por {{site.data.keyword.openwhisk_short}} admite diversos casos de uso. En las secciones siguientes se incluyen ejemplos típicos. Para ver una explicación detallada de la arquitectura sin servidor, casos de uso de ejemplo, una exposición de sus pros y sus contras y prácticas recomendadas sobre su implementación, consulte el excelente [artículo de Mike Roberts en el blog de Martin Fowler](https://martinfowler.com/articles/serverless.html).
{: shortdesc}

## Microservicios
{: #openwhisk_common_use_cases_microservices}

A pesar de sus ventajas, las soluciones basadas en microservicio siguen resultando difíciles de crear mediante tecnologías de nube convencionales, que suelen requerir el control de una compleja cadena de herramientas y conductos separados de creación y de operaciones. Los equipos pequeños y ágiles emplean demasiado tiempo con las complejidades de infraestructura y operativas, como por ejemplo, tolerancia a errores, equilibrio de la carga, escalado automático y registro. Estos equipos buscan es una forma de desarrollar código ágil y de valor añadido con los lenguajes de programación que ya conocen y que mejor les van para solucionar sus problemas específicos.

La naturaleza escalable y modular de {{site.data.keyword.openwhisk_short}} lo convierte en la herramienta ideal para implementar piezas de la lógica y convertirlas en acciones. Las acciones de {{site.data.keyword.openwhisk_short}} son independientes entre sí y se pueden implementar utilizando distintos lenguajes que reciben soporte de {{site.data.keyword.openwhisk_short}} y acceden a diversos sistemas subyacentes. Cada acción se puede desplegar y gestionar de forma independiente y se escala con independencia de otras acciones. {{site.data.keyword.openwhisk_short}} proporciona la interconectividad entre acciones mediante reglas, secuencias y convenios de denominación. Este tipo de entorno constituye una buena base para aplicaciones basadas en microservicios.

Otro argumento importante en favor de {{site.data.keyword.openwhisk_short}} es el coste de un sistema en una configuración de recuperación tras desastre. Compare microservicios con PaaS o CaaS con el uso de {{site.data.keyword.openwhisk_short}} suponiendo que tiene 10 microservicios que utilizan contenedores o tiempos de ejecución de CloudFoundry. Esta comparación equivale a 10 procesos en ejecución continua y facturables en una sola zona de disponibilidad, 20 cuando se ejecutan en 2 zonas de disponibilidad y 40 cuando se ejecutan en dos regiones con dos zonas cada una. Para conseguir el mismo objetivo con {{site.data.keyword.openwhisk_short}}, puede ejecutarlos en tantas zonas de disponibilidad o regiones como se desee sin tener que pagar nada en concepto de costes incrementales.

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) es una aplicación empresarial que aprovecha {{site.data.keyword.openwhisk_short}} y CloudFoundry para crear una aplicación de estilo factor 12. Constituye una solución inteligente de gestión de cadena de suministro que simula un entorno que ejecuta un sistema ERP. Aumenta este sistema ERP con aplicaciones para mejorar la visibilidad y la agilidad de los gestores de la cadena de suministro.

## Apps web
{: #openwhisk_common_use_cases_webapps}

Debido a la naturaleza controlada por sucesos de {{site.data.keyword.openwhisk_short}}, ofrece varias ventajas para aplicaciones de usuario, en las que las solicitudes HTTP procedentes del navegador del usuario prestan servicio como sucesos. Las aplicaciones {{site.data.keyword.openwhisk_short}} utilizan capacidad de cálculo y solo se facturan cuando están prestando servicio a solicitudes de usuario. No existe la modalidad de desocupado o en espera. Esta característica hace que {{site.data.keyword.openwhisk_short}} resulte mucho más asequible, en comparación con los contenedores tradicionales o con las aplicaciones CloudFoundry. Los dos pueden pasar la mayor parte de su tiempo desocupados, a la espera de que entren solicitudes de usuario y que se facturan por todo el tiempo que están “suspendidas”.

Se pueden crear y ejecutar aplicaciones web completas con {{site.data.keyword.openwhisk_short}}. La combinación de las API sin servidor con el alojamiento de archivos estáticos para recursos del sitio, como por ejemplo HTML, JavaScript y CSS, significa que se pueden crear aplicaciones web completamente sin servidor. La simplicidad de operar con un entorno {{site.data.keyword.openwhisk_short}} alojado es no tener que operar con nada. Ya que {{site.data.keyword.openwhisk_short}} está alojado en {{site.data.keyword.Bluemix_notm}}, supone una gran ventaja en comparación con tener que configurar y trabajar con Node.js Express u otro entorno de ejecución de servidor tradicional.

Consulte los siguientes ejemplos de cómo utilizar {{site.data.keyword.openwhisk_short}} para crear una app web:
- [Acciones web: apps web sin servidor con {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [Crear una aplicación {{site.data.keyword.openwhisk_short}} de usuario con {{site.data.keyword.Bluemix_notm}} y Node.js](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Manejadores HTTP sin servidor con {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #openwhisk_common_use_cases_iot}

De forma inherente, los escenarios de Internet de las cosas se suelen controlar mediante sensores. Por ejemplo, una acción en {{site.data.keyword.openwhisk_short}} se podría desencadenar si fuera necesario reaccionar a un sensor que supera una temperatura concreta. IoT interactions are usually stateless with the potential of high load in major spontaneous events such as natural disasters, significant weather storms, or traffic jams. Se crea la necesidad de disponer de un sistema elástico en el que la carga de trabajo normal puede ser pequeña, pero que se pueda escalar con rapidez con un tiempo de respuesta predecible. Por ello, es deseable contar con la capacidad para gestionar un gran número de sucesos sin aviso previo al sistema. Resulta muy difícil crear un sistema que se ajuste a estos requisitos con las arquitecturas de servidor tradicionales. Ya que tienden a resultar insuficientes e incapaces de gestionar una punta de tráfico o sobredimensionadas y muy caras.

Se pueden implementar aplicaciones IoT que utilizan arquitecturas de servidor tradicional. Sin embargo, en muchos casos la combinación de distintos servicios y puentes de datos requiere conductos flexibles y de alto rendimiento. Abarcan desde dispositivos IoT hasta almacén en la nube y una plataforma de análisis. A menudo los puentes preconfigurados no ofrecen las funciones de programación deseables para implementar una arquitectura para una solución particular y adaptada. Dada la gran variedad de conductos y la falta de estandarización en lo referente a fusión de datos en general (en IoT en particular), es común ver entornos en los que el conducto requiere una transformación de datos personalizada. Estas transformaciones de datos personalizadas se aplican a la conversión de formato, filtrado o amplificación. {{site.data.keyword.openwhisk_short}} constituye una excelente herramienta para implementar dicha transformación, en modalidad ‘sin servidor’, donde la lógica personalizada se aloja en una plataforma de nube elástica y completamente gestionada.

Consulte el siguiente ejemplo de aplicación IoT que utiliza {{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive y otros servicios: [Transformación sin servidor de datos en movimiento IoT con {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c).

![Ejemplo de arquitectura de una solución IoT](images/IoT_solution_architecture_example.png)

## Programa de fondo de API
{: #openwhisk_api_backend}

Las plataformas de cálculo sin servidor ofrecen a los programadores un método rápido de crear API sin servidores. {{site.data.keyword.openwhisk_short}} da soporte a la generación automática de API REST para acciones. La
[característica de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) puede invocar una acción con métodos HTTP que no sean POT y sin la clave de API de autorización de la acción a través de la pasarela de API de
{{site.data.keyword.openwhisk_short}}. Esta característica resulta útil no sólo para exponer las API a consumidores externos, sino también para crear aplicaciones de microservicios.

Además, las acciones de {{site.data.keyword.openwhisk_short}} se pueden conectar a la herramienta de gestión de API que elija (como por ejemplo [IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) y otra). Al igual que en otros casos de uso, se aplican todas las consideraciones sobre escalabilidad y otras Calidades de servicio (QoS).

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting) es una app de ejemplo que utiliza acciones de {{site.data.keyword.openwhisk_short}} a través de una API REST.

Consulte el siguiente ejemplo que incluye una descripción de [utilización de un sistema sin servidor como programa de fondo de API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples).

## Programa de fondo móvil
{: #openwhisk_common_use_cases_mobile}

Muchas aplicaciones de móvil precisan de lógica en el lado del servidor. Sin embargo, los desarrolladores de entornos móviles no suelen tener experiencia en gestión de la lógica de la parte del servidor y prefieren centrarse en la app que se ejecuta en el dispositivo. Este objetivo de desarrollo se consigue fácilmente utilizando {{site.data.keyword.openwhisk_short}} como programa de fondo en la parte del servidor, y es una buena solución. Además, el soporte integrado para Swift del servidor permite a los desarrolladores reutilizar sus habilidades de programación existentes en iOS. Puesto que las aplicaciones móviles suelen tener patrones de carga imprevisibles, es recomendable utilizar una solución {{site.data.keyword.openwhisk_short}} alojada como {{site.data.keyword.Bluemix}}. Esta solución se puede escalar para que se adapte prácticamente a cualquier demanda en la carga de trabajo sin necesidad para suministrar recursos de forma anticipada.

[Skylink](https://github.com/IBM-Bluemix/skylink) es una aplicación de ejemplo que conecta un dron mediante un iPad a IBM Cloud con análisis de imágenes casi en tiempo real que aprovecha {{site.data.keyword.openwhisk_short}}, IBM {{site.data.keyword.cloudant_short_notm}}, IBM Watson y Alchemy Vision.

[BluePic](https://github.com/IBM-Swift/BluePic) es una aplicación de compartición de fotos e imágenes que puede utilizarse para hacer fotos y compartirlas con otros usuarios de BluePic. Esta aplicación muestra cómo aprovechar, en una aplicación iOS 10 para móvil, una aplicación de servidor basada en Kitura que está escrita en Swift que utiliza {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.cos_full_notm}} para datos de imágenes. También se utiliza AlchemyAPI en la secuencia de {{site.data.keyword.openwhisk_short}} para analizar la imagen, extraer códigos de texto en función del contenido de la imagen y, finalmente, enviar una notificación push al usuario.

## Proceso de datos
{: #data-processing}

Con la cantidad de datos ahora disponible, el desarrollo de aplicaciones precisa de la capacidad de procesar nuevos datos, y
potencialmente reaccionar a ellos. Este requisito incluye el proceso tanto de registros de base de datos estructurado, como de documentos no estructurados, imágenes o vídeos. {{site.data.keyword.openwhisk_short}} se puede configurar mediante canales de información proporcionados por el sistema o personalizados para que reaccione ante cambios en los datos y ejecute automáticamente acciones en los canales de información de datos de entrada. Se pueden programar acciones para procesar cambios, transformar formatos de datos, enviar y recibir mensajes, invocar otras acciones y actualizar diversos almacenes de datos. Los almacenes de datos soportados incluyen bases de datos relacionales basadas en SQL, tablas de datos en memoria, base de datos NoSQL, archivos, intermediarios de mensajería y otros sistemas diversos. Las reglas y secuencias de {{site.data.keyword.openwhisk_short}} ofrecen la flexibilidad de realizar cambios en el conducto de proceso sin programación, y se realizan a través de simples actualizaciones de configuración. Las opciones de almacén de datos y la poca sobrecarga administrativa hacen que el sistema basado en {{site.data.keyword.openwhisk_short}} resulte muy ágil y se pueda adaptar fácilmente a los cambios de requisitos.

El proyecto [OpenChecks](https://github.com/krook/openchecks) constituye una prueba de concepto que muestra cómo se puede utilizar {{site.data.keyword.openwhisk_short}} para procesar el depósito de cheques en una cuenta bancaria utilizando el reconocimiento óptico de caracteres. Se basa en el servicio {{site.data.keyword.openwhisk_short}} de {{site.data.keyword.Bluemix_notm}} y utiliza {{site.data.keyword.cloudant}} y {{site.data.keyword.cos_full_notm}}. En un entorno local, puede utilizar CouchDB y OpenStack Swift. Otros servicios de almacenamiento podrían incluir FileNet o Cleversafe. Tesseract proporciona la biblioteca OCR.

## Aplicación cognitiva
{: #openwhisk_common_use_cases_cognitive}

Las tecnologías cognitivas pueden resultar eficientes combinadas con {{site.data.keyword.openwhisk_short}} para crear aplicaciones potentes. Por ejemplo, se puede utilizar la API IBM Alchemy y Watson Visual Recognition con {{site.data.keyword.openwhisk_short}} para extraer automáticamente información útil de vídeos sin tener que visualizarlos. Esta tecnología es una extensión “cognitiva” del caso de uso de [proceso de datos](#data-processing) que se ha descrito anteriormente. {{site.data.keyword.openwhisk_short}} también se ajusta perfectamente a la implementación de una función Bot combinada con servicios cognitivos.

Se proporciona una aplicación de ejemplo, [Dark Vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp), que hace exactamente esto. En esta aplicación, el usuario carga un vídeo o una imagen utilizando la aplicación web Dark Vision, que lo almacena en una base de datos {{site.data.keyword.cloudant_short_notm}}. Una vez cargado el vídeo, {{site.data.keyword.openwhisk_short}} detecta el nuevo vídeo escuchando los cambios de {{site.data.keyword.cloudant_short_notm}} (desencadenante). Luego {{site.data.keyword.openwhisk_short}} desencadena la acción de extractor de vídeo. Durante su ejecución, el extractor genera fotogramas (imágenes) y las guarda en {{site.data.keyword.cloudant_short_notm}}. A continuación los fotogramas se procesan con Watson Visual Recognition y los resultados se guardan en la misma base de datos {{site.data.keyword.cloudant_short_notm}}. Los resultados se pueden visualizar con la aplicación web Dark Vision o bien con una aplicación iOS. Se puede utilizar {{site.data.keyword.cos_full_notm}} además de {{site.data.keyword.cloudant_short_notm}}, en cuyo caso, los metadatos de vídeo y de imágenes se guardan en {{site.data.keyword.cloudant_short_notm}} y los archivos de contenido de medios se guardan en {{site.data.keyword.cos_short}}.

Hay disponible una [aplicación iOS Swift de ejemplo](https://github.com/gconan/BluemixMobileServicesDemoApp) que utiliza {{site.data.keyword.openwhisk_short}}, IBM Mobile Analytics y Watson para analizar el tono y publicar en un canal de Slack.

## Proceso de sucesos con Kafka o {{site.data.keyword.messagehub}}
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}} resulta ideal para utilizar con Kafka, con {{site.data.keyword.messagehub_full}} (basado en Kafka) y con otros sistemas de mensajería. La naturaleza controlada por sucesos de estos sistemas requiere que el tiempo de ejecución controlado por sucesos procese los mensajes. El tiempo de ejecución puede aplicar la lógica empresarial a dichos mensajes, que es exactamente lo que proporciona {{site.data.keyword.openwhisk_short}} con sus canales de información, desencadenantes y acciones. Se suelen utilizar Kafka y {{site.data.keyword.messagehub}} para grandes volúmenes imprevisibles de cargas de trabajo y que requieren que los consumidores de estos mensajes se ajusten de inmediato. De nuevo, esta es una situación ideal para {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} ofrece la capacidad integrada de consumir mensajes, así como de publicar los mensajes proporcionados en el paquete [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka).

Se proporciona una [aplicación de ejemplo que implementa un caso de ejemplo de proceso de sucesos](https://github.com/IBM/openwhisk-data-processing-message-hub) con {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}} y Kafka.
