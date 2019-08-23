---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Testando apps sem servidor
{: #test}

Teste cada entidade que você cria por meio da CLI para verificar se seu app sem servidor está trabalhando ou para solucionar problemas em um local onde um problema possa estar ocorrendo.
{: shortdesc}


## Testando ações
{: #test-js}

É possível testar ações executando o comando `invoke`. É possível testar a ação com ou sem parâmetros.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Exemplo Hello world**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**Saída**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Testando parâmetros armazenados em arquivos JSON
{: #test_json_file}

É possível passar um arquivo de parâmetros formatados por JSON.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**Saída de exemplo**
```
{
    "payload": "Hello, Dorothy from Kansas" }
```
{: screen}


### Testando parâmetros inseridos no formato JSON
{: #test_json}

É possível transmitir parâmetros JSON formatados com sua chamada.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**Saída de exemplo**
```
{
    "payload": "Hello, Dorothy from Kansas" }
```
{: screen}


### Testando ações de bloqueio
{: #test-block}

A chamada da ação pode ser de bloqueio ou sem bloqueio. As chamadas são sem bloqueio por padrão. Se você não precisar do resultado da ação imediatamente, use uma chamada sem bloqueio.
{: shortdesc}

As chamadas de bloqueio usam um estilo de solicitação-resposta e aguardam que o resultado de ativação esteja disponível. O período de espera é o menor de 60 segundos ou o [valor limite de tempo](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) da ação.

Execute a ação na nuvem ao executar uma chamada de bloqueio.

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**Saída de exemplo**
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
      "status": "success",
      "success": true
  }
```
{: screen}

O comando expõe as informações a seguir.
* O resultado da chamada, se ele estiver disponível dentro do período de espera previsto
* Sem a opção `--result`, o ID de ativação é exibido no resultado. O ID de ativação (`44794bd6aab74415b4e42a308d880e5b`) que pode ser usado para recuperar os logs ou o resultado da chamada.


## Testando acionadores
{: #test_triggers}

Os acionadores podem ser disparados ou ativados usando um dicionário de pares chave-valor. Às vezes, esse dicionário é referido como o evento. Os acionadores podem ser explicitamente disparados por um usuário ou disparados em nome de um usuário por uma origem de eventos externos. Como com as ações, cada disparo de um acionador que está associado a uma regra resulta em um ID de ativação.
{: shortdesc}

1. Dispare o acionador.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    Um acionador que não estiver associado com uma regra não terá efeito visível quando ele for disparado. Como nenhuma regra está associada a esse acionador, os parâmetros passados não são usados como entrada por nenhuma ação.

    **Saída de exemplo**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Verifique se a ação foi chamada, verificando o registro de ativação mais recente.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ativações
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Obtenha mais informações sobre o ID de ativação na saída de comando anterior.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **Saída de exemplo**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Testando a duração de ativações
{: #test_time}

Verifique quanto tempo uma ativação levou para ser concluída obtendo o log de ativação. Se a duração for muito longa ou você precisar ajustar o tempo limite padrão para permitir que a função seja executada mais tempo, será possível atualizar sua ação com um tempo limite.
{: shortdesc}

1. Obtenha o ID de ativação.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Saída de exemplo:
    ```
    ativações
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Obtenha o log para o ID de ativação.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    A `duração` mostra o tempo em milissegundos. A ativação demorou um pouco mais de 2 segundos para ser concluída.

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. Atualize a ação com um tempo limite em milissegundos.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Exemplo:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Testando o uso de memória
{: #test_memory}

Se o seu app for empacotado em uma imagem do Docker, será possível usar comandos do Docker para verificar o uso de memória de seu app.
{: shortdesc}

1. Crie um contêiner localmente que execute a imagem do Docker.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Obtenha uma lista dos contêineres para obter um ID do contêiner.

    ```
    docker ps
    ```
    {: pre}

3. Verifique as estatísticas do contêiner em execução.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Revise o valor de uso de memória para o contêiner. Se o valor não se ajustar dentro dos limites do sistema, ajuste seu script.

5. Depois de terminar de revisar as informações, será possível parar o contêiner em execução.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Remova o contêiner.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}








