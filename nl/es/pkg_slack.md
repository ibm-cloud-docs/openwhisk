---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: slack package, token-based, api, functions

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


# Slack
{: #pkg_slack}

El paquete preinstalado `/whisk.system/slack` ofrece una forma cómoda de utilizar las [API de Slack](https://api.slack.com/){: external}.
{: shortdesc}

El paquete incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Paquete | `url`, `channel`, `username` | Interactuar con la API de Slack. |
| `/whisk.system/slack/post` | Acción | `text`, `url`, `channel`, `username` | Publica un mensaje en un canal de Slack. |

Se recomienda la creación de un enlace de paquete con los valores de `username`, `url` y `channel`. Con enlace, no necesita especificar los valores cada vez que invoca la acción en el paquete.

## Publicación de un mensaje en un canal de Slack

La acción `/whisk.system/slack/post` publica un mensaje en un canal de Slack especificado. Se da soporte a los parámetros siguientes.

| Parámetro | Descripción |
| --- | --- |
| `url` | El URL de webhook de Slack. |
| `channel` | El canal de Slack en el que publicar el mensaje. |
| `username` | El nombre con el que publicar el mensaje. |
| `text` | Un mensaje a publicar. |
| `token` | (Opcional) Una [señal de acceso](https://api.slack.com/tokens){: external} de Slack. |

El ejemplo siguiente muestra cómo configurar Slack, crear un enlace de paquete y publicar un mensaje en un canal.

1. Configurar un [webhook de entrada](https://api.slack.com/incoming-webhooks){: external} de Slack para su equipo.

  Tras configurar Slack, obtendrá un URL de webhook parecido a
`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. El webhook es necesario en el paso siguiente.

2. Crear un enlace de paquete con sus credenciales de Slack, el canal en el que quiera publicar y el nombre de usuario con el que publicar.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Invocar la acción `post` en su enlace de paquete para publicar un mensaje en su canal Slack.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Uso de la API basada en señales de Slack

Si lo prefiere, puede optar por utilizar la API basada en señales de Slack, en lugar de la API de webhook. Si elige esta opción, pase un parámetro de `señal` con la [señal de acceso](https://api.slack.com/tokens){: external} de Slack correspondiente. A continuación, puede utilizar cualquiera de los [métodos de API de Slack](https://api.slack.com/methods){: external} como parámetro `url`. Por ejemplo, para publicar un mensaje, utilizaría un valor de parámetro `url` de [<ph class="ignoreSpelling">slack.postMessage</ph>](https://api.slack.com/methods/chat.postMessage){: external}.



