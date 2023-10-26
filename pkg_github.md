---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: github, actions, trigger, event, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# GitHub
{: #pkg_github}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The `/whisk.system/github` package that is available with {{site.data.keyword.openwhisk}}, offers a convenient way to use the [GitHub APIs](https://docs.github.com/){: external}.
{: shortdesc}

This preinstalled package is not available in the Tokyo or Sydney regions.  This package does not work with Enterprise GitHub repositories.
{: tip}

The GitHub package includes the following entities.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/github` | Package | `username`, `repository`, `accessToken` | Interact with the GitHub API. |
| `/whisk.system/github/webhook` | Feed | `events`, `username`, `repository`, `accessToken` | Fire trigger events on GitHub activity. |
{: caption="Table 1. Github package entities" caption-side="bottom"}

Creating a package binding with the `username`, `repository`, and `accessToken` values is suggested.  With binding, you don't need to specify the values each time that you use the feed in the package.

The `/whisk.system/github/webhook` feed configures a service to fire a trigger when there is activity in a specified GitHub repository. The parameters are as follows.

| Parameter | Description |
| --- | --- |
| `username` | The user name of the GitHub repository. |
| `repository` | The GitHub repository. |
| `accessToken` | Your GitHub personal access token. When you create your token, be sure to select the `repo:status` and `public_repo` scopes. Also, make sure that you don't have any webhooks that are already defined for your repository. |
| `events` | The [GitHub event type](https://docs.github.com/en/webhooks-and-events/webhooks/webhook-events-and-payloads){: external} of interest. |
{: caption="Table 2. Parameters for `/whisk.system/github/webhook` feed" caption-side="bottom"}

In the following example, a trigger is created that fires each time a new commit to a GitHub repository.

1. Generate a GitHub personal access token. Navigate to **GitHub.com** > **Settings** > **Personal access tokens** to generate a token. The access token will be used in the next step.

2. Create a package binding that is configured for your GitHub repository and with your access token.

    ```sh
    ibmcloud fn package bind /whisk.system/github <myGit> \
        --param username <myGitUser> \
        --param repository <myGitRepo> \
        --param accessToken <aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a>
    ```
    {: pre}

3. Create a trigger for the GitHub `push` event type by using your `myGit/webhook` feed.

    ```sh
    ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
    ```
    {: pre}

    A commit to the GitHub repository by using a `git push` causes the trigger to be fired by the webhook. If you [create a rule](/docs/openwhisk?topic=openwhisk-rules) to associate an action with the trigger, then the associated action is invoked. The action receives the GitHub webhook payload as an input parameter. Each GitHub webhook event has a similar JSON schema, but is a unique payload object that is determined by its event type. For more information about the payload content, see the [GitHub events and payload](https://docs.github.com/en/webhooks-and-events/webhooks/webhook-events-and-payloads){: external} API documentation.

