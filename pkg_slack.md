---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-30"

keywords: slack package, token-based, api, functions, slack, action, package

subcollection: openwhisk

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

The pre-installed `/whisk.system/slack` package that is available for {{site.data.keyword.openwhisk}}, offers a convenient way to use the [Slack APIs](https://api.slack.com/){: external}.
{: shortdesc}

The package includes the following actions,

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Package | `url`, `channel`, `username` | Interact with the Slack API. |
| `/whisk.system/slack/post` | Action | `text`, `url`, `channel`, `username` | Posts a message to a Slack channel. |

Creating a package binding with the `username`, `url`, and `channel` values is suggested. With binding, you don't need to specify the values each time that you invoke the action in the package.

## Posting a message to a Slack channel

The `/whisk.system/slack/post` action posts a message to a specified Slack channel. The following parameters are supported.
{: shortdesc}

| Parameter | Description |
| --- | --- |
| `url` | The Slack webhook URL. |
| `channel` | The Slack channel to post the message to. |
| `username` | The name to post the message as. |
| `text` | A message to post. |
| `token` | (optional) A Slack [access token](https://api.slack.com/tokens){: external}. |

The following example shows how to configure Slack, create a package binding, and post a message to a channel.

1. Configure a Slack [incoming webhook](https://api.slack.com/incoming-webhooks){: external} for your team.

   After Slack is configured, you get a webhook URL that looks like `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. The webhook is needed in the next step.

2. Create a package binding with your Slack credentials, the channel that you want to post to, and the user name to post as.

   ```sh
   ibmcloud fn package bind /whisk.system/slack mySlack \
     --param url "https://hooks.slack.com/services/..." \
     --param username "Bob" \
     --param channel "#MySlackChannel"
   ```
   {: pre}

3. Invoke the `post` action in your package binding to post a message to your Slack channel.

   ```sh
   ibmcloud fn action invoke mySlack/post --blocking --result \
     --param text "Hello from OpenWhisk!"
   ```
   {: pre}

## Using the Slack token-based API

If you prefer, you can choose to use Slack's token-based API, rather than the webhook API. If you so choose, then pass in a `token` parameter that contains your Slack [access token](https://api.slack.com/tokens){: external}. Then, you can use any of the [Slack API methods](https://api.slack.com/methods){: external} as your `url` parameter. For example, to post a message, you would use a `url` parameter value of [<ph class="ignoreSpelling">slack.postMessage</ph>](https://api.slack.com/methods/chat.postMessage){: external}.
