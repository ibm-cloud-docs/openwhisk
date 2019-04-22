---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: regions, endpoints, api, cli, plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 地區
{: #cloudfunctions_regions}

在美國南部、美國東部、德國、英國及東京 {{site.data.keyword.Bluemix_notm}} 地區可以使用 {{site.data.keyword.openwhisk_short}}。當您登入 {{site.data.keyword.Bluemix_notm}} CLI 時，可以選擇包括特定地區，方法為使用 `-a` 旗標來包括該地區的 API 端點。

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}
  
  <table>
    <tr>
      <th>地區</th>
      <th>端點</th>
    </tr>
    <tr>
      <td>美國南部</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>美國東部</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英國南部</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>歐盟中部</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>東京</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}} 端點
  <table>
    <tr>
      <th>地區</th>
      <th>端點</th>
    </tr>
    <tr>
      <td>美國南部</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>美國東部</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>英國南部</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>歐盟中部</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>東京</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
