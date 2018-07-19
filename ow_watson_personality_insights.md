---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.personalityinsightsshort}} package

The {{site.data.keyword.personalityinsightsfull}} service enables applications to derive insights from social media, enterprise data, or other digital communications. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts.
{: shortdesc}

The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and, for JSON content that is timestamped, can report temporal behavior.
* For information about the meaning of the models that the service uses to describe personality characteristics, see [Personality models](https://console.bluemix.net/docs/services/personality-insights/models.html).
* For information about the meaning of the consumption preferences, see [Consumption preferences](https://console.bluemix.net/docs/services/personality-insights/preferences.html).

**Note:** Request logging is disabled for the {{site.data.keyword.personalityinsightsshort}} service. The service neither logs nor retains data from requests and responses, regardless of whether the `X-Watson-Learning-Opt-Out` request header is set.

The {{site.data.keyword.personalityinsightsshort}} package contains the following entities. You can find additional details in the {{site.data.keyword.personalityinsightsshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Work with the {{site.data.keyword.personalityinsightsshort}} V3 service. |
| [profile](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Get a profile. |
| [profile-as-csv](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Get a profile as CSV file. |

## Creating a {{site.data.keyword.personalityinsightsshort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.personalityinsightsshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.personalityinsightsshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/personality_insights).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.personalityinsightsshort}} package
{: #install}

After you have an {{site.data.keyword.personalityinsightsshort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.personalityinsightsshort}} package into your namespace.
{: shortdesc}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the [`wskdeploy` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) and add the downloaded binary to your PATH.

To install the {{site.data.keyword.personalityinsightsshort}} package:

1. Clone the {{site.data.keyword.personalityinsightsshort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Navigate to the `packages/personality-insights-v3` directory.
    ```
    cd <filepath>/packages/personality-insights-v3
    ```
    {: pre}

3. Deploy the package.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verify that the package is added to your package list.
    ```
    ibmcloud wsk package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.personalityinsightsshort}} instance you created to the package.
    ```
    ibmcloud wsk service bind personality_insights personality-insights-v3
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

3. Verify that the package is configured with your {{site.data.keyword.personalityinsightsshort}} service instance credentials.
    ```
    ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/personality-insights-v3 parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/personality-insights-v3, displaying field parameters
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

## Using the {{site.data.keyword.personalityinsightsshort}} package
{: #usage}

To use the actions in this package, run commands in the following format:

```
ibmcloud wsk action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}
