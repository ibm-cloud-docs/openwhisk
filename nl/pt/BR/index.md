---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introdução
{: #index}

O {{site.data.keyword.openwhisk}} é uma plataforma de programação poliglota Functions-as-a-Service (FaaS) com base no Apache OpenWhisk. O {{site.data.keyword.openwhisk_short}} permite que os desenvolvedores gravem ações chamadas por código leve que executam a lógica de aplicativo de forma escalável. É possível configurar ações para ocorrerem sob demanda com base em solicitações de API baseada em HTTP de apps da web ou apps móveis ou automaticamente em resposta a solicitações baseadas em evento de serviços {{site.data.keyword.Bluemix_notm}} e eventos de terceiros.
{: shortdesc}

Como o {{site.data.keyword.openwhisk_short}} é uma plataforma sem servidor acionada por evento, você não precisa provisionar explicitamente os servidores. Os desenvolvedores que trabalham com microsserviços, dispositivos móveis, IoT e muitos outros apps podem focalizar a gravação da lógica do app em vez de se preocuparem com ajuste automático de escala, alta disponibilidade, atualizações e manutenção. O ajuste automático de escala e o balanceamento de carga prontos para utilização significam que você não precisa configurar manualmente clusters, plug-ins http e assim por diante! A IBM cuida de toda a administração de hardware, rede e software. Tudo que você tem que fazer é fornecer o código.

Clique em uma opção para começar:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Clique em um ícone para começar a usar rapidamente o {{site.data.keyword.openswhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Criar uma ação" title="Criar uma ação" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="Configurar o plug-in da CLI do {{site.data.keyword.openwhisk_short}}" title="Configurar o plug-in da CLI do {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="Ver a arquitetura da plataforma" title="Ver a arquitetura da plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>


## Criando uma ação na GUI
{: #openwhisk_start_hello_world}

Para começar a usar o {{site.data.keyword.openwhisk_short}}, tente usar o modelo de iniciação rápida HelloWorld.

1.  No **Catálogo** do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/?category=whisk), categoria **Functions**, clique em Functions.

2. Clique em **Começar a criar** > **Modelos de iniciação rápida** e selecione o modelo **Hello World**.

5. Verifique o código para a ação e, em seguida, crie a ação clicando em **Implementar**. Agora você tem uma ação chamada `hello`.

6. Execute a ação clicando em **Chamar**. Chamar uma ação executa manualmente a lógica do app que a ação define. No painel **Ativações**, é possível ver a saudação "Olá, estranho!" produzida pela ação.

Bom Trabalho! Você criou sua primeira ação. Para limpar essa ação, clique no menu overflow e selecione **Excluir ação**.

## Criando uma ação na CLI
{: #openwhisk_start_hello_world_cli}

Comece a trabalhar rapidamente com o código de exemplo HelloWorld de JavaScript. Esse exemplo cria uma ação básica `hello` que você pode chamar manualmente para executar sua lógica de app.

1. [Configure o plug-in da CLI do {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

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
    ibmcloud wsk action create hello hello.js
    ```
    {: pre}

4. Chame a ação sem passar nenhum parâmetro.
    ```
    ibmcloud wsk action invoke hello --blocking --result
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
    ibmcloud wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Saída:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Bom Trabalho! Você criou sua primeira ação. Para limpar essa ação, execute `ibmcloud wsk action delete hello`.

**O que Vem a Seguir?**
* [Experimente o exemplo de serviço de alarme para chamar a ação **hello** sempre que um evento periódico for gerado.](./openwhisk_packages.html#openwhisk_package_trigger)
* [Criar uma API de REST sem servidor.](openwhisk_apigateway.html)
* [Verifique os pacotes de ação pré-instalados para serviços {{site.data.keyword.Bluemix_notm}} como o Cloudant.](cloudant_actions.html)
