---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Implementando modelos de iniciação rápida
{: #serviceauth}

O {{site.data.keyword.openwhisk}} oferece um catálogo de Modelos para ajudar você a começar rapidamente em seu próximo projeto. Os modelos são uma combinação de Ações, Acionadores, Sequências e também podem incorporar instâncias de serviço do {{site.data.keyword.Bluemix}}. Usando Modelos, é possível criar um projeto de forma rápida e fácil e iniciar a codificação imediatamente.

Este tutorial conduz você pela implementação do Modelo do Cloudant.
{: shortdesc}

## Modelos de iniciação rápida disponíveis
{: #available-templates}

| Nome | Descrição | Tempos de Execução Suportados |
|:-----------------|:-----------------|:-----------------|
| [Eventos do Cloudant](./deploy_templates.html#cloudant-template) | Quando um DB Cloudant tiver um documento editado ou incluído, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Obter recurso HTTP](./deploy_templates.html#get-http-resource-template) | Uma ação da web que é chamada em resposta a um evento HTTP e, em seguida, busca dados da API do Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Essa ação aceitará um único parâmetro, que deve ser um objeto JSON. | Node.js, Swift, Python, PHP |
| [Eventos do Message Hub](./deploy_templates.html#messagehub-events-template) | Quando um tópico do Message Hub tiver dados novos incluídos, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Lembrete periódico do Slack](./deploy_templates.html#slack-reminder-template) | Uma ação que postará no Slack com base em um acionador periódico. | Node.js, Swift, Python, PHP |

## Implementando o modelo Eventos do Cloudant
{: #cloudant-template}

O Modelo do Cloudant cria uma sequência de ações e um Acionador que dará início a essa Sequência. O acionador é disparado quando há uma mudança no DB Cloudant conectado, que deve ser um banco de dados de gatos, com um nome e uma cor. O item de dados esperado é um gato, com um nome e uma cor definida. Quando um novo gato for incluído no banco de dados ou um gato atual for editado, os dados serão registrados no console.

1. Para criar um Modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criação**.

2. Clique em **Modelos de iniciação rápida**.

3. Clique em **Novo item do Cloudant**.

### Criando a ação do Cloudant

1. Em seguida, forneça um nome para seu pacote ou use o nome padrão fornecido `new-cloudant-item`.

2. Na lista suspensa **Ações**, selecione o tempo de execução para as ações que você possuirá (nodejs, swift, python ou php). Para este exemplo, selecione **nodejs** e clique em **Avançar**.

### Criando o acionador do Cloudant

Os acionadores chamam ações quando recebem eventos de origens de eventos. Para criar um acionador para o Modelo do Cloudant, forneça o acionador com as informações da instância de serviço do Cloudant necessárias.

#### Criar instância de serviço do Cloudant

Você pode escolher entre:
  * **Criar sua própria instância**
  * **Inserir suas próprias credenciais**

1. Para este exemplo, escolha **Criar sua própria instância**.

2. Um pop-up é aberto para levar você a uma nova guia com a página de configuração do Cloudant. Depois de criar a instância do Cloudant, deve-se criar um conjunto de Credenciais de serviço e, em seguida, fechar a guia para retornar a esta página clicando em **Ok**.

3. Agora escolha **Inserir suas próprias credenciais** e forneça as informações a seguir:
  * Nome do usuário - _Seu nome do usuário do Cloudant_
  * Senha - _Sua senha do Cloudant_
  * Host - _Este é geralmente seu `username.cloudant.com`_
  * Banco de dados - _O nome de seu banco de dados Cloudant_

### Implementar o modelo do Cloudant

Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.

## Implementando o modelo Obter recurso HTTP
{: #get-http-resource-template}

O modelo Obter recurso HTTP cria uma ação para buscar um recurso externo, a API do Yahoo Weather e, em seguida, retorna dados. A ação é ativada como uma ação da web, permitindo que ela seja chamada com uma URL que é ativada por CORS e não precisa de uma chave de autenticação, que é útil para construir backends para aplicativos da web. **Nota**: por padrão, o terminal `get-http-resource` está publicamente disponível para quem possa desejar chamá-lo.

1. Para criar um Modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criação**.

2. Clique em **Modelos de iniciação rápida**.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `get-http-resource`.

4. Escolha o tempo de execução para as ações que você possuirá: Node.js 8, Node.js 6 ou Python 3.

5. Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.

## Implementando o modelo Hello World
{: #hello-world-template}

Essa ação aceitará um único parâmetro, que deve ser um objeto JSON.

1. Para criar um Modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criação**.

2. Clique em **Modelos de iniciação rápida**.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `hello-world`.

4. Escolha o tempo de execução para as ações que você possuirá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.

## Implementando o modelo Eventos do Message Hub
{: #messagehub-events-template}

O modelo Eventos do Message Hub cria uma ação e um acionador que começa essa ação. O acionador é disparado sempre que há um novo item incluído no tópico Message Hub que é escolhido durante a criação do modelo.

1. Para criar um Modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criação**.

2. Clique em **Modelos de iniciação rápida**.

3. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `message-hub-events`.

4. Escolha o tempo de execução para as ações que você possuirá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Clique em **Avançar**.

### Criando o acionador do Message Hub

Os acionadores chamam ações quando recebem eventos de origens de eventos. Para criar um acionador para o modelo do Message Hub, forneça ao acionador as informações necessárias da instância de serviço Message Hub.

Revise o campo **Nome do acionador**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `message-hub-events-trgr`.

### Criando a instância de serviço Message Hub

Você pode escolher entre:
  * **Criar sua própria instância**
  * **Inserir suas próprias credenciais**

1. Para este exemplo, escolha **Criar sua própria instância**.

2. Um pop-up é aberto para levar você a uma nova guia com a página de configuração do Message Hub. Depois de criar a instância do Message Hub, deve-se criar um conjunto de Credenciais de serviço e, em seguida, fechar a guia para retornar a essa página clicando em **OK**.

3. Agora escolha **Inserir suas próprias credenciais** e forneça as informações a seguir:
  * Nome do usuário - _Seu nome do usuário do Message Hub_
  * Senha - _Sua senha do Message Hub_
  * kafka_admin_url - _Sua URL de REST do administrador do Message Hub_
  * Banco de dados - _O nome de seu banco de dados do Message Hub_
  * Tópico - _Tópico para assinar_

### Implementar o modelo do Message Hub

Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.

## Implementando o modelo Lembrete periódico do Slack
{: #slack-reminder-template}

O modelo Lembrete periódico do Slack posta no Slack em um intervalo fornecido pelo usuário durante a criação do acionador. Antes de criar esse modelo, acesse https://api.slack.com/incoming-webhooks para configurar a URL necessária de webhooks recebidos.

1. Revise o campo **Nome do pacote**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `periodic-slack-reminder`.

2. Escolha o tempo de execução para as ações que você possuirá: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

3. Na seção **Parâmetros**, insira a URL de webhook no campo **Valor de parâmetro** e, em seguida, clique em **Avançar**. (Exemplo: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Criando o acionador Lembrete do Slack

Os acionadores chamam ações quando recebem eventos de origens de eventos. Para criar um acionador para o modelo Lembrete do Slack, forneça ao acionador as informações necessárias da instância de serviço Message Hub.

1. Revise o campo **Nome do acionador**; é possível atualizá-lo conforme necessário. O padrão é configurado para você como `periodic-slack-reminder-trgr`.

2. Em seguida, é possível especificar o intervalo no qual o acionador deve ser disparado usando uma expressão Padrão ou Cron. É possível selecionar horários UTC para Dias úteis, Horas e Minutos. Selecione suas opções de intervalo desejadas e você estará pronto para implementação do modelo.

3. Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.
