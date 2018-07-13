---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Visualizando logs de ativação no IBM Cloud
{: #openwhisk_logs}

Os logs de ativação podem ser visualizados diretamente da [página Monitoramento do {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Os logs também são encaminhados ao [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) no qual eles são indexados, permitindo a procura de texto completa em todas as mensagens geradas e consultando convenientemente com base em campos específicos (como o nível de log).
{:shortdesc}

## Consultando logs
{: #query-logs}

Ao usar o Kibana hospedado pelo [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana), a consulta de seus logs é direta. Use a sintaxe de consulta do Kibana para localizar os logs que você está procurando.

A UI do {{site.data.keyword.openwhisk_short}} permite navegar diretamente para os logs e os resultados de suas ações no Kibana. O link **Logs** está localizado na navegação esquerda interior da [página Monitoramento do {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Ao acessar a página de detalhes de uma ação específica, o link **Logs** conduz você para os resultados (registros de ativação) dessa ação específica. O valor padrão do prazo para mostrar logs é configurado para 15 minutos. É possível mudar esse valor diretamente no Kibana no canto superior direito se você deseja exibir registros mais antigos.

Aqui estão alguns exemplos de consultas úteis para depurar erros.

### Localizando todos os logs de erro:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Localizando todos os logs de erro gerados por "myAction":
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Consultando resultados
{: #query-results}

Além de linhas de log, o [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) também indexa os resultados (registros de ativação) gerados pelo {{site.data.keyword.openwhisk_short}}. Os resultados contêm metadados ricos relevantes para ativações, como sua duração ou código de resultado (sucesso, erro). Todos os campos são consultáveis e, sendo assim, isso pode ajudá-lo a entender como suas ações do {{site.data.keyword.openwhisk_short}} estão se comportando.

Use a sintaxe de consulta do Kibana para localizar ativações que você está procurando. Aqui estão alguns exemplos de consultas úteis para depurar erros.

### Localizando todas as ativações com falha:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Como em comandos UNIX, um "`0`" indica uma ação de saída com êxito enquanto o resto é considerado um erro.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Localizando todas as ativações que falharam com um erro específico:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
