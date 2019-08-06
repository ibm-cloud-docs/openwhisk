---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# Establecimiento de políticas de acceso
{: #iam}

## Correlación de roles de IAM con {{site.data.keyword.openwhisk_short}}
{: #user-roles}

En {{site.data.keyword.openwhisk_short}}, los espacios de nombres son recursos de {{site.data.keyword.cloud_notm}} que puede utilizar para trabajar con los roles y políticas de IAM para la gestión de accesos. Todas las políticas que defina para un espacio de nombres se aplicarán también a las entidades de {{site.data.keyword.openwhisk_short}}, como acciones o desencadenantes, que contiene el espacio de nombres.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utiliza los roles de gestión de plataformas y servicios. Puede establecer políticas sobre quién puede crear espacios de nombres a nivel de la plataforma, y utilizar roles de servicio para gestionar la interacción con los propios espacios de nombres.

¿Desea obtener más información sobre los conceptos clave de IAM? Consulte [los documentos de IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Roles de gestión de plataformas

En la tabla siguiente se detallan las acciones que se correlacionan con los roles de gestión de plataformas. Los roles de gestión de plataformas permiten a los usuarios realizar tareas en los recursos de servicio a nivel de plataforma. Por ejemplo, asignar acceso de usuario al servicio, crear o suprimir ID de servicio, crear instancias y enlazar instancias con aplicaciones.
{: shortdesc}

Para obtener más información sobre cómo asignar, editar, revisar o suprimir políticas de acceso a recursos, consulte
[Gestión del acceso de IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}

<table>
  <thead>
    <tr>
      <th>Rol de plataforma</th>
      <th>Descripción</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Administrador</td>
      <td>Los usuarios pueden crear espacios de nombres. Se requiere el rol de administrador para realizar las operaciones `service id` y `apikey lock` durante la creación del servicio.</td>
    </tr>
  </tbody>
</table>

Dado que no es necesario suministrar el servicio, el rol de editor es el único rol de plataforma que necesita para trabajar con el servicio. Para obtener más información sobre los demás roles, consulte [Roles de gestión de plataformas](/docs/iam?topic=iam-userroles).

</br>

### Roles específicos de servicio
{: #service_specific_roles}

Los roles específicos de servicio determinan el ámbito de una política de acceso dentro de un servicio específico. Para
{{site.data.keyword.openwhisk_short}}, los roles se pueden aplicar a una capacidad del usuario para utilizar el servicio, como el acceso a la interfaz de usuario o la realización de llamadas de API.
{: shortdesc}

Permisos incrementales. Por ejemplo, cualquier operación que sea capaz de realizar el rol de
`escritor`, el rol de `gestor` también podrá realizarla. No obstante, el rol de `gestor` tendrá más permisos añadidos. Para ver los permisos generales de cada rol, consulte [Roles de acceso al servicio](/docs/iam?topic=iam-userroles).

Para ver qué roles se necesitan para realizar cada operación, consulte la tabla siguiente:

<table><caption>¿Qué roles pueden realizar cada operación?</caption>
  <tr>
    <th style="width:150px">Acción</th>
    <th style="width:2500px">Descripción</th>
    <th style="width:50px">Lector</th>
    <th style="width:50px">Escritor</th>
    <th style="width:50px">Gestor</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Actualizar un espacio de nombres.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Suprimir un espacio de nombres.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Ver los espacios de nombres.</td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Crear una entidad, como una acción, dentro de un espacio de nombres de Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Actualizar una entidad, como un paquete, dentro de un espacio de nombres de Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Suprimir una entidad, como un desencadenante, de un espacio de nombres de Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Ver las entidades disponibles, como las reglas, dentro de un espacio de nombres.</td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Activar una entidad, como una acción, dentro de un espacio de nombres.</td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Característica disponible" style="width:32px;" /></td>
  </tr>
</table>

</br>

### Establecimiento de políticas a través de la CLI
{: #cli-set}

Para proporcionar un recurso, por ejemplo una acción, en un acceso de espacio de nombres basado en IAM a un servicio basado en IAM, puede crear una política de acceso de IAM para el espacio de nombres en el que se encuentra el recurso.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Visión general de los componentes del mandato <code>ibmcloud iam service-policy-create</code></th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>El ID de servicio del espacio de nombres. Para ver todos los ID de servicio, ejecute <code>ibmcloud iam service-ids</code>.</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>El tipo de rol de acceso al servicio de IAM que debe tener la acción para utilizar el servicio de destino. Para ver los roles soportados para el otro servicio, ejecute <code>ibmcloud iam roles --service SERVICE_NAME</code>. Para obtener más información, consulte [Roles de acceso de IAM](/docs/iam?topic=iam-userroles#service-access-roles).</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>El nombre del otro tipo de servicio de {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>El GUID de la otra instancia de servicio a la que desea que tenga acceso la acción. Para obtener el GUID de la instancia de servicio, ejecute <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

</br>

**Pasos siguientes**
Para obtener más información sobre la gestión de las credenciales de servicio, consulte el blog [Gestión de credenciales de servicio para aplicación sin servidor](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.



