---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Origem de eventos do GitHub
{: #openwhisk_catalog_github}

O pacote `/whisk.system/github` oferece uma maneira conveniente de usar as [APIs do GitHub](https://developer.github.com/).
{: shortdesc}

O pacote inclui o feed a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/github` | pacote | username, repository, accessToken | Interagir com a API do GitHub |
| `/whisk.system/github/webhook` | alimentação | events, username, repository, accessToken | Disparar eventos acionadores na atividade do GitHub |

É sugerido criar uma ligação de pacote com os valores `username`,
`repository` e `accessToken`.  Com a ligação, não será necessário especificar os valores toda vez que usar o feed no pacote.

## Disparando um evento acionador com atividade do GitHub

O feed `/whisk.system/github/webhook` configura um serviço para disparar um acionador quando houver atividade em um repositório do GitHub especificado. Os parâmetros são os seguintes:

- `username`: o nome do usuário do repositório GitHub.
- `repository`: o repositório GitHub.
- `accessToken`: seu token de acesso pessoal do GitHub. Ao [criar o seu token](https://github.com/settings/tokens), certifique-se de selecionar os escopos **repo:status** e **public_repo**. Além disso, certifique-se de que você não tenha nenhum webhook já definido para seu repositório.
- `events`: o [tipo de evento do GitHub](https://developer.github.com/v3/activity/events/types/) de interesse.

No exemplo a seguir, é criado um acionador que dispara a cada nova confirmação em um repositório GitHub.

1. Gere um [token de acesso pessoal](https://github.com/settings/tokens) do GitHub. O token de acesso será usado na próxima etapa.

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
do GitHub](https://developer.github.com/v3/activity/events/types/).
