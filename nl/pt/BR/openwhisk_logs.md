---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Atividade de criação de log e monitoramento
{: #openwhisk_logs}

A criação de log e o monitoramento são ativados automaticamente no {{site.data.keyword.openwhisk_short}} para ajudar a solucionar problemas e a melhorar o funcionamento e o desempenho de suas ações.

## Visualizando Logs
{: #view-logs}

É possível visualizar os logs de ativação diretamente no painel Monitoramento do
{{site.data.keyword.openwhisk_short}}. Os logs também são encaminhados para o [ {{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) no qual eles são indexados, permitindo uma procura de texto completa por meio de todas as mensagens geradas e consulta conveniente com base em campos específicos.
{:shortdesc}

1. Abra a página Monitoramento do [{{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/dashboard/).

2. Opcional: para visualizar os logs somente de uma ação específica, limite o resumo de monitoramento para essa ação. Na seção Opções de filtragem, selecione o nome da ação na lista suspensa **Limitar a**.

3. Na navegação esquerda, clique em  ** Logs **. A página  {{site.data.keyword.loganalysisshort_notm}}  Kibana é aberta.

4. Opcional: para ver os logs mais antigos, mude o valor do intervalo de tempo padrão de 15 minutos clicando em **Últimos 15 minutos** no canto superior direito e selecionando um intervalo de tempo diferente.

### Consultando logs
{: #query-logs}

É possível localizar logs de ativação específicos no [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) usando a sintaxe de consulta do Kibana.

As consultas de exemplo a seguir podem ajudá-lo a depurar erros:
  * Localize todos os logs de erro:
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Localize todos os logs de erro que são gerados por "myAction":
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Consultando resultados
{: #query-results}

Além das linhas de log, o [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) também indexa os resultados ou os registros de ativação que são gerados pelo {{site.data.keyword.openwhisk_short}}. Os resultados contêm metadados de ativação, como a duração de ativação ou o código de resultado de ativação. A consulta de campos de resultados pode ajudá-lo a entender como suas ações do {{site.data.keyword.openwhisk_short}} estão se comportando.

É possível localizar logs de ativação específicos usando a sintaxe de consulta do Kibana. As consultas de exemplo a seguir podem ajudá-lo a depurar erros:

* Localize todas as ativações com falha:
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    Nos resultados, um `0` indica uma ação de saída com sucesso e todos os outros valores indicam um erro.

* Localize todas as ativações que falharam com um erro específico:
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

## Monitorando a atividade
{: #openwhisk_monitoring}

O [{{site.data.keyword.openwhisk_short}}Painel](https://console.bluemix.net/openwhisk/dashboard/) fornece um resumo gráfico de sua atividade. Utilize o painel para determinar o desempenho e o funcionamento de suas ações do {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

É possível filtrar logs selecionando quais logs de ações você deseja visualizar e selecionar o período de tempo da atividade registrada. Esses filtros são aplicados a todas as visualizações no painel. Clique em **Recarregar** a qualquer momento para atualizar o painel com os dados do log de ativação mais recentes.

### Resumo de atividades
{: #summary}

A visualização **Resumo da atividade** fornece um resumo de alto nível de seu ambiente do {{site.data.keyword.openwhisk_short}}. Use essa visualização para monitorar o funcionamento e o desempenho gerais de seu serviço ativado pelo {{site.data.keyword.openwhisk_short}}. A partir das métricas nessa visualização, é possível executar o seguinte:
* Determine a taxa de uso das ações ativadas pelo {{site.data.keyword.openwhisk_short}} de seu serviço visualizando o número de vezes que eles foram chamadas.
* Determine a taxa geral de falha entre todas as ações. Se você vir um erro, será possível isolar quais serviços ou ações tinham erros abrindo a visualização **Histograma de atividade**. Isole os próprios erros visualizando o **Log de atividades**.
* Determine o nível de desempenho de suas ações visualizando o tempo médio de conclusão associado a cada ação.

### Linha de tempo de atividades
{: #timeline}

A visualização **Linha de tempo de atividades** exibe um gráfico de barras verticais para visualizar a atividade de ações passadas e presentes. Vermelho indica erros em ações específicas. Correlacione essa visualização com o **Log de atividades** para localizar mais detalhes sobre os erros.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### Log de Atividades
{: #log}

Essa visualização **Log de atividades** exibe uma versão formatada do log de ativação. Essa visualização mostra os detalhes de cada ativação e pesquisa uma vez por minuto por novas ativações. Clique em uma ação para exibir um log detalhado.

Para obter a saída exibida no Log de atividades usando a CLI, use o comando a seguir:
```
ibmcloud fn activation poll
```
{: pre}
