---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: regions, endpoints, api, cli, plug-in, functions

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


# Regions
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} is available in the US South, US East, Germany, United Kingdom, and Tokyo {{site.data.keyword.cloud_notm}} regions. When you log in to the {{site.data.keyword.cloud_notm}} CLI, you can choose to include a specific region by including the API endpoint for the region with the `-a` flag.

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}

  <br />

  <table>
    <tr>
      <th>Region</th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>US South</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>US East</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>UK South</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>EU Central</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokyo</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}} endpoints
  <table>
    <tr>
      <th>Region</th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>US South</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>US East</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>UK South</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>EU Central</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokyo</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>

