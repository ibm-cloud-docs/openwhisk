---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Sobre o {{site.data.keyword.openwhisk_short}}

O {{site.data.keyword.openwhisk}} é uma plataforma de cálculo acionado por eventos, também referida como computação Sem servidor ou como Function as a Service (FaaS), que executa código em resposta a eventos ou chamadas diretas. A figura a seguir mostra a arquitetura de alto nível do {{site.data.keyword.openwhisk}}.
{: shortdesc}

![{{site.data.keyword.openwhisk_short}} architecture](./images/OpenWhisk.png)

Exemplos de eventos incluem mudanças em registros do banco de dados, leituras do sensor IoT que excedem uma determinada temperatura, novas consolidações de código para um repositório GitHub ou solicitações de HTTP simples de apps da web ou móveis. Os eventos de origens de eventos externos e internos são canalizados por meio de um acionador e as regras permitem que as ações reajam a esses eventos.


Ações podem ser pequenos fragmentos de código JavaScript ou Swift ou código binário
customizado integrado em um contêiner do Docker. As ações no {{site.data.keyword.openwhisk_short}} são implementadas e executadas instantaneamente sempre que um acionador é disparado. Quanto mais acionadores são disparados, mais ações são chamadas. Se nenhum acionador é disparado, nenhum código de ação está em execução, o custo permanece zero.

Além de associar ações a acionadores, é possível chamar uma ação diretamente usando a API, CLI ou iOS SDK do {{site.data.keyword.openwhisk_short}}. Um conjunto de ações também pode ser encadeado sem precisar escrever qualquer código. Cada ação na cadeia é chamada em sequência com a saída de uma ação que é passada como entrada para a próxima na sequência.

Com máquinas virtuais ou contêineres de longa execução tradicionais, é uma prática comum implementar vários contêineres ou VMs para ser resiliente contra indisponibilidades de uma única instância. No entanto, o {{site.data.keyword.openwhisk_short}} oferece um modelo alternativo sem gasto adicional de custo relacionado à resiliência. A execução sob demanda de ações fornece escalabilidade inerente e utilização ideal, pois o número de ações em execução sempre corresponde à taxa de acionador. Além disso, o desenvolvedor pode agora focar o código e não se preocupar com monitoramento, correção e proteção da infraestrutura subjacente do servidor, armazenamento, rede e sistema operacional.

Integrações com serviços e provedores de eventos podem ser incluídas com pacotes. Um pacote é um pacote configurável de feeds e ações. Um feed é uma parte de código que configura uma origem de eventos externos para disparar eventos acionadores. Por exemplo, um acionador criado com um feed de mudança do Cloudant configura um serviço para disparar o acionador sempre que um documento é modificado ou incluído em um banco de dados Cloudant. As ações em pacotes representam lógica reutilizável que um provedor de serviços pode disponibilizar para que os desenvolvedores podem usar o serviço como uma origem de eventos e chamar as APIs desse serviço.

Um catálogo de pacotes existente oferece uma forma rápida de aprimorar aplicativos com recursos úteis e de acessar serviços externos no ecossistema. Exemplos de serviços externos que são ativados pelo {{site.data.keyword.openwhisk_short}} incluem Cloudant, The Weather Company, Slack e GitHub.


