---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: regions, endpoints, api, cli, plug-in, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Regions
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

{{site.data.keyword.openwhisk}} is available in the US South, US East, Germany, United Kingdom, Tokyo, and Sydney {{site.data.keyword.cloud_notm}} regions. You can target a specific region whenever you log in to the {{site.data.keyword.cloud_notm}} CLI or change your current region by using the `-r` flag.
{: shortdesc}

```bash
ibmcloud target -r <region>
```
{: pre}

For example, to target the EU Central region:

```bash
ibmcloud target -r eu-de
```
{: pre}


## {{site.data.keyword.openwhisk_short}} endpoints
{: #cloudfunctions_endpoints}

The following endpoints are available for {{site.data.keyword.openwhisk_short}} and can be used with [{{site.data.keyword.openwhisk_short}} APIs](https://cloud.ibm.com/apidocs/functions).

| Region | Endpoint |
| --------- | -------- |
| US South | `https://us-south.functions.cloud.ibm.com` |
| US East | `https://us-east.functions.cloud.ibm.com` |
| UK South | `https://eu-gb.functions.cloud.ibm.com` |
| EU Central | `https://eu-de.functions.cloud.ibm.com` |
| Tokyo | `https://jp-tok.functions.cloud.ibm.com` |
| Sydney | `https://au-syd.functions.cloud.ibm.com` |
{: caption="{{site.data.keyword.openwhisk_short}} endpoints" caption-side="top"}

## {{site.data.keyword.openwhisk_short}} web action endpoints
{: #cloudfunctions_webaction_endpoints}

The following endpoints are available for {{site.data.keyword.openwhisk_short}} and can be used with [{{site.data.keyword.openwhisk_short}} web actions](/docs/openwhisk?topic=openwhisk-actions_web).

| Region | Endpoint |
| --------- | -------- |
| US South | `https://us-south.functions.appdomain.cloud` |
| US East | `https://us-east.functions.appdomain.cloud` |
| UK South | `https://eu-gb.functions.appdomain.cloud` |
| EU Central | `https://eu-de.functions.appdomain.cloud` |
| Tokyo | `https://jp-tok.functions.appdomain.cloud` |
| Sydney | `https://au-syd.functions.appdomain.cloud` |
{: caption="{{site.data.keyword.openwhisk_short}} endpoints" caption-side="top"}


