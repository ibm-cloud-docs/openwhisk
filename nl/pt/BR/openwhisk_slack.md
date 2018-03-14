---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote Slack
{: #openwhisk_catalog_slack}

O pacote `/whisk.system/slack` oferece uma maneira conveniente de usar as [APIs do Slack](https://api.slack.com/).
{: shortdesc}

O pacote inclui as Ações a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Pacote | url, channel, username | Interagir com a API do Slack |
| `/whisk.system/slack/post` | Ação | text, url, channel, username | Posta uma mensagem para um canal do Slack |

É sugerido criar uma ligação de pacote com os valores `username`, `url` e
`channel`. Com a ligação, não será necessário especificar os valores toda vez que você chamar a ação no pacote.

## Postando uma mensagem para um canal do Slack

A ação `/whisk.system/slack/post` posta uma mensagem para um canal do Slack especificado. Os parâmetros são os seguintes:

- `url`: a URL do webhook do Slack.
- `channel`: o canal do Slack no qual postar a mensagem.
- `username`: o nome com o qual postar a mensagem.
- `text`: uma mensagem para postar.
- `token`: (opcional) um [token de acesso](https://api.slack.com/tokens) de Slack. Veja o [Guia do Pacote do OpenWhisk](./openwhisk_catalog.html) para obter mais detalhes sobre como usar tokens de acesso do Slack.

O exemplo a seguir mostra como configurar o Slack, criar uma ligação de pacote e postar uma mensagem em um canal.

1. Configure um [webhook recebido](https://api.slack.com/incoming-webhooks) do Slack para sua equipe.
  
  Após a configuração do Slack, você obtém uma URL de webhook semelhante a
`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. O webhook será necessário na próxima etapa.
  
2. Crie uma ligação de pacote com suas credenciais do Slack, o canal no qual deseja postar e o nome de usuário com o qual postar.
  
  ```
  wsk package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}
  
3. Chame a ação `post` em sua ligação de pacote para postar uma mensagem em seu canal do Slack.
  
  ```
  wsk action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}
  

## Usando a API baseada no token de Slack

Se preferir, é possível escolher usar uma API baseada em token do Slack, em vez da API do webhook. Se você assim escolher, então, passe em um parâmetro `token` que contém o seu [token de acesso](https://api.slack.com/tokens) do Slack. Em seguida, é possível usar qualquer um dos [Métodos de API do Slack](https://api.slack.com/methods) como seu parâmetro `url`. Por exemplo, para postar uma mensagem, você utilizaria um valor de parâmetro `url` [slack.postMessage](https://api.slack.com/methods/chat.postMessage).
