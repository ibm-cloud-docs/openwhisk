---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Função como Serviço comparado
{: #openwhisk_faas_compared}

Uma arquitetura sem servidor não é uma panaceia para todos os problemas de computação, mas resolve
alguns. Há [muitos casos de uso](./openwhisk_use_cases.html) em que o design sem servidor pode ser uma boa opção. As arquiteturas a seguir são comparadas:
{: shortdesc}

1. **Função como Serviço (FaaS)** - OpenWhisk gerenciado. A IBM é o único fornecedor a oferecer o [OpenWhisk gerenciado no {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/openwhisk).

2. **Infraestrutura como Serviço (IaaS)** com o OpenWhisk Roll Your Own (RYO). Os usuários finais podem fazer download do OpenWhisk no Apache Incubation Project e instalar e executá-lo no [{{site.data.keyword.Bluemix_notm}} IaaS](https://console.ng.bluemix.net/catalog/?category=devices) ou outra [nuvem do IaaS](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29).

3. **Plataforma como Serviço (PaaS)** - tempo de execução de aplicativo gerenciado. Um bom exemplo é o tempo de execução do [Liberty for Java](https://console.ng.bluemix.net/catalog/starters/liberty-for-java) gerenciado pela implementação do {{site.data.keyword.Bluemix_notm}} CloudFoundry.

4. **Contêiner como Serviço (CaaS)** - ambiente de contêiner gerenciado. Um bom exemplo são os [Contêineres da IBM no {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/catalog/?category=containerImages).

5. **Infraestrutura como Serviço (IaaS)** com o tempo de execução Java EE. Um bom exemplo é a [VM do WebSphere Application Server no {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/catalog/services/websphere-application-server) da IBM.

Um resumo dos prós e contras para cada arquitetura é fornecido na **perspectiva de um usuário final** que está desenvolvendo e operando aplicativos nestes tempos de execução diferentes:


| Tópico | (1) FaaS do OpenWhisk | (2) RYI do OpenWhisk | (3) PaaS | (4) CaaS | (5) IaaS+Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unidade do aplicativo	|	Função única (geralmente bloco de código pequeno no contêiner do JavaScript, Swift ou Docker) - pode ter menos de um Kb, mas pode ser maior. Normalmente não mais que alguns Kbs.	|	Igual à coluna (1)	|	Depende do tempo de execução usado. Um arquivo EAR ou WAR, ou outro pacote configurável do aplicativo específico da linguagem, em geral relativamente grande - Kb ou até mesmo Mb com muitos serviços em um pacote configurável, mas pode ser tão pequeno quanto um único serviço.	|	Contêiner do Docker é a
unidade de implementação.	|	MV com o App Server com o arquivo EAR ou WAR e outras
dependências - geralmente dimensionada em Gb.	|
|	Área de cobertura do recurso	|	O usuário final não paga nem se preocupa com memória, CPU ou outros recursos. Embora a ação tenha alguma área de cobertura, o usuário não precisa se preocupar com isso	|	Alta. O usuário
final deve primeiro fornecer o ambiente IaaS e, somente então, instalar e configurar o OpenWhisk sobre ele.	|	Pequeno. O usuário final paga pela memória e CPU para executar apps, mas não paga por apps que não estiverem executando	|	Pequeno para Médio	|	Alta. O usuário final precisa pagar pelo armazenamento em disco, memória, CPUs e possivelmente outros componentes quando o app está em execução. Quando ele é interrompido, somente os custos de armazenamento incorrem	|
|	Instalação e Configuração	|	Nenhum necessário	|	Difícil - tudo é feito pelo usuário final	|	Nenhum necessário	|	Moderado - hardware, rede, OS, ferramentas de gerenciamento de contêiner mgmt fornecidas pelo fornecedor CaaSs, imagens, conectividade e instâncias por usuário final	|	Difícil - hardware, rede, OS, instalação inicial do Java EE fornecida pelo fornecedor, configuração adicional, armazenamento em cluster e ajuste de escala por usuário final	|
|	Horário de fornecimento	|	Milissegundos	|	Consulte as colunas (4) e (5)	|	Minutos	|	Minutos	|	Horas	|
|	Administração contínua	|	Nenhuma	|	Difícil	|	Nenhuma |	Moderado	|	Difícil	|
|	Ajuste de escala elástico	|	Cada ação é sempre escalada de forma instantânea e inerente, dependendo da carga. Não é necessário provisionar VMs ou outros recursos antecipadamente	|	Não fornecido - o usuário final deve fornecer capacidade de cálculo no IaaS e gerenciar o ajuste de escala de MVs. Assim que as VMs são escaladas, o OpenWhisk escala as ações automaticamente, mas os recursos já devem ter sido provisionados antecipadamente	|	Automático, mas com ajuste de escala lento. Durante o carregamento aumentado, por vários minutos, os usuários podem aguardar a conclusão da Ação de escalar. O ajuste de escala
automático requer cuidado	|	Automático, mas com ajuste de escala lento. Durante o carregamento aumentado, por vários minutos, os usuários podem aguardar a conclusão da Ação de escalar. O ajuste de escala
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
