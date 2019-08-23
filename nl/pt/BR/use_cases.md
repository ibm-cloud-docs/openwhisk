---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# Casos de Uso Comuns
{: #use_cases}

O modelo de execução que é oferecido por {{site.data.keyword.openwhisk_short}} suporta vários casos de uso. As seções a seguir incluem exemplos típicos. Para uma discussão mais detalhada da arquitetura Serverless, casos de uso de exemplo, discussão de prós e contras e melhores práticas de implementação, leia o excelente [artigo de Mike Roberts no blog de Martin Fowler](https://martinfowler.com/articles/serverless.html){: external}.
{: shortdesc}

## Microsserviços
{: #use_cases_microservices}

Apesar de seus benefícios, as soluções baseadas em microsserviço continuam difíceis de construir usando tecnologias de nuvem tradicionais, geralmente requerendo o controle de uma cadeia de ferramentas complexa e pipelines de construção e operações separados. As equipes pequenas e ágeis gastam muito tempo com complexidades infraestruturais e operacionais, como tolerância a falhas, balanceamento de carga, ajuste automático de escala e criação de log. Essas equipes desejam uma forma de desenvolver código aperfeiçoado e de valor agregado com linguagens de programação que eles já conhecem, gostam e que são mais adequadas para resolver problemas específicos.

A natureza modular e inerentemente escalável do {{site.data.keyword.openwhisk_short}} o torna ideal para implementar partes granulares de lógica em ações. As ações do {{site.data.keyword.openwhisk_short}} são independentes umas das outras e podem ser implementadas usando várias linguagens diferentes que são suportadas pelo {{site.data.keyword.openwhisk_short}} e acessar vários sistemas backend. Cada ação pode ser implementada e gerenciada independentemente, além de ser escalada independentemente de outras ações. A interconectividade entre as ações é fornecida pelo {{site.data.keyword.openwhisk_short}} na forma de regras, sequências e convenções de nomenclatura. Esse tipo de ambiente é bom para aplicativos baseados em microsserviços.

Outro argumento importante em favor do {{site.data.keyword.openwhisk_short}} é o custo de um sistema em uma configuração de recuperação de desastre. Compare microsserviços com PaaS ou CaaS vs. o uso do {{site.data.keyword.openwhisk_short}} supondo que você tenha 10 microsserviços, que usam contêineres ou tempos de execução do Cloud Foundry. Essa comparação equivale a 10 processos faturáveis e em execução continuamente em uma zona de disponibilidade única, 20 quando executados em duas zonas de disponibilidade e 40 quando executados em duas regiões com duas zonas cada. Para atingir o mesmo objetivo com o {{site.data.keyword.openwhisk_short}}, é possível executá-los em quantas zonas ou regiões de disponibilidade desejar, sem ter que pagar nada de custos incrementais.

## Apps da web
{: #use_cases_webapps}

Dada a natureza orientada a eventos do {{site.data.keyword.openwhisk_short}}, ele oferece vários benefícios para aplicativos voltados para o usuário, enquanto as solicitações de HTTP provenientes do navegador do usuário servem como os eventos. Os aplicativos {{site.data.keyword.openwhisk_short}} usam a capacidade de cálculo e são faturados apenas quando estão entregando solicitações do usuário. O modo de espera inativa ou de espera é não existente. Esse recurso torna o {{site.data.keyword.openwhisk_short}} consideravelmente menos caro quando comparado com os contêineres tradicionais ou aplicativos do Cloud Foundry. Ambas as ferramentas têm tempo inativo, aguardando solicitações do usuário de entrada e você é faturado por todo o tempo "suspenso".

É possível construir e executar um aplicativo da web completo com o {{site.data.keyword.openwhisk_short}}. Combinar APIs serverless com hosting de arquivo estático para recursos de site como HTML, JavaScript e CSS significa que é possível construir aplicativos da web completamente serverless. A simplicidade de operar um ambiente hospedado do {{site.data.keyword.openwhisk_short}}, não ter que operar nada. Como o {{site.data.keyword.openwhisk_short}} está hospedado no {{site.data.keyword.cloud_notm}}, é um grande benefício quando comparado a levantar e operar um Node.js Express ou outro tempo de execução do servidor tradicional.

## IoT
{: #use_cases_iot}

Os cenários de Internet of Things são frequentemente orientados por sensores de forma inerente. Por exemplo, uma ação no {{site.data.keyword.openwhisk_short}} poderá ser acionada se houver necessidade de reagir a um sensor que excede uma determinada temperatura. As interações IoT são geralmente stateless com o potencial de alta carga em grandes eventos espontâneos, como desastres naturais, tempestades climáticas significativas ou engarrafamentos. Uma necessidade é criada para um sistema elástico em que a carga de trabalho normal pode ser pequena, mas precisa escalar rapidamente com tempo de resposta previsível. Portanto, a capacidade de manipular vários eventos simultâneos sem aviso prévio no sistema é desejável. É difícil construir um sistema para atender esses requisitos que usam arquiteturas de servidor tradicional. Isso acontece porque eles tendem a ser subdesenvolvidos e incapazes de manipular a carga de pico no tráfego ou ser excessivamente provisionados e altamente dispendiosos.

É possível implementar aplicativos IoT que usam arquiteturas de servidor tradicional. No entanto, em muitos casos, a combinação de diferentes serviços e pontes de dados requer pipelines flexíveis e de alto desempenho. Isso abrange de dispositivos IoT a armazenamento em nuvem e uma plataforma de análise. Geralmente, as pontes pré-configuradas não têm a programabilidade de implementar e de ajustar de forma precisa uma arquitetura de solução específica. Dada a variedade de pipelines e a falta de padronização em torno da fusão de dados em geral (IoT em particular), é comum ver ambientes em que o pipeline requer transformação de dados customizados. Essas transformações de dados customizados se aplicam à conversão de formato, filtragem ou aumento. O {{site.data.keyword.openwhisk_short}} é uma ferramenta excelente para implementar tal transformação, de uma maneira ‘serverless’, em que a lógica customizada é hospedada em uma plataforma de nuvem totalmente gerenciada e elástica.

Veja o aplicativo Internet of Things de amostra a seguir que usa o {{site.data.keyword.openwhisk_short}}, Node-RED, Cognitive e outros serviços: [Transformação sem servidor dos dados em movimento da Internet of Things com o {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}.

![Exemplo de arquitetura da solução IoT](images/IoT_solution_architecture_example.png)

## Backend da
API
{: #use_cases_backend}

As plataformas computacionais Serverless fornecem aos desenvolvedores uma maneira rápida de construir APIs sem servidores. O {{site.data.keyword.openwhisk_short}} suporta a geração automática de API de REST para ações. O [recurso do {{site.data.keyword.openwhisk_short}} ](/docs/openwhisk?topic=cloud-functions-apigateway) pode chamar uma ação com métodos de HTTP diferentes de POST e sem a chave de API de autorização da ação por meio do Gateway de API do {{site.data.keyword.openwhisk_short}}. Esse recurso é útil não apenas para expor APIs a consumidores externos, mas também para construir aplicativos de microsserviços.

Além disso, as ações do {{site.data.keyword.openwhisk_short}} podem ser conectadas a uma ferramenta API Management de sua preferência (como [IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} ou outra). Semelhante a outros casos de uso, todas as considerações para escalabilidade e outras Qualidades de serviços se aplicam.

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} é um aplicativo de amostra que usa ações do {{site.data.keyword.openwhisk_short}} por meio de uma API de REST.

Veja o exemplo a seguir que inclui uma discussão sobre [uso de Serverless como um backend de API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}.

## Backend móvel
{: #use_cases_mobile}

Muitos aplicativos móveis requerem lógica do lado do servidor. No entanto, os desenvolvedores móveis geralmente não têm experiência no gerenciamento de lógica do lado do servidor e prefeririam focar o app que está em execução no dispositivo. Esse objetivo de desenvolvimento é facilmente obtido usando o {{site.data.keyword.openwhisk_short}} como o backend do lado do servidor e é uma boa solução. Além disso, o suporte integrado para o Swift do lado do servidor permite que os desenvolvedores reutilizem suas qualificações existentes de programação do iOS. Como os aplicativos móveis geralmente têm padrões de carregamento imprevisíveis, você deseja usar uma solução {{site.data.keyword.openwhisk_short}} como {{site.data.keyword.cloud}}. Essa solução pode escalar para atender praticamente a qualquer demanda de carga de trabalho sem a necessidade de provisionar recursos antecipadamente.

## Processamento de Dados
{: #use_cases_data}

Com a quantia de dados agora disponível, o desenvolvimento de aplicativo requer a capacidade de processar novos dados e, potencialmente reagir a eles. Esse requisito inclui o processamento tanto de registros do banco de dados estruturado quanto de documentos, imagens ou vídeos não estruturados. O {{site.data.keyword.openwhisk_short}} pode ser configurado por feeds customizados ou fornecidos pelo sistema para reagir a mudanças nos dados e executar ações automaticamente nos feeds de dados recebidos. As ações podem ser programadas para processar mudanças, transformar formatos de dados, enviar e receber mensagens, chamar outras ações e atualizar vários armazenamentos de dados. Os armazenamentos de dados suportados incluem bancos de dados relacionais baseados em SQL, grades de dados na memória, banco de dados NoSQL, arquivos, brokers de sistema de mensagens e vários outros sistemas. As regras e sequências do {{site.data.keyword.openwhisk_short}} fornecem flexibilidade para fazer mudanças no pipeline de processamento sem programação e são executadas por meio de atualizações de configuração simples. As opções de armazenamento de dados e a baixa manutenção administrativa tornam um sistema baseado em {{site.data.keyword.openwhisk_short}} altamente ágil e facilmente adaptável aos requisitos que mudam.

## Computação Cognitiva
{: #use_cases_cognitive}

As tecnologias cognitivas podem ser efetivamente combinadas com o {{site.data.keyword.openwhisk_short}} para criar aplicativos poderosos. Por exemplo, o IBM Alchemy API e o Watson Visual Recognition podem ser usados com o {{site.data.keyword.openwhisk_short}} para extrair automaticamente informações úteis dos vídeos sem precisar assisti-los. Essa tecnologia é a extensão "cognitiva" do caso de uso de [Processamento de dados](#use_cases_data) que foi discutido anteriormente. Outro bom uso para o {{site.data.keyword.openwhisk_short}} é implementar a função Bot combinada com serviços cognitivos.

Um aplicativo de amostra, [Dark vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external}, é fornecido e executa apenas isso. Nesse aplicativo, o usuário faz upload de um vídeo ou imagem usando o aplicativo da web Dark Vision, que o armazena em um BD {{site.data.keyword.cloudant_short_notm}}. Quando o vídeo é transferido por upload, o {{site.data.keyword.openwhisk_short}} detecta o novo vídeo recebendo as mudanças do {{site.data.keyword.cloudant_short_notm}} (acionador). Em seguida, o {{site.data.keyword.openwhisk_short}} aciona a ação do extrator de vídeo. Durante sua execução, o extrator produz quadros (imagens) e os armazena no {{site.data.keyword.cloudant_short_notm}}. Os quadros são então processados com o Watson Visual Recognition e os resultados são armazenados no mesmo DB {{site.data.keyword.cloudant_short_notm}}. Os resultados podem ser visualizados usando o aplicativo da web Dark Vision ou um aplicativo iOS. O {{site.data.keyword.cos_full_notm}} pode ser usado além do {{site.data.keyword.cloudant_short_notm}}, em que os metadados de vídeo e imagem são armazenados no {{site.data.keyword.cloudant_short_notm}} e os arquivos de mídia são armazenados no {{site.data.keyword.cos_full_notm}}.

## Processamento de eventos com o Kafka ou {{site.data.keyword.messagehub}}
{: #use_cases_events}

O {{site.data.keyword.openwhisk_short}} é ideal para ser usado em combinação com o Kafka, {{site.data.keyword.messagehub_full}} (baseado no Kafka) e outros sistemas de mensagens. A natureza acionada por eventos desses sistemas requer um tempo de execução acionado por eventos para processar mensagens. O tempo de execução pode aplicar a lógica de negócios a essas mensagens, que é exatamente o que {{site.data.keyword.openwhisk_short}} fornece, com seus feeds, acionadores e ações. O Kafka e o {{site.data.keyword.messagehub}} são frequentemente usados para volumes de carga de trabalho altos e imprevisíveis e requerem que os consumidores dessas mensagens precisem ser escaláveis em um aviso do momento. Essa situação é, mais uma vez, um ponto sensacional para o {{site.data.keyword.openwhisk_short}}. O {{site.data.keyword.openwhisk_short}} tem a capacidade integrada para consumir mensagens, bem como publicar mensagens que são fornecidas no pacote [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams).



