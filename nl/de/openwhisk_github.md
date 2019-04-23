---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: github, actions, trigger, event

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# GitHub-Ereignisquelle
{: #openwhisk_catalog_github}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar.
{: tip}

Das Paket `/whisk.system/github` bietet eine komfortable Methode zur Verwendung der [GitHub-APIs](https://developer.github.com/).
{: shortdesc}

Das Paket enthält den folgenden Feed:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/github` | Paket | username, repository, accessToken | Interaktion mit der GitHub-API |
| `/whisk.system/github/webhook` | Feed | events, username, repository, accessToken | Aktivieren eines Auslöserereignisses bei GitHub-Aktivität |

Es wird empfohlen, eine Paketbindung mit den Werten `username`, `repository` und `accessToken` zu erstellen.  Mit der Bindung brauchen Sie die Werte nicht jedes Mal anzugeben, wenn Sie den Feed im Paket verwenden.

## Auslöserereignis für GitHub-Aktivität aktivieren

Der Feed `/whisk.system/github/webhook` konfiguriert einen Service so, dass ein Auslöser aktiviert wird, wenn eine Aktivität in einem angegebenen GitHub-Repository stattfindet. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für das GitHub-Repository.
- `repository`: Das GitHub-Repository.
- `accessToken`: Ihr persönliches GitHub-Zugriffstoken. Wenn Sie Ihr [Token erstellen](https://github.com/settings/tokens), stellen Sie sicher, dass Sie die Geltungsbereiche **repo:status** und **public_repo** auswählen. Stellen Sie außerdem sicher, dass noch keine Webhooks für Ihr Repository definiert sind.
- `events`: Der interessierende [GitHub-Ereignistyp](https://developer.github.com/v3/activity/events/types/).

Im folgenden Beispiel wird ein Auslöser erstellt, der bei jeder neuen Festschreibung im GitHub-Repository aktiviert wird.

1. Generieren Sie ein [persönliches Zugriffstoken](https://github.com/settings/tokens) für GitHub. Das Zugriffstoken wird im nächsten Schritt verwendet.

2. Erstellen Sie eine Paketbindung, die für Ihr GitHub-Repository und mit Ihrem Zugriffstoken konfiguriert ist.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Erstellen Sie einen Auslöser für den GitHub-Ereignistyp `push` unter Verwendung Ihres Feeds `myGit/webhook`.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  Eine Festschreibung (Commit) im GitHub-Repository durch `git push` führt dazu, dass der Auslöser durch den Webhook aktiviert wird. Wenn eine Regel auf den Auslöser zutrifft, wird die zugeordnete Aktion aufgerufen. Von der Aktion werden die Nutzdaten für den GitHub-Webhook als Eingabeparameter empfangen. Jedes GitHub-Webhook-Ereignis weist ein ähnliches JSON-Schema auf, stellt jedoch ein eindeutiges Nutzdatenobjekt dar, das vom jeweiligen Ereignistyp abhängt. Weitere Informationen zum Nutzdateninhalt finden Sie in der API-Dokumentation unter [GitHub-Ereignisse und -Nutzdaten](https://developer.github.com/v3/activity/events/types/).
