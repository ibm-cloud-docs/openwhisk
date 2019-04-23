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

# Regionen
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} ist in den {{site.data.keyword.Bluemix_notm}}-Regionen 'US South' (USA (Süden)), 'US East' (USA (Osten)), 'Germany' (Deutschland), 'United Kingdom' (Großbritannien) und 'Tokyo' (Tokio) verfügbar. Wenn Sie sich bei der {{site.data.keyword.Bluemix_notm}}-CLI anmelden, können Sie eine bestimmte Region einschließen, indem Sie den API-Endpunkt für die Region mit dem Flag `-a` angeben. 

  ```
  ibmcloud login -a <endpunkt>
  ```
  {: pre}
  
  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>USA (Süden)</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>USA (Osten)</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Großbritannien (Süden)</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Mitteleuropa</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokio</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## {{site.data.keyword.openwhisk_short}}-Endpunkte
  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>USA (Süden)</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>USA (Osten)</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Großbritannien (Süden)</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Mitteleuropa</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokio</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
