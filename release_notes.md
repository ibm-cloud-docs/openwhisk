---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-21"

keywords: release notes, functions, what's new, 2019, 2020, 2021

subcollection: openwhisk

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}


# Release notes for {{site.data.keyword.openwhisk_short}}
{: #openwhisk-relnotes}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Use the release notes to learn about the latest changes to {{site.data.keyword.openwhisk}} that are grouped by date.
{: shortdesc}

For information about changes to the {{site.data.keyword.openwhisk_short}} CLI, see [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).
{: tip}

## October 2023
{: #openwhisk-oct23}

### 26 October 2023
{: #openwhisk-oct2623}
{: release-note}

Deprecation note for {{site.data.keyword.openwhisk}}
:    {{site.data.keyword.openwhisk}} is deprecated. As of 28 December 28 2023, you can't create new instances, and access to free instances will be removed. Existing premium plan instances are supported until October 2024. Any instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).

## July 2023
{: #openwhisk-july23}

### 19 July 2023
{: #openwhisk-july1923}
{: release-note}

Deprecation note for Cloud Foundry namespaces
:   As a result of the Cloud Foundry service deprecation, Cloud Foundry-based namespaces in {{site.data.keyword.openwhisk_short}} are deprecated and will stop working on Oct 31st, 2023. If you are still using Cloud-Foundry-based namespaces, [migrate your namespace to an IAM namespace](/docs/openwhisk?topic=openwhisk-namespaces#create_iam_namespace) to ensure that your Cloud Function workloads continue to run.

## June 2022
{: #openwhisk-june22}

### 21 June 2022
{: #openwhisk-june2122}
{: release-note}

Additional support for the IBM Cloud Object Storage trigger.
:   You can utilize the new Object Storage functionality for versioning and replication. See [Setting up the IBM Cloud Object Storage trigger](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev).

### 9 June 2022
{: #openwhisk-june922}
{: release-note}

Added information about packaging Java code. 
:   See [Packaging Java code by using the Java runtime with Docker](/docs/openwhisk?topic=openwhisk-prep#prep_java_docker).

## April 2022
{: #openwhisk-apr22}

### 29 April 2022
{: #openwhisk-apr2922}
{: release-note}

Node.js version 16 is now the default. 
:   See [JavaScript runtimes](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_javascript_environments).

## June 2021
{: #openwhisk-jun21}

### 21 June 2021
{: #openwhisk-jun2121}
{: release-note}

Overview topic for troubleshooting
:   See [Troubleshooting overview](/docs/openwhisk?topic=openwhisk-troubleshooting).

## May 2021
{: #openwhisk-may21}

### 7 May 2021
{: #openwhisk-may721}
{: release-note}

Deprecated of packages and runtimes.
:   See [runtimes](/docs/openwhisk?topic=openwhisk-runtimes).

### 1 May 2021
{: #openwhisk-may0121}
{: release-note}

Added information about error handling.
:   See [Error handing for {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-error-handing-functions).  

Added information about getting support. 
:   See [Getting help and support](/docs/openwhisk?topic=openwhisk-gettinghelp).

## April 2021
{: #openwhisk-apr21}

### 27 April 2021
{: #openwhisk-apr0721}
{: release-note}

Added information about deprecating Watson services.
:   See the individual packages.

### 12 April 2021
{: #openwhisk-apr1221}
{: release-note}



Updates to action information.
:   Added and updated information for preparing your application code. See [Preparing apps for actions](/docs/openwhisk?topic=openwhisk-prep).

New Functions CLI version 1.0.54. 
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

## March 2021
{: #openwhisk-mar21}

### 29 March 2021
{: #openwhisk-march2921}
{: release-note}

New Functions CLI version 1.0.53.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

### 25 March 2021
{: #openwhisk-mar2521}
{: release-note}

Updated list of supported versions.
:   See [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).

Updated the Composer topic.
:   See [Composer](/docs/openwhisk?topic=openwhisk-pkg_composer).

Added information about adding an IBM {{site.data.keyword.openwhisk_short}} Channel to {{site.data.keyword.mon_full_notm}}.
:   See [{{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-alerts-notify).

Added information for versioning your actions.
:   See [Versioning your actions](/docs/openwhisk?topic=openwhisk-actions#actions-version).

### 12 March 2021
{: #openwhisk-mar1221}
{: release-note}

Added support for `au-syd` (Sydney) region.
:   See [Regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions).

## January 2021
{: #openwhisk-jan21}

### 29 January 2021
{: #openwhisk-jan2921}
{: release-note}

Added a sitemap.
:   See [Sitemap for Cloud Functions](/docs/openwhisk?topic=openwhisk-sitemap).

Updated information about packaging Go source files. 
:   See [Packaging multiple Go source files](/docs/openwhisk?topic=openwhisk-prep#prep_go_multi_packages).

## December 2020
{: #openwhisk-dec20}

### 14 December 2020
{: #openwhisk-dec1420}
{: release-note}

Added versions to Quick start template docs.
:   See [Deploying quick start templates](/docs/openwhisk?topic=openwhisk-templates).

Updated securing web actions to include information about `X-Require-Whisk-Auth`.
:   See [Securing web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure).

Added tips to monitoring topic.
:   See [{{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-monitor-functions)

Updated namespace instances to indicate if the namespace name or the namespace ID is required.
:   See [Managing namespaces](/docs/openwhisk?topic=openwhisk-namespaces).

## October 2020
{: #openwhisk-oct20}

### 29 October 2020
{: #openwhisk-oct2920}
{: release-note}

GO 1.15 is now supported.
:   See [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes#runtimes_available).

## September 2020
{: #openwhisk-sep20}

### 22 September 2020
{: #openwhisk-sep2220}
{: release-note}

Added support for Node.js 12 runtime.
:   See [runtimes](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_javascript_environments).

Added `--memory` to the **`action create`** and **`action update`** commands.
:   See the [**`action create`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_action_create) and [**`action update`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_action_update) commands.

Added troubleshooting information.
:   See [troubleshooting common issues](/docs/openwhisk?topic=openwhisk-troubleshooting).

## August 2020
{: #openwhisk-aug20}

### 25 August 2020
{: #openwhisk-aug2520}
{: release-note}

New troubleshooting information.
:   See [troubleshooting common issues](/docs/openwhisk?topic=openwhisk-troubleshooting).

## July 2020
{: #openwhisk-jul20}

### 30 July 2020
{: #openwhisk-jul3020}
{: release-note}

Updated information for High availability and disaster recovery.
:   See [High availability and disaster recovery](/docs/openwhisk?topic=openwhisk-ha_dr).

### 23 July 2020
{: #openwhisk-jul2320}
{: release-note}

New information for actions that use binary files.
:   See [Creating actions from binary files](/docs/openwhisk?topic=openwhisk-actions#actions_create_binaries).

### 16 July 2020
{: #openwhisk-jul1620}
{: release-note}

New Functions CLI version 1.0.44 release.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

### 10 July 2020
{: #openwhisk-jul1020}
{: release-note}

New Functions CLI version 1.0.43.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

Additional events and information are available for Functions. 
:   See [Viewing Activity Tracker events](/docs/openwhisk?topic=openwhisk-at_events).

## June 2020
{: #openwhisk-jun20}

### 30 June 2020
{: #openwhisk-jun3020}
{: release-note}

Updates to documentation for Event streams package.
:   See [preinstalled Event streams package](/docs/openwhisk?topic=openwhisk-pkg_event_streams).

### 25 June 2020
{: #openwhisk-jun2520}
{: release-note}

New events are tracked for Functions.
:   See [Viewing Activity Tracker events](/docs/openwhisk?topic=openwhisk-at_events).

### 18 June 2020
{: #openwhisk-jun1820}
{: release-note}

Changes for web action endpoints.
:   See [Creating web actions](/docs/openwhisk?topic=openwhisk-actions_web).

### 15 June 2020
{: #openwhisk-jun1520}
{: release-note}

New Functions CLI version 1.0.41.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

### 8 June 2020
{: #openwhisk-jun820}
{: release-note}

New Functions CLI version 1.0.40.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

Monitoring of your {{site.data.keyword.openwhisk_short}} actions is provided through the {{site.data.keyword.mon_full}} service. 
:   See [Monitoring with {{site.data.keyword.mon_full}}](/docs/openwhisk?topic=openwhisk-monitor-functions).

## May 2020
{: #openwhisk-may20}

### 22 May 2020
{: #openwhisk-may2220}
{: release-note}

New tutorial for calling an action from another action.
:   See [Tutorial: Calling an action from another action](/docs/openwhisk?topic=openwhisk-tutorial_action).

### 15 May 2020
{: #openwhisk-may1520}
{: release-note}

New High availability topic.
:   See [High availability](/docs/openwhisk?topic=openwhisk-ha_dr).

Updates for supported runtimes.
:   See [runtimes](/docs/openwhisk?topic=openwhisk-runtimes).

## April 2020
{: #openwhisk-apr20}

### 24 April 2020
{: #openwhisk-apr2420}
{: release-note}

New Functions CLI version 1.0.39.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

New actions overview topic. 
:   See [Action overview](/docs/openwhisk?topic=openwhisk-actions_over).

Improved access policy information. 
:   See [Setting access policies](/docs/openwhisk?topic=openwhisk-iam).

## March 2020
{: #openwhisk-mar20}

### 24 March 2021
{: #openwhisk-mar2420}
{: release-note}

Node.js 8 is deprecated.
:   See [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).

### 9 March 2020
{: #openwhisk-mar920}
{: release-note}

New Functions CLI version 1.0.38.
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

Updated trigger parameters for CLI. 
:   See [Trigger commands](/docs/openwhisk?topic=openwhisk-functions-cli#cli_trigger).

Updated troubleshooting information for Node.js. 
:   See [Troubleshooting Node.js issues](/docs/openwhisk?topic=openwhisk-troubleshooting).

## December 2019
{: #openwhisk-dec19}

### 18 December 2019
{: #openwhisk-dec1819}
{: release-note}

New environmental variable: `__OW_TRANSACTION_ID`.
:   See [Environment variables for actions](/docs/openwhisk?topic=openwhisk-actions#actions_envvars).



Updated information for packaging your Python code. 
:   See [How do I package my Python app for deployment in Cloud Functions](/docs/openwhisk?topic=openwhisk-prep#how_to_package_python).

### 5 December 2019
{: #openwhisk-dec519}
{: release-note}

Improved documentation for the quick start templates.
:   See [Deploying quick start templates](/docs/openwhisk?topic=openwhisk-templates).

Updated the API endpoints documentation.
:   See [Regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions).

## November 2019
{: #openwhisk-nov19}

### 18 November 2019
{: #openwhisk-nov1819}
{: release-note}

New Functions CLI version 1.0.36. 
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

## October 2019
{: #openwhisk-oct19}

### 31 October 2019
{: #openwhisk-oct3119}
{: release-note}

The IBM Cloud Object Storage trigger is now available in the following regions: `us-east`, `us-south`, `eu-gb`, `eu-de`, and `jp-tok` regions. 
:   See [IBM Cloud Object Storage](/docs/openwhisk?topic=openwhisk-pkg_obstorage).

### 28 October 2019
{: #openwhisk-oct2819}
{: release-note}

Updates for binding services to entities.
:   See [Binding IBM Cloud services to Cloud Functions entities](/docs/openwhisk?topic=openwhisk-services).

Updates for memory usage.
:   See [Memory usage with Node.js runtime actions](/docs/openwhisk?topic=openwhisk-test#memory_usage)

### 14 October 2019
{: #openwhisk-oct1419}
{: release-note}

New Functions CLI version 1.0.35. 
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

Added console tasks to actions. 
:   See [Create actions](/docs/openwhisk?topic=openwhisk-actions).

## September 2019
{: #openwhisk-sep19}

### 24 September 2019
{: #openwhisk-sep2419}
{: release-note}

New Functions CLI version 1.0.34. 
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).

### 20 September 2019
{: #openwhisk-sep2019}
{: release-note}

Extend your IBM Cloud Functions app with an IBM Cloud Object Storage instance. 
:   See [IBM Cloud Object Storage](/docs/openwhisk?topic=openwhisk-pkg_obstorage).

### 11 September 2019
{: #openwhisk-sep1119}
{: release-note}

New Functions CLI version 1.0.33. 
:   See [CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions).
