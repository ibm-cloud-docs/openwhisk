---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: github, actions, trigger, event, functions

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


# GitHub
{: #pkg_github}

Esse pacote pré-instalado não está disponível na região de Tóquio.
{: tip}

O pacote `/whisk.system/github` oferece uma maneira conveniente de usar as [APIs do GitHub](https://developer.github.com/){: external}.
{: shortdesc}

O pacote GitHub inclui as entidades a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/github` | Pacote | `username`, `repository`, `accessToken` | Interagir com a API do GitHub. |
| `/whisk.system/github/webhook` | Feed | `events`, `username`, `repository`, `accessToken` | Disparar eventos acionadores na atividade do GitHub. |

É sugerido criar uma ligação de pacote com os valores `username`,
`repository` e `accessToken`.  Com a ligação, não será necessário especificar os valores toda vez que usar o feed no pacote.

## Disparando um evento acionador com atividade do GitHub

O feed `/whisk.system/github/webhook` configura um serviço para disparar um acionador quando houver atividade em um repositório do GitHub especificado. Os parâmetros são os seguintes.

| Parâmetro | Descrição |
| --- | --- |
| `username` | O nome de usuário do repositório do GitHub. |
| `repository` | O repositório do GitHub. |
| `accessToken` | Seu token de acesso pessoal do GitHub. Quando você criar seu token, certifique-se de selecionar os escopos `repo:status` e `public_repo`. Além disso, certifique-se de que você não tenha nenhum webhook já definido para seu repositório. |
| `events` | O [tipo de evento do GitHub ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.github.com/v3/activity/events/types/) de interesse. |

No exemplo a seguir, é criado um acionador que dispara a cada nova confirmação em um repositório GitHub.

1. Gere um token de acesso pessoal do GitHub. Navegue para **GitHub.com** > **Configurações** > **Tokens de acesso pessoal** para gerar um token. O token de acesso será usado na próxima etapa.

2. Crie uma ligação de pacote configurada para seu repositório GitHub e com o token de acesso.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Crie um acionador para o tipo de evento `push` do GitHub usando seu feed `myGit/webhook`.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  Uma confirmação para o repositório GitHub usando um `git push` faz
com que o acionador seja disparado pelo webhook. Se uma regra corresponde ao acionador, então a ação associada é chamada. A ação recebe a carga útil de webhook do GitHub como um parâmetro de entrada. Cada evento
de webhook do GitHub tem um esquema JSON semelhante, mas é um objeto de carga útil
exclusivo que é determinado por seu tipo de evento. Para obter mais informações sobre o
conteúdo da carga útil, consulte a documentação da API de
[Eventos e carga útil
do GitHub](https://developer.github.com/v3/activity/events/types/){: external}.



