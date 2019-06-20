---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: regions, endpoints, api, cli, plug-in

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

# 区域
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} 在 {{site.data.keyword.Bluemix_notm}} 的美国南部、美国东部、德国、英国和东京区域中可用。登录到 {{site.data.keyword.Bluemix_notm}} CLI 时，可以通过使用 `-a` 标志包含用于特定区域的 API 端点，从而选择包含该区域。

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}

  <br />

  <table>
    <tr>
      <th>区域</th>
      <th>端点</th>
    </tr>
    <tr>
      <td>美国南部</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>美国东部</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英国南部</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>欧洲中部</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>东京</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}} 端点
  <table>
    <tr>
      <th>区域</th>
      <th>端点</th>
    </tr>
    <tr>
      <td>美国南部</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>美国东部</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英国南部</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>欧洲中部</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>东京</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
