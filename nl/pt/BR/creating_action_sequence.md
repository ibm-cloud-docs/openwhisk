---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-07"

keywords: action sequence, serverless,

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


# Criando sequências de ação
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

É possível criar uma ação que encadeia uma sequência de ações juntas. O resultado de uma ação é passado
como um argumento para a próxima ação.
{: shortdesc}

Várias ações do utilitário são fornecidas no pacote `/whisk.system/utils` que podem ser usadas para criar sua primeira sequência.

1. Liste as ações no pacote `/whisk.system/utils`.
    ```
    ibmcloud fn package get -- summary /whisk.system/utils
    ```
    {: pre}

    Exemplo de Saída:
    ```
    package /whisk.system/utils: construindo blocos que formatam e montam dados
   action /whisk.system/utils/head: extrair prefixo de uma matriz
   action /whisk.system/utils/split: dividir uma sequência em uma matriz
   action /whisk.system/utils/sort: classifica uma matriz
   action /whisk.system/utils/echo: retorna a entrada
   action /whisk.system/utils/date: data e hora atual
   action /whisk.system/utils/cat: concatena a entrada em uma sequência
    ```
    {: screen}

2. Usando as ações `split` e `sort`, crie uma sequência de ações para que o resultado de `split` seja passado como um argumento para `sort`. Essa sequência de ações converte algumas linhas de texto a uma matriz e classifica as linhas.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Na saída, as linhas de divisão são classificadas em ordem alfabética.
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

** Nota **:
* Os parâmetros que são passados entre as ações na sequência são explícitos, exceto para os parâmetros padrão. Portanto, os parâmetros passados para a sequência de ações estão disponíveis somente para a primeira ação na sequência. O resultado da primeira ação na sequência se torna o objeto JSON de entrada para a segunda ação na sequência e assim por diante. Esse objeto não inclui nenhum dos parâmetros que são originalmente passados para a sequência, a menos que a primeira ação os inclua em seu resultado. Os parâmetros de entrada para uma ação são mesclados com os parâmetros padrão da ação, com o antigo tendo precedência e substituindo qualquer parâmetro padrão correspondente. Para obter mais informações sobre como chamar sequências de ações com múltiplos parâmetros nomeados, veja [Configurando parâmetros padrão em uma ação](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action).
* Uma sequência não tem um tempo limite geral separado dos tempos limite de cada ação dentro da sequência. Como uma sequência é um pipeline de operações, uma falha em uma ação quebra o pipeline. Se uma ação atingir o tempo limite, a sequência inteira será encerrada com essa falha.
