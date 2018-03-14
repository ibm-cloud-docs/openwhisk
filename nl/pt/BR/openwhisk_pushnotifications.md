---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-02"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote de Notificações Push
{: #openwhisk_catalog_pushnotifications}

O pacote `/whisk.system/pushnotifications` fornece a você a capacidade de trabalhar com um serviço de push.
{: shortdesc}

O pacote inclui a ação e o feed a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | pacote | appId, appSecret  | Trabalhar com o serviço de push |
| `/whisk.system/pushnotifications/sendMessage` | ação | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Enviar notificação push para um ou mais dispositivos especificados |
| `/whisk.system/pushnotifications/webhook` | alimentação | eventos | Disparar eventos acionadores nas atividades de dispositivo (registro, remoção de registro, assinatura, cancelamento de assinatura do dispositivo) no serviço de Push |
É sugerido criar uma ligação de pacote com os valores `appId` e
`appSecret`. Dessa forma, não será necessário especificar essas
credenciais toda vez que chamar as ações no pacote.

## Criando uma ligação de pacote de Push
{: #openwhisk_catalog_pushnotifications_create}

Para criar uma ligação de pacote de Notificações Push, deve-se especificar os parâmetros a seguir,

-  `appId`: o GUID do app do {{site.data.keyword.Bluemix}}.
-  `appSecret`: o serviço de notificação push do {{site.data.keyword.Bluemix_notm}} appSecret.

Para criar uma ligação de pacote, veja as etapas de exemplo a seguir:

1. Crie um aplicativo do {{site.data.keyword.Bluemix_notm}} no [Painel do {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

2. Inicialize o Serviço de Notificação Push e ligue o serviço ao aplicativo {{site.data.keyword.Bluemix_notm}}

3. Configure o [aplicativo
de Notificação push](https://console.bluemix.net/docs/services/mobilepush/index.html).

  Certifique-se de lembrar do `App GUID` e do `App Secret` do app do {{site.data.keyword.Bluemix_notm}} que você criou.

4. Crie uma ligação de pacote com as `/whisk.system/pushnotifications`.

  ```
  wsk package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}
  
5. Verifique se a ligação do pacote existe.

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myPush private binding
  ```


## Parâmetros de notificação push
{: #openwhisk_push_parameters}

A ação `/whisk.system/pushnotifications/sendMessage` envia notificações push para dispositivos registrados. Os parâmetros são os seguintes:

- `text`: a mensagem de notificação a ser mostrada ao usuário. Por exemplo, `-p text "Hi, OpenWhisk send a notification"`.
- `url`: uma URL que pode ser enviada junto com o alerta. Por exemplo, `-p url "https:\\www.w3.ibm.com"`.
- `deviceIds`: a lista de dispositivos especificados. Por exemplo, `-p deviceIds ["deviceID1"]`.
- `platforms`: envie notificação para os dispositivos das plataformas especificadas. 'A' para dispositivos Apple (iOS) e 'G ' para dispositivos Google (Android). Por exemplo, `-p platforms ["A"]`.
- `userIds`: envie notificação para os dispositivos dos usuários especificados. Por exemplo, `-p userIds "[\"testUser\"]"`
- `tagNames`: envie notificação para os dispositivos que estão inscritos em qualquer uma dessas tags. Por exemplo, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: esse parâmetro identifica um grupo de mensagens
- `gcmCategory`: o identificador de categoria a ser usado para as notificações push interativas.
- `gcmIcon`: especifique o nome do ícone a ser exibido para a notificação. Certifique-se de que o ícone já esteja empacotado com o aplicativo cliente.
- `gcmDelayWhileIdle`: quando esse parâmetro é configurado para true, a mensagem será enviada até que o dispositivo fique ativo.
- `gcmSync`: o sistema de mensagens do grupo de dispositivos torna possível para cada instância do app em um grupo refletir o estado mais recente do sistema de mensagens.
- `gcmVisibility`: privada/pública - a visibilidade dessa notificação, que afeta como e quando as notificações são reveladas em uma tela bloqueada segura.
- `gcmPayload`: a carga útil de JSON customizada que é enviada como parte da mensagem de notificação. Por exemplo, `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: configura a prioridade da mensagem.
- `gcmSound`: o arquivo de som (no dispositivo) que é reproduzido quando a notificação chega ao dispositivo.
- `gcmTimeToLive`: este parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento de GCM se o dispositivo estiver off-line.
- `gcmStyleType`: especifica os tipos de notificações expansíveis. Os valores possíveis são `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle`: especifica o título da notificação. O título é exibido quando a notificação é expandida. O título deve ser especificado para todas as três notificações expansíveis.
- `gcmStyleUrl`: uma URL da qual a figura precisa ser obtida para a notificação. Deve ser especificado para `picture_notification`.
- `gcmStyleText`: o texto grande que precisa ser exibido ao expandir um `bigtext_notification`. Deve ser especificado para `bigtext_notification`.
- `gcmStyleLines`: uma matriz de sequências que deve ser exibida no estilo de caixa de entrada para `inbox_notification`. Deve ser especificado para `inbox_notification`.
- `gcmLightsLedArgb`: a cor do led. O hardware faz sua melhor aproximação.
- `gcmLightsLedOnMs`: o número de milissegundos que o LED fica ligado enquanto está piscando. O hardware faz sua melhor aproximação.
- `gcmLightsLedOffMs`: o número de milissegundos que o LED fica desligado enquanto está piscando. O hardware faz sua melhor aproximação.
- `apnsBadge`: o número a ser exibido como o badge do ícone do aplicativo.
- `apnsCategory`: o identificador de categoria a ser usado para as notificações push interativas.
- `apnsIosActionKey`: o título para a chave de ação.
- `apnsPayload`: a carga útil de JSON customizada que é enviada como parte da mensagem de notificação.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: o nome do arquivo de som no pacote configurável do aplicativo. O som desse arquivo é reproduzido como um alerta.
- `apnsTitleLocKey`: a chave para uma sequência de títulos no arquivo `Localizable.strings` para a localização atual. A keystring pode ser formatada com especificadores %@ e %n$@ para tomar as variáveis que são especificadas na matriz `titleLocArgs`.
- `apnsLocKey`: uma chave para uma sequência de mensagem de alerta em um arquivo `Localizable.strings` para a localização atual (que é configurada pela preferência de linguagem do usuário). A keystring pode ser formatada com especificadores %@ e %n$@ para tomar as variáveis que são especificadas na matriz locArgs.
- `apnsLaunchImage`: o nome do arquivo de um arquivo de imagem no pacote configurável do app, com ou sem a extensão do nome do arquivo. A imagem é usada como a imagem de ativação quando os usuários tocam no botão de ação ou movem a régua de controle da ação.
- `pnsTitleLocArgs`: os valores da sequência variável para aparecerem no local dos especificadores de formato em `title-loc-key`.
- `apnsLocArgs`: os valores da sequência variável para aparecerem no local dos especificadores de formato em `locKey`.
- `apnstitle`: o título de Notificações Push rich (suportado somente no iOS 10 e acima).
- `apnsSubtitle`: o subtítulo das Notificações rich. (Suportado apenas no iOS 10 e acima).
- `apnsAttachmentUrl`: o link para a mídia de notificações do iOS (vídeo, áudio, GIF, imagens - suportados somente no iOS 10 e acima).
- `fireFoxTitle`: especifica o título a ser configurado para a Notificação WebPush.
- `fireFoxIconUrl`: a URL do ícone a ser configurado para a Notificação WebPush.
- `fireFoxTimeToLive`: esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento de GCM se o dispositivo estiver off-line.
- `fireFoxPayload`: a carga útil de JSON customizada que é enviada como parte da mensagem de notificação.
- `chromeTitle`: especifica o título a ser configurado para a Notificação WebPush.
- `chromeIconUrl`: a URL do ícone a ser configurado para a Notificação WebPush.
- `chromeTimeToLive`: esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento de GCM se o dispositivo estiver off-line.
- `chromePayload`: a carga útil de JSON customizada que é enviada como parte da mensagem de notificação.
- `safariTitle`: especifica o título a ser configurado para as Notificações push do Safari.
- `safariUrlArgs`: os argumentos de URL que precisam ser usados com essa notificação. Esses argumentos devem ser fornecidos na forma de uma Matriz JSON.
- `safariAction`: o rótulo do botão de ação.
- `chromeAppExtTitle`: especifica o título a ser configurado para a Notificação WebPush.
- `chromeAppExtCollapseKey`: esse parâmetro identifica um grupo de mensagens.
- `chromeAppExtDelayWhileIdle`: quando esse parâmetro for configurado para true, isso indica que a mensagem não será enviada até o dispositivo ficar ativo.
- `chromeAppExtIconUrl`: a URL do ícone a ser configurado para a Notificação WebPush.
- `chromeAppExtTimeToLive`: esse parâmetro especifica quanto tempo (em segundos) a mensagem será mantida no armazenamento de GCM se o dispositivo estiver off-line.
- `chromeAppExtPayload`: a carga útil de JSON customizada que é enviada como parte da mensagem de notificação.


## Enviando notificações push
{: #openwhisk_send_push_notifications}

Veja o exemplo a seguir para enviar uma notificação push do pacote de Notificação push.

Enviar uma notificação push usando a ação `sendMessage` na ligação de pacote que você criou anteriormente. Certifique-se de substituir `/myNamespace/myPush` pelo nome de seu pacote.
```
wsk action invoke /myNamespace/myPush/sendMessage --blocking --result  -p url https://example.com -p text "this is my message"  -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

```json
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


## Disparando um evento acionador na atividade do serviço de Notificações Push
{: #openwhisk_catalog_pushnotifications_fire}

O `/whisk.system/pushnotifications/webhook` configura o serviço de push para disparar um acionador quando há uma atividade de dispositivo, como registro/remoção de registro de dispositivo ou assinatura/cancelamento de assinatura em um aplicativo especificado

Os parâmetros são os seguintes:

- `appId:` o GUID do app do {{site.data.keyword.Bluemix_notm}}.
- `appSecret:` o serviço de notificação push do {{site.data.keyword.Bluemix_notm}} appSecret.
- `events:` os eventos suportados são `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe` e `onUnsubscribe`. É possível usar o caractere curinga `*` para ser notificado para todos os eventos.

Para criar um acionador que é disparado toda vez que um novo dispositivo é registrado com o aplicativo de serviço de Notificações Push, veja o exemplo a seguir:

1. Crie uma ligação de pacote que esteja configurada para o seu serviço de Notificações Push com seu appId e appSecret.
  ```
  wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crie um acionador para o tipo de evento `onDeviceRegister` do serviço de Notificações Push usando seu feed `myPush/webhook`.
  ```
  wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Você poderia criar uma regra que envia uma mensagem cada vez que um novo dispositivo é registrado. Crie uma regra usando a ação e o acionador anteriores.
  ```
  wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

  Verifique os resultados no `wsk activation poll`.

  Registre um dispositivo em seu aplicativo {{site.data.keyword.Bluemix_notm}}. É possível ver que `Rule`, `Trigger` e `Action` são executados no [painel](https://console.{Domain}/openwhisk/dashboard) do OpenWhisk.

  A ação envia uma notificação push.

