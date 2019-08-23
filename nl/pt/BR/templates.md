---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

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


# Implementando modelos de iniciação rápida
{: #templates}

O {{site.data.keyword.openwhisk}} oferece um catálogo de modelos para ajudá-lo a iniciar rapidamente seu próximo projeto. Os modelos são uma combinação de ações, acionadores, sequências. Alguns modelos também incorporam outras instâncias de serviço do {{site.data.keyword.cloud_notm}}. Usando modelos, é possível criar um projeto de modo rápido e fácil e iniciar a codificação imediatamente.
{: shortdesc}


## Modelos de iniciação rápida disponíveis
{: #available-templates}

| Nome | Descrição | Tempos de Execução Suportados | Repositório de origem |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [Eventos do {{site.data.keyword.cloudant_short_notm}}](#cloudant-template) | Quando um documento é editado ou incluído em um banco de dados {{site.data.keyword.cloudantfull}}, registre a mudança no console. | Node.js, Swift, Python, PHP | [Código](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Fazer upload da imagem](#image-template) | Use uma ação da web para fazer upload de uma imagem para um depósito da instância do {{site.data.keyword.cos_full}} e, em seguida, recuperar uma miniatura dessa imagem. | Node.js | [Código](https://github.com/ibm-functions/template-cloud-object-storage){: external}
| [Obter recurso HTTP](#get-http-resource-template) | Chame uma ação da web usando um evento HTTP e obterá dados da API Weather do Yahoo. | Node.js, Python | [Código](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | Crie uma ação básica que aceite um objeto JSON como um único parâmetro. | Node.js, Swift, Python, PHP | [Código](https://github.com/ibm-functions/template-hello-world){: external}
| [Eventos do {{site.data.keyword.messagehub}}](#messagehub-events-template) | Quando novos dados são incluídos em um tópico do {{site.data.keyword.messagehub_full}}, registre a mudança no console. | Node.js, Swift, Python, PHP | [Código](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Lembrete periódico do Slack](#slack-reminder-template) | Use um webhook para o Slack com base em um acionador periódico. | Node.js, Swift, Python, PHP | [Código](https://github.com/ibm-functions/template-reminder-slack){: external}

É possível acessar o código para cada modelo em seu repositório de código, modificá-lo conforme desejar e criar sua própria função ao redor dele.
{: tip}


## Implementando o modelo de Eventos do  {{site.data.keyword.cloudant_short_notm}}
{: #cloudant-template}

O modelo do {{site.data.keyword.cloudant_short_notm}} cria uma sequência de ações e um acionador que inicia essa sequência. O acionador é disparado quando uma mudança é feita no banco de dados de cats de exemplo do {{site.data.keyword.cloudant_short_notm}} conectado. O item de dados esperado é um gato, com um nome e uma cor definida. Quando um novo cat é incluído no banco de dados ou um cat atual é editado, os dados são registrados no console.

### Implementando o modelo de Eventos do {{site.data.keyword.cloudant_short_notm}} da UI
{: #cloudant-template-ui}

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

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
    5. Na lista **Instância do Cloudant**, é possível agora selecionar **Inserir suas próprias credenciais** e fornecer as informações a seguir.
        
        * `Username` - Seu nome de usuário do {{site.data.keyword.cloudant_short_notm}}.
        * `Password` - Sua senha do {{site.data.keyword.cloudant_short_notm}}.
        * `Host` - `<username>.cloudant.com`. 
        * `Database` - O nome do seu banco de dados do {{site.data.keyword.cloudant_short_notm}}.

6. Clique em **Implementar**. Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo de Eventos do {{site.data.keyword.cloudant_short_notm}} pela CLI
{: #cloudant-template-cli}

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
{: #image-template}

O modelo de Imagem de upload cria uma ação da web que é possível usar para fazer upload de uma imagem para um depósito do {{site.data.keyword.cos_full_notm}} por meio de uma interface pequena. Em seguida, o modelo recupera a imagem como uma miniatura e a exibe na interface da ação da web.


### Implementando o modelo de Upload de imagem da UI
{: #image-template-ui}

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Fazer upload da imagem**.

4. Insira um nome para seu pacote ou use o nome padrão `upload-image` e clique em **Avançar**.

6. O modelo requer credenciais de serviço de uma instância de serviço do {{site.data.keyword.cos_full_notm}}. Na lista **{{site.data.keyword.cos_full_notm}}**, selecione uma das opções a seguir:
  * **Criar uma instância**. Se você não tiver uma instância de serviço existente, selecione essa opção para criar uma.
      1. Na página de criação da instância de serviço do {{site.data.keyword.cos_full_notm}} que é aberta, crie uma instância de serviço.
      2. [Criar um conjunto de credenciais de serviço do HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [ Crie pelo menos um depósito ](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).
  * **Inserir suas próprias credenciais**. Selecione essa opção para inserir manualmente suas próprias credenciais para uma instância de serviço do {{site.data.keyword.cos_full_notm}}. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.
  * **Instâncias existentes**. Se você tiver alguma instância do {{site.data.keyword.cos_full_notm}} existente, selecione uma das instâncias na lista. As credenciais devem ter chaves HMAC e a instância de serviço deve ter pelo menos um depósito.

7. Clique em **Implementar**.

8. Na navegação à esquerda, clique em **Terminais**

9. Na seção **Ação da web**, copie o link sem o sufixo .json e cole-o na barra de endereço de seu navegador. A interface para a ação da web do modelo é exibida.

10. Opcional: após a implementação do modelo, é possível navegar para o painel **Ações** para customizar o código nos dois novos pacotes:
    * O pacote `cloud-object-storage`, que contém ações que funcionam com as instâncias do {{site.data.keyword.cos_full_notm}}
    * O pacote de modelo (nome padrão `upload-image`), que contém a ação `app`

### Implementando o modelo de Imagem de upload por meio da CLI
{: #image-template-cli}

1. [Instale o pacote do {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

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

3. Implemente o modelo, usando um nome de pacote customizado e o nome de seu depósito do {{site.data.keyword.cos_full_notm}} como variáveis de ambiente.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Na [página Ações ](https://cloud.ibm.com/openwhisk/actions){: external} do console do {{site.data.keyword.openwhisk_short}}, clique na ação `process-change`.

5. Na navegação à esquerda, clique em **Terminais**.

6. Na seção **Ação da web**, copie o link sem o sufixo .json e cole-o na barra de endereço de seu navegador. A interface para a ação da web do modelo é exibida.

## Implementando o modelo Obter recurso HTTP
{: #get-http-resource-template}

O modelo Obter recurso de HTTP cria uma ação para buscar um recurso externo, a API do Yahoo Weather e, em seguida, retorna dados. A ação é ativada como uma ação da web, permitindo que ela seja chamada com uma URL, que é ativada para CORS e não precisa de uma chave de autenticação, que é útil para construir back-ends para aplicativos da web.

Por padrão, o terminal `get-http-resource` está publicamente disponível para qualquer pessoa que o chame.
{: note}

### Implementando o modelo Obter recurso HTTP da UI
{: #get-http-resource-template-ui}

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Obter recurso HTTP**.

3. Insira um nome para seu pacote ou use o nome padrão `get-http-resource`.

4. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6 ou Python 3.

5. Clique em **Implementar**.

6. Chame a ação executando curl da URL a seguir: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Por exemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Obter Recurso HTTP a partir da CLI
{: #get-http-resource-template-cli}

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

4. Chame a ação executando curl da URL a seguir: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Por exemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Implementando o modelo Hello World
{: #hello-world-template}

É possível implementar essa ação Hello World básica para iniciar com o {{site.data.keyword.openwhisk_short}} ou para testar outras entidades que você criar, como acionadores e regras.

### Implementando o modelo Hello World por meio da UI
{: #hello-world-template-ui}

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Hello World**.

4. Insira um nome para seu pacote ou use o nome padrão `hello-world`.

5. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

6. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Hello World por meio da CLI
{: #hello-world-template-cli}

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

O modelo de Eventos do {{site.data.keyword.messagehub}} cria uma ação e um acionador que inicia essa ação. O acionador é disparado sempre que um novo item é incluído no tópico do {{site.data.keyword.messagehub}} que é escolhido durante a criação do modelo.

### Implementando o modelo de Eventos do {{site.data.keyword.messagehub}} da UI
{: #messagehub-events-template-ui}

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

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
        * `Username` - Seu nome de usuário do {{site.data.keyword.messagehub}}.
        * `Password` - Sua senha do {{site.data.keyword.messagehub}}.
        * `kafka_admin_url` - Sua URL REST de administrador do {{site.data.keyword.messagehub}}.
        * `Database` - O nome do seu banco de dados do {{site.data.keyword.messagehub}}.
        * `Topic` - O tópico para assinar.

6. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo de Eventos do {{site.data.keyword.messagehub}} pela CLI
{: #messagehub-events-template-cli}

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

3. Implemente o modelo usando as variáveis de ambiente a seguir.
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
{: #slack-reminder-template-ui}

1. Acesse https://api.slack.com/incoming-webhooks para configurar a URL de webhooks recebida necessária.

1. Acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external} no console do {{site.data.keyword.openwhisk_short}}.

2. Clique em  ** Modelos de iniciação rápida **.

3. Clique em **Eventos de lembrete de folga periódico**.

4. Crie a ação Lembrete de folga periódico.
    1. Forneça um nome para seu pacote ou use o nome padrão fornecido `periodic-slack-reminder`.
    2. Escolha o tempo de execução para suas ações: Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.
    3. Na seção Parâmetros, insira a URL do webhook no campo **Valor do parâmetro**, como `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Clique em **Avançar**.

5. Crie o acionador Lembrete de folga periódico. Os acionadores chamam ações quando recebem eventos de origens de eventos.
    1. Especifique o intervalo de tempo do acionador usando uma expressão Pattern ou `Cron`.
        * `Pattern` - Selecione horários UTC para dias da semana, horas e minutos.
        * `Cron` - Especifique uma sequência de cron com base na <a href="http://crontab.org">sintaxe crontab do UNIX</a>. Use 5 ou menos separados por espaços no formato `X X X X X`.
    2. Inclua a carga útil JSON do acionador.

6. Clique em **Implementar**.

Após a implementação do modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e efetuar check-out do catálogo de modelos disponíveis.

### Implementando o modelo Lembrete de folga periódico por meio da CLI
{: #slack-reminder-template-cli}

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
    <tr><td><code>ALARM_CRON</code></td><td>Uma sequência de cron com base na <a href="http://crontab.org">sintaxe de crontab UNIX</a>. Use 5 ou menos campos no formato <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Um nome customizado para o pacote</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Um nome customizado para a regra</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Um nome customizado para o acionador</td></tr>
    </tbody></table>


