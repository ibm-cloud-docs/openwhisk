---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Implementando modelos de iniciação rápida
{: #serviceauth}

O {{site.data.keyword.openwhisk}} oferece um catálogo de modelos para ajudá-lo a iniciar rapidamente seu próximo projeto. Modelos são uma combinação de ações, acionadores e sequências, que também podem incorporar instâncias de serviço do {{site.data.keyword.Bluemix}}. Usando modelos, é possível criar um projeto de modo rápido e fácil e iniciar a codificação imediatamente.

Este tutorial conduz você pela implementação dos modelos que estão disponíveis por meio do {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Modelos de iniciação rápida disponíveis
{: #available-templates}

| Nome | Descrição | Tempos de Execução Suportados |
|:-----------------|:-----------------|:-----------------|
| [Eventos do {{site.data.keyword.cloudant_short_notm}}](./deploy_templates.html#cloudant-template) | Quando o documento de um {{site.data.keyword.cloudant}} é editado ou incluído, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Fazer upload da imagem](./deploy_templates.html#cos-upload-image) | Uma ação da web no permite que você faça upload de uma imagem para um depósito da instância do {{site.data.keyword.cos_full}} e, em seguida, recupere uma miniatura dessa imagem. | Node.js |
| [Obter recurso HTTP](./deploy_templates.html#get-http-resource-template) | Uma ação da web que é chamada em resposta a um evento HTTP e, em seguida, busca dados da API do Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Esta ação aceitará um único parâmetro, que deve ser um objeto JSON. | Node.js, Swift, Python, PHP |
| [Eventos do {{site.data.keyword.messagehub}}](./deploy_templates.html#messagehub-events-template) | Quando um tópico do {{site.data.keyword.messagehub_full}} tiver novos dados incluídos, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Lembrete periódico do Slack](./deploy_templates.html#slack-reminder-template) | Uma ação que postará a Folga com base em um acionador periódico. | Node.js, Swift, Python, PHP |

## Implementando o modelo de Eventos do  {{site.data.keyword.cloudant_short_notm}}
{: #cloudant-template}

O modelo do {{site.data.keyword.cloudant_short_notm}} cria uma sequência de ações e um acionador que iniciará essa sequência. O acionador é disparado quando há uma mudança no banco de dados do {{site.data.keyword.cloudant_short_notm}}, que deve ser um banco de dados de gatos, com um nome e uma cor. O item de dados esperado é um gato, com um nome e uma cor definida. Quando um novo gato for incluído no banco de dados ou um gato atual for editado, os dados serão registrados no console.

1. Para criar um modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criando**.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em  ** Eventos do Cloudant **.

### Criando a ação do  {{site.data.keyword.cloudant_short_notm}}

1. Em seguida, forneça um nome para seu pacote ou use o nome padrão fornecido `new-cloudant-item`.

2. Na lista suspensa **Ações**, selecione o tempo de execução para as ações que você possuirá (nodejs, swift, python ou php). Para este exemplo, selecione **nodejs** e clique em **Avançar**.

### Criando o acionador do  {{site.data.keyword.cloudant_short_notm}}

Os acionadores chamam ações quando eles recebem eventos por meio de origens de eventos. Para criar um acionador para o modelo do {{site.data.keyword.cloudant_short_notm}}, forneça ao acionador as informações da instância de serviço do {{site.data.keyword.cloudant_short_notm}} necessárias.

#### Criar instância de serviço do  {{site.data.keyword.cloudant_short_notm}}

Você pode escolher entre:
  * **Criar sua própria instância**
  * **Inserir suas próprias credenciais**

1. Para este exemplo, escolha **Criar sua própria instância**.

2. Um pop-up é aberto para levá-lo para uma nova guia com a página de configuração do {{site.data.keyword.cloudant_short_notm}}. Depois de criar a instância do {{site.data.keyword.cloudant_short_notm}}, deve-se criar um conjunto de Credenciais de serviço e, em seguida, fechar a guia para retornar para essa página clicando em **Ok**.

3. Agora escolha **Inserir suas próprias credenciais** e forneça as informações a seguir:
  * Username- _ Your  {{site.data.keyword.cloudant_short_notm}}  Username _
  * Senha- _ Sua  {{site.data.keyword.cloudant_short_notm}}  senha _
  * Host - _este é geralmente seu `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * Banco de dados - _o nome de seu banco de dados do
{{site.data.keyword.cloudant_short_notm}}_

### Implementar o modelo do  {{site.data.keyword.cloudant_short_notm}}

Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

## Implementando o modelo de Imagem de Upload
{: #cos-upload-image}

O modelo de Upload de imagem cria uma ação da web que permite que você faça upload de uma imagem para um depósito do {{site.data.keyword.cos_short_notm}} por meio de uma interface pequena. Em seguida, o modelo recupera a imagem como uma miniatura e a exibe na interface da ação da web.

Para implementar o modelo:

1. Acesse o console do {{site.data.keyword.openwhisk_short}} em [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/).

2. Clique em **Iniciar a criação**.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique no modelo  ** Fazer Upload da Imagem ** .

4. Insira um nome para seu pacote ou use o nome padrão `upload-image`.

5. Clique em **Avançar**.

6. O modelo requer credenciais de serviço de uma instância de serviço do {{site.data.keyword.cos_full_notm}}. Na lista ** {{site.data.keyword.cos_short}}**, selecione uma das opções a seguir:
  * **Criar uma nova instância**: se você não tiver uma instância de serviço existente, selecione esta opção para criar uma.
      1. Na página de criação da instância de serviço do {{site.data.keyword.cos_full_notm}} que é aberta, crie uma instância de serviço.
      2. [Criar um conjunto de credenciais de serviço do HMAC](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [ Crie pelo menos um depósito ](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Inserir suas próprias credenciais**: selecione esta opção para inserir manualmente suas próprias credenciais para uma instância de serviço do {{site.data.keyword.cos_short}}. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.
  * **Instâncias existentes**: se você tiver quaisquer instâncias existentes do {{site.data.keyword.cos_short}}, selecione uma das instâncias na lista. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.

7. Clique em **Implementar**.

8. Na navegação à esquerda, clique em **Terminais**

9. Na seção Ação da web, copie o link sem o sufixo .json e cole-o na barra de endereço do navegador. A interface para a ação da web do modelo é exibida.

10. Opcional: após a implementação do modelo, é possível navegar para o painel Ações para customizar o código nos dois novos pacotes:
    * O pacote `cloud-object-storage`, que contém ações que funcionam com as instâncias do {{site.data.keyword.cos_short}}
    * O pacote de modelo (nome padrão `upload-image`), que contém a ação `app`

## Implementando o modelo Obter recurso HTTP
{: #get-http-resource-template}

O modelo Obter recurso de HTTP cria uma ação para buscar um recurso externo, a API do Yahoo Weather e, em seguida, retorna dados. A ação é ativada como uma ação da web, permitindo que ela seja chamada com uma URL que é ativada para CORS e que não precisa de uma chave de autenticação, o que é útil para a construção de backends para aplicativos da web. **Nota**: por padrão, o terminal `get-http-resource` está publicamente disponível para quem possa desejar chamá-lo.

1. Para criar um modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criando**.

2. Clique em  ** Modelos de iniciação rápida **.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `get-http-resource`.

4. Escolha o tempo de execução para as ações que você possuirá: Node.js 8, Node.js 6 ou Python 3.

5. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

## Implementando o modelo Hello World
{: #hello-world-template}

Esta ação aceitará um único parâmetro, que deve ser um objeto JSON.

1. Para criar um modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criando**.

2. Clique em  ** Modelos de iniciação rápida **.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `hello-world`.

4. Escolha o tempo de execução para as ações que você terá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

## Implementando o modelo de Eventos do  {{site.data.keyword.messagehub}}
{: #messagehub-events-template}

O modelo de Eventos do {{site.data.keyword.messagehub}} cria uma ação e um acionador que inicia essa ação. O acionador é disparado sempre que um novo item é incluído no tópico {{site.data.keyword.messagehub}} que é escolhido durante a criação do modelo.

1. Para criar um modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criando**.

2. Clique em  ** Modelos de iniciação rápida **.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `message-hub-events`.

4. Escolha o tempo de execução para as ações que você terá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Clique em **Avançar**.

### Criando o acionador do  {{site.data.keyword.messagehub}}

Os acionadores chamam ações quando eles recebem eventos por meio de origens de eventos. Para criar um acionador para o modelo do {{site.data.keyword.messagehub}}, forneça ao acionador as informações da instância de serviço do {{site.data.keyword.messagehub}} necessárias.

Revise o campo **Nome do acionador**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `message-hub-events-trgr`.

### Criando a instância de serviço do  {{site.data.keyword.messagehub}}

Você pode escolher entre:
  * **Criar sua própria instância**
  * **Inserir suas próprias credenciais**

1. Para este exemplo, escolha **Criar sua própria instância**.

2. Um pop-up é aberto para levá-lo para uma nova guia com a página de configuração do
{{site.data.keyword.messagehub}}. Depois de criar a instância do {{site.data.keyword.messagehub}}, deve-se criar um conjunto de Credenciais de serviço e, em seguida, fechar a guia para retornar para essa página clicando em **Ok**.

3. Agora escolha **Inserir suas próprias credenciais** e forneça as informações a seguir:
  * Username- _ Your  {{site.data.keyword.messagehub}}  Username _
  * Senha- _ Sua  {{site.data.keyword.messagehub}}  senha _
  * kafka_admin_url - _sua URL de REST do administrador do
{{site.data.keyword.messagehub}}_
  * Banco de dados - _o nome de seu banco de dados do {{site.data.keyword.messagehub}}_
  * Tópico - _Tópico para assinar_

### Implementar o modelo do  {{site.data.keyword.messagehub}}

Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

## Implementando o modelo Lembrete periódico do Slack
{: #slack-reminder-template}

O modelo Lembrete periódico do Slack posta no Slack em um intervalo fornecido pelo usuário durante a criação do acionador. Antes de criar esse modelo, acesse https://api.slack.com/incoming-webhooks para configurar a URL de webhooks recebida necessária.

1. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `periodic-slack-reminder`.

2. Escolha o tempo de execução para as ações que você terá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

3. Na seção **Parâmetros**, insira a URL de webhook no campo **Valor de parâmetro** e, em seguida, clique em **Avançar**. (Exemplo: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Criando o acionador Lembrete do Slack

Os acionadores chamam ações quando eles recebem eventos por meio de origens de eventos. Para criar um acionador para o modelo Lembrete do Slack, forneça ao acionador as informações da instância de serviço do {{site.data.keyword.messagehub}} necessárias.

1. Revise o campo **Nome do acionador**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `periodic-slack-reminder-trgr`.

2. Em seguida, é possível especificar o intervalo no qual o acionador deve ser disparado usando uma expressão de Padrão ou Cron. É possível selecionar horários UTC para Dias úteis, Horas e Minutos. Selecione suas opções de intervalo desejadas e você estará pronto para implementação do modelo.

3. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.
