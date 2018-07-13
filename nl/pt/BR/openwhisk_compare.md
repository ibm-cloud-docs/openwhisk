---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Função como Serviço comparado
{: #openwhisk_faas_compared}

O {{site.data.keyword.openwhisk}} entrega o OpenWhisk em um ambiente altamente escalável, sem servidor. É possível comparar a arquitetura sem servidor e o cálculo com custo reduzido do {{site.data.keyword.openwhisk_short}} com outros modelos de arquitetura.
{: shortdesc}

## Comparação de arquiteturas do OpenWhisk
{: #architecture_comparison}

As arquiteturas do OpenWhisk a seguir são comparadas:

1. **Function as a Service (FaaS)** no [{{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk). A IBM é o único fornecedor a oferecer o OpenWhisk gerenciado. Uma boa introdução ao modelo de programação sem servidor usando uma plataforma FaaS está disponível no [blog de Martin Fowler](https://martinfowler.com/articles/serverless.html) e é possível ver [casos de uso](./openwhisk_use_cases.html) para executar o OpenWhisk com um design sem servidor.

2. **Infraestrutura como Serviço (IaaS)** com o OpenWhisk Roll Your Own (RYO). É possível fazer download do OpenWhisk no Apache Incubation Project e executá-lo no [{{site.data.keyword.Bluemix_notm}} IaaS](https://console.ng.bluemix.net/catalog/?category=devices).

3. **Plataforma como Serviço (PaaS)** como um tempo de execução do aplicativo gerenciado. Um bom exemplo é o tempo de execução do [Liberty for Java](https://console.ng.bluemix.net/catalog/starters/liberty-for-java) que é gerenciado pela implementação do {{site.data.keyword.Bluemix_notm}} Foundry.

4. **Container as a Service (CaaS)** como um ambiente de contêiner gerenciado. Um bom exemplo é o [{{site.data.keyword.containerlong_notm}}](/docs/containers/container_index.html#container_index).

5. **Infraestrutura como Serviço (IaaS)** com o tempo de execução Java EE. Um bom exemplo é a [VM do WebSphere Application Server no {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/catalog/services/websphere-application-server).

A tabela a seguir compara elementos de cada arquitetura da perspectiva de um desenvolvedor que está criando e operando aplicativos:


| Tópico | (1) FaaS no {{site.data.keyword.openwhisk_short}} | (2) IaaS com OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS com Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unidade do aplicativo	|	Função única (geralmente bloco de código pequeno no contêiner do JavaScript, Swift ou Docker) - pode ter menos de um Kb, mas pode ser maior. Normalmente não mais que alguns Kbs.	|	Igual à coluna (1)	|	Depende do tempo de execução usado. Um arquivo EAR ou WAR, ou outro pacote configurável do aplicativo específico da linguagem, em geral relativamente grande - Kb ou até mesmo Mb com muitos serviços em um pacote configurável, mas pode ser tão pequeno quanto um único serviço.	|	Contêiner do Docker é a
unidade de implementação.	|	MV com o App Server com o arquivo EAR ou WAR e outras
dependências - geralmente dimensionada em Gb.	|
|	Área de cobertura do recurso	|	O usuário final não paga nem se preocupa com memória, CPU ou outros recursos. Embora a ação tenha alguma área de cobertura, o usuário não precisa se preocupar com isso	|	Alta. O usuário
final deve primeiro fornecer o ambiente IaaS e, somente então, instalar e configurar o OpenWhisk sobre ele.	|	Pequeno. O usuário final paga pela memória e CPU para executar apps, mas não paga por apps que não estiverem executando	|	Pequeno para Médio	|	Alta. O usuário final precisa pagar pelo armazenamento em disco, memória, CPUs e possivelmente outros componentes quando o app está em execução. Quando ele é interrompido, somente os custos de armazenamento incorrem	|
|	Instalação e Configuração	|	Nenhum necessário	|	Difícil - tudo é feito pelo usuário final	|	Nenhum necessário	|	Moderado - hardware, rede, OS, ferramentas de gerenciamento de contêiner mgmt fornecidas pelo fornecedor CaaSs, imagens, conectividade e instâncias por usuário final	|	Difícil - hardware, rede, OS, instalação inicial do Java EE fornecida pelo fornecedor, configuração adicional, armazenamento em cluster e ajuste de escala por usuário final	|
|	Horário de fornecimento	|	Milissegundos	|	Consulte as colunas (4) e (5)	|	Minutos	|	Minutos	|	Horas	|
|	Administração contínua	|	Nenhuma	|	Difícil	|	Nenhuma	|	Moderado	|	Difícil	|
|	Ajuste de escala elástico	|	Cada ação é sempre instantaneamente e inerentemente escalada, dependendo da carga. Não é necessário provisionar VMs ou outros recursos antecipadamente	|	Não fornecido - o usuário final deve fornecer capacidade de cálculo no IaaS e gerenciar o ajuste de escala de MVs. Depois que as VMs forem escaladas, o OpenWhisk escalará as ações automaticamente, mas os recursos já deverão ter sido provisionados antecipadamente	|	Automático, mas com ajuste de escala lento. Durante o aumento de carga, os usuários podem aguardar a conclusão da ação de escala por vários minutos. O ajuste de escala
automático requer cuidado	|	Automático, mas com ajuste de escala lento. Durante o aumento de carga, os usuários podem aguardar a conclusão da ação de escala por vários minutos. O ajuste de escala
automático requer cuidado	|	Não fornecido	|
|	Planejamento da capacidade	|	Não necessário. O FaaS fornece automaticamente tanta capacidade quanto for necessária	|	É necessário fornecer capacidade suficiente antecipadamente ou planejá-la	|	Um planejamento de
capacidade é necessário, mas algum aumento de capacidade automático é fornecido	|	Um planejamento de
capacidade é necessário, mas algum aumento de capacidade automático é fornecido	|	É necessário provisionar estaticamente capacidade suficiente para manipular a carga de trabalho de pico	|
|	Conexões e estado persistentes	|	Limitado - não é possível manter uma conexão persistente, exceto em casos de armazenamento em cache de contêiner. Geralmente, o estado deve ser mantido no recurso externo	|	Igual à coluna (1)	|	Suportado - pode manter um soquete ou uma conexão aberta durante longos períodos e pode armazenar
o estado na memória entre as chamadas	|	Suportado - pode manter um soquete ou uma conexão aberta durante longos períodos e pode armazenar
o estado na memória entre as chamadas	|	Suportado - pode manter um soquete ou uma conexão aberta durante longos períodos e pode armazenar
o estado na memória entre as chamadas	|
|	Manutenção	|	Nenhuma - a pilha inteira é gerenciada pela IBM.	|	Significativa - depende do ambiente de destino. O usuário deve fornecer hardware, rede, OS, armazenamento, DB, instalar e manter o OpenWhisk e assim por diante.	|	Nenhuma - toda a pilha é gerenciada pelo fornecedor.	|	Significativa - o usuário deve criar e manter imagens customizadas, implementar e gerenciar contêineres, conexões entre contêineres e assim por diante.	|	Significativa - o usuário deve alocar VMs, gerenciar e escalar servidores Java EE individualmente.	|
|	Alta Disponibilidade (HA) e Recuperação de Desastre (DR)	|	Inerente/sem custos extras	|	Roll Your Own (RYO) 	|	Disponível com custo extra	|	Contêineres com falha podem ser reiniciados automaticamente	|	Disponível com custo
extra, semiautomático. MVs podem efetuar failover automaticamente	|
|	Segurança	|	Fornecedor fornecido	|	Roll Your Own (RYO)	|	Combinação de RYO e enviado pelo fornecedor	|	Combinação de RYO e enviado pelo fornecedor	|	Roll Your Own (RYO)	|
|	Velocidade do desenvolvedor	|	Mais alta	|	Mais alta	|	Mais alta	|	Média	|	Lenta	|
|	Utilização de recurso (recursos inativos que ainda precisam ser pagos)	|	Os recursos nunca estão inativos porque eles são chamados somente mediante solicitação. Quando a carga de trabalho está ausente, nenhum custo ou alocação de recurso ocorre.	|	Por que essa opção está usando IaaS ou CaaS - considerações semelhantes se aplicam como
nas colunas (4) e (5)	|	Alguns recursos podem estar inativos e o ajuste automático de escala ajuda a eliminar recursos inativos. Um número de instâncias em execução deve estar sempre presente e deve ser usado com menos de 50% de sua capacidade. Instâncias interrompidas não incorrem em nenhum custo	|	Semelhante à coluna (3)	|	Alguns recursos podem estar inativos, mas o ajuste automático de escala não é suportado. Um número de instâncias em execução deve estar sempre presente e deve ser usada com menos de 50% de sua capacidade. As instâncias interrompidas podem incorrer em custos de armazenamento	|
|	Maturidade	|	Maturidade antecipada	|	Maturidade antecipada	|	Maturidade antecipada	|	Maturidade moderada	|	Altamente maduro	|
|	Limites de recurso	|	[Existem alguns limites](./openwhisk_reference.html#openwhisk_syslimits)	|	Depende de recursos alocados	|	Não	|	Não	|	Não	|
|	Latência para serviços raramente utilizados	|	Solicitações raras podem ver inicialmente tempo de resposta de vários segundos, mas permanecem em intervalo de milissegundo para solicitações subsequentes	|	Depende	|	Baixa	|	Baixa	|	Baixa - supondo que o sistema tenha recursos suficientes	|
|	Tipo de ponto sensacional de aplicativo	|	Processamento de eventos, Internet das Coisas (IoT), backend móvel,
microsserviços. Definitivamente inadequado para aplicativos monolíticos. Veja [casos de uso](./openwhisk_use_cases.html)	|	Igual à coluna (1), mas quando o usuário deseja
executar em nuvem não IBM ou executar no local.	|	Aplicativos da web com carga de trabalho 24x7 e serviços
stateful que precisam manter a conexão aberta por longos períodos de tempo. Pode ser usado para executar
microsserviços ou aplicativos monolíticos	|	Ideal para aplicativos de microsserviços.	|	Aplicativos corporativos tradicionais que são migrados do local para a nuvem. Ideal para aplicativos monolíticos	|
|	Carregando granularidade e faturamento	|	[Por blocos de 100 milissegundos](https://console.ng.bluemix.net/openwhisk/learn/pricing)	|	Depende da implementação - se IaaS ou CaaS for usado, considerações semelhantes se aplicarão - veja as colunas (4) e (5)	|	Normalmente cobrado por hora (raramente por minuto) para pacote configurável
de recursos (CPU + memória + algum espaço em disco)	|	Semelhante à coluna (3)	|	Semelhante à coluna (3)	|
|	Custo Total de Propriedade (TCO)	|	Para seu ponto sensacional, os aplicativos devem custar uma ordem de magnitude menor que as alternativas. Como os recursos são escalados automaticamente, o fornecimento excessivo não ocorre.	|	Para implementações na nuvem, é provável que seja mais caro que o FaaS do OpenWhisk, mas a implementação no local pode ser mais barata que as arquiteturas tradicionais	|	Relativamente baixo - o usuário não precisa provisionar ou gerenciar recursos e pode focar o desenvolvimento de aplicativo. Algum nível de fornecimento excessivo comparado ao sem servidor	|	Moderado - o usuário precisa provisionar e gerenciar contêineres e aplicativo e pode ver algum nível de fornecimento excessivo comparado ao sem servidor ou PaaS	|	Relativamente alto - considere que a migração de aplicativos anteriores para o modelo nativo em nuvem pode ser muito cara, essa pode ser uma opção viável e econômica para esses apps.	|

## Considerações de custo
{: #cost_considerations}

A infraestrutura para seus ambientes de teste, preparação, teste de carregamento e outros pode ser dispendiosa. Leva tempo para configurá-los e, como eles costumam operar 24x7, eles são muitas vezes subutilizados e consomem grandes quantias de capacidade. Usando uma arquitetura sem servidor, os custos para qualquer número de ambientes são gerados com base no carregamento em vez do número de ambientes definidos.
{: shortdesc}

Para estimar custos para um aplicativo sem servidor, é possível usar a [calculadora de precificação ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/learn/pricing).

### Capacidade ilimitada
{: #limitless_capacity}

Em arquiteturas tradicionais, cada serviço consome a quantia de capacidade alocada para elas e você é faturado pelo consumo de capacidade. A arquitetura sem servidor do {{site.data.keyword.openwhisk_short}} reduz a restrição na granularidade de sua arquitetura de microsserviços.

Quando não em uso, o {{site.data.keyword.openwhisk_short}} não custa nada. Seu código é executado quando há uma chamada HTTP, mudança de estado do banco de dados ou outro tipo de evento que aciona a execução do código. Você é faturado por milissegundo de tempo de execução arredondado para os 100 ms mais próximos, não por hora de utilização de VM, independentemente de se essa VM estava executando trabalho útil. Como você paga somente quando os eventos são consumidos e não com base no número de ambientes, é possível dividir seu app em 100, 1.000 ou até mais microsserviços.

### Executar ações em qualquer região
{: #actions_region}

Em arquiteturas tradicionais, o código deve estar em execução em cada região a ser executado lá e a infraestrutura para essa região também deve ser paga. Com o {{site.data.keyword.openwhisk_short}}, ações podem ser implementadas e disponibilizadas para serem executadas em qualquer região sem nenhum custo extra. É possível aumentar a disponibilidade e a resiliência de seu código sem as tradicionais restrições de custo.

### Redundância por design
{: #redundancy_design}

Em arquiteturas tradicionais, os apps devem ser redundantes. Com o {{site.data.keyword.openwhisk_short}}, os processos não precisam ser altamente disponíveis (HA) porque os apps sem servidor são stateless e acionados por evento de solicitação por design. Eliminando a necessidade de criar explicitamente a redundância, a natureza stateless de apps sem servidor pode reduzir significativamente os custos de infraestrutura.
