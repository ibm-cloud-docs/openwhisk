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

# Regiones
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} está disponible en las regiones de {{site.data.keyword.Bluemix_notm}} del sur de EE.UU., este de EE.UU., Alemania, Reino Unido y Tokio. Al iniciar sesión en la CLI de
{{site.data.keyword.Bluemix_notm}}, puede optar por incluir una región específica incluyendo el punto final de API de la región con el distintivo `-a`.

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}
  
  <table>
    <tr>
      <th>Región</th>
      <th>Punto final</th>
    </tr>
    <tr>
      <td>EE.UU. sur</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>EE.UU. este</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Reino Unido sur</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>UE central</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokio</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## Puntos finales de {{site.data.keyword.openwhisk_short}}
  <table>
    <tr>
      <th>Región</th>
      <th>Punto final</th>
    </tr>
    <tr>
      <td>EE.UU. sur</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>EE.UU. este</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Reino Unido sur</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>UE central</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokio</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