## Como o {{site.data.keyword.openwhisk_short}} funciona
{: #openwhisk_how}

Sendo um projeto de software livre, o OpenWhisk se posiciona nos ombros de gigantes, incluindo Nginx, Kafka, Consul, Docker, CouchDB. Todos esses componentes se juntam para formar um “serviço de programação baseada em eventos Serverless”. Para explicar todos os componentes em mais detalhes, vamos rastrear uma chamada de uma ação por meio do sistema conforme ela acontece. Uma chamada no OpenWhisk é a coisa principal que um mecanismo sem servidor faz: executar o código que o usuário alimentou no sistema e retornar os resultados dessa execução.

### Criando a ação

Para dar explicação de algum contexto, podemos criar uma ação no sistema primeiro. Em seguida, usar essa ação para explicar os conceitos enquanto rastreia todo o sistema. Os comandos a seguir presumem que a [CLI do OpenWhisk está configurada corretamente](https://github.com/openwhisk/openwhisk/tree/master/docs#setting-up-the-openwhisk-cli).

Primeiro, crie um arquivo *action.js* contendo o código a seguir, que imprime "Hello World" para stdout e retorna um objeto JSON contendo "world" sob a chave "hello".
```javascript
function main() {
    console.log('Hello World');
    return { hello: 'world' };
}
```
{: codeblock}

Crie a ação executando o comando a seguir:
```
wsk action create myAction action.js
```
{: pre}

Agora, execute o comando a seguir para chamar essa ação:
```
wsk action invoke myAction --result
```
{: pre}

## O fluxo interno de processamento
O que acontece atrás dos cenários do OpenWhisk?

![Fluxo de processamento do OpenWhisk](images/OpenWhisk_flow_of_processing.png)

### Inserindo o sistema: nginx

Primeiro, a API voltada para o usuário do OpenWhisk é completamente baseada em HTTP e segue um design RESTful. Como consequência, o comando que é enviado por meio do wsk-CLI é essencialmente uma solicitação de HTTP com relação ao sistema OpenWhisk. O comando específico traduz aproximadamente para:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Observe a variável *$userNamespace* aqui. Um usuário tem acesso a pelo menos um namespace. Para simplificar, suponha que o usuário possua o namespace no qual *myAction* é colocado.

O primeiro ponto de entrada no sistema é por meio de **nginx**, “um servidor proxy reverso e HTTP”. Ele é usado para finalização de SSL e para encaminhamento de chamadas HTTP apropriadas para o próximo componente.

### Inserindo o sistema: Controlador

O Nginx encaminha a solicitação de HTTP ao **Controlador**, o próximo componente no caminho do OpenWhisk. É uma implementação baseada no Scala da API de REST real (com base no **Akka** e **Spray**) e, portanto, serve como a interface para tudo que um usuário pode fazer. Incluindo solicitações [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) para suas entidades no OpenWhisk e chamada de ações.

O Controlador primeiro esclarece o que o usuário está tentando fazer. Ele faz isso com base no método de HTTP usado em sua solicitação de HTTP. Conforme a tradução acima, o usuário está emitindo uma solicitação de POST para uma ação existente, que o controlador traduz para uma **chamada de uma ação**.

Dada a função central do Controlador (portanto, o nome), todas as etapas a seguir o envolverão até um certo ponto.

### Autenticação e autorização: CouchDB

Agora o controlador verifica quem você é (*Autenticação*) e se tem o privilégio para fazer o que deseja fazer com essa entidade (*Autorização*). As credenciais incluídas na solicitação são verificadas com relação ao banco de dados denominado **assuntos** em uma instância **CouchDB**.

Nesse caso, é verificado se o usuário existe no banco de dados do OpenWhisk e se tem o privilégio para chamar a ação *myAction*, que presume-se ser uma ação em um namespace que o usuário possui. Esse último fornece efetivamente ao usuário o privilégio para chamar a ação.

Como tudo está adequado, a porta se abre para o próximo estágio de processamento.

### Obtendo a ação: CouchDB… novamente

Como o controlador agora está certo de que o usuário tem permissão e tem os privilégios para chamar a ação, ele carrega essa ação (neste caso *myAction*) do banco de dados **whisks** no CouchDB.

O registro da ação contém principalmente o código a ser executado e os parâmetros padrão que você deseja passar para a sua ação, mesclados com os parâmetros incluídos na solicitação de chamada real. Ele também contém as restrições de recurso que são impostas na execução, como a memória que é permitido consumir.

Nesse caso específico, a ação não toma nenhum parâmetro (a definição de parâmetro da função é uma lista vazia). Portanto, supõe-se que os parâmetros padrão não estão configurados, incluindo parâmetros específicos para a ação, contribuindo para o caso mais trivial desse ponto de vista.

### Quem está lá para chamar a ação: Consul

O controlador (ou mais especificamente a parte de balanceamento de carga dele) tem tudo no local agora para executar seu código, no entanto, ele precisa saber quem está disponível para fazer isso. O **Consul**, uma descoberta de serviço, é usado para monitorar executores disponíveis no sistema verificando seu status de funcionamento continuamente. Esses executores são chamados de **Invocadores**.

O controlador, agora sabendo quais invocadores estão disponíveis, escolhe um deles para chamar a ação solicitada.

Vamos supor neste caso que o sistema tem três invocadores disponíveis, Invocadores 0 - 2, e que o controlador escolheu *Invocador 2* para chamar a ação em questão.

### Forme uma linha: Kafka

De agora em diante, principalmente duas coisas ruins podem acontecer com a solicitação de chamada que você enviou:

1. O sistema pode travar, perdendo sua chamada.
2. O sistema pode estar sob uma carga tão pesada que a chamada precisa esperar que outras chamadas concluam primeiro.

A resposta para ambas é **Kafka**, “um sistema de mensagens de publicação/assinatura distribuídas e de alto rendimento”. O controlador e o invocador se comunicam unicamente por meio de mensagens que são armazenadas em buffer e persistidas pelo Kafka. O Kafka eleva a carga de armazenamento em buffer na memória, arriscando um *OutOfMemoryException*, fora do controlador e do invocador, enquanto também assegura que as mensagens não sejam perdidas no caso de travamento do sistema.

Para obter a ação chamada, o controlador publica uma mensagem para o Kafka, que contém a ação a ser chamada e os parâmetros a serem passados para essa ação (neste caso, nenhum). Essa mensagem é endereçada ao invocador, que o controlador escolhe na lista que obteve do Consul.

Depois que o Kafka confirma que obteve a mensagem, a solicitação de HTTP para o usuário é respondida com um **ActivationId**. O usuário pode usar isso mais tarde para obter acesso aos resultados dessa chamada específica. Este é um modelo de chamada assíncrona, em que a solicitação de HTTP é finalizada quando o sistema aceita a solicitação para chamar uma ação. Um modelo síncrono (denominado chamada de bloqueio) está disponível, mas não é coberto aqui.

### Chamando o código: invocador

O **Invocador** é o coração do OpenWhisk. A obrigação do invocador é chamar uma ação. Ele também é implementado no Scala. Mas há ainda muito mais. Para executar ações de uma maneira isolada e segura, ele usa o **Docker**.

O Docker é usado para configurar um novo ambiente autocontido (chamado *contêiner*) para cada ação que chamamos de uma maneira rápida, isolada e controlada. Para cada chamada de ação, um contêiner do Docker é gerado e o código de ação é injetado. O código é então executado usando os parâmetros que são passados para ele, o resultado é obtido e o contêiner é destruído. As otimizações de desempenho podem ser feitas nesse estágio para reduzir a sobrecarga e tornar os tempos de resposta o mais baixo possível. 

Nesse caso, tendo uma ação baseada no *Node.js* em questão, o invocador inicia um contêiner do Node.js. Em seguida, injeta o código de *myAction*, executa-o sem parâmetros, extrai o resultado, salva os logs e destrói o contêiner do Node.js novamente.

### Armazenando os resultados: CouchDB novamente

Conforme o resultado é obtido pelo invocador, ele é armazenado no banco de dados **whisks** como uma ativação sob o ActivationId. O banco de dados **whisks** reside em **CouchDB**.

Nesse caso específico, o invocador obtém o objeto JSON resultante de volta da ação, captura o log gravado pelo Docker, coloca tudo no registro de ativação e armazena-o no banco de dados. Consulte o exemplo a seguir:

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

Observe como o registro contém tanto o resultado retornado quanto os logs gravados. Ele também contém o horário de início e de encerramento da chamada da ação. Os registros de ativação contêm mais campos, mas são divididos neste exemplo para simplicidade.

Agora é possível usar a API de REST novamente (inicie na etapa 1 novamente) para obter sua ativação e, portanto, o resultado de sua ação. Para fazer isso, execute este comando:

```bash
wsk activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre} 

### Sumário

É possível ver como um simples **wsk action invoked myAction** passa por diferentes estágios do sistema do {{site.data.keyword.openwhisk_short}}. O sistema em si consiste principalmente em somente dois componentes customizados, o **Controlador** e o **Invocador**. Todo o resto já está lá, desenvolvido por muitas pessoas na comunidade de software livre.

É possível localizar informações adicionais sobre o {{site.data.keyword.openwhisk_short}} nos tópicos a seguir:

* [Nomes de entidades](./openwhisk_reference.html#openwhisk_entities)
* [Semântica de ação](./openwhisk_reference.html#openwhisk_semantics)
* [  Limites
](./openwhisk_reference.html#openwhisk_syslimits)
* [REST API](./openwhisk_reference.html#openwhisk_ref_restapi)
