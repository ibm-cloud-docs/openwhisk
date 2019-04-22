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

# 지역
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}}는 미국 남부, 미국 동부, 독일, 영국 및 도쿄 {{site.data.keyword.Bluemix_notm}} 지역에서 사용할 수 있습니다. {{site.data.keyword.Bluemix_notm}} CLI에 로그인할 때, `-a` 플래그와 함께 영역에 대한 API 엔드포인트를 포함하도록 선택할 수 있습니다. 

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}
  
  <table>
    <tr>
      <th>지역</th>
      <th>엔드포인트</th>
    </tr>
    <tr>
      <td>미국 남부</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>미국 동부</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>영국 남부</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>유럽 중부</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>도쿄</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}} 엔드포인트
  <table>
    <tr>
      <th>지역</th>
      <th>엔드포인트</th>
    </tr>
    <tr>
      <td>미국 남부</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>미국 동부</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>영국 남부</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>유럽 중부</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>도쿄</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
