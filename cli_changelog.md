---

copyright:
  years: 2019, 2019
lastupdated: "2019-10-04"

keywords: cli, functions

subcollection: cloud-functions

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
| 1.0.34 | 24 Sept 2019 | Fixed a bug with the `fn property get` command that displayed incorrect authentication key and default namespace after switching API hosts or regions. |
| 1.0.33 | 11 Sept 2019 | <ul><li>Fixed a bug that results in the incorrect {{site.data.keyword.openwhisk_short}} API host being retained after switching API hosts or regions.</li><li>Fixed a bug with the `ibmcloud fn namespace delete` command that reported a success message when deletion failed.</li><li>Updated the `ibmcloud fn api` command to support `create`, `delete`, `get`, and `list` for IAM-based namespaces.</li><li>Updated the `ibmcloud fn namespace get --properties` command to display CRNs and Service IDs.</li></ul> |
| 1.0.30 | 03 April 2019 | <ul><li>Improved the `service bind` handling of IAM and org and space-based services.</li><li>Added a fix for handling API endpoint `https://cloud.ibm.com`.</li></ul> |
| 1.0.29 | 06 February 2019 | Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, see the [Deployment](/docs/openwhisk?topic=cloud-functions-deploy#deploy) documentation. |
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
| 1.0.10 | 09 April 2018 | <ul><li>Added new `--web-secure` option to the `ibmcloud wsk action create` and `update` commands to secure web action endpoints.</li><li>Fixed back-to-back path parameter [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}.</li></ul> |
| 1.0.9 | 16 March 2018 | Enabled support for service bind at the package level. |
| 1.0.8 | 22 February 2018 | Enabled support for IAM service bind. |
| 1.0.7 | 02 February 2018 | <ul><li>Updated `ibmcloud wsk api` to accept path parameters such as `/api/{id}`. For more information, see [API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway).</li><li>Restored proxy support.</li><li>Removed `swift:3`.</li></ul> |
| 1.0.6 | 30 January 2018 | Fixed a bug with the command `ibmcloud wsk service bind` for actions inside a package. |
{: caption="Changes in the IBM Cloud Functions CLI" caption-side="top"}
