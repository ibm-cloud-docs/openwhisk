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

O pacote `/whisk.system/slack` pré-instalado oferece uma maneira conveniente de usar as [APIs do Slack](https://api.slack.com/){: external}.
{: shortdesc}

O pacote inclui as ações a seguir:

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Pacote | `url`, `channel`, `username` | Interagir com a API do Slack. |
| `/whisk.system/slack/post` | Ação | `text`, `url`, `channel`, `username` | Posta uma mensagem para um canal do Slack. |

É sugerido criar uma ligação de pacote com os valores `username`, `url` e
`channel`. Com a ligação, não será necessário especificar os valores toda vez que você chamar a ação no pacote.

## Postando uma mensagem para um canal do Slack

A ação `/whisk.system/slack/post` posta uma mensagem para um canal do Slack especificado. Os parâmetros a seguir são suportados.

| Parâmetro | Descrição |
| --- | --- |
| `url` | A URL do webhook do Slack. |
| `channel` | O canal do Slack no qual postar a mensagem. |
| `username` | O nome para postar a mensagem. |
| `text` | Uma mensagem para postar. |
| `token` | (opcional) Um [token de acesso](https://api.slack.com/tokens){: external} do Slack. |

O exemplo a seguir mostra como configurar o Slack, criar uma ligação de pacote e postar uma mensagem em um canal.

1. Configure um [webhook recebido](https://api.slack.com/incoming-webhooks){: external} do Slack para sua equipe.

  Após a configuração do Slack, você obtém uma URL de webhook semelhante a
`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. O webhook será necessário na próxima etapa.

2. Crie uma ligação de pacote com suas credenciais do Slack, o canal no qual deseja postar e o nome de usuário com o qual postar.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Chame a ação `post` em sua ligação do pacote para postar uma mensagem em seu canal do Slack.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Usando a API baseada no token de Slack

Se preferir, é possível escolher usar uma API baseada em token do Slack, em vez da API do webhook. Se você assim escolher, então, passe em um parâmetro `token` que contém o seu [token de acesso](https://api.slack.com/tokens){: external} do Slack. Em seguida, é possível usar qualquer um dos [Métodos de API do Slack](https://api.slack.com/methods){: external} como seu parâmetro `url`. Por exemplo, para postar uma mensagem, você usaria um valor de parâmetro `url` de [<ph class="ignoreSpelling">slack.postMessage</ph>](https://api.slack.com/methods/chat.postMessage){: external}.



