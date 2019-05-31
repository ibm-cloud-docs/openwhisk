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

# 地域
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} は、米国南部、米国東部、ドイツ、英国、および東京の {{site.data.keyword.Bluemix_notm}} 地域で使用可能です。 {{site.data.keyword.Bluemix_notm}} CLI にログインすると、`-a` フラグを使用して特定地域の API エンドポイントを組み込むことにより、その地域を含めることができます。

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}

  <br />

  <table>
    <tr>
      <th>地域</th>
      <th>エンドポイント</th>
    </tr>
    <tr>
      <td>米国南部</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>米国東部</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英国南部</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>中欧</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>東京</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}} のエンドポイント
  <table>
    <tr>
      <th>地域</th>
      <th>エンドポイント</th>
    </tr>
    <tr>
      <td>米国南部</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>米国東部</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英国南部</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>中欧</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>東京</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
