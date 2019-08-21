---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions, functions

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


# Detalhes e limites do sistema
{: #limits}

As seções a seguir fornecem detalhes técnicos sobre o sistema {{site.data.keyword.openwhisk}} e as configurações de limite.
{: shortdesc}

## Limites do sistema
{: #limits_syslimits}

### Ações
{: #limits_actions}

O {{site.data.keyword.openwhisk_short}} possui alguns limites do sistema, incluindo o quanto de memória uma ação pode usar e quantas chamadas de ação são permitidas por minuto.

A tabela a seguir lista os
limites padrão para ações.

| Limit | Descrição | Padrão | Mín. | Máxima |
| --- | ---| --- | --- | --- |
| `codeSize` | O tamanho máximo do código para a ação é 48 MB. Para ações JavaScript, use uma ferramenta para concatenar todo o código-fonte, que inclui dependências, em um único arquivo empacotado. Esse limite é fixo e não pode ser mudado. | 48 | 1 | 48 | 
| `concurrent`| O número de ativações que estão em execução ou enfileiradas para execução para um namespace não pode exceder 1.000. Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Consulte [Aumentando limites fixos](/docs/openwhisk?topic=cloud-functions-limits#limits_increase) para obter instruções sobre como aumentar esse limite. | 1000 | 1 | 1000* |
| `logs`| O limite de log N está no intervalo [0 MB..10 MB] e é configurado por ação. Um usuário pode mudar o limite do log de ações quando uma ação é criada ou atualizada. Os logs que excedem o limite configurado são truncados, assim quaisquer novas entradas de log são ignoradas e um aviso é incluído como a última saída da ativação para indicar que a ativação excedeu o limite de log configurado. | 10 | 0 | 10 |
| `memória` | O limite de memória M está no intervalo de [128 MB...2.048 MB] e é configurado por ação em MB. Um usuário pode mudar o limite de memória quando uma ação é criada. Um contêiner não pode usar mais memória do que está alocada pelo limite. | 256 | 128 | 2048 |
| `minuteRate` | Não mais que N ativações podem ser enviadas por namespace por minuto. O limite de taxa N é configurado para 5000 e limita o número de chamadas de ação em janelas de 1 minuto. Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`. Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Consulte [Aumentando limites fixos](#limits_increase) para obter instruções sobre como aumentar esse limite. | 5000 | 1 | 5000* | 
| `openulimit` | O número máximo de arquivos abertos para uma ação é 1024 (para os limites máximo e flexível). Esse limite é fixo e não pode ser mudado. Quando uma ação é chamada, o comando de execução do docker usa o argumento `--ulimit nofile=1024:1024` para configurar o valor `openulimit`. Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external}. | 1024 | 0 | 1024 | 
| `parâmetros` | O tamanho máximo dos parâmetros que podem ser anexados em MB. O limite de tamanho para os parâmetros totais na criação ou atualização de uma Ação/Pacote/Acionador é de 5 MB. Uma entidade com parâmetros muito grandes é rejeitada em tentar criar ou atualizar isso. Esse limite é fixo e não pode ser mudado. | 5 | 0 | 5 | 
| `proculimit` | O número máximo de processos disponíveis para o contêiner de ação é 1024. Esse limite é fixo e não pode ser mudado. Quando uma ação é chamada, o comando de execução do docker usa o argumento `--pids-limit 1024` para configurar o valor `proculimit`. Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external}. | 1024 | 0 | 1024 | 
| `resultado` | O tamanho máximo de saída de um resultado da chamada de ação em MB. Esse limite é fixo e não pode ser mudado. | 5 | 0 | 5 | 
| `sequenceMaxActions` | O número máximo de ações que compõem uma sequência. Esse limite é fixo e não pode ser mudado. | 50 | 0 | 50* | 
| `tempo limite` | O limite do tempo limite N está no intervalo [100 ms..600000 ms ] e é configurado por ação em milissegundos. Um usuário pode mudar o limite do tempo limite quando uma ação é criada. Um contêiner executado mais de N milissegundos é finalizado. | 60000 | 100 | 600000 | 

### Aumentando limites fixos
{: #limits_increase}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados se um business case puder justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://cloud.ibm.com/openwhisk){: external} da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Functions** para a Área de suporte técnico.

### disparos
{: #limits_triggers}

Os acionadores estão sujeitos a uma taxa de disparo por minuto, conforme documentado na tabela a seguir.

| Limit | Descrição | Padrão | Mín. | Máxima |
| --- | --- | --- | --- | --- |
| `minuteRate` | O limite de taxa N é configurado para 5000 e limita o número de acionadores que um usuário pode disparar em janelas de 1 minuto. Um usuário não pode mudar o limite do acionador quando um acionador é criado. Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`. Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](#limits_triggersfixed) para obter instruções detalhadas sobre como aumentar esse limite. | 5000* | 5000* | 5000* |

### Aumentando limites fixos
{: #limits_triggersfixed}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados caso um caso de negócios possa justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://cloud.ibm.com/openwhisk){: external} da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Functions** para a Área de suporte técnico.


## entidades do {{site.data.keyword.openwhisk_short}}
{: #limits_entities_ov}

### Namespaces e pacotes
{: #limits_namespaces}

As ações, os acionadores e as regras do {{site.data.keyword.openwhisk_short}} pertencem a um namespace e, às vezes, a um pacote.

Os pacotes podem conter ações e feeds. Um pacote não pode conter outro pacote, portanto, o aninhamento de pacote não é permitido. Além disso, as entidades não precisam estar contidas em um pacote.

É possível criar novos namespaces baseados em IAM executando `ibmcloud fn namespace create`.  Os namespaces baseados em Cloud Foundry são feitos por meio de uma combinação de um nome de organização e espaço. Por exemplo, caso você seja destinado para a organização `user@email.com` e o espaço `dev`, você terá como destino o namespace baseado no Cloud Foundry do {{site.data.keyword.openwhisk_short}} chamado `user@email.com_dev`. 

O namespace `/whisk.system` é reservado para entidades distribuídas com o sistema {{site.data.keyword.openwhisk_short}}.

O [Serverless Framework](https://serverless.com/) não é suportado para namespaces baseados no IAM
{: note}


### Nomes Completos
{: #limits_fullnames}

O nome completo de uma entidade é `/namespaceName/[packageName]/entityName`. Observe que a `/` é usada para delimitar namespaces, pacotes e entidades. Além disso, os namespaces devem ter como prefixo uma `/`.

Por conveniência, o namespace poderá ser deixado de fora se ele for o namespace padrão do usuário. Por exemplo, considere um usuário cujo namespace padrão seja `/myOrg`. Seguem
exemplos de nomes completos de diversas entidades e seus aliases.

| Nome Completo | Alias | Namespace | Pacote | Nome |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

É possível usar esse esquema de nomenclatura ao usar a CLI do {{site.data.keyword.openwhisk_short}}, entre outros locais.

Mais precisamente, um nome deve corresponder à expressão regular a seguir (expressa com sintaxe Java metacaractere): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semântica de ação
{: #limits_semantics}

As seções a seguir descrevem detalhes sobre ações do {{site.data.keyword.openwhisk_short}}.

### Statelessness
{: #limits_stateless}

As implementações de ação são stateless ou *idempotente*. Embora o sistema não cumpra essa propriedade, não é garantido que qualquer estado mantido por uma ação estará disponível entre chamadas.

Além disso, múltiplas instanciações de uma ação podem existir, com cada instanciação com seu próprio estado. Uma chamada de ação pode ser despachada para qualquer uma dessas instanciações.

### Saída e entrada de chamada
{: #limits_invocationio}

A entrada e a saída de uma ação é um dicionário de pares de valores de chaves. A chave é uma sequência e o valor um valor JSON válido.

### Ordem de chamada de ações
{: #limits_ordering}

Chamadas de uma ação não são ordenadas. Se o usuário chamar uma ação duas vezes a
partir da linha de comandos ou a API REST, a segunda chamada poderá ser executada antes
da primeira. Se
as ações tiverem efeitos colaterais, elas poderão ser observadas em
qualquer ordem.

Além disso, não é garantido que as ações sejam executadas automaticamente. Duas ações podem ser executadas simultaneamente e seus efeitos secundários podem ser intercalados. O OpenWhisk não assegura qualquer
modelo de consistência simultâneo específico para efeitos colaterais. 

### Execuções de ação
{: #limits_exec}

Quando uma solicitação de chamada é recebida, o sistema registra a solicitação e despacha uma ativação.

O sistema retorna um ID de ativação (com uma chamada sem bloqueio) que confirma que ele foi recebido.
Se uma falha de rede ou outra falha intervém antes de você receber uma resposta de HTTP, é possível que o {{site.data.keyword.openwhisk_short}} recebeu e processou a solicitação.

O sistema tenta chamar a ação uma vez, resultando em um dos quatro resultados a seguir:

| Resultado | Descrição |
| --- | --- |
| `success` | A chamada de ação foi concluída com êxito. |
| `application error` | A chamada de ação foi bem-sucedida, mas a ação retornou um valor de erro de propósito, por exemplo, porque uma condição prévia sobre os argumentos não foi atendida. |
| `action developer error` | A ação foi chamada, mas foi concluída de forma anormal, por exemplo, a ação não detectou uma exceção ou um erro de sintaxe existia. |
| `whisk internal error` | O sistema não foi capaz de chamar a ação. O resultado é registrado no campo `status` do registro de ativação, como documento em uma seção a seguir. |

Para cada chamada recebida com sucesso e pela qual o usuário pode ser faturado tem um registro de ativação.

Quando o resultado é *erro de desenvolvedor de ação*, a ação pode ser parcialmente executada e gerar efeitos colaterais visíveis externos. É responsabilidade do usuário verificar se esses efeitos colaterais aconteceram e emitir a lógica de nova tentativa. Alguns *erros internos de whisk* indicam que uma ação começa a ser executada, mas falha antes de a ação registrar a conclusão.

## Registro de ativação
{: #limits_activation}

Cada chamada de ação e disparo do acionador resulta em um registro de ativação.

Um registro de ativação contém os campos a seguir:

| Campo | Descrição
| --- | --- |
| `activationId` | O ID de ativação. |
| `start` e `end` | Registros de data e hora que registram o início e o término da ativação. Os valores estão no [formato de hora do UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15){: external}. |
| `namespace` e `name` | O namespace e o nome da entidade. |
| `logs` | Uma matriz de sequências com os logs que são produzidos pela ação durante sua ativação. Cada elemento de matriz corresponde a uma saída de linha para
`stdout` ou `stderr` pela ação e inclui a hora e o
fluxo da saída do log. A estrutura é a seguinte: `TIMESTAMP STREAM: LOG_OUTPUT`. |
| `response` | Um dicionário que define as chaves `success`, `status` e `result`. `status`: o resultado de ativação, que pode ser um dos valores a seguir: "sucesso", "erro de aplicativo", "erro de desenvolvedor da ação", "erro interno do whisk". `success`: será `true` se e somente se o status for `"success"`. |
| `resultado` | Um dicionário que contém o resultado de ativação. Se a ativação foi bem-sucedida, o resultado contém o valor que é retornado pela ação. Se a ativação foi mal
sucedida, `result` terá a chave `error`, geralmente com
uma explicação da falha. |




