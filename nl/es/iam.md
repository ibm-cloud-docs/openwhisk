---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# Gestión del acceso
{: #iam}

{{site.data.keyword.openwhisk}} admite Identity and Access Management (IAM). Ahora puede definir políticas de IAM para sus recursos como espacios de nombres.
{: shortdesc}

Las políticas de IAM solo están disponibles en la región de Tokio para {{site.data.keyword.openwhisk_short}}. Si trabaja en la región de Tokio, debe utilizar políticas de IAM para controlar el acceso.
{: tip}

</br>

## Correlación de roles de IAM con {{site.data.keyword.openwhisk_short}}
{: #user-roles}

En {{site.data.keyword.openwhisk_short}}, los espacios de nombres se consideran un recurso de IBM Cloud que le permite trabajar con políticas y roles de IAM para la gestión del acceso. Todas las políticas que defina para un espacio de nombres se aplicarán también a las entidades de {{site.data.keyword.openwhisk_short}}, como acciones o desencadenantes, que contiene el espacio de nombres.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utiliza los roles de gestión de plataformas y servicios. Puede establecer políticas sobre quién puede crear espacios de nombres a nivel de la plataforma, mientras utiliza roles de servicio para gestionar la interacción con los propios espacios de nombres.

¿Desea obtener más información sobre los conceptos clave de IAM? Consulte [los documentos de IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Roles de gestión de plataformas

En la tabla siguiente se detallan las acciones que se correlacionan con los roles de gestión de plataformas. Los roles de gestión de plataformas permiten a los usuarios realizar tareas en los recursos de servicio a nivel de plataforma. Por ejemplo, asignar acceso de usuario al servicio, crear o suprimir ID de servicio, crear instancias y enlazar instancias con aplicaciones.
{: shortdesc}

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
      <td>Los usuarios pueden crear espacios de nombres.</td>
    </tr>
  </tbody>
</table>

Debe tener el rol de administrador para la gestión de la plataforma para poder trabajar con el servicio. Para obtener más información sobre los roles, consulte [Roles de gestión de plataformas](/docs/iam?topic=iam-userroles).

</br>

### Roles específicos del servicio

Los roles específicos del servicio determinan el ámbito de una política de acceso dentro de un servicio específico. Para
{{site.data.keyword.openwhisk_short}}, los roles se pueden aplicar a una capacidad del usuario para utilizar el servicio, como el acceso a la interfaz de usuario o la realización de llamadas de API.
{: shortdesc}


Es importante tener en cuenta que los permisos se crean unos sobre otros. Por ejemplo, cualquier operación que sea capaz de realizar el rol de
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

Para obtener información sobre la asignación de roles de usuario en la interfaz de usuario, consulte [Gestión del acceso de IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Establecimiento de políticas de acceso de IAM
{: #set-iam}

Cuando un servicio invoca una acción, la acción tiene una respuesta. Debido a que la respuesta se envía desde el espacio de nombres o la acción a un servicio, se considera información de salida. Si desea limitar el nivel de influencia que el espacio de nombres tiene sobre otros servicios, es posible que quiera crear una política de acceso.
{: shortdesc}

Para obtener información sobre cómo asignar, editar, revisar o suprimir políticas de acceso a recursos, consulte
[Gestión del acceso de IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Acceso a otros recursos desde un espacio de nombres
{: #namespace-access}

Puede acceder a otros recursos desde un espacio de nombres gestionado por IAM utilizando una señal de IAM. Una señal representa la autenticación y verifica la identidad del recurso. La señal de IAM es necesaria para la autenticación al acceder a servicios o recursos gestionados por IAM.
{: shortdesc}

De forma similar a cómo un ID de usuario identifica a un usuario, un ID de servicio representa un recurso específico. Esto implica que las políticas de IAM se pueden aplicar a los recursos que gestionen permisos de acceso. Al igual que un usuario, un recurso debe autenticarse para verificar su identidad. Dentro de Functions, esto lo puede aprovechar la implementación de las acciones al acceder a otros servicios o recursos.

Al crear un nuevo espacio de nombres gestionado por IAM, Functions crea automáticamente un ID de servicio correspondiente que identifica el espacio de nombres y una clave de API. En tiempo de ejecución, Cloud Functions pasa la clave de API al código de acción como el valor de la variable de entorno `__OW_IAM_NAMESPACE_API_KEY`. El código de acción puede utilizar esta clave de API para generar una señal de IAM. La mayoría de los SDK soportados, como Cloudant, Watson y COS, se autentican con la propia clave de IAM. Otros servicios o recursos gestionados por IAM que utilizan una API REST, se autentican con la señal derivada de la clave de IAM.

¿No está seguro de cómo se relacionan las claves de API y las señales? Obtenga más información en
[los documentos de IAM](/docs/iam?topic=iam-iamapikeysforservices).
