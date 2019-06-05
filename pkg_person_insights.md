---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: personality insights, cognitive, serverless, functions

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

# {{site.data.keyword.personalityinsightsshort}}
{: #pkg_person_insights}

The installable {{site.data.keyword.personalityinsightsfull}} service enables applications to derive insights from social media, enterprise data, or other digital communications. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts.
{: shortdesc}

The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and, for JSON content that is timestamped, can report temporal behavior.
* For more information about the meaning of the models that the service uses to describe personality characteristics, see [Personality models](/docs/services/personality-insights?topic=personality-insights-models).
* For more information about the meaning of the consumption preferences, see [Consumption preferences](/docs/services/personality-insights?topic=personality-insights-preferences).

Request logging is disabled for the {{site.data.keyword.personalityinsightsshort}} service. The service doesn't log retain data from requests and responses, regardless of whether the `X-Watson-Learning-Opt-Out` request header is set.
{: note}

The {{site.data.keyword.personalityinsightsshort}} package contains the following entities. You can find additional details in the {{site.data.keyword.personalityinsightsshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.personalityinsightsshort}} V3 service. |
| [`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `content`, `content_type`, `content_language`, `accept_language`, `raw_scores`, `csv_headers`, `consumption_preferences` | Get a profile. |
| [`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `content`, `content_type`, `content_language`, `accept_language`, `raw_scores`, `csv_headers`, `consumption_preferences` | Get a profile as CSV file. |

## Creating a {{site.data.keyword.personalityinsightsshort}} service instance
{: #service_instance_insights}

Before you install the package, you must create a {{site.data.keyword.personalityinsightsshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.personalityinsightsshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/services/personality_insights).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.personalityinsightsshort}} package
{: #install_insights}

After you have a {{site.data.keyword.personalityinsightsshort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.personalityinsightsshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #personalityinsights_cli}

**Before you begin**

[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.personalityinsightsshort}} package:

1. Clone the {{site.data.keyword.personalityinsightsshort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
    ```
    {: pre}

3. Verify that the package is added to your package list.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.personalityinsightsshort}} instance you created to the package.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command:
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    **Example output**
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.personalityinsightsshort}} service instance credentials.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    **Example output**
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #personalityinsights_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create).

2. By using the namespace switcher, select the namespace that you want to install the package into. 

3. Click **Install Packages**.

4. Click the **Watson** package group.

5. Click the **Personality Insights** package.

6. Click **Install**.

7. Once the package is installed you are redirected to the actions page and can search for your new package, which is named `personality-insights-v3`.

8. To use the actions in the `personality-insights-v3` package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions.
  * To bind service credentials to individual actions, complete the following steps in the UI.

  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the `personality-insights-v3` package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new parameter. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.personalityinsightsshort}} package
{: #usage_insights}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

This package's functions use the current version of {{site.data.keyword.personalityinsightsshort}}, 2017-10-13. Try out the `profile` action.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. This excerpt is just some filler text and probably won't return anything very interesting from the personality insights service. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts. The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and for JSON content that is timestamped, can report temporal behavior."
```
{: pre}

