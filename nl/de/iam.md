---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# Zugriff verwalten
{: #iam}

{{site.data.keyword.openwhisk}} unterstützt Identity and Access Management (IAM). Sie können jetzt IAM-Richtlinien für Ihre Ressourcen, z. B. Namensbereiche, definieren.
{: shortdesc}

IAM-Richtlinien sind in der Region 'Tokio' (Tokyo) nur für {{site.data.keyword.openwhisk_short}} verfügbar. Wenn Sie in der Region 'Tokio' tätig sind, müssen Sie IAM-Richtlinien verwenden, um den Zugriff zu steuern.
{: tip}

</br>

## IAM-Rollen {{site.data.keyword.openwhisk_short}} zuordnen
{: #user-roles}

In {{site.data.keyword.openwhisk_short}} werden Namensbereiche als IBM Cloud-Ressource betrachtet, die es Ihnen ermöglicht, mit IAM-Rollen und -Richtlinien für das Zugriffsmanagement zu arbeiten. Alle Richtlinien, die Sie für einen Namensbereich festlegen, gelten auch für die {{site.data.keyword.openwhisk_short}}-Entitäten, z. B. Aktionen oder Auslöser, die der Namensbereich enthält.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} verwendet sowohl die Plattform- als auch die Servicemanagementrollen. Sie können Richtlinien dafür, wer Namensbereiche erstellen kann, auf Plattformebene festlegen, während Sie die Servicerollen verwenden, um Interaktionen mit den Namensbereichen selbst zu verwalten. 

Wünschen Sie weitere Informationen zu zentralen IAM-Konzepten? Lesen Sie die [IAM-Dokumente](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Plattformmanagementrollen

Die folgende Tabelle enthält detaillierte Informationen zu den Aktionen, die Plattformmanagementrollen zugeordnet sind. Plattformmanagementrollen ermöglichen Benutzern, Tasks für Serviceressourcen auf Plattformebene auszuführen. Weisen Sie beispielsweise Benutzerzugriff für den Service zu, erstellen oder löschen Sie Service-IDs, erstellen Sie Instanzen und binden Sie Instanzen an Anwendungen.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Plattformrolle</th>
      <th>Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Administrator</td>
      <td>Benutzer können Namensbereiche erstellen. </td>
    </tr>
  </tbody>
</table>

Sie müssen über die Administratorrolle für das Plattformmanagement verfügen, um mit dem Service zu arbeiten. Weitere Informationen zu den Rollen finden Sie unter [Plattformmanagementrollen](/docs/iam?topic=iam-userroles). 

</br>

### Servicespezifische Rollen

Servicespezifische Rollen bestimmen den Geltungsbereich einer Zugriffsrichtlinie mit einem spezifischen Service. Für {{site.data.keyword.openwhisk_short}} können die Rollen die Fähigkeit eines Benutzers steuern, den Service zu verwenden, z. B. auf die Benutzerschnittstelle zuzugreifen oder API-Aufrufe auszuführen.
{: shortdesc}


Beachten Sie, dass die Berechtigungen aufeinander basieren. Beispielsweise können alle Operationen, die die Rolle `writer` ausführen kann, auch von der Rolle `manager` ausgeführt werden. Dabei hätte jedoch die Rolle `manager` noch zusätzliche Berechtigungen. Die allgemeinen Berechtigungen für die einzelnen Rollen finden Sie unter [Servicezugriffsrollen](/docs/iam?topic=iam-userroles). 

In der folgenden Tabelle können Sie sehen, welche Rollen für die verschiedenen Operationen erforderlich sind: 

<table><caption>Welche Rollen können welche Operationen ausführen?</caption>
  <tr>
    <th style="width:150px">Aktion</th>
    <th style="width:2500px">Beschreibung</th>
    <th style="width:50px">Leseberechtigter</th>
    <th style="width:50px">Schreibberechtigter</th>
    <th style="width:50px">Manager</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Aktualisieren eines Namensbereichs. </td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Löschen eines Namensbereichs. </td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Anzeigen des verfügbaren Namensbereichs. </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Erstellen einer Entität, z. B. einer Aktion, in einem Funktionsnamensbereich. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Aktualisieren einer Entität, z. B. eines Pakets, in einem Funktionsnamensbereich. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Löschen einer Entität, z. B. eines Auslösers, aus einem Funktionsnamensbereich. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Anzeigen der verfügbaren Entitäten, z. B. Regeln, in einem Namensbereich. </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Aktivieren einer Entität, z. B. einer Aktion, in einem Namensbereich. </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
</table>

Weitere Informationen zum Zuweisen von Benutzerrollen in der Benutzerschnittstelle finden Sie unter [IAM-Zugriff verwalten](/docs/iam?topic=iam-iammanidaccser#iammanidaccser). 

</br>


## IAM-Zugriffsrichtlinien festlegen
{: #set-iam}

Wenn ein Service eine Aktion aufruft, hat die Aktion eine Antwort. Da die Antwort von dem Namensbereich oder der Aktion an einen Service gesendet wird, wird sie als ausgehende Information betrachtet. Wenn Sie die Auswirkungen, die Ihr Namensbereich auf andere Services hat, begrenzen möchten, können Sie eine Zugriffsrichtlinie erstellen.
{: shortdesc}

Weitere Informationen zum Zuordnen, Bearbeiten, Überprüfen oder Löschen von Ressourcenzugriffsrichtlinien finden Sie unter [IAM-Zugriff verwalten](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Auf andere Ressourcen aus einem Namensbereich zugreifen
{: #namespace-access}

Sie können aus einem von IAM verwalteten Namensbereich mithilfe eines IAM-Tokens auf andere Ressourcen zugreifen. Ein Token stellt die Authentifizierung dar und überprüft die Identität der Ressource. Das IAM-Token ist für die Authentifizierung beim Zugriff auf von IAM verwaltete Services oder Ressourcen erforderlich.
{: shortdesc}

Ähnlich dazu, wie eine Benutzer-ID einen Benutzer identifiziert, stellt eine Service-ID eine bestimmte Ressource dar. Das heißt, dass IAM-Richtlinien auf solche Ressourcen angewendet werden können, die Zugriffsberechtigungen verwalten. Genau wie ein Benutzer muss sich eine Ressource authentifizieren, um ihre Identität zu bestätigen. Innerhalb von Functions kann dies von der Implementierung der Aktionen beim Zugriff auf andere Services oder Ressourcen genutzt werden. 

Wenn Sie einen neuen von IAM-verwalteten Namensbereich erstellen, erstellt Functions automatisch eine entsprechende Service-ID, die den Namensbereich und einen API-Schlüssel angibt. Zur Laufzeit übergibt Cloud Functions den API-Schlüssel an den Aktionscode als Wert der Umgebungsvariablen `__OW_IAM_NAMESPACE_API_KEY`. Der Aktionscode kann diesen API-Schlüssel verwenden, um ein IAM-Token zu generieren. Die meisten unterstützten SDKs, z. B. Cloudant, Watson und COS, authentifizieren sich mit dem IAM-Schlüssel selbst. Andere von IAM verwaltete Services oder Ressourcen, die eine REST-API verwenden, authentifizieren sich mit dem Token, das aus dem IAM-Schlüssel abgeleitet wird. 

Sie sind sich nicht ganz sicher, wie API-Schlüssel und -Token zusammenpassen? Weitere Informationen finden Sie in der [Dokumentation zu IAM](/docs/iam?topic=iam-iamapikeysforservices). 
