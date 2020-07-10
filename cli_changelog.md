---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-08"

keywords: cli, functions

subcollection: openwhisk

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:external: target="_blank" .external}

# CLI version history
{: #cli_versions}

A historical record of versions that show highlights and bug fixes.
{:shortdesc}

| Version | Date | Description |
| --------- | -------- | -------- |
| 1.0.43 | 8 July 2020 | <ul><li>You can target an IAM namespace without first targetting an `org` and `space`.</li><li>Updated the default version for Golang actions to Golang version 1.13.</li></ul>
| 1.0.40 | 30 May 2020 | <ul><li>Added new `ibmcloud fn namespace target` command to allow users to target any IAM or CF based namespaces.</li></ul><ul><li>Added print out information to notify users the region under which namespaces are listed when users issue `ibmcloud fn namespace list` command.</li></ul><ul><li>Disallow users to set apihost using `ibmcloud fn property set --apihost` command.</li></ul>|
| 1.0.39 | 22 April 2020 | <ul><li>Fixed a bug related to `wsk deploy` actions in IAM namespaces.</li></ul> |
| 1.0.38 | 2 March 2020 | <ul><li>Introduced two new flags for trigger commands, `--trigger-param` and `--feed-param`. These flags allow you to distinguish between trigger parameters and trigger feed parameters, making trigger create and update commands more flexible. For more information, see [Create trigger command](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_create).</li><li>Resolved a bug related to `wsk deploy` APIs where `wsk deploy` ignores `require-whisk-auth` annotation when deploying APIs based on actions with `require-whisk-auth` annotations.</li><li>Improved the messages for `service bind` command with the `--verbose` flag.</li><li>Improved error messages for `ibmcloud fn property set --namespace CFnamespace`.</li><li>Made changes for `wsk deploy` and Travis environment compatibility.</li><li>Fixed a bug where `wsk deploy` puts an invalid API Gateway access token when you use `wsk deploy` in Travis environment.</li></ul> |
| 1.0.36 | 13 Nov 2019 | The `service bind` command was updated to issue correct warnings and search information. The command also reports error and success messages based on users' targets and `service bind` results. A user can have either a resource group or a Cloud Foundry namespace targeted, neither, or both. |
| 1.0.35 | 13 Oct 2019 | <ul><li>Fixed a bug with the `ibmcloud fn property get --apihost` command that caused changes to the IBM Cloud API endpoint.</li><li>Removed support from the `ibmcloud fn property set --namespace <NAMESPACE>` command for targeting Cloud-Foundry based namespaces. This command now targets only IAM-based namespaces.</li><li>Improved the writing to the configuration file for the `ibmcloud fn deploy` command.</li></ul> |
| 1.0.34 | 24 Sept 2019 | Fixed a bug with the `fn property get` command that displayed incorrect authentication key and default namespace after switching API hosts or regions. |
| 1.0.33 | 11 Sept 2019 | <ul><li>Fixed a bug that results in the incorrect {{site.data.keyword.openwhisk_short}} API host being retained after switching API hosts or regions.</li><li>Fixed a bug with the `ibmcloud fn namespace delete` command that reported a success message when deletion failed.</li><li>Updated the `ibmcloud fn api` command to support `create`, `delete`, `get`, and `list` for IAM-based namespaces.</li><li>Updated the `ibmcloud fn namespace get --properties` command to display CRNs and Service IDs.</li></ul> |
| 1.0.30 | 03 April 2019 | <ul><li>Improved the `service bind` handling of IAM and org and space-based services.</li><li>Added a fix for handling API endpoint `https://cloud.ibm.com`.</li></ul> |
| 1.0.29 | 06 February 2019 | Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, see the [Deployment](/docs/openwhisk?topic=openwhisk-deploy#deploy) documentation. |
| 1.0.28 | 21 January 2019 | Added an error message when `update`, `delete`, `get namespace name` exists multiple times. |
| 1.0.27 | 11 December 2018 | <ul><li>Added `namespace get` fixes.</li><li>Added a fix for `--save-as` when an action is a black box action.</li></ul> |
| 1.0.26 | 30 November 2018 | Enabled `fn property get --auth` to correctly return the auth key in a new environment. |
| 1.0.25 | 23 November 2018 | <ul><li>Improved error message result display.</li><li>Added a `fn namespace get` fix to correctly display namespace properties.</li></ul> |
| 1.0.23 | 15 October 2018 | Added support for ruby (`.rb`) action code recognition. |
| 1.0.22 | 20 August 2018 | Added us-east region support. |
| 1.0.21 | 01 August 2018 | Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: `ibmcloud fn <command>` and `ibmcloud fn <command>`. You can also still use `ibmcloud wsk <command>`. |
| 1.0.19 | 02 July 2018 | Minor bug fixes and improvements. |
| 1.0.18 | 20 June 2018 | <ul><li>Added a fix for unbinding user-provided service instances.</li><li>Performance improvements.</li></ul> |
| 1.0.17 | 12 June 2018 | Added support for binding (`ibmcloud wsk service bind`) and unbinding (`ibmcloud wsk service unbind`) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command. |
| 1.0.16 | 24 May 2018 | Minor bug fixes and improvements. |
| 1.0.15 | 21 May 2018 | Minor bug fixes and improvements. |
| 1.0.14 | 17 May 2018 | Enabled support for the `&` character in org and space names. |
| 1.0.13 | 07 May 2018 | Minor bug fixes and error handling improvements. |
| 1.0.12 | 30 April 2018 | {{site.data.keyword.cloud_notm}} SDK updates to maintain `bx` CLI compatibility. |
| 1.0.11 | 23 April 2018 | Minor bug fixes and improvements. |
| 1.0.10 | 09 April 2018 | <ul><li>Added new `--web-secure` option to the `ibmcloud wsk action create` and `update` commands to secure web action endpoints.</li><li>Fixed back-to-back path parameter [defect](https://github.com/apache/openwhisk-cli/issues/237){: external}.</li></ul> |
| 1.0.9 | 16 March 2018 | Enabled support for service bind at the package level. |
| 1.0.8 | 22 February 2018 | Enabled support for IAM service bind. |
| 1.0.7 | 02 February 2018 | <ul><li>Updated `ibmcloud wsk api` to accept path parameters such as `/api/{id}`. For more information, see [API Gateway](/docs/openwhisk?topic=openwhisk-apigateway).</li><li>Restored proxy support.</li><li>Removed `swift:3`.</li></ul> |
| 1.0.6 | 30 January 2018 | Fixed a bug with the command `ibmcloud wsk service bind` for actions inside a package. |
{: caption="Changes in the IBM Cloud Functions CLI" caption-side="top"}
