---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: github, actions, trigger, event, functions

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


# GitHub
{: #pkg_github}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar.
{: tip}

Das Paket `/whisk.system/github` bietet eine komfortable Methode zur Verwendung der [GitHub-APIs](https://developer.github.com/){: external}.
{: shortdesc}

Das GitHub-Paket enthält die folgenden Entitäten. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/github` | Paket | `username`, `repository`, `accessToken` | Mit der GitHub-API interagieren. |
| `/whisk.system/github/webhook` | Feed | `events`, `username`, `repository`, `accessToken` | Auslöserereignisse für GitHub-Aktivität aktivieren. |

Es wird empfohlen, eine Paketbindung mit den Werten `username`, `repository` und `accessToken` zu erstellen.  Mit der Bindung brauchen Sie die Werte nicht jedes Mal anzugeben, wenn Sie den Feed im Paket verwenden.

## Auslöserereignis für GitHub-Aktivität aktivieren

Der Feed `/whisk.system/github/webhook` konfiguriert einen Service so, dass ein Auslöser aktiviert wird, wenn eine Aktivität in einem angegebenen GitHub-Repository stattfindet. Die Parameter lauten wie folgt. 

| Parameter | Beschreibung |
| --- | --- |
| `username` | Der Benutzername des GitHub-Repositorys. |
| `repository` | Das GitHub-Repository. |
| `accessToken` | Ihr persönliches GitHub-Zugriffstoken. Stellen Sie sicher, dass Sie bei der Erstellung Ihres Tokens die Bereiche `repo:status` und `public_repo` auswählen. Stellen Sie außerdem sicher, dass noch keine Webhooks für Ihr Repository definiert sind. |
| `events` | Der jeweilige [GitHub-Ereignistyp![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.github.com/v3/activity/events/types/). |

Im folgenden Beispiel wird ein Auslöser erstellt, der bei jeder neuen Festschreibung im GitHub-Repository aktiviert wird.

1. Generieren Sie ein persönliches GitHub-Zugriffstoken. Navigieren Sie zu **GitHub.com** > **Einstellungen** > **Persönliche Zugriffstoken**, um ein Token zu generieren. Das Zugriffstoken wird im nächsten Schritt verwendet.

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

  Eine Festschreibung (Commit) im GitHub-Repository durch `git push` führt dazu, dass der Auslöser durch den Webhook aktiviert wird. Wenn eine Regel auf den Auslöser zutrifft, wird die zugeordnete Aktion aufgerufen. Von der Aktion werden die Nutzdaten für den GitHub-Webhook als Eingabeparameter empfangen. Jedes GitHub-Webhook-Ereignis weist ein ähnliches JSON-Schema auf, stellt jedoch ein eindeutiges Nutzdatenobjekt dar, das vom jeweiligen Ereignistyp abhängt. Weitere Informationen zum Nutzdateninhalt finden Sie in der API-Dokumentation unter [GitHub-Ereignisse und -Nutzdaten](https://developer.github.com/v3/activity/events/types/){: external}.



