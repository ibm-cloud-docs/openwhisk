---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Monitorar a atividade com o Painel do {{site.data.keyword.openwhisk_short}}

O [Painel do {{site.data.keyword.openwhisk}}](https://{DomainName}/openwhisk/dashboard/) fornece um resumo gráfico de sua atividade. Use o painel para determinar o desempenho e o funcionamento de suas ações do {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Clique em **Recarregar** a qualquer momento para atualizar o painel com os dados do log de ativação mais recentes.

## Resumo de atividades
{: #summary}

Essa visualização fornece um resumo de alto nível de seu ambiente do {{site.data.keyword.openwhisk_short}}. Use a visualização **Resumo da atividade** para monitorar o funcionamento geral e o desempenho de seu serviço ativado pelo {{site.data.keyword.openwhisk_short}}. A partir das métricas nessa visualização, é possível executar o seguinte:
* Determine a taxa de uso de ações ativadas pelo {{site.data.keyword.openwhisk_short}} de seu serviço visualizando o número de vezes que elas foram chamadas.
* Determine a taxa geral de falha entre todas as ações. Se você identificar um erro, será possível isolar quais serviços ou ações tiveram erros por meio da visualização **Histograma de atividade**. Isole os próprios erros visualizando o **Log de atividades**.
* Determine com que sucesso as suas ações estão sendo executadas visualizando o tempo médio de conclusão que está associado com cada ação.

<!-- For tips on improving performance, see troubleshooting? -->

## Linha de tempo de atividades
{: #timeline}

A visualização **Linha de tempo de atividade** exibe um gráfico de barras verticais para visualizar a atividade de ações passadas e presentes. Vermelho indica erros dentro das Ações específicas. Correlacione essa visualização com o **Log de atividades** para localizar mais detalhes sobre os erros.

## Histograma de atividade
{: #histogram}

A visualização **Histograma de atividade** exibe um gráfico de barras horizontais para visualizar a atividade de ações passadas e presentes. Vermelho indica erros dentro das Ações específicas. Correlacione essa visualização com o **Log de atividades** para localizar mais detalhes sobre os erros.

## Log de Atividades
{: #log}

Essa visualização exibe uma versão formatada do log de ativação. Ela mostra os detalhes de cada ativação, mas pesquisa uma vez por minuto para ver se há novas ativações. Clique em uma ação para exibir um log detalhado.

Para obter a saída exibida no Log de atividades usando a CLI, use o comando a seguir:
{: tip}

  ```
  wsk activation poll
  ```
  {: pre}

## Opções de filtros
{: #filtering}

Selecione quais logs de ação você deseja visualizar e selecione o prazo da atividade registrada.

Esses filtros são aplicados a todas as visualizações no painel.
{: tip}
