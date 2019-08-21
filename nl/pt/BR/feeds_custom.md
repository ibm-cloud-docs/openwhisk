---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: feeds, serverless, functions

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



# Criando feeds do provedor de eventos customizados
{: #feeds_custom}

O {{site.data.keyword.openwhisk_short}} suporta uma API aberta, em que qualquer usuário pode expor um serviço de produtor de evento como um feed em um pacote.
{: shortdesc}


## Arquitetura de feed
{: #feeds_arch}

É possível criar um feed usando um dos três padrões arquiteturais: **Ganchos**, **Pesquisa** e **Conexões**.

### Ganchos

Com o padrão de ganchos, um feed é configurado usando um [webhook](https://en.wikipedia.org/wiki/Webhook){: external} que é exposto por outro serviço. Nessa estratégia, um webhook é configurado em um serviço externo para POSTAR diretamente em uma URL para disparar um acionador. Esse método é de longe a opção mais fácil e mais atraente para implementar feeds de baixa frequência.

Por exemplo, o [pacote GitHub](/docs/openwhisk?topic=cloud-functions-pkg_github) e o [pacote de Notificação push](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) usam um webhook.


### Chamada Seletiva

Com o padrão de pesquisa, uma ação do {{site.data.keyword.openwhisk_short}} é organizada para pesquisar um terminal periodicamente para buscar novos dados. Esse padrão é relativamente fácil de construir, mas as frequências de eventos são limitadas pelo intervalo de pesquisa.

### Economia Conectada

Com o padrão de conexões, um serviço separado mantém uma conexão persistente com uma origem de feed. A implementação baseada em conexão pode interagir com um terminal em serviço usando intervalos de pesquisa longos ou configurar uma notificação push.

Por exemplo, o [pacote do {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) usa o padrão de conexões.



##  Implementando ações de feed
{: #feeds_actions}

A ação de feed é uma ação e aceita os parâmetros a seguir.

| Parâmetro | Descrição |
| --- | --- |
| `lifecycleEvent` | `CREATE`, `READ`, `UPDATE`, `DELETE`, `PAUSE` ou `UNPAUSE`. |
| `triggerName` | O nome completo do acionador, que contém eventos que são produzidos por meio desse feed. |
| `authKey` | As credenciais de autenticação básica do usuário do {{site.data.keyword.openwhisk_short}} que possui o acionador. |

A ação de feed também pode aceitar quaisquer outros parâmetros que ele precisa para gerenciar o feed. Por exemplo, a ação de feed de mudanças do {{site.data.keyword.cloudant}} espera receber parâmetros que incluam `dbname` e `username`.

Quando o usuário cria um acionador por meio da CLI com o parâmetro `--feed`, a ação de feed é chamada automaticamente com os parâmetros apropriados.

Por exemplo, um usuário cria uma ligação **mycloudant** para o pacote `cloudant` com um nome de usuário e senha como parâmetros ligados. Quando o usuário emite o comando a seguir na CLI:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Em seguida, algo equivalente ao comando a seguir é executado:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

A ação de feed que é denominada *changes* usa esses parâmetros e deve executar qualquer ação necessária para configurar um fluxo de eventos do {{site.data.keyword.cloudant_short_notm}}. A ação de feed ocorre usando a configuração apropriada, que é direcionada para o acionador.

Para o feed {{site.data.keyword.cloudant_short_notm}}*changes*, a ação conversa diretamente com um serviço de *{{site.data.keyword.cloudant_short_notm}} acionador* que é implementado com uma arquitetura baseada em conexão.

Um protocolo de ação de feed semelhante ocorre para `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementando feeds com ganchos
{: #feeds_hooks}

Configure um feed usando um gancho quando um produtor de eventos suportar um recurso de retorno de chamada de webhook.

Com esse método, não é necessário manter nenhum serviço persistente fora do {{site.data.keyword.openwhisk_short}}. Todo o gerenciamento de feed acontece naturalmente por meio de **ações de feed** stateless do {{site.data.keyword.openwhisk_short}}, que negociam diretamente com uma API de webhook de terceiros.

Quando chamada com `CREATE`, a ação de feed simplesmente instala um webhook para algum outro serviço, solicitando que o serviço remoto efetue POST de notificações para a URL apropriada do `fireTrigger` no {{site.data.keyword.openwhisk_short}}.

O webhook é direcionado para enviar notificações para uma URL como:

`POST /namespaces/{namespace}/triggers/{triggerName}`

O formulário com a solicitação de POST é interpretado como um documento JSON que define parâmetros no evento acionador. As regras do {{site.data.keyword.openwhisk_short}} passam esses parâmetros acionadores para quaisquer ações para disparar como resultado do evento.

## Implementando feeds com pesquisa
{: #feeds_polling}

É possível configurar uma ação para pesquisar uma origem de feed inteiramente dentro do {{site.data.keyword.openwhisk_short}}, sem a necessidade de manter quaisquer conexões persistentes ou serviço externo.

Para feeds em que um webhook não está disponível, mas não precisam de tempos de resposta de alto volume ou baixa latência, é possível usar a pesquisa.

Para configurar um feed baseado em pesquisa, a ação de feed usa as etapas a seguir quando chamada para `CREATE`:

1. A ação de feed configura um acionador periódico com uma frequência específica, usando o feed `whisk.system/alarms`.
2. O desenvolvedor de feed cria uma ação `pollMyService` que pesquisa o serviço remoto e retorna quaisquer novos eventos.
3. A ação de feed configura uma *regra* *T -> pollMyService*.

Esse procedimento implementa um acionador baseado em pesquisa inteiramente usando ações do {{site.data.keyword.openwhisk_short}}, sem nenhuma necessidade de um serviço separado.

## Implementando feeds usando conexões
{: #feeds_connections}

As duas opções anteriores de arquitetura são simples e fáceis de implementar. No entanto, se você desejar um feed de alto desempenho, será possível usar conexões persistentes e técnicas de pesquisa longa ou semelhantes.

Como as ações do {{site.data.keyword.openwhisk_short}} devem ser de execução curta, uma ação não pode manter uma conexão persistente com um terceiro. Em vez disso, é possível levantar um serviço separado, chamado **serviços do provedor**, fora do {{site.data.keyword.openwhisk_short}} que é executado o tempo todo. Um serviço do provedor pode manter conexões com origens de eventos de terceiros que suportam notificações de pesquisa longa ou outras baseadas em conexão.

O serviço do provedor tem uma API de REST que permite que a **ação de feed** do {{site.data.keyword.openwhisk_short}} controle o feed. O serviço do provedor age como um proxy entre o provedor de evento e o {{site.data.keyword.openwhisk_short}}. Quando ele recebe eventos do terceiro, ele os envia para o {{site.data.keyword.openwhisk_short}} disparando um acionador.

O feed **changes** do {{site.data.keyword.cloudant_short_notm}} é o exemplo canônico, pois ele representa um serviço `cloudanttrigger`, que media entre as notificações do {{site.data.keyword.cloudant_short_notm}} sobre uma conexão persistente e os acionadores do {{site.data.keyword.openwhisk_short}}.


O feed **alarme** é implementado com um padrão semelhante.

A arquitetura baseada em conexão é a opção de desempenho mais alto, mas as operações são mais laboriosas do que as arquiteturas de pesquisa e de gancho.






