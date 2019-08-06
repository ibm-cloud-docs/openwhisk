---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolución de problemas
{: #troubleshooting}

Si tiene problemas mientras trabaja con {{site.data.keyword.openwhisk}}, considere estas técnicas para resolverlos y obtener ayuda.
{: shortdesc}



## La acción está fallando
{: #ts_action_fails}

{: tsSymptoms}
La acción está fallando.

{: tsCauses}
Una explicación para una app que falla es que se está utilizando un entorno de ejecución en desuso. Una acción no se puede completar satisfactoriamente hasta que el entorno de ejecución esté actualizado a una versión soportada.

{: tsResolve}
Para ver qué entorno de ejecución se utiliza para la acción, ejecute `ibmcloud fn action get ACTION_NAME` y compruebe el valor de `deprecated=true` en la respuesta de la consulta. Si el entorno de ejecución está en desuso, [actualice el entorno de ejecución](/docs/openwhisk?topic=cloud-functions-actions#actions_update).



## Se han alcanzado los límites del sistema
{: #ts_limit_reached}

{: tsSymptoms}
Su función no se está ejecutando correctamente y podría dar como resultado un mensaje como `{"error":"signal: killed"}`.

{: tsCauses}
Todas las funciones tienen unos [límites de sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) mínimos y máximos, como por ejemplo el máximo uso de memoria o un tiempo de espera.

{: tsResolve}
Algunos límites se pueden aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Para aumentar el valor límite, póngase en contacto con el soporte de IBM abriendo una incidencia directamente desde la consola web de IBM Cloud Functions.

1. Seleccione **Support** (soporte).
2. Seleccione **Add Ticket** en el menú desplegable.
3. Seleccione **Technical** para el tipo de incidencia.
4. Seleccione **Functions** para el área técnica de soporte.



## Obtención de ayuda y soporte
{: #gettinghelp}

¿Todavía tiene problemas con su función?
{: shortdesc}

-   Para ver si {{site.data.keyword.cloud_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/status?selected=status){: external}.
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utilice los foros para hacer una pregunta, etiquete la pregunta para que la vean los equipos de desarrollo de
{{site.data.keyword.cloud_notm}}.
    -   Si tiene preguntas técnicas sobre el desarrollo de funciones con {{site.data.keyword.openwhisk}}, publique su pregunta en [Stack Overflow ](https://stackoverflow.com/search?q=ibm-cloud-functions){: external} y etiquete la pregunta como `ibm-cloud-functions`.
    -   Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM Developer Answers ](https://developer.ibm.com/answers/topics/functions){: external}. Incluya las etiquetas `ibm-cloud` y `functions`.
    Consulte [Obtener ayuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obtener más información sobre el uso de los foros.
-   Póngase en contacto con el soporte de IBM abriendo un caso. Para obtener información sobre cómo abrir un caso de soporte de IBM, o sobre los niveles de soporte y la gravedad de los casos, consulte [Cómo contactar con el servicio de soporte](/docs/get-support?topic=get-support-getting-customer-support).
Cuando informe de un problema, incluya el ID de activación. Para obtener un ID de activación, ejecute `ibmcloud fn activation list`.
{: tip}

