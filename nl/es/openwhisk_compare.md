---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: functions compared, openwhisk, architecture, limitless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Comparación de Función como servicio
{: #openwhisk_faas_compared}

{{site.data.keyword.openwhisk}} proporciona OpenWhisk en un entorno sin servidor altamente escalable. Puede comparar la arquitectura sin servidor de {{site.data.keyword.openwhisk_short}} y el cálculo rentable con otros modelos de arquitectura.
{: shortdesc}

## Comparación entre arquitecturas de OpenWhisk
{: #architecture_comparison}

Se comparan las siguientes arquitecturas de OpenWhisk:

1. **Función como servicio (FaaS)** en [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk). IBM es el único proveedor que ofrece OpenWhisk gestionado. Encontrará una buena introducción al modelo de programación sin servidor mediante una plataforma FaaS en el [blog de Martin Fowler](https://martinfowler.com/articles/serverless.html), donde puede ver [casos de uso](/docs/openwhisk?topic=cloud-functions-openwhisk_common_use_cases) para ejecutar OpenWhisk con un diseño sin servidor.

2. **Infraestructura como servicio (IaaS)** con OpenWhisk Roll Your Own (RYO). Puede descargar OpenWhisk desde Apache Incubation Project y ejecutarlo en [{{site.data.keyword.Bluemix_notm}} IaaS](https://cloud.ibm.com/catalog/?category=devices).

3. **Plataforma como servicio (PaaS)** como tiempo de ejecución de aplicaciones gestionado. Un buen ejemplo es el tiempo de ejecución [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java) que está gestionado por la implementación de {{site.data.keyword.Bluemix_notm}} Foundry.

4. **Contenedor como servicio (CaaS)** como entorno de contenedor gestionado. Un buen ejemplo es [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-container_index#container_index).

5. **Infraestructura como servicio (IaaS)** con tiempo de ejecución Java EE. Un buen ejemplo es [WebSphere Application Server VM on {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server).

En la tabla siguiente se comparan los elementos de cada arquitectura desde la perspectiva de un desarrollador que está creando y utilizando aplicaciones:


| Tema | (1) FaaS en {{site.data.keyword.openwhisk_short}} | (2) IaaS con OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS con Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unidad de aplicación	|	Una sola función (generalmente un pequeño bloque de código en JavaScript, Swift o contenedor Docker) - puede tener menos de un Kb, pero puede ser mayor. Generalmente no más de unos cuantos Kb.	|	Igual que columna (1)	|	Depende del tiempo de ejecución utilizado. Un archivo EAR o WAR, o un paquete de aplicación específico de otro lenguaje, generalmente de tamaño relativamente grande - Kb o incluso Mb con muchos servicios en un paquete, pero puede ser tan pequeño como un solo servicio.	|	Contenedor Docker es la unidad de despliegue.	|	VM con App Server con archivo EAR o WAR y otras dependencias - generalmente en unidades de Gb.	|
|	Tamaño de recurso	|	El usuario final no paga ni se preocupa de la memoria, CPU u otros recursos. Aunque la acción tenga un cierto tamaño, el usuario no se tiene que preocupar de eso	|	Alto. El usuario final debe suministrar un entorno IaaS y después instalar y configurar OpenWhisk sobre el mismo	|	Pequeño. El usuario final paga por la memoria y CPU necesarias para ejecutar las apps, pero no paga por las apps que no se ejecutan	|	Pequeño a mediano	|	Alto. El usuario final tiene que pagar por almacenamiento de disco, memoria, CPU y posiblemente otros componentes cuando la app se ejecuta. Cuando está detenida, solo se incurren en costes de almacenamiento	|
|	Instalación y configuración	|	No es necesario	|	Alto - todo lo hace el usuario final	|	No es necesario	|	Moderado - herramientas de gestión de hardware, red, SO y contenedor proporcionadas por el proveedor de CaaSs, imágenes, conectividad e instancias por el usuario final	|	Alto - instalación de hardware, red, SO y Java EE inicial proporcionada por el proveedor, configuración adicional, clúster y escalado por el usuario final	|
|	Tiempo de suministro	|	Milisegundos	|	Vea las columnas (4) y (5)	|	Minutos	|	Minutos	|	Horas	|
|	Administración continua	|	Ninguna	|	Alta	|	Ninguna	|	Moderada	|	Alta	|
|	Escalado elástico	|	Cada acción se escala de forma instantánea y de modo inherente en función de la carga. No es necesario suministrar las VM ni otros recursos por adelantado	|	No se proporciona - el usuario final debe proporcionar capacidad de cálculo en IaaS y gestionar el escalado de las VM. Una vez escaladas las VM, OpenWhisk escala las acciones automáticamente, pero los recursos se deben suministrar por adelantado	|	Automático, pero escalado lento. Durante una mayor carga, las acciones de escala pueden tardar varios minutos en completarse. El escalado automático requiere un ajuste cuidadoso	|	Automático, pero escalado lento. Durante una mayor carga, las acciones de escala pueden tardar varios minutos en completarse. El escalado automático requiere un ajuste cuidadoso	|	No se proporciona	|
|	Planificación de la capacidad	|	No es necesaria. FaaS proporciona automáticamente la capacidad necesaria	|	Es necesario suministrar suficiente capacidad de antemano o en un script	|	Se necesita cierta planificación de la capacidad, pero se proporciona cierto aumento automático de la capacidad	|	Se necesita cierta planificación de la capacidad, pero se proporciona cierto aumento automático de la capacidad	|	Se necesita suministrar suficiente capacidad de forma estática para gestionar puntas de carga de trabajo	|
|	Conexiones permanentes y estado	|	Limitado - no se puede mantener una conexión permanente, excepto en casos de almacenamiento de contenedor en memoria caché. Generalmente el estado se debe mantener en un recurso externo	|	Igual que columna (1)	|	Soportado - se puede mantener una conexión o socket abierto durante mucho tiempo, se puede guardar el estado en memoria entre llamadas	|	Soportado - se puede mantener una conexión o socket abierto durante mucho tiempo, se puede guardar el estado en memoria entre llamadas	|	Soportado - se puede mantener una conexión o socket abierto durante mucho tiempo, se puede guardar el estado en memoria entre llamadas	|
|	Mantenimiento	|	Ninguno, IBM gestiona toda la pila.	|	Significativo - depende del entorno de destino, el usuario debe suministrar hardware, red, SO, almacenamiento, BD, instalación y mantenimiento de OpenWhisk, etc.	|	Ninguno, el proveedor gestiona toda la pila.	|	Significativo - el usuario debe crear y mantener imágenes personalizadas, desplegar y gestionar contenedores, conexiones entre contenedores, etc.	|	Significativo - el usuario debe asignar las VM, gestionar y escalar servidores Java EE individualmente.	|
|	Alta disponibilidad (HA) y Recuperación tras desastre (DR)	|	Inherente / sin costes adicionales	|	Roll Your Own (RYO) 	|	Disponible con cargo adicional	|	Los contenedores anómalos se pueden reiniciar automáticamente	|	Disponible con cargo adicional, semiautomático. Las VM se pueden migrar tras error automáticamente	|
|	Seguridad	|	Proporcionada por el proveedor	|	Roll Your Own (RYO)	|	Combinación de RYO y proporcionado por el proveedor	|	Combinación de RYO y proporcionado por el proveedor	|	Roll Your Own (RYO)	|
|	Velocidad de desarrollador	|	Máxima	|	Máxima	|	Máxima	|	Promedia	|	Lenta	|
|	Utilización de recursos (recursos desocupados por los que aún se tiene que pagar)	|	Los recursos nunca están desocupados, ya que solo se invocan a petición. Cuando falta la carga de trabajo, no se produce ninguna asignación de recursos ni coste.	|	Como esta opción utiliza IaaS o CaaS, se aplican consideraciones similares a las de las columnas (4) y (5)	|	Algunos recursos pueden estar desocupados, y el escalado automático ayuda a eliminar los recursos desocupados. Siempre tiene que haber un determinado número de instancias en ejecución y es probable que se utilicen a menos del 50 % de su capacidad. Las instancias detenidas no cuestan nada	|	Parecido a la columna (3)	|	Algunos recursos pueden estar desocupados, pero no se da soporte al escalado automático. Siempre tiene que haber un determinado número de instancias en ejecución y es probable que se utilicen a menos del 50 % de su capacidad. Las instancias detenidas pueden incurrir en el coste de almacenamiento	|
|	Madurez	|	Madurez temprana	|	Madurez temprana	|	Madurez temprana	|	Madurez moderada	|	Madurez elevada	|
|	Límites de recursos	|	[Existen ciertos límites](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)	|	Depende de los recursos asignados	|	No	|	No	|	No	|
|	Latencia para servicios que se utilizan poco	|	Es posible que las solicitudes poco frecuentes muestren un tiempo de respuesta de varios segundos, pero se mantiene en un rango de milisegundos para las siguientes solicitudes	|	Depende	|	Bajo	|	Bajo	|	Bajo - suponiendo que el sistema tenga suficientes recursos	|
|	Tipo de aplicación ideal	|	Proceso de sucesos, IoT, proceso de fondo móvil, microservicios. Definitivamente no apto para aplicaciones monolíticas. Consulte los [casos de uso](/docs/openwhisk?topic=cloud-functions-openwhisk_common_use_cases)	|	Igual que la columna (1), pero cuando el usuario desea trabajar con una nube que no es de IBM o de forma local.	|	Aplicaciones web con una carga de trabajo de tipo 24x7, servicios que necesitan mantener la conexión abierta durante largos periodos de tiempo. Se puede utilizar para ejecutar microservicios o aplicaciones monolíticas	|	Ideal para aplicaciones de microservicios.	|	Aplicaciones empresariales tradicionales que se migran de local a la nube. Ideal para aplicaciones monolíticas	|
|	Granularidad de los cargos y facturación	|	[Por bloques de 100 milisegundos](https://cloud.ibm.com/openwhisk/learn/pricing)	|	Depende de la implementación - si se utiliza IaaS o CaaS, se aplican consideraciones similares - Consulte las columnas (4) y (5)	|	Normalmente se factura por hora (raramente por minuto) por el paquete de recursos (CPU + memoria + cierto espacio de disco)	|	Parecido a la columna (3)	|	Parecido a la columna (3)	|
|	Coste total de propiedad (TCO)	|	Para alcanzar el punto ideal, es probable que el coste de las aplicaciones sea una orden de magnitud menor que el de las alternativas. Puesto que los recursos se escalan automáticamente, nunca se produce un exceso de suministro.	|	Para despliegues en la nube, es probable que resulte más caro que OpenWhisk FaaS, pero un despliegue local puede resultar más económico que las arquitecturas tradicionales	|	Relativamente bajo - el usuario no tiene que suministrar ni gestionar recursos, y se puede centrar en el desarrollo de la aplicación. Cierto nivel de exceso de suministro en comparación con la opción sin servidor	|	Moderado - el usuario tiene que suministrar y gestionar los contenedores y la aplicación, y se podría producir un nivel de exceso de suministro en comparación con la opción sin servidor y PaaS	|	Relativamente alto - teniendo en cuenta que la migración de aplicaciones antiguas al modelo nativo de nube puede resultar muy caro, esta puede ser una opción más viable y económica para este tipo de apps.	|

## Consideraciones sobre el coste
{: #cost_considerations}

La infraestructura para sus entornos de pruebas, transferencia, prueba de carga y otros puede resultar costosa. Se necesita tiempo para configurarlos, y, como suelen funcionar las 24 horas del día, a menudo se infrautilizan y consumen gran cantidad de capacidad. Mediante una arquitectura sin servidor, los costes de los entornos se generan en función de la carga, no del número de entornos definidos.
{: shortdesc}

Para estimar los costes de una aplicación sin servidor, puede utilizar la [calculadora de precios ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/learn/pricing).

### Capacidad sin límite
{: #limitless_capacity}

En las arquitecturas tradicionales, cada servicio consume la cantidad de capacidad que tiene asignada, y se le factura por el consumo de capacidad. La arquitectura sin servidor de {{site.data.keyword.openwhisk_short}} reduce la restricción sobre la granularidad de la arquitectura de microservicios.

Cuando no se utiliza, {{site.data.keyword.openwhisk_short}} no cuesta nada. El código se ejecuta cuando hay una llamada HTTP, un cambio de estado de base de datos u otro tipo de suceso que active la ejecución del código. Se le facturará por milisegundo de tiempo de ejecución, redondeado al alza hasta la siguiente unidad de 100 ms, no por hora de utilización de VM independientemente de si dicha VM ha realizado o no trabajo útil. Puesto que solo paga cuando se consumen sucesos, y no según el número de entornos, puede desglosar la app en 100, 1000 o incluso más microservicios.

### Ejecute acciones en cualquier región
{: #actions_region}

En las arquitecturas tradicionales, el código se debe ejecutar en cada región para que se ejecute allí, y también se debe pagar la infraestructura correspondiente a dicha región. Con {{site.data.keyword.openwhisk_short}}, las acciones se pueden desplegar y poner a disposición de los usuarios para que se ejecutan en cualquier región sin ningún coste adicional. Puede aumentar la disponibilidad y la capacidad de recuperación de su código sin las restricciones de coste tradicionales.

### Redundancia por diseño
{: #redundancy_design}

En las arquitecturas tradicionales, las apps deben ser redundantes. Con {{site.data.keyword.openwhisk_short}}, no es necesario que los procesos tengan alta disponibilidad (HA) porque, por diseño, las apps sin servidor no tienen estado y se controlan mediante sucesos de solicitud. Al eliminar la necesidad de crear redundancia de forma explícita, la naturaleza sin estado de las apps sin servidor puede reducir significativamente los costes de la infraestructura.
