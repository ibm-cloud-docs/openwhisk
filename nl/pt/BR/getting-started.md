---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions

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


# Introdução ao IBM {{site.data.keyword.openwhisk_short}}
{: #getting-started}

Com o {{site.data.keyword.openwhisk}}, é possível usar sua linguagem de programação favorita para gravar o código leve que executa a lógica do aplicativo de uma maneira escalável. É possível executar o código on demand com solicitações de API baseadas em HTTP por meio de aplicativos ou executar código em resposta a serviços do {{site.data.keyword.cloud_notm}} e eventos de terceiros. A plataforma de programação Function-as-a-Service (Faas) é baseada no projeto de software livre Apache OpenWhisk.
{: shortdesc}

## Trabalhando com ações
{: #gs_actions}

Com o {{site.data.keyword.openwhisk}}, é possível criar fragmentos de código stateless que são configurados para executar uma tarefa específica que é chamada de ação. Para saber mais sobre ações e outros termos do Functions, consulte [terminologia](/docs/openwhisk?topic=cloud-functions-about).
{:shortdesc}

Clique em uma opção para iniciar.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Clique em um ícone para começar rapidamente com {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Criar uma ação" title="Criar uma ação" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" title="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="Ver a arquitetura da plataforma" title="Ver a arquitetura da plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>

## Criando uma ação na GUI
{: #gs_hello_world}

Para iniciar com o {{site.data.keyword.openwhisk_short}}, tente criar o modelo de iniciação rápida Hello World.

1. Crie uma conta do [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external} ou efetue login em uma conta existente.

2. Navegue até o [painel do {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}.

2. Clique em **Iniciar criação** > **Modelos de iniciação rápida** e selecione o modelo **Hello World**.

3. Crie um pacote para suas ações, inserindo um nome exclusivo no campo **Nome do pacote**.

4. Selecione um tempo de execução no menu suspenso na seção **Helloworld de ação**. É possível visualizar o código para a ação de amostra em cada tempo de execução disponível antes de implementar o modelo.

5. Clique em **Implementar**. Você criou uma ação. Ótimo trabalho!

6. Execute a ação clicando em **Chamar**. Chamar uma ação executa manualmente a lógica do aplicativo que a ação define. No painel **Ativações**, é possível ver a saudação "Olá, estranho!" produzida pela ação.

7. Opcional: clique em **Mudar entrada** para mudar a ação ou tentar a sua própria.

  a. Cole o código a seguir na caixa **Mudar entrada** e mude o valor do nome.
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. Em seguida, clique em **Chamar** para executar a ação com suas atualizações. O resultado é mostrado no painel **Ativações**. É possível repetir esse processo quantas vezes você quiser.

Bom Trabalho! Você criou sua primeira ação. Para limpar essa ação, clique no menu overflow e selecione **Excluir ação**.

## Criando uma ação na CLI
{: #gs_hello_world_cli}

Faça-o funcionar rapidamente com o código de exemplo [Hello World JavaScript](/docs/openwhisk?topic=cloud-functions-prep#prep-js). Este exemplo cria uma ação básica `hello`, que é possível chamar manualmente para executar sua lógica do aplicativo.

## O que Vem a Seguir?
{: #gs_next_steps}

Então, você concluiu a sua primeira implementação de modelo. Para onde você vai daqui?

* Familiarize-se com a [terminologia](/docs/openwhisk?topic=cloud-functions-about#about_technology).
* Comece com [suas próprias ações](/docs/openwhisk?topic=cloud-functions-actions).
* Aprenda sobre como organizar ações em [pacotes](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Opção avançada - Criar uma [API de REST sem servidor](/docs/openwhisk?topic=cloud-functions-apigateway).



