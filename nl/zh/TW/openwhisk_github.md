---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 GitHub 套件
{: #openwhisk_catalog_github}

`/whisk.system/github` 套件提供一種簡便的方來使用 [GitHub API](https://developer.github.com/)。
{: shortdesc}

該套件包含下列資訊來源：

| 實體| 類型| 參數| 說明
|
| --- | --- | --- | --- |
| `/whisk.system/github` | 套件| username、repository、accessToken| 與 GitHub API 互動|
| `/whisk.system/github/webhook` | 資訊來源| events、username、repository、accessToken| 在 GitHub 活動上發動觸發程式事件|

建議使用 `username`、`repository` 及 `accessToken` 值來建立套件連結。使用連結，您就不需要每次使用套件中的資訊來源時都指定值。

## 發動 GitHub 活動的觸發程式事件

`/whisk.system/github/webhook` 資訊來源會配置服務在指定 GitHub 儲存庫中有活動時發動「觸發程式」。參數如下所示：

- `username`：GitHub 儲存庫的使用者名稱。
- `repository`：GitHub 儲存庫。
- `accessToken`：GitHub 個人存取記號。當您[建立您的記號](https://github.com/settings/tokens)時，請務必選取 **repo:status** 和 **public_repo** 範圍。同時，請確定您尚未定義儲存庫的任何 Webhook。
- `events`：感興趣的 [GitHub 事件類型](https://developer.github.com/v3/activity/events/types/)。

在下列範例中，建立的「觸發程式」會在每次新確定至 GitHub 儲存庫時發動。

1. 產生 GitHub [個人存取記號](https://github.com/settings/tokens)。
  
  下一步將會使用存取記號。
  
2. 建立使用存取記號針對 GitHub 儲存庫而配置的套件連結。
  
  ```
  wsk package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}
  
3. 使用 `myGit/webhook` 資訊來源，以建立 GitHub `push` 事件類型的「觸發程式」。
  
  ```
wsk trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}
  
  使用 `git push` 來確定 GitHub 儲存庫時，會導致 Webhook 發動「觸發程式」。如果「規則」符合「觸發程式」，則會呼叫相關聯的「動作」。
此「動作」會接收 GitHub Webhook 有效負載作為輸入參數。每一個 GitHub Webhook 事件的 JSON 綱目都類似，但是為其事件類型所決定的唯一有效負載物件。
如需有效負載內容的相關資訊，請參閱 [GitHub 事件和有效負載](https://developer.github.com/v3/activity/events/types/) API 文件。
  
