---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}} pacote

O pacote do {{site.data.keyword.mobilepushshort}} fornece um conjunto de ações para interagir com as instâncias de serviço do {{site.data.keyword.mobilepushfull}}. Essas ações permitem que você envie uma mensagem ou crie, atualize ou exclua um webhook.
{: shortdesc}

O pacote do  {{site.data.keyword.mobilepushshort}}  inclui as ações a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/push-notifications` | pacote | apikey, appGuid | Trabalhe com uma instância do {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | ação | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Envie notificação push para um ou mais dispositivos especificados. |
| `/push-notifications/webhook` | ação | eventos | Dispare eventos acionadores em atividades de dispositivo (registro de dispositivo, remoção de registro, assinatura ou cancelamento de assinatura) no serviço de Push. |

## Criando uma instância de serviço do  {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Antes de instalar o pacote, deve-se criar uma instância do {{site.data.keyword.mobilepushshort}}.

1. [Crie uma instância de serviço do {{site.data.keyword.mobilepushshort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Crie um conjunto de credenciais de serviço ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) para a instância de serviço de Notificações push.

3. [Configure a instância de serviço {{site.data.keyword.mobilepushshort}}![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

## Instalando o pacote do  {{site.data.keyword.mobilepushshort}}
{: #install_push}

Depois de ter uma instância de serviço do {{site.data.keyword.mobilepushshort}}, use a CLI ou a UI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.mobilepushshort}} em seu namespace.

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #install_cli}

Antes de Iniciar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar o pacote do  {{site.data.keyword.mobilepushshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.mobilepushshort}} .
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Navegue para o diretório `runtimes / nodejs`.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Implemente o pacote.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Verifique se o pacote `push-notifications` está incluído em sua lista de pacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Saída:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Ligar as credenciais da instância de serviço do {{site.data.keyword.mobilepushshort}} que você criou para o pacote.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #install_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando o alternador de namespace no canto superior direito, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_short}}.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **IBM {{site.data.keyword.mobilepushshort}}** e, em seguida, clique no pacote **IBM {{site.data.keyword.mobilepushshort}}**.

5. Na seção Tempos de Execução Disponíveis, selecione NodeJS a partir da lista suspensa e, em seguida, clique em **Instalar**.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página Ações e poderá procurar por seu novo pacote, que é denominado **push-notifications**.

7. Para usar as ações no pacote **push-notifications**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação no pacote **push-notifications** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.mobilepushshort}}
{: #usage_push}

### Enviar uma notificação push
{: #push_notif}

Para enviar uma mensagem por meio do serviço de notificação push usando a ação `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

Exemplo de Saída:
```
{
  "response": {
      "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

## Criar um webhook
{: #webhook}

Para criar um webhook para o serviço {{site.data.keyword.mobilepushshort}} para eventos onDeviceRegister:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

Exemplo de Saída:
```
{
  "response": {
    "result": {
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}
