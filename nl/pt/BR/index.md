---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Introdução
{: #index}

Com o {{site.data.keyword.openwhisk}}, é possível usar sua linguagem de programação favorita para gravar o código leve que executa a lógica do aplicativo de uma maneira escalável. É possível executar o código on demand com solicitações de API baseadas em HTTP por meio de aplicativos ou automaticamente em resposta a serviços {{site.data.keyword.Bluemix_notm}} e eventos de terceiros. A plataforma de programação Function-as-a-Service (Faas) é baseada no projeto de software livre Apache OpenWhisk.
{: shortdesc}

Como o {{site.data.keyword.openwhisk_short}} é sem servidor, você não está limitado nos idiomas que podem ser usados e não é necessário gastar tempo provisionando explicitamente a infraestrutura de back-end. É possível se concentrar na gravação da lógica do app, em vez de se preocupar com o auto-scaling, a alta disponibilidade, as atualizações ou a manutenção. Pronto para uso, a IBM fornece o hardware, a rede, a administração de software, o balanceamento de carga, os plug-ins, etc. Você só tem que trazer o código!

## Trabalhando com ações
{: #creating_actions}
{: #openwhisk_actions}

Com o {{site.data.keyword.openwhisk}}, é possível criar fragmentos de código stateless que são configurados para executar uma tarefa específica que é chamada de ação.
{:shortdesc}

**O que é uma ação?**
{: #what_is_an_action}

Uma ação é uma pequena parte de código que pode ser chamada ou configurada para ser executada automaticamente em resposta a um evento. Em qualquer caso, cada execução resulta em um registro que é identificado por um ID de ativação exclusivo. A entrada e o resultado de uma ação podem ser vistos como pares de valores de chave. A chave é uma sequência e o valor é um valor JSON válido. Uma ação pode ser gravada no idioma de sua escolha e fornecida para o serviço como um código-fonte ou uma imagem do Docker. O código de ação é executado quando é chamado diretamente pela API do Cloud Functions, pela CLI ou pelo iOS SDK. Uma ação pode responder automaticamente a eventos do IBM Cloud ou serviços de terceiros.

**Por que eu utilizaria uma ação?**
{: #why_use_an_action}

Usando ações, você limita a quantidade de tempo que seu código está em execução, o que reduz os custos de sua sobrecarga.

Por exemplo, é possível usar ações para detectar rostos em uma imagem, responder a mudanças em um banco de dados, agregar um conjunto de chamadas de API ou até mesmo postar um tweet.

**Posso usar mais de uma ação de cada vez?**
{: #more_than_one_action}

Sim! É possível usar ações para chamar outras ações ou sequenciar ações juntas para criar sequências. Para fazer isso funcionar, a saída de uma ação seria a entrada para outra ação que forneceria uma saída que pode ser usada para acionar outra ação e assim por diante. É possível até mesmo empacotar o grupo de ações que você cria para formar um pacote. Com um pacote, é possível reutilizar ações ou sequências comuns chamando o pacote em vez de configurar a ação ou a sequência novamente.

Clique em uma opção para começar:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Clique em um ícone para começar rapidamente com {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="/docs/openwhisk?topic=cloud-functions-index#openwhisk_start_hello_world" alt="Criar uma ação" title="Criar uma ação" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" title="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="Ver a arquitetura da plataforma" title="Ver a arquitetura da plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>

## Criando uma ação na GUI
{: #openwhisk_start_hello_world}

Para começar a usar o {{site.data.keyword.openwhisk_short}}, tente usar o modelo de iniciação rápida HelloWorld.

1.  Visite o painel do Functions no [{{site.data.keyword.Bluemix_notm}}**Catálogo**![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk).

2. Clique em **Iniciar criação** > **Modelos de iniciação rápida** e selecione o modelo **Hello World**.

3. Crie um pacote para suas ações, inserindo um nome exclusivo no campo **Nome do pacote**.

4. Selecione um tempo de execução na lista suspensa na seção **Action helloworld**. É possível visualizar o código para a ação de amostra em cada tempo de execução disponível antes de implementar o modelo.

5. Clique em **Implementar**. Você criou uma ação. Ótimo trabalho!

6. Execute a ação clicando em **Chamar**. Chamar uma ação executa manualmente a lógica do aplicativo que a ação define. No painel **Ativações**, é possível ver a saudação "Olá, estranho!" produzida pela ação.

7. Opcional: clique em **Mudar entrada** para mudar a ação ou tentar a sua própria. Em seguida, clique em **Chamar** para executar a ação com suas atualizações. O resultado é mostrado no painel **Ativações**. É possível repetir esse processo quantas vezes você quiser.

Bom Trabalho! Você criou sua primeira ação. Para limpar essa ação, clique no menu overflow e selecione **Excluir ação**.

## Criando uma ação na CLI
{: #openwhisk_start_hello_world_cli}

Comece a trabalhar rapidamente com o código de exemplo HelloWorld de JavaScript. Este exemplo cria uma ação básica `hello`, que é possível chamar manualmente para executar sua lógica do aplicativo.

1. [Configure o plug-in da CLI do {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

2. Salve o código a seguir em um arquivo nomeado **hello.js**.

    ```javascript
    /**
     * Hello World como uma ação do OpenWhisk.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Crie a ação `hello`.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Chame a ação sem passar nenhum parâmetro.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Saída:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Chame a ação novamente, passando o parâmetro de nome, para testar a lógica de app.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Saída:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Bom Trabalho! Você criou sua primeira ação. Para limpar essa ação, execute `ibmcloud fn action
delete hello`.

## O que Vem a Seguir?
{: #next-steps}

Então, você concluiu seu primeiro modelo de implementação, para onde você vai daqui? Você pode:

* Familiarize-se com a [terminologia](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology).
* Comece com [suas próprias ações](/docs/openwhisk?topic=cloud-functions-openwhisk_actions).
* Aprenda sobre como organizar ações em [pacotes](/docs/openwhisk?topic=cloud-functions-openwhisk_packages).
* Opção avançada: crie uma [API de REST sem servidor](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
