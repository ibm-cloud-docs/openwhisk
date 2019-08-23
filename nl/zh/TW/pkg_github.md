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

在東京地區無法使用此預先安裝的套件。
{: tip}

`/whisk.system/github` 套件提供一種簡便的方來使用 [GitHub API](https://developer.github.com/){: external}。
{: shortdesc}

GitHub 包中包含下列實體。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/github` |套件|`username`、`repository`、`accessToken`|與 GitHub API 互動。|
|`/whisk.system/github/webhook` |資訊來源|`events`、`username`、`repository`、`accessToken`|對 GitHub 活動發動觸發程式事件。|

建議使用 `username`、`repository` 及 `accessToken` 值來建立套件連結。使用連結，您就不需要每次使用套件中的資訊來源時都指定值。

## 發動 GitHub 活動的觸發程式事件

`/whisk.system/github/webhook` 資訊來源會配置服務在指定 GitHub 儲存庫中有活動時發動觸發程式。參數如下。

| 參數 |說明|
| --- | --- |
| `username` |GitHub 儲存庫的使用者名稱。|
| `repository` |GitHub 儲存庫。|
| `accessToken` |GitHub 個人存取記號。建立記號時，請確保選取 `repo:status` 和 `public_repo` 範圍。同時，請確定您尚未定義儲存庫的任何 Webhook。|
| `events` |感興趣的 [GitHub 事件類型 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.github.com/v3/activity/events/types/)。|

在下列範例中，建立的觸發程式會在每次新確定至 GitHub 儲存庫時發動。

1. 產生 GitHub 個人存取記號。導覽至 **GitHub.com** > **設定** > **個人存取記號**以產生記號。下一步將會使用存取記號。

2. 建立使用存取記號針對 GitHub 儲存庫而配置的套件連結。
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. 使用 `myGit/webhook` 資訊來源，來建立 GitHub `push` 事件類型的觸發程式。
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  使用 `git push` 來確定 GitHub 儲存庫時，會導致 Webhook 發動觸發程式。如果規則符合觸發程式，則會呼叫相關聯的動作。此動作會接收 GitHub Webhook 有效負載作為輸入參數。每一個 GitHub Webhook 事件的 JSON 綱目都類似，但是為其事件類型所決定的唯一有效負載物件。如需有效負載內容的相關資訊，請參閱 [GitHub 事件和有效負載](https://developer.github.com/v3/activity/events/types/){: external} API 文件。



