---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

subcollection: cloud-functions

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


# Namensbereiche verwalten
{: #namespaces}

Mit {{site.data.keyword.openwhisk}} können Sie mit IAM (Identity and Access Management) verwaltete Namensbereiche erstellen, um Entitäten wie Aktionen oder Auslöser zu gruppieren. Anschließend können Sie IAM-Zugriffsrichtlinien für den Namensbereich erstellen. Eine Übersicht über IAM finden Sie im [{{site.data.keyword.openwhisk_short}}Blogbeitrag zur Ankündigung der IAM-Aktivierung](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## Was ist ein Namensbereich? 

Namensbereiche enthalten {{site.data.keyword.openwhisk_short}}-Entitäten (wie z. B. Aktionen und Auslöser) und gehören zu einer Ressourcengruppe. Sie können anderen Benutzern Zugriff auf Ihre Entitäten gewähren, indem Sie ihnen Zugriff auf den Namensbereich erteilen.

Der vollständig qualifizierte Name einer Entität ist `/namespaceName/packageName/entityName`. 

### Was geschieht, wenn ich einen Namensbereich erstelle? 

In {{site.data.keyword.openwhisk_short}} erstellte Namensbereiche werden als IAM-Serviceinstanz erkannt.
Während der Erstellung eines Namensbereichs können Sie die [Ressourcengruppe](/docs/resources?topic=resources-rgs) angeben, in der die Serviceinstanz hinzugefügt werden soll.

Wenn Sie einen Namensbereich erstellen, werden die folgenden Komponenten erstellt: 

| Komponente | Beschreibung |
| --- | --- | 
| Service-ID | Sie können die Service-ID als Funktions-ID für ausgehende Aufrufe verwenden. Alle Aktionen, die in diesem Namensbereich erstellt werden, können diese Service-ID für den Zugriff auf andere Ressourcen verwenden. Dem Benutzer der Funktion wird standardmäßig die Rolle "Leseberechtigter" zugeordnet. Der Zugriff als Leseberechtigter bedeutet, dass er Namensbereichsentitäten lesen und Aktionen aufrufen kann. Die Rolle "Leseberechtigter" wird von Auslösern verwendet, um Aktionen aufzurufen. Um den eingehenden Datenverkehr zu steuern, können Sie anderen Benutzern Zugriff gewähren, wie z. B. die Zuordnung der Rolle "Leseberechtigter" zum Aufrufen von Aktionen. |
| API-Schlüssel | Ein API-Schlüssel für die Service-ID, die für die Generierung von IAM-Tokens verwendet werden kann. Sie können die Tokens verwenden, um den Namensbereich bei anderen {{site.data.keyword.cloud_notm}}-Services zu authentifizieren. Der API-Schlüssel wird für Aktionen als Umgebungsvariable `__OW_IAM_NAMESPACE_API_KEY` bereitgestellt. |

Zeigen Sie alle Ihre Service-IDs an. 
```
ibmcloud iam service-ids
```
{: pre}

Zeigen Sie die API-Schlüssel an, die einer Service-ID zugeordnet sind.  
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

Löschen Sie keine API-Schlüssel.
{: tip}

### Gibt es Einschränkungen für Namensbereiche? 

Das [Erstellen von APIs mit API-Gateway](/docs/openwhisk?topic=cloud-functions-apigateway) und das [Mobil-SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) werden für von IAM verwaltete Namensbereiche nicht unterstützt.  

Die Namen aller Entitäten, zu denen Aktionen, Auslöser, Regeln, Pakete und Namensbereiche gehören, sind eine Folge von Zeichen mit dem folgenden Format: 
* Das erste Zeichen muss ein alphanumerisches Zeichen oder ein Unterstreichungszeichen sein.
* Die nachfolgenden Zeichen können alphanumerische Zeichen, Leerzeichen oder die folgenden Zeichen sein: `_`, `@`, `.`, `-`.
* Das letzte Zeichen kann kein Leerzeichen sein.

### Wie gehe ich vor, wenn ein Cloud Foundry-basierter Namensbereich vorliegt? 

Ihre Cloud Foundry-basierten Namensbereiche funktionieren noch immer. Um neue Funktionen nutzen zu können, müssen Sie jedoch einen für IAM aktivierten Namensbereich erstellen. 


## IAM-basierten Namensbereich in der Benutzerschnittstelle erstellen
{: #create_iam_ui}

1. Klicken Sie in der [{{site.data.keyword.openwhisk_short}}-Konsole](https://cloud.ibm.com/openwhisk){: external} auf das Dropdown-Menü für den Namensbereich. 

2. Klicken Sie auf **Namensbereich erstellen**. 

3. Geben Sie einen Anzeigenamen für den Namensbereich und eine Kurzbeschreibung ein, z. B. die Arten von Aktionen oder Paketen, die in diesem Namensbereich erstellt werden sollen. 

4. Wählen Sie die Ressourcengruppe aus, in der der Namensbereich erstellen werden soll, sowie einen Standort, an dem die Namensbereichsressource bereitgestellt werden soll. 

5. Klicken Sie auf **Erstellen**. 

6. Zum Anzeigen der Serviceinstanz für die Namensbereichsressource rufen Sie das [{{site.data.keyword.cloud_notm}}-Dashboard](https://cloud.ibm.com/resources){: external} auf und suchen Sie Ihren Namensbereich im Teilfenster **Funktionsnamensbereiche**. 

Bei Bedarf können Sie den Namen oder die Beschreibung des Namensbereichs auf der Seite **Namensbereichseinstellungen** in der {{site.data.keyword.openwhisk_short}}-Konsole ändern. 

## IAM-basierten Namensbereich mit der Befehlszeilenschnittstelle erstellen
{: #namespaces_create}

Sie können einen von IAM verwalteten Namensbereich als Teil einer Ressourcengruppe erstellen und Zugriffsrichtlinien für Ihre Ressourcen verwalten, indem Sie die Ressourcengruppe als Ziel angeben, wenn ein Namensbereich erstellt wird. Wenn andere Benutzer Zugriff auf Ihren Namensbereich benötigen oder wenn Sie aus den Aktionen Ihres Namensbereichs heraus auf andere Ressourcen zugreifen möchten, stellen Sie sicher, dass Sie IAM-Richtlinien einrichten, nachdem Ihr Namensbereich erstellt wurde.
{: shortdesc}

1. Geben Sie die Ressourcengruppe als Ziel an, in der Sie den Namensbereich erstellen möchten. Wenn Sie noch keine [Ressourcengruppe](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) erstellt haben, können Sie die `Standardgruppe` als Ziel angeben.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Erstellen Sie einen für IAM aktivierten Namensbereich. Optional: Verwenden Sie das Flag `-n` oder `--description`, um eine Beschreibung für Ihren Namensbereich anzugeben. Wenn Ihre Beschreibung länger als ein Wort ist, muss sie in Anführungszeichen eingeschlossen werden. 

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>Der Anzeigename für den IAM-basierten Namensbereich.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Optional: Fügen Sie dem Namensbereich eine Beschreibung hinzu, z. B. welche Art von Aktionen oder Paketen erstellt werden soll. Wenn Ihre Beschreibung länger als ein Wort ist, muss sie in Anführungszeichen eingeschlossen werden. </td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Optional: Fügen Sie dem Namensbereich eine Beschreibung hinzu, z. B. welche Art von Aktionen oder Paketen erstellt werden soll. Wenn Ihre Beschreibung länger als ein Wort ist, muss sie in Anführungszeichen eingeschlossen werden. </td>
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
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Beispielausgabe:

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  Sie können auch alle Namensbereiche auflisten, darunter IAM-basierte und Cloud Foundry-basierte Namensbereiche.

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Bevor Sie Entitäten im Namensbereich erstellen können, müssen Sie den Kontext der Befehlszeilenschnittstelle auf den Namensbereich setzen, indem Sie diesen als Ziel angeben. 

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

Wenn Sie eine Eigenschaft wie die Eigenschaft `-- namespace` festgelegt haben, bleibt die Festlegung erhalten, bis Sie sie manuell aufheben. Wenn Sie zwischen verschiedenen IAM-Namensbereichen oder zwischen Cloud Foundry- und IAM-Namensbereichen umschalten möchten, müssen Sie die Festlegung der Namensbereichseigenschaft aufheben und die Eigenschaft erneut festlegen. Weitere Informationen siehe [`ibmcloud fn property set`].
{: note}

## Namensbereich mit der API erstellen
{: #namespaces_create_api}

Sie können einen von IAM verwalteten Namensbereich als Teil einer Ressourcengruppe erstellen und Zugriffsrichtlinien für Ihre Ressourcen verwalten, indem Sie die Ressourcengruppe als Ziel angeben, wenn ein Namensbereich erstellt wird. Wenn andere Benutzer Zugriff auf Ihren Namensbereich benötigen oder wenn Sie aus den Aktionen Ihres Namensbereichs heraus auf andere Ressourcen zugreifen möchten, stellen Sie sicher, dass Sie IAM-Richtlinien einrichten, nachdem Ihr Namensbereich erstellt wurde.
{: shortdesc}

1. Geben Sie die Ressourcengruppe als Ziel an, in der Sie den Namensbereich erstellen möchten. Wenn Sie noch keine [Ressourcengruppe](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) erstellt haben, können Sie die `Standardgruppe` als Ziel angeben.

  ```
  ibmcloud target -g default
  ```
  {: pre}

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
        <td>Ihr {{site.data.keyword.cloud_notm}} Identity and Access Management-Token (IAM). Zum Abrufen Ihres IAM-Tokens führen Sie <code>ibmcloud iam oauth-tokens</code> aus.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>Der Name des Namensbereichs.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>Die ID der Ressourcengruppe, in der der Namensbereich erstellt werden soll. Zum Anzeigen von Ressourcengruppen-IDs führen Sie <code>ibmcloud resource groups</code> aus.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>Die ID des Ressourcenplans, z. B. 'functions-base-plan'.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Optional: Fügen Sie dem Namensbereich eine Beschreibung hinzu, z. B. welche Art von Aktionen oder Paketen er enthalten wird.</td>
      </tr>
    </tbody>
  </table>

  **Beispielausgabe**

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

  **Beispielausgabe**

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

Weitere Informationen zur Arbeit mit HTTP REST finden Sie in der [Dokumentation zur {{site.data.keyword.openwhisk_short}}-API](/apidocs/functions).
{: tip}

### Auf andere Ressourcen aus einem Namensbereich zugreifen
{: #namespace-access}

Aktionen rufen normalerweise andere {{site.data.keyword.cloud_notm}}-Ressourcen und -Services auf, für die die entsprechende Authentifizierung erforderlich ist. Wenn diese Services IAM-fähig sind und IAM-Tokens akzeptieren, können Sie die Funktions-ID des Namensbereichs für die abgehende Kommunikation nutzen.
{: shortdesc}

Wie unter [IAM-Zugriff verwalten](/docs/iam?topic=iam-iammanidaccser#iammanidaccser) beschrieben, wird für jeden Namensbereich eine Service-ID erstellt, die den Namensbereich darstellt. Sie können dieser Service-ID Zugriff auf andere Services und Ressourcen gewähren, indem Sie die entsprechenden Rollen über das IAM-Richtlinienmanagement zuweisen. Weitere Informationen zum Erstellen von Service-IDs für den Zugriff auf andere IAM-fähige Services finden Sie in [Service-IDs erstellen und verwenden](/docs/iam?topic=iam-serviceids#serviceids). 

Zur Laufzeit übergibt {{site.data.keyword.openwhisk_short}} einen API-Schlüssel der Service-ID des Namensbereichs als Wert der Umgebungsvariablen `__OW_IAM_NAMESPACE_API_KEY` an den Aktionscode. Der Aktionscode kann diesen API-Schlüssel verwenden, um ein IAM-Token zu generieren. Die meisten unterstützten {{site.data.keyword.openwhisk_short}}-SDKs, wie z. B. Cloudant, {{site.data.keyword.watson}} und {{site.data.keyword.cos_full_notm}}, authentifizieren sich mit dem IAM-Schlüssel selbst. Bei anderen von IAM verwalteten Services oder Ressourcen, die eine REST-API verwenden, können Sie sich mit dem Token authentifizieren, das aus dem IAM-API-Schlüssel abgeleitet wird. Weitere Informationen finden Sie unter [IAM-Zugriffstoken für Benutzer- oder Service-ID erstellen](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Sie verstehen nicht, wie API-Schlüssel und Tokens zusammenarbeiten? Weitere Informationen finden Sie in [der IAM-Dokumentation](/docs/iam?topic=iam-iamapikeysforservices). 

## Nächste Schritte
{: #namespaces_next}

Nachdem Sie nun einen Namensbereich erstellt haben, können Sie IAM-Zugriffsrichtlinien erstellen, um ihn zu schützen. Lesen Sie zunächst [Zugriff verwalten](/docs/openwhisk?topic=cloud-functions-iam). 

Weitere Informationen zur Verwaltung von IAM-basierten Namensbereichen finden Sie in der [{{site.data.keyword.openwhisk_short}}-Referenz zu REST-APIs](/apidocs/functions). 








