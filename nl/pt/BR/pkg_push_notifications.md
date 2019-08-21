---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: push notifications, functions, webhooks

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


# Notificações push
{: #pkg_push_notifications}


## Pacotes
{: #pkg_push_packages}

| Pacote | Disponibilidade | Descrição |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | Pré-instalado (Não disponível em Tóquio) | Envie notificação push para um ou mais dispositivos especificados. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | Pré-instalado (Não disponível em Tóquio) | Envie notificações push móveis em eventos de dispositivo. |
| [`/push-notifications`](#pkg_push_mobile_send) | Instalável | Interaja com as instâncias de serviço do {{site.data.keyword.mobilepushfull}}. Envie uma mensagem, crie, atualize ou exclua um webhook. |

## Enviando notificações push
{: #pkg_push_send}

O pacote pré-instalado não está disponível na região de Tóquio. Consulte o pacote instalável do [Push Notification](#pkg_push_mobile_send) para a ação `sendMessage` usando a autenticação do IAM
{: tip}

Aprenda como criar uma ligação de pacote do Push Notification e envie uma notificação push simples usando o pacote `/whisk.system/pushnotifications`.
{: shortdesc}

O pacote inclui as ações e os feeds a seguir:

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | Pacote | `appId`, `appSecret`, `admin_url` | Trabalhe com o serviço de Push. |
| `/whisk.system/pushnotifications/sendMessage` | Ação | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Envie notificação push para um ou mais dispositivos especificados. |


Para obter informações sobre como disparar eventos acionadores quando houver atividade de dispositivo, consulte [Push móvel em eventos de dispositivo](#pkg_push_mobile).

### Criando uma ligação de pacote de Push
{: #pkg_push_create}

Para criar uma ligação de pacote de Notificações Push, os parâmetros a seguir deverão ser especificados.

| Parâmetro | Descrição |
| --- | --- |
| `appId` | O **GUID do app** do {{site.data.keyword.cloud}}. |
| `appSecret` | O **segredo do app** do serviço {{site.data.keyword.cloud_notm}} Push Notification. |

Para criar uma ligação de pacote, veja as etapas a seguir:

1. Crie um aplicativo {{site.data.keyword.cloud_notm}} no [Painel do {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external}.

2. Inicialize o Serviço de notificação push e ligue o serviço ao aplicativo {{site.data.keyword.cloud_notm}}.

3. Configure o [aplicativo
de Notificação push](/docs/services/mobilepush?topic=mobile-pushnotification-getting-started).

  Certifique-se de lembrar do **App GUID** e do **App Secret** do app do {{site.data.keyword.cloud_notm}} que você criou.

4. Crie uma ligação de pacote com as `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Verifique se a ligação de pacote existe:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Saída de exemplo**
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### Parâmetros de notificação push
{: #pkg_push_params}

A ação `/whisk.system/pushnotifications/sendMessage` envia notificações push para dispositivos registrados. Os parâmetros são os seguintes.

| Parâmetro | Descrição |
| --- | --- |
| `text` | A mensagem de notificação a ser mostrada para o usuário. Por exemplo, `-p text "Hi, OpenWhisk send a notification"`. |
| `url` | Uma URL que pode ser enviada juntamente com o alerta. Por exemplo, `-p url "https:\\www.w3.ibm.com"`. |
| `apiHost` | Uma sequência opcional que especifica o host da API. O padrão é `mobile.ng.bluemix.net`.  Por exemplo: `-p apiHost "mobile.eu-gb.bluemix.net"`. |
| `deviceIds` | A lista de dispositivos especificados. Por exemplo, `-p deviceIds ["deviceID1"]`. |
| `platforms` | Enviar notificação para os dispositivos das plataformas especificadas. 'A' para dispositivos Apple (iOS) e 'G ' para dispositivos Google (Android). Por exemplo, `-p platforms ["A"]`. |
| `userIds` | Enviar notificação para os dispositivos dos usuários especificados. Por exemplo, `-p userIds "[\"testUser\"]"`. |
| `tagNames` | Enviar notificação para os dispositivos que estão inscritos em qualquer uma dessas tags. Por exemplo, `-p tagNames "[\"tag1\"]"`. |
| `gcmCollapseKey` | Esse parâmetro identifica um grupo de mensagens. |
| `gcmCategory` | O identificador de categoria a ser usado para as notificações push interativas. |
| `gcmIcon` | Especifique o nome do ícone a ser exibido para a notificação. Certifique-se de que o ícone já esteja empacotado com o aplicativo cliente. |
| `gcmDelayWhileIdle` | Quando esse parâmetro é configurado como true, a mensagem é enviada até que o dispositivo se torne ativo. |
| `gcmSync` | O sistema de mensagens do grupo de dispositivos possibilita que cada instância do app em um grupo reflita o estado do sistema de mensagens mais recente. |
| `gcmVisibility` | privado/público - Visibilidade dessa notificação, que afeta como e quando as notificações são reveladas em uma tela bloqueada segura. |
| `gcmPayload` | A carga útil JSON customizada que é enviada como parte da mensagem de notificação. Por exemplo, `-p gcmPayload "{\"hi\":\"hello\"}"`. |
| `gcmPriority` | Configura a prioridade da mensagem. |
| `gcmSound` | O arquivo de som (no dispositivo) que é reproduzido quando a notificação chega ao dispositivo. |
| `gcmTimeToLive` | Esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento GCM se o dispositivo estiver off-line. |
| `gcmStyleType` | Especifica o tipo de notificações expansíveis. Os valores possíveis são `bigtext_notification`, `picture_notification`, `inbox_notification`. |
| `gcmStyleTitle` | Especifica o título da notificação. O título é exibido quando a notificação é expandida. O título deve ser especificado para todas as três notificações expansíveis. |
| `gcmStyleUrl` | Uma URL por meio da qual a figura precisa ser obtida para a notificação. Deve ser especificado para `picture_notification`. |
| `gcmStyleText` | O texto grande que precisa ser exibido na expansão de um `bigtext_notification`. Deve ser especificado para `bigtext_notification`. |
| `gcmStyleLines` | Uma matriz de sequências que deve ser exibida no estilo de caixa de entrada para `inbox_notification`. Deve ser especificado para `inbox_notification`. |
| `gcmLightsLedArgb` | A cor do LED. O hardware faz sua melhor aproximação. |
| `gcmLightsLedOnMs` | O número de milissegundos para o LED estar aceso enquanto está piscando. O hardware faz sua melhor aproximação. |
| `gcmLightsLedOffMs` | O número de milissegundos para o LED estar desligado enquanto está piscando. O hardware faz sua melhor aproximação. |
| `apnsBadge` | O número a ser exibido como o badge do ícone do aplicativo. |
| `apnsCategory` | O identificador de categoria a ser usado para as notificações push interativas. |
| `apnsIosActionKey` | O título para a tecla de ação. |
| `apnsPayload` | A carga útil JSON customizada que é enviada como parte da mensagem de notificação. |
| `apnsType` | ['DEFAULT', 'MIXED', 'SILENT']. |
| `apnsSound` | O nome do arquivo de som no pacote configurável do aplicativo. O som desse arquivo é reproduzido como um alerta. |
| `apnsTitleLocKey` | A chave para uma sequência de título no arquivo `Localizable.strings` para o código de idioma atual. A keystring pode ser formatada com especificadores %@ e %n$@ para tomar as variáveis que são especificadas na matriz `titleLocArgs`. |
| `apnsLocKey` | Uma chave para uma sequência de mensagem de alerta em um arquivo `Localizable.strings` para o código de idioma atual (que é configurado pela preferência de idioma do usuário). A keystring pode ser formatada com especificadores %@ e %n$@ para tomar as variáveis que são especificadas na matriz `locArgs`. |
| `apnsLaunchImage` | O nome do arquivo de um arquivo de imagem no pacote configurável do app, com ou sem a extensão do nome do arquivo. A imagem é usada como a imagem de ativação quando os usuários tocam no botão de ação ou movem a régua de controle da ação. |
| `pnsTitleLocArgs` | Valores de sequência variável a aparecerem no lugar dos especificadores de formato em `title-loc-key`. |
| `apnsLocArgs` | Valores de sequência variável a aparecerem no lugar dos especificadores de formato em `locKey`. |
| `apnstitle` | O título de notificações push detalhadas (suportado apenas no iOS 10 e acima). |
| `apnsSubtitle` | O subtítulo das notificações detalhadas. (Suportado apenas no iOS 10 e acima).
| `apnsAttachmentUrl` | O link para a mídia de notificações do iOS (vídeo, áudio, GIF, imagens - suportado apenas no iOS 10 e acima). |
| `fireFoxTitle` | Especifica o título a ser configurado para a Notificação push da web. |
| `fireFoxIconUrl` | A URL do ícone a ser configurada para a Notificação push da web. |
| `fireFoxTimeToLive` | Esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento GCM se o dispositivo estiver off-line. |
| `fireFoxPayload` | A carga útil JSON customizada que é enviada como parte da mensagem de notificação. |
| `chromeTitle` | Especifica o título a ser configurado para a Notificação push da web. |
| `chromeIconUrl` | A URL do ícone a ser configurada para a Notificação push da web. |
| `chromeTimeToLive` | Esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento GCM se o dispositivo estiver off-line. |
| `chromePayload` | A carga útil JSON customizada que é enviada como parte da mensagem de notificação. |
| `safariTitle` | Especifica o título a ser configurado para as Notificações push do Safari. |
| `safariUrlArgs` | Os argumentos de URL que precisam ser usados com essa notificação. Esses argumentos devem ser fornecidos na forma de uma Matriz JSON. |
| `safariAction` | O rótulo do botão de ação. |
| `chromeAppExtTitle` | Especifica o título a ser configurado para a Notificação push da web. |
| `chromeAppExtCollapseKey` | Esse parâmetro identifica um grupo de mensagens. |
| `chromeAppExtDelayWhileIdle` | Quando esse parâmetro é configurado como true, ele indica que a mensagem não é enviada até que o dispositivo se torne ativo. |
| `chromeAppExtIconUrl` | A URL do ícone a ser configurada para a Notificação push da web. |
| `chromeAppExtTimeToLive` | Esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento GCM se o dispositivo estiver off-line. |
| `chromeAppExtPayload` | A carga útil JSON customizada que é enviada como parte da mensagem de notificação. |

### Exemplo de notificação push
{: #pkg_push_ex}

Consulte o exemplo a seguir para enviar uma notificação push por meio do pacote do Push Notification.

Envie uma notificação push usando a ação **sendMessage** na ligação de pacote anteriormente criada. Certifique-se de substituir `/myNamespace/myPush` pelo nome de seu pacote.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

**Saída de exemplo**
```
{
  "result": {
  "pushResponse":
    {
      "messageId":"11111H",
      "message":{
        "alert":"this is my message",
        "url":""
      },
      "settings":{
        "apns":{
          "sound":"default"
        },
        "gcm":{
          "sound":"default"
          },
        "target":{
          "deviceIds":["T1","T2"]
        }
      }
    }
  },
      "status": "success",
      "success": true
  }
```
{: screen}

## Enviando notificações push em eventos de dispositivo móvel
{: #pkg_push_mobile}

Esse pacote pré-instalado não está disponível na região de Tóquio.
{: tip}

Aprenda como configurar o serviço Push Notification para que dispare um acionador quando houver atividade do dispositivo, como registro/remoção de registro do dispositivo ou assinatura/cancelamento de assinatura em um aplicativo especificado.
{: shortdesc}

### Parâmetros
{: #pkg_push_mobile_params}

Os parâmetros de `/whisk.system/pushnotifications/webhook` são os seguintes:

| Parâmetro | Descrição |
|--- | --- |
| `appId` | O GUID do app do {{site.data.keyword.cloud_notm}}. |
| `appSecret` | O `appSecret` do serviço {{site.data.keyword.cloud_notm}} Push Notification. |
| `events` | `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`. É possível usar o caractere curinga "`*`" para ser notificado para todos os eventos. |

### Disparando um evento acionador na atividade do serviço Push Notifications
{: #pkg_push_mobile_trigger}

Para criar um acionador que é disparado toda vez que um novo dispositivo é registrado com o aplicativo de serviço de Notificações Push, veja o exemplo a seguir:

1. Crie uma ligação de pacote que seja configurada para o seu serviço Push Notifications usando seu `appId` e `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crie um acionador para o tipo de evento `onDeviceRegister` do serviço de Notificações Push usando seu feed `myPush/webhook`.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. É possível criar uma regra que envia uma mensagem cada vez que um novo dispositivo é registrado. Crie uma regra usando a ação e o acionador anteriores.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Verifique os resultados usando o comando `ibmcloud fn activation poll`.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Registre um dispositivo em seu aplicativo {{site.data.keyword.cloud_notm}}. É possível ver que a `rule`, o `trigger` e a `action` são executados no {{site.data.keyword.openwhisk}}painel do [](https://cloud.ibm.com/openwhisk/dashboard){: external}.

  **Saída**
  
  A ação envia uma notificação push.


## Enviando notificações push ou criando, atualizando e excluindo webhooks
{: #pkg_push_mobile_send}

O pacote instalável do {{site.data.keyword.mobilepushshort}} fornece um conjunto de ações para interagir com as instâncias de serviço do {{site.data.keyword.mobilepushfull}}. Essas ações permitem que você envie uma mensagem ou crie, atualize ou exclua um webhook.
{: shortdesc}

O pacote do  {{site.data.keyword.mobilepushshort}}  inclui as ações a seguir:

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/push-notifications` | Pacote | `apikey`, `appGuid` | Trabalhe com uma instância do {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | Ação | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Envie notificação push para um ou mais dispositivos especificados. |
| `/push-notifications/webhook` | Ação | `events` | Dispare eventos acionadores em atividades de dispositivo (registro de dispositivo, remoção de registro, assinatura ou cancelamento de assinatura) no serviço de Push. |

### Criando uma instância de serviço do  {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Antes de instalar o pacote, deve-se criar uma instância do {{site.data.keyword.mobilepushshort}}.

1. [Crie uma instância de serviço do {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Crie um conjunto de credenciais de serviço](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) para a instância do serviço Push Notifications.

3. [Configure a instância de serviço do {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

### Instalando o pacote do  {{site.data.keyword.mobilepushshort}}
{: #pkg_push_mobile_install}

Depois de ter uma instância de serviço do {{site.data.keyword.mobilepushshort}}, use a CLI ou a UI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.mobilepushshort}} em seu namespace.

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_cli}

Antes de iniciar:
  1. [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

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

    **Saída**
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Ligue as credenciais por meio da instância de serviço do {{site.data.keyword.mobilepushshort}} que você criou para o pacote.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myOrg_mySpace/push-notifications parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package /myOrg_mySpace/push-notifications, displaying field parameters
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
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
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
{: #pkg_push_mobile_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Usando o menu suspenso de namespace no canto superior direito, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_full_notm}}.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **IBM {{site.data.keyword.mobilepushshort}}** e, em seguida, clique no pacote **IBM {{site.data.keyword.mobilepushshort}}**.

5. Na seção Tempos de execução disponíveis, selecione nodeJS na lista suspensa e, em seguida, clique em **Instalar**.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página Ações e poderá procurar por seu novo pacote, que é denominado **push-notifications**.

7. Para usar as ações no pacote **push-notifications**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação no pacote **push-notifications** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo **parâmetro**. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

### Enviar uma notificação push
{: #pkg_push_mobile_sendmsg}

Para enviar uma mensagem por meio do serviço de notificação push usando a ação `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

**Saída de exemplo**
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

### Criar um webhook
{: #pkg_push_mobile_hook}

Para criar um webhook para o serviço {{site.data.keyword.mobilepushshort}} para eventos `onDeviceRegister`:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

**Saída de exemplo**
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



