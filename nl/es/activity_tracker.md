---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Visualización de sucesos de {{site.data.keyword.cloudaccesstrailshort}}
{: #activity_tracker}

Puede ver, gestionar y auditar las actividades iniciadas por el usuario que se han realizado en la instancia del servicio {{site.data.keyword.openwhisk}} mediante el servicio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}


Para obtener más información sobre cómo funciona el servicio, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## Dónde ver los sucesos
{: #view}

Los sucesos están disponibles en el **dominio de cuenta** de {{site.data.keyword.cloudaccesstrailshort}} que está disponible en la región de {{site.data.keyword.Bluemix_notm}} donde está disponible el recurso del espacio de nombres de {{site.data.keyword.openwhisk_short}}. Para obtener más información, consulte [Visualización de sucesos de cuenta](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events).

1. Inicie sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}.
2. Desde el catálogo, suministre una instancia del servicio {{site.data.keyword.cloudaccesstrailshort}} en la misma cuenta que la instancia de {{site.data.keyword.openwhisk}}.
3. En el separador **Gestión** del panel de control de {{site.data.keyword.cloudaccesstrailshort}}, pulse **Ver en Kibana**.
4. Establezca el intervalo de tiempo para el que desea ver los registros. El valor predeterminado es 15 minutos.
5. En la lista **Campos disponibles**, pulse **Tipo**. Pulse el icono de lupa de **Activity Tracker** para limitar los registros a aquellos que el servicio rastrea.
6. Puede utilizar los otros campos disponibles para limitar la búsqueda.

Para que los usuarios que no sean el propietario de la cuenta puedan ver los registros, debe utilizar el plan premium. Para permitir que otros usuarios vean sucesos, consulte [Concesión de permisos para ver sucesos de cuentas](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## Lista de sucesos
{: #events}

Consulte la tabla siguiente para ver una lista de los sucesos que se envían a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Acción</th>
      <th>Descripción</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Crear un recurso de espacio de nombres basado en IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migrar un espacio de nombres basado en Cloud Foundry para que pase a estar habilitado para IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Actualizar propiedades de recursos de un espacio de nombres basado en IAM, como el nombre de visualización o la descripción</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Suprimir un recurso de espacio de nombres</td>
    </tr>
  </tbody>
</table>
