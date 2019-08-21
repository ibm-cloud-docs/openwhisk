---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions

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


# Monitorando a atividade
{: #monitor}

Obtenha insight sobre o desempenho de suas ações implementadas com o {{site.data.keyword.openwhisk}}. As métricas podem ajudá-lo a localizar gargalos ou prever possíveis problemas de produção com base na duração da ação, resultados de ativações de ação ou quando você atingirá os limites de ativação de ação.
{: shortdesc}

As métricas são coletadas automaticamente para todas as entidades. Dependendo se suas ações estão em um namespace baseado em IAM ou em Cloud Foundry, as métricas estão localizadas na conta ou no espaço do IBM Cloud. Essas métricas são enviadas para o {{site.data.keyword.monitoringlong}} e são disponibilizadas por meio do Grafana, onde é possível configurar seus painéis, criar alertas com base nos valores de evento de métricas e mais. Para obter mais informações sobre métricas, consulte a [documentação do {{site.data.keyword.monitoringlong_notm}}](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started).

## Criando um painel
{: #monitor_dash}

Introdução ao criar um painel de monitoramento Grafana.

1. Acesse uma das URLs a seguir.
  <table>
    <thead>
      <tr>
        <th>Região do {{site.data.keyword.openwhisk_short}}</th>
        <th>Endereço de monitoramento</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>União Europeia Central</td>
        <td>`Metrics.eu-de.bluemix.net`</td>
      </tr>
      <tr>
        <td>Sul do Reino Unido</td>
        <td>`Metrics.eu-gb.bluemix.net`</td>
      </tr>
      <tr>
        <td>Sul dos Estados Unidos</td>
        <td>`Metrics.ng.bluemix.net`</td>
      </tr>
      <tr>
        <td>Leste dos EUA</td>
        <td>Não disponível</td>
      </tr>
    </tbody>
  </table>

2. Selecione o domínio de métricas.
    * Namespaces baseados em IAM:
        1. Clique no nome do usuário.
        2. Na lista suspensa **Domínio**, selecione **conta**.
        3. Na lista suspensa **Conta**, selecione a conta do IBM Cloud em que seu namespace baseado em IAM está localizado.
    * Namespaces baseados em Cloud Foundry:
        1. Clique no nome do usuário.
        2. Na lista suspensa **Domínio**, selecione **espaço**.
        3. Use as listas suspensas **Organização** e **Espaço** para selecionar seu namespace baseado no Cloud Foundry.

3. Crie um painel.
    * Para usar o painel pré-fabricado do {{site.data.keyword.openwhisk_short}}:
        1. Navegue para **Página inicial > Importar**.
        3. Insira o ID para o painel do {{site.data.keyword.openwhisk_short}} pré-fabricado, `8124`, no campo **Painel do Grafana.net**.
        4. Clique em **Importar**.
    * Para criar um painel customizado, navegue para **Página inicial > Criar novo**.

Após uma ação ser executada, novas métricas são geradas e são pesquisáveis no Grafana. Nota: pode levar até 10 minutos para que a ação executada seja exibida no Grafana.




## Usando os painéis
{: #monitor_dash_use}

O [{{site.data.keyword.openwhisk_short}}Painel](https://cloud.ibm.com/openwhisk/dashboard){: external} fornece um resumo gráfico de sua atividade. Utilize o painel para determinar o desempenho e o funcionamento de suas ações do {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

É possível filtrar logs selecionando quais logs de ações você deseja visualizar e selecionar o período de tempo da atividade registrada. Esses filtros são aplicados a todas as visualizações no painel. Clique em **Recarregar** a qualquer momento para atualizar o painel com os dados do log de ativação mais recentes.

### Resumo de atividades
{: #monitor_dash_sum}

A visualização **Resumo da atividade** fornece um resumo de alto nível de seu ambiente do {{site.data.keyword.openwhisk_short}}. Use essa visualização para monitorar o funcionamento e o desempenho gerais de seu serviço ativado pelo {{site.data.keyword.openwhisk_short}}. A partir das métricas nessa visualização, é possível executar o seguinte:
* Determine a taxa de uso das ações ativadas pelo {{site.data.keyword.openwhisk_short}} de seu serviço visualizando o número de vezes que eles foram chamadas.
* Determine a taxa geral de falha entre todas as ações. Se você vir um erro, será possível isolar quais serviços ou ações tinham erros abrindo a visualização **Histograma de atividade**. Isole os próprios erros visualizando o **Log de atividades**.
* Determine o nível de desempenho de suas ações visualizando o tempo médio de conclusão associado a cada ação.

### Linha de tempo de atividades
{: #monitor_dash_time}

A visualização **Linha de tempo de atividades** exibe um gráfico de barras verticais para visualizar a atividade de ações passadas e presentes. Vermelho indica erros em ações específicas. Correlacione essa visualização com o **Log de atividades** para localizar mais detalhes sobre os erros.



### Log de Atividades
{: #monitor_dash_log}

Essa visualização **Log de atividades** exibe uma versão formatada do log de ativação. Essa visualização mostra os detalhes de cada ativação e pesquisa uma vez por minuto por novas ativações. Clique em uma ação para exibir um log detalhado.

Para obter a saída exibida no Log de atividades usando a CLI, use o comando a seguir:
```
ibmcloud fn activation poll
```
{: pre}




## Formato da métrica
{: #monitor_metric}

As métricas refletem dados que são coletados de suas ativações de ação que são agregadas por minuto. As métricas são pesquisáveis no desempenho da ação ou no nível de simultaneidade de ação.


### Métricas de desempenho da ação
{: #monitor_metric_perf}

As métricas de desempenho da ação são valores que são calculados para uma única ação. As métricas de desempenho da ação englobam as características de sincronização das execuções e o status das ativações. Nota: se você não especificar o nome de um pacote durante a criação, o nome do pacote padrão será usado. Essas métricas têm o formato a seguir:

```
ibmcloud.public.functions. < region>.action.namespace. < namespace>. < package>. < action>. < metric_name>
```
{: codeblock}

Os caracteres a seguir são convertidos em traços (`-`): ponto (.), um sinal de arroba (@), espaço em branco (), e comercial (&), sublinhado (_), dois-pontos (:)
{: tip}

Exemplo: se você tivesse uma ação denominada `hello-world` no namespace baseado no Cloud Foundry `user@email.com_dev` na região `us-south`, uma métrica de desempenho da ação pareceria semelhante à seguinte:

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### Métricas de simultaneidade de ação
{: #monitor_metric_con}

Métricas de simultaneidade de ação são calculadas com base nos dados de todas as ações ativas em um namespace. A simultaneidade de ação inclui o número de chamadas simultâneas e as regulações do sistema que poderiam potencialmente ocorrer quando os limites de simultaneidade forem excedidos. Essas métricas têm o formato a seguir:

```
ibmcloud.public.functions. < region>.action.namespace. < namespace>.action-performance. < package>. < action>. < metric_name>
```
{: codeblock}

Exemplo: se você tivesse um namespace baseado no IAM denominado `myNamespace` na região `us-south`, uma métrica de simultaneidade de ação seria semelhante à seguinte:

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### Métricas disponíveis
{: #monitor_metric_av}

Como você pode ter milhares ou milhões de ativações de ação, os valores de métrica são representados como uma agregação de eventos produzidos por várias ativações. Os valores são agregados das maneiras a seguir:
* Soma: todos os valores de métrica são incluídos.
* Média: uma média aritmética é calculada.
* Média somada: uma média aritmética é calculada com base em componentes e incluindo componentes diferentes juntos.

Confira a tabela a seguir para ver as métricas que estão disponíveis para você.

<table>
  <thead>
    <tr>
      <th>Nome da métrica</th>
      <th>Descrição</th>
      <th>Tipo</th>
      <th>Categoria</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code> duração </code></td>
      <td>A duração média de ação, o tempo de execução da ação faturado.</td>
      <td>Média</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>O tempo que é gasto para inicializar o contêiner de ação.</td>
      <td>Média</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>O tempo médio que é gasto em uma fila aguardando que uma ativação seja planejada.</td>
      <td>Média</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>O número geral de ativações que foram acionadas no sistema.</td>
      <td>Soma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>O número de ativações bem-sucedidas do código de ação.</td>
      <td>Soma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>O número de ativações malsucedidas que são causadas por erros de aplicativo. Por exemplo, erros normais das ações. Para obter mais informações sobre como as métricas de desempenho de ação são derivadas, confira [Entendendo o registro de ativação](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record){: external}.</td>
      <td>Soma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>O número de ativações malsucedidas que são causadas pelo desenvolvedor. Por exemplo, a violação da [interface de proxy de ação](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface){: external} por exceções não manipuladas no código de ação.</td>
      <td>Soma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>O número de ativações malsucedidas causadas por erros internos do {{site.data.keyword.openwhisk_short}}.</td>
      <td>Soma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>A soma de ativações que foram reguladas porque o limite de taxa de simultaneidade foi excedido. Nenhuma métrica será emitida se o limite não for atingido.</td>
      <td>Soma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>A soma de ativações que foram reguladas porque o limite por minuto foi excedido. Nenhuma métrica será emitida se o limite não for atingido.</td>
      <td>Soma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>O número de chamadas simultâneas no sistema.</td>
      <td>Média somada</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

As métricas para ações que existem como parte de um namespace padrão estão disponíveis na categoria padrão.
{: tip}




