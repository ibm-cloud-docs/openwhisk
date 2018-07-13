---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Serverless Framework
{: #openwhisk_goserverless}

O [Serverless Framework](https://serverless.com/) é uma estrutura de software livre
para construir aplicativos sem servidor. Usando um arquivo manifest simples, os desenvolvedores podem definir funções de serverless, conectá-las a origens de eventos e declarar serviços de nuvem que são necessários pelo seu aplicativo. A estrutura manipula a implementação desses aplicativos sem servidor nos provedores em nuvem. Ele também permite que os desenvolvedores monitorem serviços em produção, apresentem atualizações e ajudem nos problemas de depuração. Ela também possui um ecossistema vibrante de plug-ins de terceiros para
estender a funcionalidade da estrutura. OpenWhisk é a tecnologia que orienta o Serverless Framework.
{:shortdesc}

O OpenWhisk tem [seu próprio
plug-in de provedor para o Serverless Framework](https://github.com/serverless/serverless-openwhisk). Os desenvolvedores que usam o Serverless Framework podem escolher implementar seus aplicativos em qualquer instância de plataforma do OpenWhisk (hospedada no {{site.data.keyword.Bluemix_notm}} ou outra nuvem ou privada). O suporte para múltiplos provedores também significa que mover um aplicativo entre plataformas é muito mais fácil, permitindo o desenvolvimento de aplicativos serverless de multinuvem por desenvolvedores.

## Informações Iniciais
{: #openwhisk_goserverless_starting}

[Guia de introdução para OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/) do Serverless Framework oficial.
* Um guia que aborda instalação, fluxo de trabalho de desenvolvimento, melhores práticas, guia passo a passo para construir e implementar um aplicativo OpenWhisk funcional e muito mais.

Assista a [o Serverless Framework e o OpenWhisk](https://youtu.be/GJY10W98Itc), um vídeo que explica como usar o Serverless Framework com o plug-in do provedor do OpenWhisk.

## Documentação
{: #openwhisk_goserverless_docs}

A documentação mais recente sobre como usar o OpenWhisk com o Serverless Framework
[pode ser localizada aqui](https://serverless.com/framework/docs/providers/openwhisk/).

## Amostras
{: #openwhisk_goserverless_samples}
[O repositório de exemplos GitHub do Serverless
Framework](https://github.com/serverless/examples) agora apresenta o OpenWhisk mostrando como construir APIs HTTP, planejadores baseados em
cron, funções de encadeamento e muito mais.
