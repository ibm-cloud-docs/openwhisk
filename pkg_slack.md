---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-08"

keywords: slack package, token-based, api

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Slack
{: #openwhisk_catalog_slack}

The pre-installed `/whisk.system/slack` package offers a convenient way to use the [Slack APIs](https://api.slack.com/).
{: shortdesc}

The package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/slack` | package | url, channel, username | Interact with the Slack API |
| `/whisk.system/slack/post` | action | text, url, channel, username | Posts a message to a Slack channel |

Creating a package binding with the `username`, `url`, and `channel` values is suggested. With binding, you don't need to specify the values each time that you invoke the action in the package.

## Posting a message to a Slack channel

The `/whisk.system/slack/post` action posts a message to a specified Slack channel. The parameters are as follows:

- `url`: The Slack webhook URL.
- `channel`: The Slack channel to post the message to.
- `username`: The name to post the message as.
- `text`: A message to post.
- `token`: (optional) A Slack [access token](https://api.slack.com/tokens).

The following example shows how to configure Slack, create a package binding, and post a message to a channel.

1. Configure a Slack [incoming webhook](https://api.slack.com/incoming-webhooks) for your team.

  After Slack is configured, you get a webhook URL that looks like `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. The webhook is needed in the next step.

2. Create a package binding with your Slack credentials, the channel that you want to post to, and the user name to post as.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Invoke the **post** action in your package binding to post a message to your Slack channel.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Using the Slack token-based API

If you prefer, you can choose to use Slack's token-based API, rather than the webhook API. If you so choose, then pass in a `token` parameter that contains your Slack [access token](https://api.slack.com/tokens). Then, you can use any of the [Slack API methods](https://api.slack.com/methods) as your `url` parameter. For example, to post a message, you would use a `url` parameter value of [slack.postMessage](https://api.slack.com/methods/chat.postMessage).
