---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Implementando feeds
{: #openwhisk_feeds}

O {{site.data.keyword.openwhisk_short}} suporta uma API aberta, em que qualquer usuário pode expor um serviço de produtor de evento como um **feed** em um **pacote**. A seção a seguir descreve opções de arquitetura e de implementação para fornecer o seu próprio feed.
{: shortdesc}

Esse material é destinado a usuários avançados do {{site.data.keyword.openwhisk_short}} que pretendem publicar os seus próprios feeds. A maioria dos usuários do {{site.data.keyword.openwhisk_short}} pode ignorar seguramente a seção de arquitetura a seguir.

## Arquitetura de feed

Há pelo menos três padrões arquiteturais para criar um feed: **Ganchos**, **Pesquisa** e **Conexões**.

### Ganchos
No padrão *Ganchos*, um feed é configurado usando um recurso [webhook](https://en.wikipedia.org/wiki/Webhook) que é exposto por outro serviço. Nessa estratégia, um webhook é configurado em um serviço externo para POSTAR diretamente em uma URL para disparar um acionador. Esse método é de longe a opção mais fácil e mais atraente para implementar feeds de baixa frequência.

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### Chamada Seletiva
No padrão "Pesquisa", uma *Ação* do {{site.data.keyword.openwhisk_short}} é organizada para pesquisar um terminal periodicamente para buscar novos dados. Esse padrão é relativamente fácil de construir, mas as frequências de eventos são limitadas pelo intervalo de pesquisa.

### Economia Conectada
No padrão "Conexões", um serviço separado mantém uma conexão persistente com uma origem do feed. A implementação baseada em conexão pode interagir com um terminal em serviço usando intervalos de pesquisa longa ou para configurar uma notificação push.

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## Diferença entre feed e acionador

Os feeds e acionadores estão intimamente relacionados, mas são tecnicamente conceitos distintos.   

- O {{site.data.keyword.openwhisk_short}} processa **eventos** que fluem para o sistema.

- Um **Acionador** é tecnicamente um nome para uma classe de eventos. Cada evento pertence a exatamente um acionador; por analogia, um acionador é semelhante a um *tópico* em sistemas de publicação/assinatura baseados em tópico. Uma **Regra** *T -> A* significa "sempre que um evento do acionador *T* chegar, chame a ação *A* com a carga útil do acionador.

- Um **feed** é um fluxo de eventos que pertencem a algum acionador *T*. Um feed é controlado por uma **Ação de feed**, que manipula a criação, a exclusão, a pausa e a continuação do fluxo de eventos que compõem um feed. A Ação de feed geralmente interage com serviços externos que produzem os eventos, usando uma API de REST que gerencia notificações.

##  Implementando ações de feed

A *Ação de feed* é uma *Ação* normal do {{site.data.keyword.openwhisk_short}} e aceita os parâmetros a seguir:
* **lifecycleEvent**: um 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' ou 'UNPAUSE'.
* **triggerName**: o nome completo do acionador, que contém eventos que são produzidos nesse feed.
* **authKey**: as credenciais de aut. básica do usuário do {{site.data.keyword.openwhisk_short}} que é proprietário do acionador.

A ação de feed também pode aceitar qualquer outro parâmetro que ela precise para gerenciar o feed. Por exemplo, a ação de feed de mudanças do cloudant espera receber parâmetros que incluem *'dbname'*, *'username'* e assim por diante.

Quando o usuário cria um acionador na CLI com o parâmetro **--feed**, o sistema chama automaticamente chama a ação de feed com os parâmetros apropriados.

Por exemplo, suponha que o usuário crie uma ligação `mycloudant` para o pacote `cloudant` com um nome de usuário e senha como parâmetros ligados. Quando o usuário emite o comando a seguir na CLI:

`wsk trigger create T --feed mycloudant/changes -p dbName myTable`

Então, nos bastidores, o sistema faz algo equivalente ao comando a seguir:

`wsk action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype`

A ação de feed que é denominada *mudanças* toma esses parâmetros e deve executar qualquer ação necessária para configurar um fluxo de eventos do Cloudant. A ação de feed ocorre usando a configuração apropriada, que é direcionada ao acionador *T*.    

Para o feed *mudanças* do Cloudant, a ação conversa diretamente com um serviço *acionador do Cloudant* que é implementado com uma arquitetura baseada em conexão.

Um protocolo de ação de feed semelhante ocorre para `wsk trigger delete`, `wsk trigger update` e `wsk trigger get`.    

## Implementando feeds com ganchos

Será fácil configurar um feed usando um gancho se o produtor de evento suportar um recurso webhook/retorno de chamada.

Com esse método, _não há necessidade_ de levantar qualquer serviço persistente fora do {{site.data.keyword.openwhisk_short}}. Todo o gerenciamento de feed acontece naturalmente por meio de *Ações de feed* stateless do {{site.data.keyword.openwhisk_short}}, que negociam diretamente com uma API de webhook de terceiro.

Quando chamado com `CREATE`, a ação de feed simplesmente instala um webhook para algum outro serviço, solicitando ao serviço remoto para POSTAR notificações para a URL `fireTrigger` apropriada no {{site.data.keyword.openwhisk_short}}.

O webhook é direcionado para enviar notificações para uma URL como:

`POST /namespaces/{namespace}/triggers/{triggerName}`

O formulário com a solicitação de POST será interpretado como um documento JSON que define parâmetros no evento acionador. As regras do {{site.data.keyword.openwhisk_short}} passam esses parâmetros acionadores para quaisquer ações para disparar como resultado do evento.

## Implementando feeds com pesquisa

É possível configurar uma *Ação* do {{site.data.keyword.openwhisk_short}} para pesquisar uma origem do feed inteiramente no {{site.data.keyword.openwhisk_short}}, sem a necessidade de levantar qualquer conexão persistente ou serviço externo.

Para feeds nos quais um webhook não está disponível, mas não precisam de alto volume ou tempos de resposta de baixa latência, a pesquisa é uma opção atraente.

Para configurar um feed baseado em pesquisa, a ação de feed usa as etapas a seguir quando chamada para `CREATE`:

1. A ação de feed configura um acionador periódico (*T*) com a frequência desejada, usando o feed `whisk.system/alarms`.
2. O desenvolvedor de feed cria uma ação `pollMyService` que pesquisa o serviço remoto e retorna qualquer evento novo.
3. A ação de feed configura uma *Regra* *T -> pollMyService*.

Esse procedimento implementa um acionador baseado em pesquisa inteiramente usando ações {{site.data.keyword.openwhisk_short}}, sem qualquer necessidade de um serviço separado.

## Implementando feeds usando conexões

As duas opções anteriores de arquitetura são simples e fáceis de implementar. No entanto, se você deseja um feed de alto desempenho, não há substituto para conexões persistentes e técnicas de pesquisa
longa ou similares.

Como as ações do {{site.data.keyword.openwhisk_short}} devem ser de execução curta, uma ação não pode manter uma conexão persistente com um terceiro. Em vez disso, é possível levantar um serviço separado, chamado *serviços do provedor*, fora do {{site.data.keyword.openwhisk_short}} que é executado o tempo todo. Um serviço do provedor pode manter conexões com origens de eventos de terceiros que suportam notificações de pesquisa longa ou outras baseadas em conexão.

O serviço do provedor tem uma API de REST que permite que a *ação de feed* do {{site.data.keyword.openwhisk_short}} controle o feed. O serviço do provedor age como um proxy entre o provedor de evento e o {{site.data.keyword.openwhisk_short}}. Quando ele recebe eventos do terceiro, ele os envia para o {{site.data.keyword.openwhisk_short}} disparando um acionador.

O feed de *mudanças* do Cloudant é o exemplo canônico porque ele levanta um serviço `cloudanttrigger`, que media entre as notificações do Cloudant em uma conexão persistente e os acionadores do {{site.data.keyword.openwhisk_short}}.
<!-- TODO: add a reference to the open source implementation -->

O feed *alarme* é implementado com um padrão semelhante.

A arquitetura baseada em conexão é a opção de desempenho mais alto, mas impõe mais sobrecarga em operações que são comparadas com as arquiteturas de pesquisa e gancho.   
