---

copyright:
  years: 2019, 2021
lastupdated: "2021-10-12"

keywords: cli, functions, changelog, updates, fixes

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# CLI version history
{: #cli_versions}

A historical record of versions that show highlights and bug fixes for {{site.data.keyword.openwhisk}} ClI commands.
{: shortdesc}

| Version | Date | Description |
| --------- | -------- | -------- |
| 1.0.54 | 09 April 2021 | <ul><li>Fixed the token expiration issue by increasing the frequency of refreshing IAM token. </li></ul>
| 1.0.53 | 26 March 2021 | <ul><li>Explicitly display namespace names with <strong><code>ibmcloud fn list</code></strong> command.</li></ul>
| 1.0.50 | 12 March 2021 | <ul><li>Enabled Sydney(au-syd) region.</li></ul>
| 1.0.49 | 07 Jan 2021 | <ul><li>Fixed a bug that affects Tokyo(jp-tok) region.</li></ul>
| 1.0.48 | 16 Dec 2020 | <ul><li>Added notification about duplicate namespace names when using <code>namespace target</code> command.</li></ul>
| 1.0.47 | 25 Sept 2020 | <ul><li>Upgraded dependent <code>ibmcloud cli sdk</code> version.</li></ul>
| 1.0.45 | 12 August 2020 | <ul><li>Added runtime deprecation warning messages after users target namespaces.</li></ul>
| 1.0.44 | 16 July 2020 | <ul><li>Fixed a bug that is associated with <code>namespace target</code> command and improved the error messages.</li></ul>
| 1.0.43 | 8 July 2020 | <ul><li>Fixed IAM namespace targeting so that you can target a namespace without first targeting an <code>org</code> and <code>space</code>.</li><li>Updated the default version for <code>Golang</code> actions to <code>Golang</code> version 1.13.</li></ul>
| 1.0.40 | 30 May 2020 | <ul><li>Added new <strong><code>ibmcloud fn namespace target</code></strong> command to allow users to target any IAM or CF-based namespaces.</li></ul><ul><li>Added <code>print out</code> information to notify users the region under which namespaces are listed when users issue <code>ibmcloud fn namespace list</code> command.</li></ul><ul><li>Disallow users to set <code>apihost</code> by using the <code>ibmcloud fn property set --apihost</code> command.</li></ul>|
| 1.0.39 | 22 April 2020 | <ul><li>Fixed a bug that is related to <code>wsk deploy</code> actions in IAM namespaces.</li></ul> |
| 1.0.38 | 2 March 2020 | <ul><li>Introduced two new flags for trigger commands, <code>--trigger-param</code> and <code>--feed-param</code>. With these flags, you can distinguish between trigger parameters and trigger feed parameters, making trigger create and update commands more flexible. For more information, see [Create trigger command](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_create).</li><li>Resolved a bug that is related to <code>wsk deploy</code> APIs where <code>wsk deploy</code> ignores <code>require-whisk-auth</code> annotation when you deploy APIs based on actions with <code>require-whisk-auth</code> annotations.</li><li>Improved the messages for <code>service bind</code> command with the <code>--verbose</code> flag.</li><li>Improved error messages for <code>ibmcloud fn property set --namespace CFnamespace</code>.</li><li>Changes for <code>wsk deploy</code> and Travis environment compatibility.</li><li>Fixed a bug where <code>wsk deploy</code> puts an invalid API Gateway access token when you use <code>wsk deploy</code> in Travis environment.</li></ul> |
| 1.0.36 | 13 November 2019 | The **`service bind`** command was updated to issue correct warnings and search information. The command also reports error and success messages based on users' targets and `service bind` results. A user can have either a resource group or a Cloud Foundry namespace that is targeted, neither, or both. |
| 1.0.35 | 13 October 2019 | <ul><li>Fixed a bug with the <strong><code>ibmcloud fn property get --apihost</code></strong> command that caused changes to the IBM Cloud API endpoint.</li><li>Removed support from the <code>ibmcloud fn property set --namespace <NAMESPACE></code> command for targeting Cloud Foundry-based namespaces. This command now targets only IAM-based namespaces.</li><li>Improved the writing to the configuration file for the <strong><code>ibmcloud fn deploy</code></strong> command.</li></ul> |
| 1.0.34 | 24 September 2019 | Fixed a bug with the **`ibmcloud fn property get`** command that displayed incorrect authentication key and default namespace after you switch API hosts or regions. |
| 1.0.33 | 11 September 2019 | <ul><li>Fixed a bug where the incorrect {{site.data.keyword.openwhisk_short}} API host is retained  after you switch API hosts or regions.</li><li>Fixed a bug with the <code>ibmcloud fn namespace delete</code> command that reported a success message when deletion failed.</li><li>Updated the <strong><code>ibmcloud fn api</code></strong> command to support <code>create</code>, <code>delete</code>, <code>get</code>, and <code>list</code> for IAM-based namespaces.</li><li>Updated the <code>ibmcloud fn namespace get --properties</code> command to display CRNs and Service IDs.</li></ul> |
| 1.0.30 | 03 April 2019 | <ul><li>Improved the <strong><code>service bind</code></strong> command handling of IAM and org and space-based services.</li><li>Added a fix for handling API endpoint <code>https://cloud.ibm.com</code>.</li></ul> |
| 1.0.29 | 06 February 2019 | Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, see the [Deployment](/docs/openwhisk?topic=openwhisk-deploy#deploy) documentation. |
| 1.0.28 | 21 January 2019 | Added an error message when `update`, `delete`, `get namespace name` exists multiple times. |
| 1.0.27 | 11 December 2018 | <ul><li>Added <code>namespace get</code> fixes.</li><li>Added a fix for <code>--save-as</code> when an action is a black box action.</li></ul> |
| 1.0.26 | 30 November 2018 | Enabled `ibmcloud fn property get --auth` to correctly return the auth key in a new environment. |
| 1.0.25 | 23 November 2018 | <ul><li>Improved error message result display.</li><li>Added a <code>fn namespace get</code> fix to correctly display namespace properties.</li></ul> |
| 1.0.23 | 15 October 2018 | Added support for ruby (`.rb`) action code recognition. |
| 1.0.22 | 20 August 2018 | Added us-east region support. |
| 1.0.21 | 01 August 2018 | Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: **`ibmcloud fn <command>`** and **`ibmcloud fn <command>`**. You can also still use **`ibmcloud wsk <command>`**. |
| 1.0.19 | 02 July 2018 | Minor bug fixes and improvements. |
| 1.0.18 | 20 June 2018 | <ul><li>Added a fix for unbinding user-provided service instances.</li><li>Performance improvements.</li></ul> |
| 1.0.17 | 12 June 2018 | Added support for binding (**`ibmcloud wsk service bind`**) and unbinding (**`ibmcloud wsk service unbind`**) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command. |
| 1.0.16 | 24 May 2018 | Minor bug fixes and improvements. |
| 1.0.15 | 21 May 2018 | Minor bug fixes and improvements. |
| 1.0.14 | 17 May 2018 | Enabled support for the `&` character in org and space names. |
| 1.0.13 | 07 May 2018 | Minor bug fixes and error handling improvements. |
| 1.0.12 | 30 April 2018 | {{site.data.keyword.cloud_notm}} SDK updates to maintain `bx` CLI compatibility. |
| 1.0.11 | 23 April 2018 | Minor bug fixes and improvements. |
| 1.0.10 | 09 April 2018 | <ul><li>Added new <code>--web-secure</code> option to the <strong><code>ibmcloud wsk action create</code></strong> and <strong><code>update</code></strong> commands to secure web action endpoints.</li><li>Fixed back-to-back path parameter [defect](https://github.com/apache/openwhisk-cli/issues/237){: external}.</li></ul> |
| 1.0.9 | 16 March 2018 | Enabled support for service bind at the package level. |
| 1.0.8 | 22 February 2018 | Enabled support for IAM service bind. |
| 1.0.7 | 02 February 2018 | <ul><li>Updated <strong><code>ibmcloud wsk api</code></strong> to accept path parameters such as <code>/api/{id}</code>. For more information, see [API Gateway](/docs/openwhisk?topic=openwhisk-apigateway).</li><li>Restored proxy support.</li><li>Removed <code>swift:3</code>.</li></ul> |
| 1.0.6 | 30 January 2018 | Fixed a bug with the command **`ibmcloud wsk service bind`** for actions inside a package. |
{: caption="Changes in the IBM Cloud Functions CLI" caption-side="top"}


