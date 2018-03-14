---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 使用動作中的服務
{: #binding_services}

您可以利用 [{{site.data.keyword.openwhisk}} CLI 外掛程式](./bluemix_cli.html)來將服務連結至動作。{{site.data.keyword.openwhisk_short}} 提供 `service bind` 指令，讓您的 {{site.data.keyword.Bluemix}} 服務認證可供您的「雲端功能」程式碼在執行時期使用。`service bind` 指令不會與 Cloud Foundry 中可用的 `cf bind-service` 指令混淆。它只是一種自動化方式，可在包含服務認證的現有「動作」上建立新參數。{{site.data.keyword.openwhisk_short}} `service bind` 指令更有彈性，可讓您將任何 {{site.data.keyword.Bluemix_notm}} 服務連結至 {{site.data.keyword.openwhisk_short}} 中所定義的任何「動作」。唯一要注意的是您必須為要連結的服務定義認證。
{: shortdesc}

## 如何將服務連結至動作
{: #cli_bind}

使用 [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) CLI 外掛程式所提供的 `bx wsk service bind` 指令，將服務連結至「動作」。您可以在[限制](./binding_services.html#limitations)一節中找到其他資訊。

`bind` 的使用語法：
```
bx wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

`service bind` 指令需要服務類型，以及要連結的「動作」名稱。例如，若您要將 Watson Conversation 服務連結至名為 `hello` 的「動作」，則呼叫會類似下列指令：
```
bx wsk service bind conversation hello
```
{: pre}

這會產生下列輸出：
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

此指令會搜尋現有 Watson Conversation 服務的現行空間、取得它找到的第一個 Conversation 服務，然後擷取屬於此服務的所有認證。使用屬於此服務的第一組認證，它會將那些認證作為參數連結至指定的「`hello` 動作」。輸出會確切顯示出「動作」所連結的服務，以及該服務用來與之連結的一組認證。

若要驗證認證是否已順利連結，請發出下列指令：
```
bx wsk action get hello parameters
```
{: pre}

輸出範例：
```
ok: got action Hello World
{
    "parameters": [
        {
            "key": "var1",
            "value": "val1"
        },
        {
            "key": "dog",
            "value": "cat"
        },
        {
            "key": "__bx_creds",
            "value": {
                "conversation": {
                    "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                },
            }
        }
    ],
}
```

從這裡，您可以看到此 Conversation 服務的認證（及其他服務類型的任何其他認證）屬於名為 `__bx_creds` 的參數，現在可以從「動作」碼中使用該參數，就像可以使用任何其他已連結的參數一樣。「動作」會挑選第一個可用的 Conversation 服務，其中包括定義在該服務中的第一組認證。 

如需將參數傳遞給「動作」的進一步資訊，以及執行 `action update` 作業時對認證有何影響，請參閱下列文件：[建立並呼叫動作](openwhisk_actions.html#openwhisk_pass_params)。

`wsk service` 指令支援下列兩個旗標：

<dl>
    <dt>--instance</dt>
    <dd>特定服務的名稱，其服務類型為您想要使用的類型。</dd>
    <dt>--keyname</dt>
    <dd>特定認證的名稱，其在您想要使用的服務內。</dd>
</dl>

若要瞭解如何使用這些旗標，請參閱下列範例。使用前一個 `bx wsk service bind` 指令，假設實際上有兩個 Conversation 服務，且「動作」預設值已結束連結不正確的服務/認證。您可以使用 `--instance` 和 `--keyname` 旗標來重新執行指令，以確定您將正確的服務連結至正確的「動作」。首先，請查看可用的服務，以及這些服務連結了哪些認證。如果要列出我們的服務，應該看到類似下列的輸出：

```
bx service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

從這個輸出中，我們看到 **Conversation-qp** 是列出的兩個服務中的第一個，且它是起始 `bx wsk service bind conversation hello` 指令已結束連結的服務。或許您想要改為連結至 **Conversation-uc** 服務。因此，若要絕對確定，您可以檢查 **Conversation-uc** 所包含的認證，以確保您使用一組正確的認證來連結。

```
bx service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

您想要從這個服務連結至 "Credentials-2"。若要確保「動作」執行想要的行為，請執行下列指令：
```
bx wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

這會產生下列輸出：
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

從輸出中，您可以看到一組正確的認證已連結至「動作」。同樣地，若要驗證，您可以查看下列 `bx wsk action get` 指令。
```
bx wsk action get hello parameters
```
{: pre}

這會產生下列結果：
```
ok: got action Hello World
{
    "parameters": [
        {
            "key": "var1",
            "value": "val1"
        },
        {
            "key": "dog",
            "value": "cat"
        },
        {
            "key": "__bx_creds",
            "value": {
                "conversation": {
                    "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

支援一般除錯旗標，且會從呼叫中印出回應標頭。

## 如何從動作取消連結服務
{: #cli_unbind}

使用 `bx wsk service unbind`，從「動作」取消連結服務。`service unbind` 指令會移除 `service bind` 指令所建立的現有連結。

`unbind` 的使用語法：
```
bx wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## 限制
{: #limitations}

「`service` 動作」不支援任何自訂旗標，但支援一般除錯及詳細旗標。「動作」會尋找 `__bx_creds` 連結參數，並移除所列服務類型的參照。如果該服務類型是唯一列出的，則「動作」會將 `__bx_creds` 參數值設為空值。如果有多個服務連結到「動作」，則會保留 `__bx_creds` 參數與任何仍連結的服務。

每一種類型只能有一個服務連結到「動作」。不支援在單一「動作」內連結相同類型的多個服務。
{: tip}

