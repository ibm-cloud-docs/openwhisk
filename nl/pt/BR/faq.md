---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-12"

keywords: faq, runtimes, actions, memory, monitoring

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:faq: data-hd-content-type='faq'}


# Perguntas mais frequentes
{: #faq}

Esta FAQ fornece respostas às perguntas comuns sobre o serviço {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## Quais tempos de execução de idioma são suportados?
{: #runtimes}
{: faq}

Os idiomas a seguir são suportados:

<table>
  <tr>
    <th id="language-col">Idioma</th>
    <th id="kind-identifier-col">Identificador de tipo</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code>, <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code>, <code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift: 4.1</code>, <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code>, <code>php:7.1</code></td>
  </tr>
  <tr>
    <td id="language-col-ruby" headers="language-col">Ruby</td>
    <td headers="kind-identifier-col language-col-ruby"><code>ruby:2.5</code></td>
  </tr>
  <tr>
    <td id="language-col-java" headers="language-col">Java</td>
    <td headers="kind-identifier-col language-col-java"><code>java (JDK 8)</code></td>
  </tr>
  <tr>
    <td headers="language-col" colspan="2">Outros idiomas são suportados usando ações do Docker.</td>
  </tr>
</table>
{: caption="Tabela 1. Tempos de execução suportados" caption-side="top"}


## Qual é o tempo máximo que minha função pode executar?
{: #max-runtime}
{: faq}

O tempo limite máximo é 10 minutos. O valor padrão é configurado como 1 minuto, mas pode ser mudado por meio da CLI, especificando um novo valor em milissegundos usando o sinalizador `--timeout`. Também é possível mudar o valor por meio da GUI na seção de detalhes da ação.


## Qual é a memória máxima que minha função pode usar?
{: #max-memory}
{: faq}

É possível usar até 2048 MB de memória para cada função. O padrão é configurado para 256 MB, mas é possível mudá-lo usando o sinalizador `--memory` ou por meio da GUI na seção de detalhes da ação.


## Qual é a diferença entre uma ação e uma ação da web?
{: #difference}
{: faq}

A diferença principal entre uma ação e uma ação da web é o objeto de saída de resposta. Para [ações da web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions), o resultado representa uma resposta de HTTP, em que é um mínimo, a saída JSON deve ter um campo `body`. Opcionalmente, ele pode também conter um statusCode e cabeçalhos.

## Como posso ver meus logs de ação?
{: #logs}
{: faq}

Depois que as métricas são coletadas, é possível ver seus logs usando o [serviço do {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs).


## Como o monitoramento funciona?
{: #monitor}
{: faq}

É possível obter insight sobre o desempenho de suas ações que são implementadas com o {{site.data.keyword.openwhisk_short}} usando o {{site.data.keyword.monitoringlong}}. Também é possível monitorar o funcionamento e o desempenho de suas ações, usando o painel para ver um resumo gráfico de sua atividade.


