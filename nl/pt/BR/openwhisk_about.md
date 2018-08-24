---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Arquitetura da plataforma
{: #openwhisk_about}

O {{site.data.keyword.openwhisk}} é uma plataforma de cálculo acionado por eventos, também referida como computação Sem servidor ou como Function as a Service (FaaS), que executa código em resposta a eventos ou chamadas diretas.
{: shortdesc}

## Tecnologia do {{site.data.keyword.openwhisk_short}}
{: #technology}

Aprenda sobre alguns conceitos básicos da tecnologia por trás do {{site.data.keyword.openwhisk_short}}:

<dl>
<dt>Ação</dt>
<dd>Uma [ação](openwhisk_actions.html) é uma parte do código que executa uma tarefa específica. Uma ação pode ser escrita na linguagem de sua escolha, como pequenos fragmentos de código JavaScript ou Swift ou código binário customizado integrado em um contêiner do Docker. Você fornece a ação do Cloud Functions como código-fonte ou como uma imagem do Docker.
<br><br>Uma ação executa trabalho quando ela é chamada diretamente usando a API, CLI ou iOS SDK do {{site.data.keyword.openwhisk_short}}. Uma ação também pode responder automaticamente a eventos de serviços {{site.data.keyword.Bluemix_notm}} e serviços de terceiros usando um acionador.</dd>
<dt>Sequência</dt>
<dd>Um conjunto de ações que podem ser encadeadas em uma [sequência](openwhisk_actions.html#openwhisk_create_action_sequence) sem ter que gravar qualquer código. Uma sequência é uma cadeia de ações, chamadas em ordem, em que a saída de uma ação é passada como entrada para a próxima ação. Isso permite combinar ações existentes para reutilização rápida e fácil. Uma sequência pode então ser chamada exatamente como uma ação, por meio de uma API de REST ou automaticamente em resposta a eventos.
</dd>
<dt>Evento</dt>
<dd>Exemplos de eventos incluem mudanças em registros do banco de dados, leituras do sensor IoT que excedem uma determinada temperatura, novas consolidações de código para um repositório GitHub ou solicitações de HTTP simples de apps da web ou móveis. Os eventos de origens de eventos externos e internos são canalizados por meio de um acionador e as regras permitem que as ações reajam a esses eventos.</dd>
<dt>Acionador</dt>
<dd>[Acionadores](openwhisk_triggers_rules.html#openwhisk_triggers_create) são um canal nomeado para uma classe de eventos. Um acionador é uma declaração de que você deseja reagir a um determinado tipo de evento, seja de um usuário ou por uma origem de eventos.</dd>
<dt>Regra</dt>
<dd>Uma [regra](openwhisk_triggers_rules.html#openwhisk_rules_use) associa um acionador a uma ação. Toda vez que o acionador for disparado, a regra usará o evento acionador como entrada e chamará a ação associada. Com o conjunto apropriado de regras, é possível que um único evento acionador chame várias ações ou que uma ação seja chamada como uma resposta a eventos de vários acionadores.</dd>
<dt>Feed</dt>
<dd>Um [feed](openwhisk_feeds.html#openwhisk_feeds) é uma maneira conveniente de configurar uma origem de eventos externos para disparar eventos acionadores que podem ser consumidos pelo {{site.data.keyword.openwhisk_short}}. 
Por exemplo, um feed do Git pode disparar um evento acionador para cada confirmação em um repositório Git.</dd>
<dt>Pacote</dt>
<dd>Integrações com serviços e provedores de eventos podem ser incluídas com pacotes. Um [pacote](openwhisk_packages.html) é um pacote configurável de feeds e ações. Um feed é uma parte do código que configura uma origem de eventos externos para disparar eventos acionadores. Por exemplo, um acionador que é criado com um feed de mudanças do {{site.data.keyword.cloudant}} configura um serviço para disparar o acionador toda vez que um documento é modificado ou incluído em um banco de dados {{site.data.keyword.cloudant_short_notm}}. As ações em pacotes representam lógica reutilizável que um provedor de serviços pode disponibilizar para que os desenvolvedores podem usar o serviço como uma origem de eventos e chamar as APIs desse serviço.
<br><br>Um catálogo de pacotes existente oferece uma forma rápida de aprimorar aplicativos com recursos úteis e de acessar serviços externos no ecossistema. Exemplos de serviços externos que têm pacotes do {{site.data.keyword.openwhisk_short}} incluem {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack e GitHub.</dd>
</dl>

## Como o {{site.data.keyword.openwhisk_short}} funciona
{: #openwhisk_how}

Para explicar todos os componentes em mais detalhes, vamos rastrear uma chamada de uma ação por meio do sistema {{site.data.keyword.openwhisk_short}}. Uma chamada executa o código que o usuário alimenta no sistema e retorna os resultados dessa execução. A figura a seguir mostra a arquitetura de alto nível do {{site.data.keyword.openwhisk_short}}.

![{{site.data.keyword.openwhisk_short}} architecture](./images/OpenWhisk.png)


## Como o processamento interno do OpenWhisk funciona
{: #openwhisk_internal}

O que acontece atrás dos cenários do OpenWhisk?

O OpenWhisk é um projeto de software livre que combina componentes, incluindo Nginx, Kafka, Docker e CouchDB, para formar um serviço de programação baseada em evento sem servidor.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="O fluxo interno de processamento por trás do OpenWhisk" style="width:550px; border-style: none"/>

### Inserindo o sistema: nginx

Primeiro, a API voltada para o usuário do OpenWhisk é completamente baseada em HTTP e segue um design RESTful. Como consequência, o comando que é enviado por meio da CLI é uma solicitação de HTTP com relação ao sistema OpenWhisk. O comando específico traduz aproximadamente para:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Observe a variável *$userNamespace* aqui. Um usuário tem acesso a pelo menos um namespace. Para simplificar, suponha que o usuário possua o namespace no qual *myAction* é colocado.

O primeiro ponto de entrada no sistema é por meio de **nginx**, “um servidor proxy reverso e HTTP”. Ele é usado para finalização de SSL e para encaminhamento de chamadas HTTP apropriadas para o próximo componente.

### Inserindo o sistema: Controlador

O Nginx encaminha a solicitação de HTTP ao **Controlador**, o próximo componente no caminho do OpenWhisk. É uma implementação baseada no Scala da API de REST real (com base no **Akka** e **Spray**) e, portanto, serve como a interface para tudo que um usuário pode fazer. Incluindo solicitações [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) para suas entidades no OpenWhisk e chamada de ações.

O Controlador primeiro esclarece o que o usuário está tentando fazer. Ele faz isso com base no método de HTTP usado em sua solicitação de HTTP. Conforme a tradução acima, o usuário está emitindo uma solicitação de POST para uma ação existente, que o Controlador traduz para uma **chamada de uma ação**.

Dada a função central do Controlador (portanto, o nome), todas as etapas a seguir o envolverão até um certo ponto.

### Autenticação e autorização: CouchDB

Agora o controlador verifica quem você é (*Autenticação*) e se tem o privilégio para fazer o que deseja fazer com essa entidade (*Autorização*). As credenciais incluídas na solicitação são verificadas com relação ao banco de dados denominado **assuntos** em uma instância **CouchDB**.

Nesse caso, é verificado que o usuário existe no banco de dados do OpenWhisk e que tem o privilégio para chamar a ação *myAction*, que se supõe que seja uma ação em um namespace que o usuário possui. Esse último fornece efetivamente ao usuário o privilégio para chamar a ação.

Como tudo está adequado, a porta se abre para o próximo estágio de processamento.

### Obtendo a ação: CouchDB… novamente

Como o Controlador agora está certo de que o usuário tem permissão e tem os privilégios para chamar a ação, ele carrega essa ação (neste caso *myAction*) do banco de dados **whisks** em CouchDB.

O registro da ação contém principalmente o código a ser executado e os parâmetros padrão que você deseja passar para a sua ação, mesclados com os parâmetros incluídos na solicitação de chamada real. Ele também contém as restrições de recurso que são impostas na execução, como a memória que é permitido consumir.

Nesse caso específico, a ação não toma nenhum parâmetro (a definição de parâmetro da função é uma lista vazia). Portanto, supõe-se que os parâmetros padrão não estão configurados, incluindo parâmetros específicos para a ação, contribuindo para o caso mais trivial desse ponto de vista.


### Quem está lá para chamar a ação: Load Balancer

O Load Balancer, que faz parte do Controlador, tem uma visão global dos executores disponíveis no sistema verificando seu status de funcionamento continuamente. Esses executores são chamados de **Invocadores**. O Load Balancer, sabendo quais invocadores estão disponíveis, escolhe um deles para chamar a ação solicitada.

### Forme uma linha: Kafka

De agora em diante, principalmente duas coisas ruins podem acontecer com a solicitação de chamada que você enviou:

1. O sistema pode travar, perdendo sua chamada.
2. O sistema pode estar sob uma carga tão pesada que a chamada precisa esperar que outras chamadas concluam primeiro.

A resposta para ambas é **Kafka**, “um sistema de mensagens de publicação/assinatura distribuídas e de alto rendimento”. O controlador e o invocador se comunicam unicamente por meio de mensagens que são armazenadas em buffer e persistidas pelo Kafka. O Kafka eleva a carga de armazenamento em buffer na memória, arriscando um *OutOfMemoryException*, fora do controlador e do invocador, enquanto também assegura que as mensagens não sejam perdidas no caso de travamento do sistema.

Para obter a ação chamada então, o Controlador publica uma mensagem para o Kafka, que contém a ação a ser chamada e os parâmetros a serem passados para essa ação (neste caso nenhum). Essa mensagem é endereçada ao invocador, que o controlador escolhe na lista que obteve do Consul.

Depois que o Kafka confirma que obteve a mensagem, a solicitação de HTTP para o usuário é respondida com um **ActivationId**. O usuário pode usar isso mais tarde para obter acesso aos resultados dessa chamada específica. Esse é um modelo de chamada assíncrona, em que a solicitação de HTTP é finalizada quando o sistema aceita a solicitação para chamar uma ação. Um modelo síncrono (denominado chamada de bloqueio) está disponível, mas não é coberto aqui.

### Chamando o código: invocador

O **Invocador** é o coração do OpenWhisk. A obrigação do Invocador é chamar uma ação. Ele também é implementado no Scala. Mas há ainda muito mais. Para executar ações de uma maneira isolada e segura, ele usa o **Docker**.

O Docker é usado para configurar um novo ambiente autoencapsulado (chamado *contêiner*) para cada ação que chamamos de uma maneira rápida, isolada e controlada. Para cada chamada de ação, um contêiner do Docker é gerado e o código de ação é injetado. O código é então executado usando os parâmetros que são passados para ele, o resultado é obtido e o contêiner é destruído. As otimizações de desempenho podem ser feitas nesse estágio para reduzir a sobrecarga e tornar os tempos de resposta o mais baixo possível.

Nesse caso, tendo uma ação baseada em *Node.js* em questão, o Invocador inicia um contêiner do Node.js. Em seguida, injeta o código de *myAction*, executa-o sem parâmetros, extrai o resultado, salva os logs e destrói o contêiner do Node.js novamente.

### Armazenando os resultados: CouchDB novamente

Conforme o resultado é obtido pelo invocador, ele é armazenado no banco de dados **whisks** como uma ativação sob o ActivationId. O banco de dados **whisks** reside em **CouchDB**.

Nesse caso específico, o Invocador obtém o objeto JSON resultante de volta da ação, captura o log gravado pelo Docker, coloca tudo no registro de ativação e armazena-o no banco de dados. Consulte o exemplo a seguir:
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

Observe como o registro contém tanto o resultado retornado quanto os logs gravados. Ele também contém os horários de início e de encerramento da chamada da ação. Os registros de ativação contêm mais campos, mas são divididos neste exemplo para simplicidade.

Agora é possível usar a API de REST novamente (iniciar na etapa 1 novamente) para obter sua ativação e, portanto, o resultado de sua ação. Para fazer isso, execute este comando:

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Sumário

É possível ver como uma ação simples de **ibmcloud fn action invoked myAction**
passa por diferentes estágios do sistema {{site.data.keyword.openwhisk_short}}. O sistema em si consiste principalmente em somente dois componentes customizados, o **Controlador** e o **Invocador**. Todo o resto já está lá, desenvolvido por muitas pessoas na comunidade de software livre.

É possível localizar informações adicionais sobre o {{site.data.keyword.openwhisk_short}} nos tópicos a seguir:

* [Nomes de entidades](./openwhisk_reference.html#openwhisk_entities)
* [Semântica de ação](./openwhisk_reference.html#openwhisk_semantics)
* [  Limites
](./openwhisk_reference.html#openwhisk_syslimits)
* [referência de API REST](https://console.bluemix.net/apidocs/functions)
