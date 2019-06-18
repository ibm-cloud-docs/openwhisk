---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: planning

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Planejando apps sem servidor
{: #plan}

Antes de iniciar a criação de sua função, aprenda sobre as decisões que devem ser tomadas ao longo do caminho.
{: shortdesc}

## Revise o suporte de tempo de execução
{: #plan_runtime}

O {{site.data.keyword.openwhisk_short}} fornece [tempos de execução](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) padrão para executar o código. Mas, como o {{site.data.keyword.openwhisk_short}} é sem servidor, você não está limitado nos tempos de execução que podem ser usados. É possível criar tempos de execução customizados criando sua própria [imagem customizada do Docker](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) para empacotar seu código.
{: shortdesc}



## Arquitetar seu código
{: #plan_architect}

Pode ser necessário fazer alguns ajustes para o seu código existente para que ele seja executado no {{site.data.keyword.openwhisk}}. Se você ainda não tiver escrito seu código, mantenha estes aspectos em mente enquanto você grava.
{: shortdesc}

1. Componentizar seu código.

    Se você já tiver um app que deseje tornar sem servidor, considere como você pode precisar dividir seu app em partes menores. Cada função contém um conjunto de acionadores para o código que você deseja executar. Por exemplo, se um problema for criado no Github, execute esse código JavaScript. Se o seu app contiver vários desses eventos ou ações, considere separá-los em funções diferentes.

2. Use os serviços {{site.data.keyword.Bluemix_notm}} em vez de estruturas.

    Em vez de usar estruturas para tornar os recursos disponíveis em sua função no tempo de execução, é possível usar serviços {{site.data.keyword.Bluemix_short}}. Muitas tarefas comuns que as estruturas ajudam a concluir estão disponíveis como serviços no {{site.data.keyword.Bluemix_short}}.
    {: shortdesc}

    Por exemplo, em vez de usar uma estrutura para autenticação, tente {{site.data.keyword.appid_full}}. Se você precisar de armazenamento de arquivo externo, tente {{site.data.keyword.cos_full}}.

    Se o recurso que você deseja incorporar não estiver disponível como um serviço no {{site.data.keyword.Bluemix_short}}, será possível sempre integrar esse recurso a alimentações e também a APIs, sem a necessidade de estruturas.

3. [Verifique se seu código atende aos limites do sistema.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    Seu código deve se ajustar dentro dos limites do sistema para execução. Por exemplo, se o seu código empacotado for maior que 48 MB, você poderá precisar continuar dividindo-o em partes menores ou empacotá-lo como uma imagem do Docker.

    Os aplicativos que contêm muitos módulos de terceiros, bibliotecas nativas ou ferramentas externas podem atingir esse limite. Se você criar uma ação de pacote .zip ou .jar que seja maior que 48 MB, deverá estender a imagem de tempo de execução com dependências e, em seguida, usar um único arquivo de origem ou archive menor do que 48 MB. Por exemplo, ao construir um tempo de execução do Docker customizado que inclui bibliotecas compartilhadas necessárias, as dependências não precisam estar presentes no archive. Os arquivos de origem privados podem ser empacotados no archive e injetados no tempo de execução.

4. Determine os parâmetros que devem ser injetados em seu código.

    Em ações serverless, os dados são fornecidos incluindo parâmetros nas ações. Os parâmetros são declarados como um argumento para a função principal sem servidor. Mais comumente, esses parâmetros são credenciais para um serviço, mas eles podem ser qualquer coisa que torne seu código reutilizável com diferentes acionadores.

5. [Verifique os requisitos estruturais para seu código a ser usado em uma função.](/docs/openwhisk?topic=cloud-functions-prep)

    Se você já tem um app ou planeja desenvolver um script a ser usado, seu código provavelmente precisa de alguns ajustes para torná-lo consumível pelo {{site.data.keyword.openwhisk}}. O próprio código deve atender a alguns requisitos estruturais, como os parâmetros de entrada e os resultados de saída e pode precisar ser empacotado em um único arquivo que inclua todas as suas dependências.







## Determine sua origem de eventos
{: #plan_source}

Comece a pensar sobre a sua função em termos do evento que você deseja que ocorra para acionar o código para execução. Talvez você queira que o código seja executado em cada compromisso com um repositório do Github. Ou talvez você queira que seu código seja executado toda vez que uma atualização for feita em um banco de dados Cloudant.
{: shortdesc}

Depois de escolher um evento para acionar sua função, revise os pacotes que já estão disponíveis para uso. Você pode ser capaz de usar um para simplificar seu desenvolvimento de função. Caso contrário, será possível criar seu próprio pacote para a sua origem de eventos.

Talvez seja necessário voltar para o seu código e fazer algumas revisões com base em suas seleções de eventos.


## Organizar implementações entre ambientes
{: #plan_environments}

Decida como você deseja implementar suas funções entre ambientes, como ambientes de desenvolvimento, preparação e produção.
{: shortdesc}

Como o {{site.data.keyword.openwhisk_short}} é um serviço baseado no Cloud Foundry, é possível gerenciar a implementação de suas funções nas organizações e espaços do Cloud Foundry que são fornecidos para você no {{site.data.keyword.Bluemix_notm}}. Para organizar suas funções entre esses ambientes, você pode optar por criar uma organização por função. Em seguida, crie um espaço para cada ambiente de que você precisar. Em vez disso, você também poderia ter uma organização para cada ambiente e criar um espaço por função. Não importa como você organiza suas organizações e espaços, escolha uma estrutura que permita que você gerencie efetivamente suas entidades de função.

Também é possível usar [namespaces](/docs/openwhisk?topic=cloud-functions-namespaces) para isolar recursos. Cada espaço do {{site.data.keyword.Bluemix_notm}} contém um namespace Open Whisk por padrão. É possível agrupar entidades, tais como ações ou acionadores em um namespace e, em seguida, criar políticas de Identidade e Acesso (IAM) para gerenciar permissões de usuário para esse grupo.



