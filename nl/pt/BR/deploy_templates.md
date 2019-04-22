---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: deploy, deployment templates, templates, example, quickstart

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Implementando modelos de iniciação rápida
{: #serviceauth}

O {{site.data.keyword.openwhisk}} oferece um catálogo de modelos para ajudá-lo a iniciar rapidamente seu próximo projeto. Os modelos são uma combinação de ações, acionadores, sequências. Alguns modelos também incorporam outras instâncias de serviço do {{site.data.keyword.Bluemix_notm}}. Usando modelos, é possível criar um projeto de modo rápido e fácil e iniciar a codificação imediatamente.
{: shortdesc}

## Modelos de iniciação rápida disponíveis
{: #available-templates}

| Nome | Descrição | Tempos de Execução Suportados |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}}Eventos](/docs/openwhisk?topic=cloud-functions-serviceauth#cloudant-template) | Quando um documento é editado ou incluído em um banco de dados {{site.data.keyword.cloudantfull}}, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Fazer upload da imagem](/docs/openwhisk?topic=cloud-functions-serviceauth#cos-upload-image) | Use uma ação da web para fazer upload de uma imagem para um depósito da instância do {{site.data.keyword.cos_full}} e, em seguida, recuperar uma miniatura dessa imagem. | Node.js |
| [Obter recurso HTTP](/docs/openwhisk?topic=cloud-functions-serviceauth#get-http-resource-template) | Chame uma ação da web usando um evento HTTP e obterá dados da API Weather do Yahoo. | Node.js, Python |
| [Hello World](/docs/openwhisk?topic=cloud-functions-serviceauth#hello-world-template) | Crie uma ação básica que aceite um objeto JSON como um único parâmetro. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}}Eventos](/docs/openwhisk?topic=cloud-functions-serviceauth#messagehub-events-template) | Quando novos dados são incluídos em um tópico do {{site.data.keyword.messagehub_full}}, registre a mudança no console. | Node.js, Swift, Python, PHP |
| [Lembrete periódico do Slack](/docs/openwhisk?topic=cloud-functions-serviceauth#slack-reminder-template) | Use um webhook para o Slack com base em um acionador periódico. | Node.js, Swift, Python, PHP |

## Implementando o modelo de Eventos do  {{site.data.keyword.cloudant_short_notm}}
{: #cloudant-template}

O modelo do {{site.data.keyword.cloudant_short_notm}} cria uma sequência de ações e um acionador que inicia essa sequência. O acionador é disparado quando uma mudança é feita no banco de dados de cats de exemplo do {{site.data.keyword.cloudant_short_notm}} conectado. O item de dados esperado é um gato, com um nome e uma cor definida. Quando um novo cat é incluído no banco de dados ou um cat atual é editado, os dados são registrados no console.

### Implementando o modelo de Eventos do {{site.data.keyword.cloudant_short_notm}} da UI

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em  ** Eventos do Cloudant **.

4. Crie a ação do {{site.data.keyword.cloudant_short_notm}}.
    1. Forneça um nome para seu pacote ou use o nome padrão fornecido `new-cloudant-item`.
    2. Na lista **Ações**, selecione `Node.js 6` para o exemplo de banco de dados de cats e clique em **Avançar**.

5. Crie o acionador do {{site.data.keyword.cloudant_short_notm}}. Os acionadores chamam ações quando recebem eventos de origens de eventos. Para criar o acionador, deve-se fornecer uma instância de serviço e credenciais de serviço do {{site.data.keyword.messagehub}}.
    1. Na lista **Instância Cloudant**, selecione **Criar sua própria instância**. A página de configuração do {{site.data.keyword.cloudant_short_notm}} é aberta.
    2. Crie a instância de serviço do {{site.data.keyword.cloudant_short_notm}}.
    3. Crie um conjunto de Credenciais de serviço.
    4. Clique em **OK** para fechar a página de configuração do {{site.data.keyword.cloudant_short_notm}} e retornar para o console do {{site.data.keyword.openwhisk_short}}.
    5. Na lista **Instância do Cloudant**, é possível agora selecionar **Inserir suas próprias credenciais** e fornecer as informações a seguir:
      * Nome do usuário: seu nome do usuário do {{site.data.keyword.cloudant_short_notm}}
      * Senha: sua senha do {{site.data.keyword.cloudant_short_notm}}
      * Host: `<username>.cloudant.com`
      * Banco de dados: o nome de seu banco de dados do {{site.data.keyword.cloudant_short_notm}}

5. Clique em **Implementar**. Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo de Eventos do {{site.data.keyword.cloudant_short_notm}} pela CLI

1. Clone o repositório do modelo.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Navegue para o diretório para o tempo de execução da ação que você deseja usar: `nodejs-6`, `nodejs`, `php`, `python` ou `swift`.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo, usando as variáveis de ambiente a seguir.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Entendendo as variáveis de ambiente</caption>
    <thead>
    <th colspan=2>Entendendo as variáveis de ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Seu nome de usuário do {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>Sua senha do {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>O nome de seu banco de dados {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Um nome customizado para o pacote</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Um nome customizado para a regra</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Um nome customizado para o acionador</td></tr>
    </tbody></table>

## Implementando o modelo de Imagem de Upload
{: #cos-upload-image}

O modelo de Imagem de Upload cria uma ação da web que permite fazer upload de uma imagem para um {{site.data.keyword.cos_short}}depósito por meio de uma interface pequena. Em seguida, o modelo recupera a imagem como uma miniatura e a exibe na interface da ação da web.

### Implementando o modelo de Upload de imagem da UI

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Fazer upload da imagem**.

4. Insira um nome para seu pacote ou use o nome padrão `upload-image` e clique em **Avançar**.

6. O modelo requer credenciais de serviço de uma instância de serviço do {{site.data.keyword.cos_full_notm}}. Na lista ** {{site.data.keyword.cos_short}}**, selecione uma das opções a seguir:
  * **Criar uma nova instância**: se você não tiver uma instância de serviço existente, selecione esta opção para criar uma.
      1. Na página de criação da instância de serviço do {{site.data.keyword.cos_full_notm}} que é aberta, crie uma instância de serviço.
      2. [Criar um conjunto de credenciais de serviço do HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [ Crie pelo menos um depósito ](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).
  * **Inserir suas próprias credenciais**: selecione essa opção para inserir manualmente suas próprias credenciais para uma instância de serviço do {{site.data.keyword.cos_short}}. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.
  * **Instâncias existentes**: se você tiver quaisquer instâncias existentes do {{site.data.keyword.cos_short}}, selecione uma das instâncias na lista. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.

7. Clique em **Implementar**.

8. Na navegação à esquerda, clique em **Terminais**

9. Na seção **Ação da web**, copie o link sem o sufixo .json e cole-o na barra de endereço de seu navegador. A interface para a ação da web do modelo é exibida.

10. Opcional: após a implementação do modelo, é possível navegar para o painel **Ações** para customizar o código nos dois novos pacotes:
    * O pacote `cloud-object-storage`, que contém ações que funcionam com as instâncias do {{site.data.keyword.cos_short}}
    * O pacote de modelo (nome padrão `upload-image`), que contém a ação `app`

### Implementando o modelo de Imagem de upload por meio da CLI

1. [Instale o pacote do {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_cli).

2. Clone o repositório do modelo.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. Navegue para o diretório de tempo de execução `nodejs`.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo, usando um nome de pacote customizado e o nome de seu depósito do {{site.data.keyword.cos_short}} como variáveis de ambiente.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Na [página Ações ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/actions) do console do {{site.data.keyword.openwhisk_short}}, clique na ação `process-change`.

5. Na navegação à esquerda, clique em **Terminais**.

6. Na seção **Ação da web**, copie o link sem o sufixo .json e cole-o na barra de endereço de seu navegador. A interface para a ação da web do modelo é exibida.

## Implementando o modelo Obter recurso HTTP
{: #get-http-resource-template}

O modelo Obter recurso de HTTP cria uma ação para buscar um recurso externo, a API do Yahoo Weather e, em seguida, retorna dados. A ação é ativada como uma ação da web, permitindo que ela seja chamada com uma URL que é ativada para CORS e que não precisa de uma chave de autenticação, o que é útil para a construção de backends para aplicativos da web. **Nota**: por padrão, o terminal `get-http-resource` está publicamente disponível para qualquer pessoa que o chame.

### Implementando o modelo Obter recurso HTTP da UI

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Obter recurso HTTP**.

3. Insira um nome para seu pacote ou use o nome padrão `get-http-resource`.

4. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6 ou Python 3.

5. Clique em **Implementar**.

6. Chame a ação enrolando a URL a seguir: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Exemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Obter Recurso HTTP a partir da CLI

1. Clone o repositório do modelo.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Navegue para o diretório para o tempo de execução da ação que você deseja usar: `nodejs-6`, `nodejs` ou `python`.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo, usando um nome de pacote customizado como uma variável de ambiente.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Chame a ação enrolando a URL a seguir: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Exemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Implementando o modelo Hello World
{: #hello-world-template}

É possível implementar essa ação Hello World básica para iniciar com o {{site.data.keyword.openwhisk_short}} ou para testar outras entidades que você criar, como acionadores e regras.

### Implementando o modelo Hello World por meio da UI

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Hello World**.

4. Insira um nome para seu pacote ou use o nome padrão `hello-world`.

5. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

6. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Hello World por meio da CLI

1. Clone o repositório do modelo Hello World.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Navegue para o diretório para o tempo de execução da ação que você deseja usar: `nodejs-6`, `nodejs`, `php`, `python` ou `swift`.
    ```
    modelo cd-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## Implementando o modelo de Eventos do  {{site.data.keyword.messagehub}}
{: #messagehub-events-template}

O modelo de Eventos do {{site.data.keyword.messagehub}} cria uma ação e um acionador que inicia essa ação. O acionador é disparado sempre que um novo item é incluído no tópico {{site.data.keyword.messagehub}} que é escolhido durante a criação do modelo.

### Implementando o modelo de Eventos do {{site.data.keyword.messagehub}} da UI

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Eventos do {{site.data.keyword.messagehub}}**.

4. Crie a ação do {{site.data.keyword.messagehub}}.
    1. Forneça um nome para seu pacote ou use o nome padrão fornecido `message-hub-events`.
    2. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.
    3. Clique em **Avançar**.

5. Crie o acionador do {{site.data.keyword.messagehub}}. Os acionadores chamam ações quando recebem eventos de origens de eventos. Para criar o acionador, deve-se fornecer uma instância de serviço e credenciais de serviço do {{site.data.keyword.messagehub}}.
    1. Na lista **Instância do MessageHub**, selecione **Criar sua própria instância**. A página de criação do {{site.data.keyword.messagehub}} é aberta.
    2. Crie a instância de serviço do {{site.data.keyword.messagehub}}.
    3. Crie um conjunto de Credenciais de serviço.
    4. Clique em **OK** para fechar a página de criação do {{site.data.keyword.messagehub}} e retornar para o console do {{site.data.keyword.messagehub}}.
    5. Na lista **Instância do MessageHub**, é possível agora selecionar **Inserir suas próprias credenciais**e fornecer as seguintes informações:
      * Nome do usuário: seu nome do usuário do {{site.data.keyword.messagehub}}
      * Senha: sua senha do {{site.data.keyword.messagehub}}
      * kafka_admin_url: sua URL de REST do administrador do {{site.data.keyword.messagehub}}
      * Banco de dados: o nome de seu banco de dados do {{site.data.keyword.messagehub}}
      * Tópico: o tópico para assinar

5. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo de Eventos do {{site.data.keyword.messagehub}} pela CLI

1. Clone o repositório do modelo.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Navegue para o diretório para o tempo de execução da ação que você deseja usar: `nodejs-6`, `nodejs`, `php`, `python` ou `swift`.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo, usando as variáveis de ambiente a seguir.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Entendendo as variáveis de ambiente</caption>
    <thead>
    <th colspan=2>Entendendo as variáveis de ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>Sua URL de REST do administrador do {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>O tópico para assinar</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>Seu nome de usuário do {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>Sua senha do {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Um nome customizado para o pacote</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Um nome customizado para a regra</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Um nome customizado para o acionador</td></tr>
    </tbody></table>

## Implementando o modelo Lembrete periódico do Slack
{: #slack-reminder-template}

O modelo Lembrete periódico do Slack posta no Slack em um intervalo fornecido pelo usuário durante a criação do acionador.

### Implementando o Modelo Periodic Slack Reminder da UI

1. Acesse https://api.slack.com/incoming-webhooks para configurar a URL de webhooks recebida necessária.

1. Acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create) no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Eventos de lembrete de folga periódico**.

4. Crie a ação Lembrete de folga periódico.
    1. Forneça um nome para seu pacote ou use o nome padrão fornecido `periodic-slack-reminder`.
    2. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.
    3. Na seção Parâmetros, insira a URL do webhook no campo **Valor do parâmetro**, como `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Clique em **Avançar**.

5. Crie o acionador Lembrete de folga periódico. Os acionadores chamam ações quando recebem eventos de origens de eventos.
    1. Especifique o intervalo de tempo do acionador usando uma expressão Padrão ou Cron.
        * Padrão: selecione tempos UTC para os dias da semana, horas e minutos.
        * Cron: especifique uma sequência Cron com base na <a href="http://crontab.org">sintaxe de crontab UNIX</a>. Use 5 ou menos separados por espaços no formato `X X X X X`.
    2. Inclua a carga útil JSON do acionador.

6. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Lembrete de folga periódico por meio da CLI

1. Acesse https://api.slack.com/incoming-webhooks para configurar a URL de webhooks recebida necessária.

1. Clone o repositório do modelo.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Navegue para o diretório para o tempo de execução da ação que você deseja usar: `nodejs-6`, `nodejs`, `php`, `python` ou `swift`.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. Implemente o modelo, usando as variáveis de ambiente a seguir.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Entendendo as variáveis de ambiente</caption>
    <thead>
    <th colspan=2>Entendendo as variáveis de ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>A URL do webhook, tal como <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>Uma sequência Cron com base na <a href="http://crontab.org">sintaxe de crontab UNIX</a>. Use 5 ou menos campos separados por espaços no formato <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Um nome customizado para o pacote</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Um nome customizado para a regra</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Um nome customizado para o acionador</td></tr>
    </tbody></table>
