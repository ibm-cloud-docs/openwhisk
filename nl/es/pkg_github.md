---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: github, actions, trigger, event

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

# GitHub
{: #pkg_github}

Este paquete preinstalado no está disponible en la región de Tokio.
{: tip}

El paquete `/whisk.system/github` ofrece una forma cómoda de utilizar las [API de GitHub](https://developer.github.com/).
{: shortdesc}

El paquete incluye el canal de información siguiente:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/github` | paquete | username, repository, accessToken | Interactuar con la API de GitHub |
| `/whisk.system/github/webhook` | canal de información | events, username, repository, accessToken | Activar sucesos desencadenantes en caso de actividad de GitHub |

Se recomienda la creación de un enlace de paquete con los valores de `username`, `repository` y `accessToken`.  Con enlace, no necesita especificar los valores cada vez que use el canal de información en el paquete.

## Activación de un suceso desencadenante con actividad GitHub

El canal de información `/whisk.system/github/webhook` configura un servicio para activar un desencadenante cuando haya actividad en el repositorio de GitHub especificado. Los parámetros son según se indica a continuación:

- `username`: el nombre de usuario del repositorio GitHub.
- `repository`: el repositorio GitHub.
- `accessToken`: su señal de acceso personal de GitHub. Cuando [cree su señal](https://github.com/settings/tokens), asegúrese de seleccionar los ámbitos **repo:status** y **public_repo**. Además, asegúrese de que no tiene webhooks que ya estén definidos en su repositorio.
- `events`: el [tipo de suceso GitHub](https://developer.github.com/v3/activity/events/types/) de interés.

En el siguiente ejemplo, se crea un desencadenante que se activa cada vez que se realiza una nueva confirmación en un repositorio de GitHub.

1. Generar una [señal de acceso personal](https://github.com/settings/tokens) de GitHub. La señal de acceso se usará en el paso siguiente.

2. Crear un enlace de paquete configurado para su repositorio de GitHub y con su señal de acceso.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Crear un desencadenante para el tipo de suceso `push` de GitHub usando su canal de información `myGit/webhook`.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  Una confirmación en el repositorio GitHub utilizando `git push` provoca que el webhook active el desencadenante. Si una regla coincide con el desencadenante, se invoca la acción asociada. La acción recibe la carga útil de webhook de GitHub como parámetro de entrada. Cada suceso de webhook de GitHub tiene un esquema JSON similar, pero es un objeto de carga útil exclusivo determinado por su tipo de suceso. Para obtener más información sobre el contenido de la carga útil, consulte la documentación de la API de
[Carga útil y sucesos GitHub](https://developer.github.com/v3/activity/events/types/).

