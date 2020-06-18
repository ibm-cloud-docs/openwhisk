---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-18"

keywords: regions, endpoints, api, cli, plug-in, functions

subcollection: openwhisk

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Regions
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} is available in the US South, US East, Germany, United Kingdom, and Tokyo {{site.data.keyword.cloud_notm}} regions. You can target a specific region whenever you log in to the {{site.data.keyword.cloud_notm}} CLI or change your current region by using the `-r` flag.

```
ibmcloud target -r <region>
```
{: pre}

For example, to target the EU Central region:

```
ibmcloud target -r eu-de
```
{: pre}


## {{site.data.keyword.openwhisk_short}} endpoints

The following endpoints are available for {{site.data.keyword.openwhisk_short}} can be used with [{{site.data.keyword.openwhisk_short}} APIs](https://cloud.ibm.com/apidocs/functions).

| Region | Endpoint |
| --------- | -------- |
| US South | `https://us-south.functions.cloud.ibm.com` |
| US East | `https://us-east.functions.cloud.ibm.com` |
| UK South | `https://eu-gb.functions.cloud.ibm.com` |
| EU Central | `https://eu-de.functions.cloud.ibm.com` |
| Tokyo | `https://jp-tok.functions.cloud.ibm.com` |
{: caption="{{site.data.keyword.openwhisk_short}} endpoints" caption-side="top"}

## {{site.data.keyword.openwhisk_short}} web action endpoints

The following endpoints are available for {{site.data.keyword.openwhisk_short}} can be used with [{{site.data.keyword.openwhisk_short}} web actions](/docs/openwhisk?topic=openwhisk-actions_web).

| Region | Endpoint |
| --------- | -------- |
| US South | `https://us-south.functions.appdomain.cloud` |
| US East | `https://us-east.functions.appdomain.cloud` |
| UK South | `https://eu-gb.functions.appdomain.cloud` |
| EU Central | `https://eu-de.functions.appdomain.cloud` |
| Tokyo | `https://jp-tok.functions.appdomain.cloud` |
{: caption="{{site.data.keyword.openwhisk_short}} endpoints" caption-side="top"}
