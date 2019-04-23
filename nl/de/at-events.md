---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}


# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse anzeigen
{: #activity_tracker}

Sie können vom Benutzer initiierte Aktivitäten anzeigen, verwalten und überwachen, die in Ihrer {{site.data.keyword.openwhisk}}-Serviceinstanz ausgeführt werden, indem Sie den {{site.data.keyword.cloudaccesstrailshort}}-Service verwenden.
{: shortdesc}


Weitere Informationen zur Funktionsweise dieses Service finden Sie in der [{{site.data.keyword.cloudaccesstrailshort}}-Dokumentation](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla). 


## Anzeigen von Ereignissen
{: #view}

Ereignisse sind in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** verfügbar, die sich in der {{site.data.keyword.Bluemix_notm}}-Region befindet, in der die {{site.data.keyword.openwhisk_short}}-Namensbereichsressource verfügbar ist. Weitere Informationen finden Sie unter [Kontoereignisse anzeigen](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events). 

1. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an. 
2. Stellen Sie im Katalog eine Instanz des {{site.data.keyword.cloudaccesstrailshort}}-Service in demselben Konto wie Ihre Instanz von {{site.data.keyword.openwhisk}} bereit. 
3. Klicken Sie auf der Registerkarte **Verwalten** des {{site.data.keyword.cloudaccesstrailshort}}-Dashboards auf **In Kibana anzeigen**. 
4. Wählen Sie den Zeitrahmen aus, für den Sie Protokolle anzeigen möchten. Der Standardwert ist 15 Minuten. 
5. Klicken Sie in der Liste **Verfügbare Felder** auf **Typ**. Klicken Sie auf das Lupensymbol für **Activity Tracker**, um nur solche Protokolle anzuzeigen, die von dem Service verfolgt werden. 
6. Sie können die anderen verfügbaren Felder nutzen, um die Suche weiter einzugrenzen. 

Damit andere Benutzer als der Kontoeigner Protokolle anzeigen können, müssen Sie den Premium-Plan verwenden. Um andere Benutzer Ereignisse anzeigen zu lassen, lesen Sie den Abschnitt [Berechtigungen zum Anzeigen von Kontoereignissen erteilen](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## Ereignisliste
{: #events}

In der folgenden Tabelle finden Sie eine Liste der Ereignisse, die an {{site.data.keyword.cloudaccesstrailshort}} gesendet werden.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Aktion</th>
      <th>Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Erstellen einer IAM-basierten Namensbereichressource</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migrieren eines Cloud Foundry-basierten Namensbereichs, um ihn IAM-fähig zu machen </td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Aktualisieren von IAM-basierten Namensbereichsressourceneigenschaften wie den Anzeigenamen oder die Beschreibung </td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Löschen einer Namensbereichsressource </td>
    </tr>
  </tbody>
</table>
