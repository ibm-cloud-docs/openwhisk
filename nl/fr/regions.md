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

# Régions
{: #cloudfunctions_regions}

{{site.data.keyword.openwhisk_short}} est disponible dans les régions {{site.data.keyword.Bluemix_notm}} Sud des Etats-Unis, Est des Etats-Unis, Allemagne, Royaume-Uni et Tokyo. Lorsque vous vous connectez à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, vous pouvez choisir d'inclure une région spécifique en incluant le noeud final d'API pour la région avec l'indicateur `-a`.

  ```
  ibmcloud login -a <noeud final>
  ```
  {: pre}

  <br />

  <table>
    <tr>
      <th>Région</th>
      <th>Noeud final</th>
    </tr>
    <tr>
      <td>Sud des Etats-Unis</td>
      <td><code>api.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Est des Etats-Unis</td>
      <td><code>api.us-east.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Sud du Royaume-Uni</td>
      <td><code>api.eu-gb.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Europe centrale</td>
      <td><code>api.eu-de.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokyo</td>
      <td><code>api.jp-tok.cloud.ibm.com</code></td>
    </tr>
  </table>

## Noeuds finaux {{site.data.keyword.openwhisk_short}}
  <table>
    <tr>
      <th>Région</th>
      <th>Noeud final</th>
    </tr>
    <tr>
      <td>Sud des Etats-Unis</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Est des Etats-Unis</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Sud du Royaume-Uni</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Europe centrale</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokyo</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>
