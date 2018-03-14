---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Implementando modelos no {{site.data.keyword.openwhisk_short}}
{: #serviceauth}

O {{site.data.keyword.openwhisk}} oferece um catálogo de Modelos para ajudar você a começar rapidamente em seu próximo projeto. Os modelos são uma combinação de Ações, Acionadores, Sequências e também podem incorporar instâncias de serviço do {{site.data.keyword.Bluemix}}. Usando Modelos, é possível criar um projeto de forma rápida e fácil e iniciar a codificação imediatamente. 

Este tutorial conduz você pela implementação do Modelo do Cloudant.
{: shortdesc}

## Modelos Disponíveis
{: #available-templates}

| Nome | Descrição | Tempos de Execução Suportados | 
|:-----------------|:-----------------|:-----------------|
| Eventos do Cloudant | Quando um DB Cloudant tiver um documento editado ou incluído, registre a mudança no console. | Node.js, Swift, Python, PHP |
| Obter recurso de HTTP | Uma ação da web que é chamada em resposta a um evento HTTP e, em seguida, busca dados da API do Yahoo Weather. | Node.js, Python |
| Hello World | Esta ação aceitará um único parâmetro, que deve ser um objeto JSON. | Node.js, Swift, Python, PHP |
| Eventos de hub de mensagens | Quando um tópico de Hub de mensagens tiver dados novos incluídos, registre a mudança no console. | Node.js, Swift, Python, PHP | 
| Lembrete periódico de folga | Uma ação que postará a Folga com base em um acionador periódico. | Node.js, Swift, Python, PHP |

## Implementando o modelo Eventos do Cloudant
{: #cloudant-template}

O Modelo do Cloudant cria uma sequência de ações e um Acionador que dará início a essa Sequência. O acionador é disparado quando há uma mudança no DB Cloudant conectado, que deve ser um banco de dados de gatos, com um nome e uma cor. O item de dados esperado é um gato, com um nome e uma cor definida. Quando um novo gato for incluído no banco de dados ou um gato atual for editado, os dados serão registrados no console.

1. Para criar um Modelo, acesse [{{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}}](https://dev-console.stage1.bluemix.net/openwhisk/) e, em seguida, clique em **Iniciar criação**. 

2. Clique em **Implementar modelo**.

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

### Implementar o Modelo do Cloudant

1. Clique em **Implementar**.

Após a implementação do Modelo, é possível fazer edições adicionais no código para customizá-lo conforme necessário ou voltar e verificar o catálogo de Modelos disponíveis.

