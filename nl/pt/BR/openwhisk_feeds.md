---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: feed, event, polling, hooks, trigger, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criando feeds do provedor de eventos customizados
{: #openwhisk_feeds}

O {{site.data.keyword.openwhisk_short}} suporta uma API aberta, em que qualquer usuário pode expor um serviço de produtor de evento como um feed em um pacote. A seção a seguir descreve opções de arquitetura e de implementação para fornecer o seu próprio feed customizado.
{: shortdesc}

Esse material é destinado a usuários avançados do {{site.data.keyword.openwhisk_short}} que pretendem publicar os seus próprios feeds. A maioria dos usuários do {{site.data.keyword.openwhisk_short}} pode ignorar seguramente a seção de arquitetura a seguir.

## Arquitetura de feed

Há pelo menos três padrões arquiteturais para criar um feed: **Ganchos**, **Pesquisa** e **Conexões**.

### Ganchos
No padrão *Ganchos*, um feed é configurado usando um recurso [webhook](https://en.wikipedia.org/wiki/Webhook) que é exposto por outro serviço.   Nessa estratégia, um webhook é configurado em um serviço externo para POSTAR diretamente em uma URL para disparar um acionador. Esse método é de longe a opção mais fácil e mais atraente para implementar feeds de baixa frequência.



### Chamada Seletiva
No padrão "Pesquisa", uma ação do {{site.data.keyword.openwhisk_short}} é organizada para pesquisar um terminal periodicamente para buscar novos dados. Esse padrão é relativamente fácil de construir, mas as frequências de eventos são limitadas pelo intervalo de pesquisa.

### Economia Conectada
No padrão "Conexões", um serviço separado mantém uma conexão persistente com uma origem do feed. A implementação baseada em conexão pode interagir com um terminal em serviço usando intervalos de pesquisa longa ou para configurar uma notificação push.





## Diferença entre feed e acionador

Feeds e acionadores estão intimamente relacionados, mas tecnicamente têm conceitos distintos.   

- O {{site.data.keyword.openwhisk_short}} processa **eventos** que fluem para o sistema.

- Um **acionador** é tecnicamente um nome para uma classe de eventos. Cada evento pertence a exatamente um acionador; por analogia, um acionador é semelhante a um *tópico* em sistemas de pub-sub baseados em tópico. Uma **regra** *T -> A* significa que "sempre que um evento a partir do acionador *T* chegar, chame a ação *A* com a carga útil do acionador.

- Um **feed** é um fluxo de eventos em que todos pertencem a algum acionador *T*. Um feed é controlado por uma **ação de feed**, que manipula a criação, a exclusão, a pausa e a continuação do fluxo de eventos que compõem um feed. A ação de feed geralmente interage com serviços externos que produzem os eventos usando uma API de REST que gerencia notificações.

##  Implementando ações de feed

A *ação de feed* é uma *ação* normal do {{site.data.keyword.openwhisk_short}} e aceita os parâmetros a seguir:
* **lifecycleEvent**: um 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' ou 'UNPAUSE'.
* **triggerName**: o nome completo do acionador, que contém eventos que são produzidos por meio desse feed.
* **authKey**: as credenciais de autenticação básica do usuário do {{site.data.keyword.openwhisk_short}} que possui o acionador.

A ação de feed também pode aceitar quaisquer outros parâmetros que ele precisa para gerenciar o feed. Por exemplo, a ação de feed de mudanças do {{site.data.keyword.cloudant}} espera receber parâmetros que incluem *'dbname'*, *'username'* e assim por diante.

Quando o usuário cria um acionador a partir da CLI com o parâmetro **--feed**, o sistema automaticamente chama a ação de feed com os parâmetros apropriados.

Por exemplo, suponha que o usuário crie uma ligação **mycloudant** para o pacote `cloudant` com um nome de usuário e senha como parâmetros ligados. Quando o usuário emite o comando a seguir na CLI:
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

Então, nos bastidores, o sistema faz algo equivalente ao comando a seguir:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

A ação de feed que é denominada *changes* usa esses parâmetros e deve executar qualquer ação necessária para configurar um fluxo de eventos do {{site.data.keyword.cloudant_short_notm}}. A ação de feed ocorre usando a configuração apropriada, que é direcionada para o acionador *T*.

Para o feed *changes* do {{site.data.keyword.cloudant_short_notm}}, por uma casualidade a ação conversa diretamente com um serviço do *acionador do {{site.data.keyword.cloudant_short_notm}}* que é implementado com uma arquitetura baseada em conexão.

Um protocolo de ação de feed semelhante ocorre para `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementando Feeds com Ganchos

Será fácil configurar um feed usando um gancho se o produtor de evento suportar um recurso webhook/retorno de chamada.

Com esse método, _não há necessidade_ de levantar qualquer serviço persistente fora do {{site.data.keyword.openwhisk_short}}. Todo o gerenciamento de feed acontece naturalmente por meio das *ações de feed* do {{site.data.keyword.openwhisk_short}} stateless, que negociam diretamente com uma API do webhook de terceiro.

Quando chamada com `CREATE`, a ação de feed simplesmente instala um webhook para algum outro serviço, solicitando que o serviço remoto efetue POST de notificações para a URL apropriada do `fireTrigger` no {{site.data.keyword.openwhisk_short}}.

O webhook é direcionado para enviar notificações para uma URL como:

`POST /namespaces/{namespace}/triggers/{triggerName}`

O formulário com a solicitação de POST é interpretado como um documento JSON que define parâmetros no evento acionador. As regras do {{site.data.keyword.openwhisk_short}} passam esses parâmetros acionadores para quaisquer ações para disparar como resultado do evento.

## Implementando Feeds com Polling

É possível configurar uma *ação* do {{site.data.keyword.openwhisk_short}} para pesquisar uma origem do feed inteiramente no {{site.data.keyword.openwhisk_short}}, sem a necessidade de levantar qualquer conexão persistente ou serviço externo.

Para feeds nos quais um webhook não está disponível, mas não precisam de alto volume ou tempos de resposta de baixa latência, a pesquisa é uma opção atraente.

Para configurar um feed baseado em pesquisa, a ação de feed usa as etapas a seguir quando chamada para `CREATE`:

1. A ação de feed configura um acionador periódico (*T*) com a frequência desejada usando o feed `whisk.system/alarms`.
2. O desenvolvedor de feed cria uma ação `pollMyService` que pesquisa o serviço remoto e retorna quaisquer novos eventos.
3. A ação de feed configura uma *regra* *T -> pollMyService*.

Esse procedimento implementa um acionador baseado em pesquisa inteiramente usando ações do {{site.data.keyword.openwhisk_short}}, sem nenhuma necessidade de um serviço separado.

## Implementando feeds usando o Connections

As duas opções anteriores de arquitetura são simples e fáceis de implementar. No entanto, se você deseja um feed de alto desempenho, não há substituto para conexões persistentes e técnicas de pesquisa longa ou similares.

Como as ações do {{site.data.keyword.openwhisk_short}} devem ser de execução curta, uma ação não pode manter uma conexão persistente com um terceiro. Em vez disso, é possível levantar um serviço separado, chamado *serviços do provedor*, fora do {{site.data.keyword.openwhisk_short}} que é executado o tempo todo. Um serviço do provedor pode manter conexões com origens de eventos de terceiros que suportam notificações de pesquisa longa ou outras baseadas em conexão.

O serviço do provedor tem uma API de REST que permite que a *ação de feed* do {{site.data.keyword.openwhisk_short}} controle o feed. O serviço do provedor age como um proxy entre o provedor de evento e o {{site.data.keyword.openwhisk_short}}. Quando ele recebe eventos do terceiro, ele os envia para o {{site.data.keyword.openwhisk_short}} disparando um acionador.

O feed *changes* do {{site.data.keyword.cloudant_short_notm}} é o exemplo canônico, já que ele aciona um serviço `cloudanttrigger`, que media entre as notificações do {{site.data.keyword.cloudant_short_notm}} sobre uma conexão persistente e os acionadores do {{site.data.keyword.openwhisk_short}}.


O feed *alarme* é implementado com um padrão semelhante.

A arquitetura baseada em conexão é a opção de desempenho mais alto, mas impõe mais sobrecarga em operações que são comparadas com as arquiteturas de pesquisa e gancho.
