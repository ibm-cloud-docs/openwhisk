---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Namensbereiche erstellen
{: #openwhisk_namespaces}

In der Region 'Tokio' (Tokyo) verwendet {{site.data.keyword.openwhisk_short}} von IAM (Identity and Access Management) verwaltete Namensbereiche, um Entitäten wie Aktionen oder Auslöser zu gruppieren. Anschließend können Sie Zugriffsrichtlinien für den Namensbereich erstellen.
{: shortdesc}

Wenn Sie einen {{site.data.keyword.openwhisk_short}}-Namensbereich erstellen, wird er als IAM-Serviceinstanz erkannt. Von IAM verwaltete Serviceinstanzen müssen in einer
[Ressourcengruppe](/docs/resources?topic=resources-rgs) erstellt werden. Sie können entweder eine eigene Ressourcengruppe erstellen oder die Standardeinstellung verwenden. Wenn Sie die IAM-Serviceinstanzen anzeigen möchten, die sich in Ihrem Konto befinden,
können Sie `ibmcloud resource service-instances` ausführen. 

Die folgenden Artefakte werden zusammen mit Ihrem Namensbereich erstellt. Löschen Sie sie nicht. 

* Es wird eine Service-ID erstellt, die als funktionale ID verwendet werden kann, wenn Sie ausgehende Anrufe tätigen. Alle Aktionen, die in diesem Namensbereich erstellt werden, können diese Service-ID für den Zugriff auf andere Ressourcen verwenden. Wenn Sie alle Ihre Service-IDs anzeigen möchten, führen Sie `ibmcloud iam service-ids` aus. 

* Es wird ein API-Schlüssel für die oben genannte Service-ID erstellt, mit dem IAM-Tokens generiert werden können. Sie können die Tokens dann verwenden, um den Namensbereich bei anderen IBM Cloud-Services zu authentifizieren. Der API-Schlüssel wird den Aktionen als Umgebungsvariable bereitgestellt. 


## Einschränkungen
{: #limitations}

Das [Erstellen von APIs mit einem API-Gateway](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) und das Verwenden des [mobilen SDK](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk) werden für von IAM verwaltete Namensbereiche derzeit nicht unterstützt. 

</br>

Um den {{site.data.keyword.openwhisk_short}}-Back-End-Service am Standort Tokio als Ziel anzugeben, müssen Sie `apihost` an alle CLI-Aufrufe wie `ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com` anhängen. Dies ist eine temporäre Lösung, bis der Standort von `ibmcloud target -r jp-tok` als Ziel verwendet werden kann.
{: tip}



</br>
</br>


## Namensbereich mit der Befehlszeilenschnittstelle erstellen
{: #create_iam_cli}

Sie können einen von IAM verwalteten Namensbereich als Teil einer Ressourcengruppe erstellen und Zugriffsrichtlinien für Ihre Ressourcen verwalten, indem Sie die Ressourcengruppe als Ziel angeben, wenn ein Namensbereich erstellt wird. Wenn andere Benutzer Zugriff auf Ihren Namensbereich benötigen oder wenn Sie aus den Aktionen Ihres Namensbereichs heraus auf andere Ressourcen zugreifen möchten, stellen Sie sicher, dass Sie IAM-Richtlinien einrichten, nachdem Ihr Namensbereich erstellt wurde.
{: shortdesc}

1. Geben Sie die Ressourcengruppe als Ziel an, in der Sie den Namensbereich erstellen möchten. Wenn Sie noch keine [Ressourcengruppe](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) erstellt haben, können Sie die `Standardgruppe` als Ziel angeben. 

    ```
    ibmcloud target -g default
    ```
    {: pre}

2. Erstellen Sie einen von IAM aktivierten Namensbereich. 

    ```
    ibmcloud fn namespace create <namespace_name> [-n <description>]
    ```
    {: pre}

    <table>
      <thead>
        <tr>
          <th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten dieses Befehls</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>&lt;namespace_name&gt;</code></td>
          <td>Der Anzeigename für den neuen IAM-basierten Namensbereich. </td>
        </tr>
        <tr>
          <td><code>-n &lt;description&gt;</code></td>
          <td>Optional: Fügen Sie dem Namensbereich eine Beschreibung hinzu, z. B. welche Art von Aktionen oder Paketen er enthalten wird. </td>
        </tr>
      </tbody>
    </table>

    Beispielausgabe:
    ```
    ok: created namespace myNamespace
    ```
    {: screen}

3. Überprüfen Sie, dass Ihr neuer Namensbereich erstellt wird. 

    ```
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
    ```
    {: pre}

    Beispielausgabe:

    ```
    Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
    ```
    {: screen}

    Sie können auch alle Namensbereiche auflisten, darunter IAM-basierte und Cloud Foundry-basierte Namensbereiche.
    ```
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. Bevor Sie Entitäten im neuen Namensbereich erstellen, legen Sie Ihren CLI-Kontext auf den Namensbereich fest, indem Sie diesen als Ziel angeben.
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## Namensbereich mit der API erstellen
{: #create_iam_api}

Sie können einen von IAM verwalteten Namensbereich als Teil einer Ressourcengruppe erstellen und Zugriffsrichtlinien für Ihre Ressourcen verwalten, indem Sie die Ressourcengruppe als Ziel angeben, wenn ein Namensbereich erstellt wird. Wenn andere Benutzer Zugriff auf Ihren Namensbereich benötigen oder wenn Sie aus den Aktionen Ihres Namensbereichs heraus auf andere Ressourcen zugreifen möchten, stellen Sie sicher, dass Sie IAM-Richtlinien einrichten, nachdem Ihr Namensbereich erstellt wurde.
{: shortdesc}



2. Erstellen Sie einen von IAM aktivierten Namensbereich. 

    ```
    curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
    ```
    {: pre}

    <table>
      <thead>
        <tr>
          <th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten dieses Befehls</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>&lt;IAM_token&gt;</code></td>
          <td>Ihr IBM Cloud Identity and Access Management-Token (IAM). Zum Abrufen Ihres IAM-Tokens führen Sie <code>ibmcloud iam oauth-tokens</code> aus. </td>
        </tr>
        <tr>
          <td><code>-n &lt;name&gt;</code></td>
          <td>Der Name des Namensbereichs. </td>
        </tr>
        <tr>
          <td><code>-n &lt;resource_group_id&gt;</code></td>
          <td>Die ID der Ressourcengruppe, in der der Namensbereich erstellt werden soll. Zum Anzeigen von Ressourcengruppen-IDs führen Sie <code>ibmcloud resource groups</code> aus. </td>
        </tr>
        <tr>
          <td><code>-n &lt;resource_plan_id&gt;</code></td>
          <td>Die ID des Ressourcenplans, z. B. 'functions-base-plan'. </td>
        </tr>
        <tr>
          <td><code>-n &lt;description&gt;</code></td>
          <td>Optional: Fügen Sie dem Namensbereich eine Beschreibung hinzu, z. B. welche Art von Aktionen oder Paketen er enthalten wird. </td>
        </tr>
      </tbody>
    </table>

    Beispielausgabe:
    ```
    {
      "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
    ```
    {: screen}

3. Überprüfen Sie, dass Ihr neuer Namensbereich erstellt wird. 

    ```
    curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
    ```
    {: pre}

    Sie können auch alle Namensbereiche auflisten, darunter IAM-basierte und Cloud Foundry-basierte Namensbereiche.
    ```
    curl --request GET \
      --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
      "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
          "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
        },
        {
          "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
      ]
    }
    ```
    {: screen}


Weitere Informationen zur Arbeit mit HTTP REST finden Sie in der [Dokumentation zur Cloud Functions-API](https://cloud.ibm.com/apidocs/functions).
{: tip}

</br>
</br>


## Nächste Schritte
{: #next}

Nachdem Sie nun einen Namensbereich erstellt haben, können Sie IAM-Zugriffsrichtlinien erstellen, um ihn zu schützen. Lesen Sie zunächst [Zugriff verwalten](/docs/openwhisk?topic=cloud-functions-iam#iam). Weitere Informationen zur Verwaltung von IAM-basierten Namensbereichen finden Sie in der [{{site.data.keyword.openwhisk_short}}-REST-API-Referenz](https://cloud.ibm.com/apidocs/functions). 
