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

此预安装包在东京区域中不可用。
{: tip}

通过 `/whisk.system/github` 包，可以方便地使用 [GitHub API](https://developer.github.com/){: external}。
{: shortdesc}

GitHub 包中包含以下实体。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/github`|包|`username`、`repository`、`accessToken`|与 GitHub API 进行交互。|
|`/whisk.system/github/webhook`|订阅源|`events`、`username`、`repository`、`accessToken`|对 GitHub 活动触发触发器事件。|

建议使用 `username`、`repository` 和 `accessToken` 值创建包绑定。通过绑定，就无需在每次使用包中的订阅源时指定这些值。

## 使用 GitHub 活动触发触发器事件

`/whisk.system/github/webhook` 订阅源将服务配置为当指定的 GitHub 存储库中发生活动时触发触发器。参数如下所示。

|参数|描述|
| --- | --- |
|`username`|GitHub 存储库的用户名。|
|`repository`|GitHub 存储库。|
|`accessToken`|您的 GitHub 个人访问令牌。创建令牌时，请确保选择 `repo:status` 和 `public_repo` 作用域。此外，请确保还没有为存储库定义任何 Webhook。|
|`events`|关注的 [GitHub 事件类型 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.github.com/v3/activity/events/types/)。|

在以下示例中，创建了每次对 GitHub 存储库执行新落实时触发的触发器。

1. 生成 GitHub 个人访问令牌。导航至 **GitHub.com** > **设置** > **个人访问令牌**以生成令牌。下一步中将使用此访问令牌。

2. 使用访问令牌创建为您的 GitHub 存储库配置的包绑定。
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. 使用 `myGit/webhook` 订阅源为 GitHub `push` 事件类型创建触发器。
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  使用 `git push` 对 Github 存储库执行的落实将导致触发器由 Webhook 触发。如果规则与触发器相匹配，那么将会调用关联的操作。该操作会接收 GitHub Webhook 有效内容作为输入参数。每一个 GitHub Webhook 事件都具有相似的 JSON 模式，但是却是事件类型所确定的唯一有效内容对象。
有关更多有效内容的内容信息，请参阅 [GitHub 事件和有效内容](https://developer.github.com/v3/activity/events/types/){: external} API 文档。



