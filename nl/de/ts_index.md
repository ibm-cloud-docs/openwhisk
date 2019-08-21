---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Fehlerbehebung
{: #troubleshooting}

Wenn bei der Arbeit mit {{site.data.keyword.openwhisk}} Fehler auftreten, kommen diese Methoden zur Fehlerbehebung und zum Anfordern von Hilfe infrage.
{: shortdesc}



## Aktion ist fehlgeschlagen
{: #ts_action_fails}

{: tsSymptoms}
Die Aktion ist fehlgeschlagen.

{: tsCauses}
Ursache für das Fehlschlagen einer App kann sein, dass eine veraltete Laufzeit verwendet wird. Eine Aktion kann erst erfolgreich abgeschlossen werden, wenn die Laufzeit auf eine unterstützte Version aktualisiert wird.

{: tsResolve}
Wenn Sie sehen möchten, welche Laufzeitkomponente für die Aktion verwendet wird, führen Sie `ibmcloud fn action get ACTION_NAME` aus und suchen Sie in der Abfrageantwort nach `deprecated=true`. Wenn die Laufzeit veraltet ist, [aktualisieren Sie die Laufzeit](/docs/openwhisk?topic=cloud-functions-actions#actions_update).



## Systemlimits wurden erreicht
{: #ts_limit_reached}

{: tsSymptoms}
Ihre Funktion wird nicht ordnungsgemäß ausgeführt und bewirkt möglicherweise die Ausgabe einer Nachricht wie z. B. `{"error":"signal: killed"}`.

{: tsCauses}
Für alle Funktionen gelten Mindest- und Höchstgrenzen ([Systemgrenzwerte](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)), wie z. B. die maximale Speicherbelegung oder Zeitlimits.

{: tsResolve}
Manche Grenzwerte können erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsgrenzwerte rechtfertigt. Wenn Sie einen Grenzwert erhöhen möchten, wenden Sie sich an IBM Support, indem Sie ein Ticket direkt über die IBM Cloud Functions-Webkonsole öffnen.

1. Wählen Sie **Support**aus.
2. Wählen Sie **Ticket hinzufügen** im Dropdown-Menü aus.
3. Wählen Sie **Technisch** als Tickettyp aus.
4. Wählen Sie **Funktionen** als technischen Bereich der Unterstützung aus.



## Hilfe und Unterstützung anfordern
{: #gettinghelp}

Sie haben noch Probleme mit Ihrer Funktion?
{: shortdesc}

-   Um zu prüfen, ob {{site.data.keyword.cloud_notm}} verfügbar ist, [rufen Sie die {{site.data.keyword.cloud_notm}}-Statusseite auf](https://cloud.ibm.com/status?selected=status){: external}. 
-   Sehen Sie in den Foren nach, ob andere Benutzer dasselbe Problem hatten. Wenn Sie die Foren verwenden, um eine Frage zu stellen, taggen Sie Ihre Frage so, dass sie von den {{site.data.keyword.cloud_notm}}-Entwicklungsteams gefunden wird.
    -   Wenn Sie technische Fragen zum Entwickeln von Funktionen mit {{site.data.keyword.openwhisk}} haben, posten Sie Ihre Frage auf [Stack Overflow](https://stackoverflow.com/search?q=ibm-cloud-functions){: external} und kennzeichnen Sie Ihre Frage mit dem Tag `ibm-cloud-functions`. 
    -   Wenn Sie Fragen zum Service haben und einführende Informationen wünschen, verwenden Sie das Forum [IBM Developer Answers](https://developer.ibm.com/answers/topics/functions){: external}. Fügen Sie die Tags `ibm-cloud` und `functions` ein.
    Weitere Informationen zur Verwendung der Foren finden Sie im Abschnitt [Hilfe aufrufen](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Wenden Sie sich an den IBM Support, indem Sie einen Fall öffnen. Weitere Informationen zum Öffnen eines IBM Support-Falls oder zu Support-Levels und Fallprioritäten finden Sie unter [Support kontaktieren](/docs/get-support?topic=get-support-getting-customer-support).
Wenn Sie ein Problem melden, geben Sie auch Ihre Aktivierungs-ID an. Um eine Aktivierungs-ID zu erhalten, führen Sie `ibmcloud fn activation list` aus.
{: tip}

