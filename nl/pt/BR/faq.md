---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: faq, runtimes, actions, memory, monitoring, functions

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
{:faq: data-hd-content-type='faq'}



# Perguntas mais frequentes
{: #faq}

Esta FAQ fornece respostas às perguntas comuns sobre o serviço {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## Quais tempos de execução de idioma são suportados?
{: #supported-runtimes}

Os idiomas a seguir são suportados:

<table>
  <tr>
    <th>Idioma</th>
    <th>Identificador de tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (padrão), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (padrão)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (padrão)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (padrão)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (padrão)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Outros idiomas são suportados usando ações do Docker.</td>
  </tr>
</table>
{: caption="Tabela 1. Tempos de execução suportados" caption-side="top"}


## Qual é o tempo máximo que minha função pode executar?
{: #max-runtime}

O tempo limite máximo é 10 minutos. O valor padrão é configurado como 1 minuto, mas pode ser mudado por meio da CLI, especificando um novo valor em milissegundos usando o sinalizador `--timeout`. Também é possível mudar o valor por meio da GUI na seção de detalhes da ação.

## Qual é a memória máxima que minha função pode usar?
{: #max-memory}

É possível usar até 2048 MB de memória para cada função. O padrão é configurado como 256 MB, mas é possível mudá-lo usando o sinalizador `--memory` ou por meio da GUI na seção de detalhes da ação.

## Qual é a diferença entre uma ação e uma ação da web?
{: #difference}

A diferença principal entre uma ação e uma ação da web é o objeto de saída de resposta. Para [ações da web](/docs/openwhisk?topic=cloud-functions-actions_web), o resultado representa uma resposta HTTP, em que, no mínimo, a saída JSON contém um campo `body`. Opcionalmente, ele também pode conter um `statusCode` e `headers`.

## Como posso ver meus logs de ação?
{: #logs_faq}

Depois que as métricas são coletadas, é possível ver seus logs usando o [serviço do {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-logs).

## Como o monitoramento funciona?
{: #monitor_faq}

É possível obter insight sobre o desempenho de suas ações que são implementadas com o {{site.data.keyword.openwhisk_short}} usando o {{site.data.keyword.monitoringlong}}. Também é possível monitorar o funcionamento e o desempenho de suas ações, usando o painel para ver um resumo gráfico de sua atividade.










