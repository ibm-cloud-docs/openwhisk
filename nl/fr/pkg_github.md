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

Ce package préinstallé n'est pas disponible dans la région Tokyo.
{: tip}

Le package `/whisk.system/github` permet d'utiliser les [API GitHub](https://developer.github.com/){: external}.
{: shortdesc}

Le package GitHub contient les entités suivantes. 

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/github` | Package | `username`, `repository`, `accessToken` | Interagir avec l'API GitHub. |
| `/whisk.system/github/webhook` | Flux | `events`, `username`, `repository`, `accessToken` | Exécuter des événements déclencheurs en cas d'activité GitHub. |

Il est recommandé de créer une liaison de package avec les valeurs `username`, `repository` et `accessToken`.  Grâce à la liaison, il n'est pas nécessaire de spécifier les valeurs à chaque fois que vous utilisez le flux dans le package.

## Exécution d'un événement déclencheur avec une activité GitHub

Le flux `/whisk.system/github/webhook` configure un service pour qu'il exécute un déclencheur lorsqu'il existe une activité dans un référentiel GitHub spécifié. Les paramètres sont les suivants. 

| Paramètre | Description |
| --- | --- |
| `username` | Nom d'utilisateur du référentiel GitHub. |
| `repository` | Référentiel GitHub. |
| `accessToken` | Votre jeton d'accès personnel GitHub. Lorsque vous créez votre jeton, veillez à sélectionner les portées `repo:status` et `public_repo`. De plus, vérifiez qu'aucun webhook n'est défini pour votre référentiel. |
| `events` | [Type d'événement GitHub![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.github.com/v3/activity/events/types/) qui vous intéresse. |

L'exemple suivant illustre la création d'un déclencheur qui s'exécute chaque fois qu'une nouvelle validation est effectuée dans un référentiel GitHub.

1. Générez un jeton d'accès personnel GitHub. Accédez à **GitHub.com** > **Settings** > **Personal access tokens** pour générer un jeton. Le jeton d'accès va être utilisé à l'étape suivante.

2. Créez une liaison de package configurée pour votre référentiel GitHub et avec votre jeton d'accès.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Créez un déclencheur pour le type d'événement `push` GitHub à l'aide de votre flux `myGit/webhook`.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  Une validation au référentiel GitHub via une commande `git push` provoque l'exécution du déclencheur par le webhook. Si une règle correspond au déclencheur, l'action associée est appelée. L'action reçoit le contenu de webhook GitHub comme paramètre d'entrée. Chaque événement de webhook GitHub comporte un schéma JSON similaire, mais un objet de contenu unique qui est déterminé par son type d'événement. Pour plus d'informations sur le contenu, voir la documentation de l'API [GitHub Events Types and Payloads](https://developer.github.com/v3/activity/events/types/){: external}.



