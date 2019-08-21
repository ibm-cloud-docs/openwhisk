---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# Zugriffsrichtlinien festlegen
{: #iam}

## IAM-Rollen {{site.data.keyword.openwhisk_short}} zuordnen
{: #user-roles}

In {{site.data.keyword.openwhisk_short}} sind Namensbereiche {{site.data.keyword.cloud_notm}}-Ressourcen, die Sie für die Arbeit mit IAM-Rollen und Richtlinien für das Zugriffsmanagement verwenden können. Alle Richtlinien, die Sie für einen Namensbereich festlegen, gelten auch für die {{site.data.keyword.openwhisk_short}}-Entitäten, z. B. Aktionen oder Auslöser, die der Namensbereich enthält.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} verwendet sowohl die Plattform- als auch die Servicemanagementrollen. Sie können mit Richtlinien festlegen, wer Namensbereiche auf der Plattformebene erstellen kann. Außerdem können Sie die Servicerollen für die Verwaltung der Interaktion mit den Namensbereichen selbst verwenden. 

Wünschen Sie weitere Informationen zu zentralen IAM-Konzepten? Lesen Sie die [IAM-Dokumente](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Plattformmanagementrollen

Die folgende Tabelle enthält detaillierte Informationen zu den Aktionen, die Plattformmanagementrollen zugeordnet sind. Plattformmanagementrollen ermöglichen Benutzern, Tasks für Serviceressourcen auf Plattformebene auszuführen. Weisen Sie beispielsweise Benutzerzugriff für den Service zu, erstellen oder löschen Sie Service-IDs, erstellen Sie Instanzen und binden Sie Instanzen an Anwendungen.
{: shortdesc}

Weitere Informationen zum Zuordnen, Bearbeiten, Überprüfen oder Löschen von Ressourcenzugriffsrichtlinien finden Sie in [IAM-Zugriff verwalten](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}

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
      <td>Benutzer können Namensbereiche erstellen. Die Administratorrolle ist erforderlich, um die Operationen `service id` und `apikey lock` bei der Erstellung des Service ausführen zu können. </td>
    </tr>
  </tbody>
</table>

Da der Service nicht bereitgestellt werden muss, ist die Bearbeiterrolle die einzige Plattformrolle, die Sie für die Arbeit mit dem Service benötigen. Weitere Informationen zu den übrigen Rollen finden Sie in [Plattformmanagementrollen](/docs/iam?topic=iam-userroles). 

</br>

### Servicespezifische Rollen
{: #service_specific_roles}

Servicespezifische Rollen bestimmen den Geltungsbereich einer Zugriffsrichtlinie innerhalb eines bestimmten Service. Für {{site.data.keyword.openwhisk_short}} können die Rollen die Fähigkeit eines Benutzers steuern, den Service zu verwenden, z. B. auf die Benutzerschnittstelle zuzugreifen oder API-Aufrufe auszuführen.
{: shortdesc}

Berechtigungen bauen aufeinander auf. Beispielsweise können alle Operationen, die die Rolle `writer` ausführen kann, auch von der Rolle `manager` ausgeführt werden. Dabei hätte jedoch die Rolle `manager` noch zusätzliche Berechtigungen. Die allgemeinen Berechtigungen für die einzelnen Rollen finden Sie unter [Servicezugriffsrollen](/docs/iam?topic=iam-userroles).

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
    <td>Aktualisieren eines Namensbereichs.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Löschen eines Namensbereichs.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Anzeigen des verfügbaren Namensbereichs.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Erstellen einer Entität, z. B. einer Aktion, in einem Funktionsnamensbereich.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Aktualisieren einer Entität, z. B. eines Pakets, in einem Funktionsnamensbereich.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Löschen einer Entität, z. B. eines Auslösers, aus einem Funktionsnamensbereich.</td>
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
    <td>Aktivieren einer Entität, z. B. einer Aktion, in einem Namensbereich.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature verfügbar" style="width:32px;" /></td>
  </tr>
</table>

</br>

### Richtlinien über die Befehlszeilenschnittstelle festlegen
{: #cli-set}

Wenn Sie einer Ressource wie einer Aktion in einem IAM-basierten Namensbereich Zugriff auf einen IAM-basierten Service erteilen möchten, können Sie eine IAM-Zugriffsrichtlinie für den Namensbereich erstellen, in dem die Ressource enthalten ist. 

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu den Komponenten des Befehls <code>ibmcloud iam service-policy-create</code></th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>Die Service-ID für den Namensbereich. Führen Sie den Befehl <code>ibmcloud iam service-ids</code> aus, um alle Service-IDs anzuzeigen. </td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>Der Typ der IAM-Servicezugriffsrolle, die die Aktion für die Verwendung des Zielservice haben muss. Führen Sie den Befehl <code>ibmcloud iam roles --service SERVICE_NAME</code> aus, um die unterstützten Rollen für den anderen Service anzuzeigen. Weitere Informationen finden Sie in [IAM-Zugriffsrollen](/docs/iam?topic=iam-userroles#service-access-roles). </td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>Der Name des anderen {{site.data.keyword.cloud_notm}}-Servicetyps. </td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>Die GUID der anderen Serviceinstanz, auf die die Aktion Zugriff haben soll. Führen Sie den Befehl <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code> aus, um die GUID der Serviceinstanz abzurufen. </td>
    </tr>
  </tbody>
</table>

</br>

**Nächste Schritte**
Weitere Informationen zum Verwalten von Serviceberechtigungsnachweisen finden Sie im Blogbeitrag zur [Verwaltung von Serviceberechtigungsnachweisen für serverunabhängige Anwendungen](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}. 



