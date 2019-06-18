---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# Visualizando Logs
{: #logs}

A criação de log é ativada automaticamente no {{site.data.keyword.openwhisk}} para ajudá-lo a solucionar problemas. Também é possível usar o serviço {{site.data.keyword.cloudaccesstraillong}} para controlar como os usuários e os aplicativos interagem com o serviço {{site.data.keyword.openwhisk_short}}.


## Visualizando logs de ação conforme eles ocorrem
{: #logs_poll}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários, em resposta a vários eventos ou como parte de uma sequência de ações. Para obter informações sobre quando as ações foram chamadas e qual foi a saída, pode ser útil monitorar os
logs de ação.

É possível usar a CLI do {{site.data.keyword.openwhisk_short}} para observar a saída de ações à medida que são chamadas.

1. Inicie um loop de pesquisa que verifica continuamente os logs de ativações.

    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Alterne para outra janela e chame uma ação.

    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: /whisk.system/samples/helloWorld chamada com id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Na janela de pesquisa, é possível ver o log de ativação.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    Também é possível ver os logs de qualquer ação que for executada em seu nome no
{{site.data.keyword.openwhisk_short}} em tempo real.




## Visualizando detalhes de ativação
{: #activation_details}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários, em resposta a vários eventos ou como parte de uma sequência de ações. Sempre que uma ação é chamada, um registro de ativação é criado para essa chamada. Para obter informações
sobre o resultado da chamada de ação, é possível obter detalhes sobre ativações.

Para obter todos os IDs de registro de ativação em um namespace:
```
ibmcloud fn activation list
```
{: pre}

Para obter detalhes sobre um registro de ativação específico que resultou de uma chamada de ação:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Exemplo de Saída:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Entendendo a saída do comando <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo a saída do comando
<code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code> namespace </code></td>
<td>O namespace no qual essa ativação se encontra. Isso pode ser diferente do namespace no qual a ação se
encontra.</td>
</tr>
<tr>
<td><code> name </code></td>
<td>O nome da ação.</td>
</tr>
<tr>
<td><code> version </code></td>
<td>A versão de semântica da ação.</td>
</tr>
<tr>
<td><code> subject </code></td>
<td>A conta do usuário que ativou o item.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID desse registro de ativação.</td>
</tr>
<tr>
<td><code> iniciar </code></td>
<td>Horário em que a ativação foi iniciada.</td>
</tr>
<tr>
<td><code>termina
</code></td>
<td>Horário em que a ativação foi concluída.</td>
</tr>
<tr>
<td><code> duração </code></td>
<td>O tempo, em milissegundos, que a ativação levou para ser concluída.</td>
</tr>
<tr>
<td><code> response </code></td>
<td><ul><li><code>status</code>: o status de saída da ativação.</li>
<li><code> statusCode </code>: o código de status. Se a ação falhou, o código de erro HTTP.</li>
<li><code> success </code>: Se a ação foi concluída com êxito.</li>
<li><code>result</code>: o valor de retorno da ativação.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Logs para esta ativação.</td>
</tr>
<tr>
<td><code> annotations </code></td>
<td>Anotações sobre esta ação. Para obter uma lista de possíveis anotações de ativação, consulte o
[Tópico de referência de anotações](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code> publicar </code></td>
<td>Se a ação é publicada publicamente.</td>
</tr>
</tbody></table>



## Visualizando logs do painel do
{{site.data.keyword.loganalysisfull_notm}}
{: #logs_view}

É possível visualizar os logs de ativação diretamente no painel Monitoramento do
{{site.data.keyword.openwhisk_short}}. Os logs também são encaminhados para o [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) no qual eles são indexados, permitindo a procura de texto completa por meio de todas as mensagens geradas e a consulta conveniente com base em campos específicos.
{:shortdesc}

**Nota**: a criação de log não está disponível para a região Leste dos EUA.

1. Abra a [página Monitoramento do {{site.data.keyword.openwhisk_short}}![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/dashboard).

2. Opcional: para visualizar os logs somente de uma ação específica, limite o resumo de monitoramento para essa ação. Na seção Opções de filtragem, selecione o nome da ação na lista suspensa **Limitar a**.

3. Na navegação esquerda, clique em  ** Logs **. A página  {{site.data.keyword.loganalysisshort_notm}}  Kibana é aberta.

4. Opcional: para ver os logs mais antigos, mude o valor do intervalo de tempo padrão de 15 minutos clicando em **Últimos 15 minutos** no canto superior direito e selecionando um intervalo de tempo diferente.

### Consultando logs
{: #logs_query}

É possível localizar logs de ativação específicos no [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) usando a sintaxe de consulta do Kibana.

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
{: #logs_query_results}

Além das linhas de log, o [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) também indexa os resultados ou os registros de ativação que são gerados pelo {{site.data.keyword.openwhisk_short}}. Os resultados contêm metadados de ativação, como a duração de ativação ou o código de resultado de ativação. A consulta de campos de resultados pode ajudá-lo a entender como suas ações do {{site.data.keyword.openwhisk_short}} estão se comportando.

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

