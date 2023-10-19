---

copyright:
  years: 2019, 2023
lastupdated: "2023-10-19"

keywords: cli, functions, changelog, change log, updates, fixes

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# CLI version history
{: #cli_versions}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

A historical record of versions that show highlights and bug fixes for {{site.data.keyword.openwhisk}} ClI commands.
{: shortdesc}

| Version | Date | Description |
| --------- | -------- | -------- |
| 1.0.60 | 09 September 2022| Removed `fn api` commands. |
| 1.0.54 | 09 April 2021 |  - Fixed the token expiration issue by increasing the frequency of refreshing IAM token.  |
| 1.0.53 | 26 March 2021 |  - Explicitly display namespace names with **`ibmcloud fn list`** command. |
| 1.0.50 | 12 March 2021 |  - Enabled Sydney(au-syd) region. |
| 1.0.49 | 07 Jan 2021 |  - Fixed a bug that affects Tokyo(jp-tok) region. |
| 1.0.48 | 16 Dec 2020 |  - Added notification about duplicate namespace names when using `namespace target` command. |
| 1.0.47 | 25 Sept 2020 |  - Upgraded dependent `ibmcloud cli sdk` version. |
| 1.0.45 | 12 August 2020 |  - Added runtime deprecation warning messages after users target namespaces. |
| 1.0.44 | 16 July 2020 |  - Fixed a bug that is associated with `namespace target` command and improved the error messages. |
| 1.0.43 | 8 July 2020 |  - Fixed IAM namespace targeting so that you can target a namespace without first targeting an `org` and `space`.  \n - Updated the default version for `Golang` actions. |
| 1.0.40 | 30 May 2020 |  - Added new **`ibmcloud fn namespace target`** command to allow users to target any IAM or CF-based namespaces.  \n - Added `print out` information to notify users the region under which namespaces are listed when users issue `ibmcloud fn namespace list` command.  \n - Disallow users to set `apihost` by using the `ibmcloud fn property set --apihost` command. |
| 1.0.39 | 22 April 2020 |  - Fixed a bug that is related to `wsk deploy` actions in IAM namespaces. |
| 1.0.38 | 2 March 2020 |  - Introduced two new flags for trigger commands, `--trigger-param` and `--feed-param`. With these flags, you can distinguish between trigger parameters and trigger feed parameters, making trigger create and update commands more flexible. For more information, see [Create trigger command](/docs/openwhisk?topic=openwhisk-functions-cli#cli_trigger_create).  \n - Resolved a bug that is related to `wsk deploy` APIs where `wsk deploy` ignores `require-whisk-auth` annotation when you deploy APIs based on actions with `require-whisk-auth` annotations.  \n - Improved the messages for `service bind` command with the `--verbose` flag.  \n - Improved error messages for `ibmcloud fn property set --namespace CFnamespace`.  \n - Changes for `wsk deploy` and Travis environment compatibility. |
| 1.0.36 | 13 November 2019 | The **`service bind`** command was updated to issue correct warnings and search information. The command also reports error and success messages based on users' targets and `service bind` results. A user can have either a resource group or a Cloud Foundry namespace that is targeted, neither, or both. |
| 1.0.35 | 13 October 2019 |  - Fixed a bug with the **`ibmcloud fn property get --apihost`** command that caused changes to the IBM Cloud API endpoint.  \n - Removed support from the `ibmcloud fn property set --namespace <NAMESPACE>` command for targeting Cloud Foundry-based namespaces. This command now targets only IAM-based namespaces.  \n - Improved the writing to the configuration file for the **`ibmcloud fn deploy`** command. |
| 1.0.34 | 24 September 2019 | Fixed a bug with the **`ibmcloud fn property get`** command that displayed incorrect authentication key and default namespace after you switch API hosts or regions. |
| 1.0.33 | 11 September 2019 |  - Fixed a bug where the incorrect {{site.data.keyword.openwhisk_short}} API host is retained  after you switch API hosts or regions.  \n - Fixed a bug with the `ibmcloud fn namespace delete` command that reported a success message when deletion failed.  \n - Updated the **`ibmcloud fn api`** command to support `create`, `delete`, `get`, and `list` for IAM-based namespaces.  \n - Updated the `ibmcloud fn namespace get --properties` command to display CRNs and Service IDs. |
| 1.0.30 | 03 April 2019 |  - Improved the **`service bind`** command handling of IAM and org and space-based services.  \n - Added a fix for handling API endpoint `https://cloud.ibm.com`. | |
| 1.0.29 | 06 February 2019 | Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, see the [Deployment](/docs/openwhisk?topic=openwhisk-deploy#deploy) documentation. |
| 1.0.28 | 21 January 2019 | Added an error message when `update`, `delete`, `get namespace name` exists multiple times. |
| 1.0.27 | 11 December 2018 |  - Added `namespace get` fixes.  \n - Added a fix for `--save-as` when an action is a black box action. |
| 1.0.26 | 30 November 2018 | Enabled `ibmcloud fn property get --auth` to correctly return the auth key in a new environment. |
| 1.0.25 | 23 November 2018 |  - Improved error message result display.  \n - Added a `fn namespace get` fix to correctly display namespace properties. |
| 1.0.23 | 15 October 2018 | Added support for ruby (`.rb`) action code recognition. |
| 1.0.22 | 20 August 2018 | Added us-east region support. |
| 1.0.21 | 01 August 2018 | Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: **`ibmcloud fn <command>`** and **`ibmcloud fn <command>`**. You can also still use **`ibmcloud wsk <command>`**. |
| 1.0.19 | 02 July 2018 | Minor bug fixes and improvements. |
| 1.0.18 | 20 June 2018 |  - Added a fix for unbinding user-provided service instances.  \n - Performance improvements. |
| 1.0.17 | 12 June 2018 | Added support for binding (**`ibmcloud wsk service bind`**) and unbinding (**`ibmcloud wsk service unbind`**) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command. |
| 1.0.16 | 24 May 2018 | Minor bug fixes and improvements. |
| 1.0.15 | 21 May 2018 | Minor bug fixes and improvements. |
| 1.0.14 | 17 May 2018 | Enabled support for the `&` character in org and space names. |
| 1.0.13 | 07 May 2018 | Minor bug fixes and error handling improvements. |
| 1.0.12 | 30 April 2018 | {{site.data.keyword.cloud_notm}} SDK updates to maintain `bx` CLI compatibility. |
| 1.0.11 | 23 April 2018 | Minor bug fixes and improvements. |
| 1.0.10 | 09 April 2018 |  - Added new `--web-secure` option to the **`ibmcloud wsk action create`** and **`update`** commands to secure web action endpoints.  \n - Fixed back-to-back path parameter [defect](https://github.com/apache/openwhisk-cli/issues/237){: external}. |
| 1.0.9 | 16 March 2018 | Enabled support for service bind at the package level. |
| 1.0.8 | 22 February 2018 | Enabled support for IAM service bind. |
| 1.0.7 | 02 February 2018 |  - Updated **`ibmcloud wsk api`** to accept path parameters such as `/api/{id}`.  \n - Restored proxy support.  \n - Removed `swift:3`. |
| 1.0.6 | 30 January 2018 | Fixed a bug with the command **`ibmcloud wsk service bind`** for actions inside a package. |
{: caption="Changes in the IBM Cloud Functions CLI" caption-side="top"}


