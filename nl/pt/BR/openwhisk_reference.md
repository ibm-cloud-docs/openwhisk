---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Detalhes e limites do sistema
{: #openwhisk_reference}

As seções a seguir fornecem detalhes técnicos sobre o sistema {{site.data.keyword.openwhisk}} e as configurações de limite.
{: shortdesc}

## entidades do {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Namespaces e pacotes
{: #openwhisk_entities_namespaces}

As ações, os acionadores e as regras do {{site.data.keyword.openwhisk_short}} pertencem a um namespace e, às vezes, a um pacote.

Os pacotes podem conter ações e feeds. Um pacote não pode conter outro pacote, portanto, o aninhamento de pacote não é permitido. Além disso, as entidades não precisam estar contidas em um pacote.

No {{site.data.keyword.Bluemix_notm}}, um par de organização + espaço corresponde a um namespace do {{site.data.keyword.openwhisk_short}}. Por exemplo, a organização `BobsOrg` e o espaço `dev` corresponderiam ao namespace do {{site.data.keyword.openwhisk_short}} `/BobsOrg_dev`.



É possível criar novos namespaces baseados em Cloud Foundry [criando as organizações e os espaços do Cloud Foundry](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#region_info). O namespace `/whisk.system` é reservado para entidades distribuídas com o sistema {{site.data.keyword.openwhisk_short}}.


### Nomes Completos
{: #openwhisk_entities_fullyqual}

O nome completo de uma entidade é `/namespaceName/[packageName]/entityName`. Observe que a `/` é usada para delimitar namespaces, pacotes e entidades. Além disso, os namespaces devem ter como prefixo uma `/`.

Por conveniência, o Namespace pode ser deixado desligado se for o namespace padrão do usuário. Por exemplo, considere um usuário cujo namespace padrão seja `/myOrg`. Seguem
exemplos de nomes completos de diversas entidades e seus aliases.



| Nome Completo | Alias | Namespace | Pacote | Nome |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

É possível usar esse esquema de nomenclatura ao usar a CLI do {{site.data.keyword.openwhisk_short}}, entre outros locais.

### Nomes de entidades
{: #openwhisk_entities_names}

Os nomes de todas as entidades, incluindo ações, acionadores, regras, pacotes e
namespaces, são uma sequência de caracteres que seguem o formato a seguir:

* O primeiro caractere deve ser um caractere alfanumérico ou um sublinhado.
* Os caracteres subsequentes podem ser alfanuméricos, espaços ou qualquer um dos valores a seguir: `_`, `@`, `.`, `-`.
* O último caractere não pode ser um espaço.

Mais precisamente, um nome deve corresponder à expressão regular a seguir (expressa com sintaxe Java metacaractere): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semântica de ação
{: #openwhisk_semantics}

As seções a seguir descrevem detalhes sobre ações do {{site.data.keyword.openwhisk_short}}.

### Statelessness
{: #openwhisk_semantics_stateless}

As implementações de ação são stateless ou *idempotente*. Embora o sistema não cumpra essa propriedade, não é garantido que qualquer estado mantido por uma ação estará disponível entre chamadas.

Além disso, múltiplas instanciações de uma ação podem existir, com cada instanciação com seu próprio estado. Uma chamada de ação pode ser despachada para qualquer uma dessas instanciações.

### Saída e entrada de chamada
{: #openwhisk_semantics_invocationio}

A entrada e a saída de uma ação é um dicionário de pares de valores de chaves. A chave é uma sequência e o valor um valor JSON válido.

### Ordem de chamada de ações
{: #openwhisk_ordering}

Chamadas de uma ação não são ordenadas. Se o usuário chamar uma ação duas vezes a
partir da linha de comandos ou a API REST, a segunda chamada poderá ser executada antes
da primeira. Se
as ações tiverem efeitos colaterais, elas poderão ser observadas em
qualquer ordem.

Além disso, não é garantido que as ações sejam executadas automaticamente. Duas ações podem ser executadas simultaneamente e seus efeitos secundários podem ser intercalados. O OpenWhisk não assegura qualquer
modelo de consistência simultâneo específico para efeitos colaterais. Quaisquer efeitos colaterais de simultaneidade são dependentes da implementação.

### Garantias de execução de ação
{: #openwhisk_atmostonce}

Quando uma solicitação de chamada é recebida, o sistema registra a solicitação e despacha uma ativação.

O sistema retorna um ID de ativação (com uma chamada sem bloqueio) que confirma que ele foi recebido.
Se uma falha de rede ou outra falha intervém antes de você receber uma resposta de HTTP, é possível que o {{site.data.keyword.openwhisk_short}} recebeu e processou a solicitação.

O sistema tenta chamar a ação uma vez, resultando em um dos quatro resultados a seguir:
- *sucesso*: a chamada da ação foi concluída com sucesso.
- *erro de aplicativo*: a chamada da ação foi bem-sucedida, mas a ação retornou um valor de erro de propósito, por exemplo, porque uma condição prévia dos argumentos não foi atendida.
- *erro de desenvolvedor de ação*: a ação foi chamada, mas foi concluída de forma anormal, por exemplo, a ação não detectou uma exceção ou um erro de sintaxe existia.
- *erro interno de whisk*: o sistema não pôde chamar a ação.
O resultado é registrado no campo `status` do registro de ativação, como documento em uma seção a seguir.

Para cada chamada recebida com sucesso e pela qual o usuário pode ser faturado tem um registro de ativação.

Quando o resultado é *erro de desenvolvedor de ação*, a ação pode ser parcialmente executada e gerar efeitos colaterais visíveis externos. É de responsabilidade do usuário verificar se tais efeitos colaterais aconteceram e emitir uma lógica de nova tentativa, se desejado. Alguns *erros internos de whisk* indicam que uma ação começa a ser executada, mas falha antes de a ação registrar a conclusão.

## Registro de ativação
{: #openwhisk_ref_activation}

Cada chamada de ação e disparo do acionador resulta em um registro de ativação.

Um registro de ativação contém os campos a seguir:

- *activationId*: o ID de ativação.
- *start* e *end*: registros de data e hora de início e término da ativação. Os valores estão no [formato de hora do UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* e `name`: o namespace e o nome da entidade.
- *logs*: uma matriz de sequências com os logs produzidos pela ação
durante sua ativação. Cada elemento de matriz corresponde a uma saída de linha para
`stdout` ou `stderr` pela ação e inclui a hora e o
fluxo da saída do log. A estrutura é a seguinte: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: um dicionário que define as chaves `success`, `status` e `result`:
  - *status*: o resultado de ativação, que pode ser um dos valores a seguir: "sucesso", "erro de aplicativo", "erro de desenvolvedor da ação", "erro interno do whisk".
  - *success*: é `true` se e somente se o status for `"sucesso"`
- *result*: um dicionário que contém o resultado da ativação. Se a ativação foi bem-sucedida, o resultado contém o valor que é retornado pela ação. Se a ativação foi mal
sucedida, `result` terá a chave `error`, geralmente com
uma explicação da falha.

## API da REST
{: #openwhisk_ref_restapi}

As informações sobre a API de REST do {{site.data.keyword.openwhisk_short}} podem ser localizadas na [Referência da API de REST](https://cloud.ibm.com/apidocs/functions).

## Limites do sistema
{: #openwhisk_syslimits}

### Ações
O {{site.data.keyword.openwhisk_short}} possui alguns limites do sistema, incluindo o quanto de memória uma ação pode usar e quantas chamadas de ação são permitidas por minuto.

A tabela a seguir lista os
limites padrão para ações.

| Limit | Descrição | Padrão | Mín. | Máxima |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_codesize) | O tamanho máximo do código de ação em MB. | 48 | 1 | 48 |
| [simultâneo](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_concurrent) | Não mais que N ativações podem ser enviadas por namespace, executando ou enfileiradas para execução. | 1000 | 1 | 1000* |
| [logs](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_logs) | Um contêiner não tem permissão para gravar mais de N MB para stdout. | 10 | 0 | 10 |
| [memória](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_memory) | Um contêiner não tem permissão para alocar mais de N MB de memória. | 256 | 128 | 2048 |
| [minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_minuterate) | Não mais que N ativações podem ser enviadas por namespace por minuto. | 5000 | 1 | 5000* |
| [openulimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_openulimit) | O número máximo de arquivos abertos para uma ação. | 1024 | 0 | 1024 |
| [parâmetros](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_parameters) | O tamanho máximo dos parâmetros que podem ser anexados em MB. | 5 | 0 | 5 |
| [proculimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_proculimit) | O número máximo de processos disponíveis para uma ação. | 1024 | 0 | 1024 |
| [resultado](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_result) | O tamanho máximo do resultado da chamada de ação em MB. | 5 | 0 | 5 |
| [sequenceMaxActions](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_sequencemax) | O número máximo de ações que compõem uma sequência especificada. | 50 | 0 | 50* |
| [tempo limite](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_timeout) | Um contêiner não tem permissão para executar por mais de N milissegundos. | 60000 | 100 | 600000 |

### Aumentando limites fixos
{: #increase_fixed_limit}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados se um business case puder justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://cloud.ibm.com/openwhisk) da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Functions** para a Área de suporte técnico.

#### codeSize (MB) (Fixo: 48 MB)
{: #openwhisk_syslimits_codesize}
* O tamanho máximo do código para a ação é 48 MB.
* Para ações JavaScript, use uma ferramenta para concatenar todo o código-fonte, que inclui dependências, em um único arquivo empacotado.
* Esse limite é fixo e não pode ser mudado.

#### simultâneo (Fixo: 1000*)
{: #openwhisk_syslimits_concurrent}
* O número de ativações que estão em execução ou enfileiradas para execução para um namespace não pode exceder 1.000.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit) para obter instruções detalhadas sobre como aumentar esse limite.

#### logs (MB) (Padrão: 10 MB)
{: #openwhisk_syslimits_logs}
* O limite de log N está no intervalo [0 MB..10 MB] e é configurado por ação.
* Um usuário pode mudar o limite do log de ações quando uma ação é criada ou atualizada.
* Os logs que excedem o limite configurado são truncados, assim quaisquer novas entradas de log são ignoradas e um aviso é incluído como a última saída da ativação para indicar que a ativação excedeu o limite de log configurado.

#### memória (MB) (Padrão: 256 MB)
{: #openwhisk_syslimits_memory}
* O limite de memória M está no intervalo de [128 MB...2.048 MB] e é configurado por ação em MB.
* Um usuário pode mudar o limite de memória quando uma ação é criada.
* Um contêiner não pode usar mais memória do que está alocada pelo limite.

#### minuteRate (Fixo: 5000*)
{: #openwhisk_syslimits_minuterate}
* O limite de taxa N é configurado para 5000 e limita o número de chamadas de ação em janelas de 1 minuto.
* Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit) para obter instruções detalhadas sobre como aumentar esse limite.

#### openulimit (Fixo: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* O número máximo de arquivos abertos para uma ação é 1024 (para os limites máximo e flexível).
* Esse limite é fixo e não pode ser mudado.
* Quando uma ação é chamada, o comando de execução do docker usa o argumento `--ulimit nofile=1024:1024` para configurar o valor `openulimit`.
* Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parâmetros (Fixo: 5 MB)
{: #openwhisk_syslimits_parameters}
* O limite de tamanho para os parâmetros totais na criação ou atualização de uma Ação/Pacote/Acionador é de 5 MB.
* Uma entidade com parâmetros muito grandes é rejeitada em tentar criar ou atualizar isso.
* Esse limite é fixo e não pode ser mudado.

#### proculimit (Fixo: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* O número máximo de processos disponíveis para o contêiner de ação é 1024.
* Esse limite é fixo e não pode ser mudado.
* Quando uma ação é chamada, o comando de execução do docker usa o argumento `--pids-limit 1024` para configurar o valor `proculimit`.
* Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### resultado (Fixo: 5 MB)
{: #openwhisk_syslimits_result}
* O tamanho máximo de saída de um resultado da chamada de ação em MB.
* Esse limite é fixo e não pode ser mudado.

#### sequenceMaxActions (Fixo: 50*)
{: #openwhisk_syslimits_sequencemax}
* O número máximo de ações que compõem uma sequência especificada.
* Esse limite é fixo e não pode ser mudado.

#### tempo limite (ms) (Padrão: 60s)
{: #openwhisk_syslimits_timeout}
* O limite do tempo limite N está no intervalo [100 ms..600000 ms ] e é configurado por ação em milissegundos.
* Um usuário pode mudar o limite do tempo limite quando uma ação é criada.
* Um contêiner executado mais de N milissegundos é finalizado.

### disparos

Os acionadores estão sujeitos a uma taxa de disparo por minuto, conforme documentado na tabela a seguir.

| Limit | Descrição | Padrão | Mín. | Máxima |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_tminuterate) | Não mais que N acionadores podem ser disparados por namespace por minuto. | 5000* | 5000* | 5000* |

### Aumentando limites fixos
{: #increase_fixed_tlimit}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados se um business case puder justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://cloud.ibm.com/openwhisk) da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Functions** para a Área de suporte técnico.

#### minuteRate (Fixo: 5000*)
{: #openwhisk_syslimits_tminuterate}

* O limite de taxa N é configurado para 5000 e limita o número de acionadores que um usuário pode disparar em janelas de 1 minuto.
* Um usuário não pode mudar o limite do acionador quando um acionador é criado.
* Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_tlimit) para obter instruções detalhadas sobre como aumentar esse limite.
