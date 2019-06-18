---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: actions, serverless, javascript, node, node.js

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
{:gif: data-image-type='gif'}


# Criando Ações
{: #actions}

Crie uma ação, que é uma função de nível superior que retorna um objeto JSON. É possível combinar ações em um pacote para simplificar o gerenciamento de suas ações.
{: shortdesc}

Antes de iniciar: para criar uma ação, seu código-fonte deve atender a determinados requisitos. Por exemplo, se você deseja criar uma ação por meio do código que está contido em múltiplos arquivos, empacote seu código como um único arquivo antes de criar a ação. Consulte [Preparando seu código de app para sem servidor](/docs/openwhisk?topic=cloud-functions-prep) para obter detalhes sobre os requisitos para cada tempo de execução.


## Criando ações por meio da CLI
{: #actions_cli}

1. Crie uma ação.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  Exemplo:
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Exemplo de Saída:

  ```
  ok: ação hello criada
  ```
  {: screen}

  Dicas:
  - Para economizar no custo, é possível configurar limites.
      - Para configurar um limite para uso de memória, inclua `--memory VALUE` em seu comando create, em que o valor está em megabytes.
      - Para configurar um tempo limite, inclua `--timeout VALUE` em seu comando create, em que o valor está em milissegundos.
  - Se você empacotou seu código como uma imagem do Docker, inclua `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` em seu comando create em vez do caminho local para seu app e o sinalizador --kind. Gerencie bem suas imagens não usando a tag `latest` sempre que possível. Quando a tag `latest` é usada, a imagem com essa tag é usada, o que pode nem sempre ser a imagem criada mais recentemente.  
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  


2. Verifique se a ação está em sua lista de ações.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemplo de Saída:

  ```
  ações
  hello       privada
  ```
  {: screen}


## Atualizando apps ou tempos de execução em ações
{: #actions_update}

É possível executar o comando update sempre que precisar atualizar o código em seu app ou migrar para uma versão mais recente de um tempo de execução. Por exemplo, uma vez que o Node.js versão 8 está no modo de manutenção, talvez você deseje alternar o tempo de execução para o Node.js 10.

Ao migrar para uma nova versão de tempo de execução, talvez você precise mudar o código em seu app para estar em conformidade com a nova versão de tempo de execução. Na maioria dos casos, as versões de runtime são compatíveis.
{: tip}

1. Atualize o app localmente.

2. Se você empacotou seu app como uma imagem do Docker, faça upload da imagem mais recente para o Docker Hub. Isso permitirá que o sistema puxe a sua nova imagem do Docker na próxima vez em que ele executar o código para a sua ação. Se houver uma execução de contêiner que use uma versão anterior de sua imagem do Docker, quaisquer novas chamadas continuarão a usar essa imagem. Deve-se executar o comando update para que novas chamadas sejam iniciadas usando a nova imagem.

3. Atualize uma ação e inclua o caminho local para seu app ou a imagem do Docker.
    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    Exemplo:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    Exemplo de Saída:

    ```
    ok: updated action hello
    ```
    {: screen}

    Se você empacotou seu código como uma imagem do Docker, inclua `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` em seu comando create em vez do caminho para o app local e o sinalizador --kind. Gerencie bem suas imagens não usando a tag `latest` sempre que possível. Quando a tag `latest` é usada, a imagem com essa tag é usada, o que pode nem sempre ser a imagem criada mais recentemente. 
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## Ligando parâmetros a ações
{: #actions_params}

É possível ligar parâmetros a ações para configurar parâmetros padrão. Os parâmetros de ligação servem como parâmetros padrão para ações, a menos que os parâmetros sejam fornecidos na chamada.
{: shortdesc}

Antes de iniciar, [crie a ação](#actions_cli).

Para ligar os parâmetros:

1. Atualize uma ação e ligue os parâmetros padrão a ela.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    Exemplo:
    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: updated action MyApp
    ```
    {: screen}

    Se você modifica seus parâmetros de credenciais sem serviço, executar um comando `action update` com novos parâmetros remove quaisquer parâmetros que existem atualmente, mas não estão especificados no comando `action update`. Por exemplo, se você executar `action update -p key1 new-value -p key2 new-value`, mas omitir quaisquer outros parâmetros que foram configurados, esses parâmetros não existirão mais após a ação ser atualizada. Todos os serviços que foram ligados à ação também serão removidos. Se você vinculou um serviço, deverá [ligar os serviços à sua ação](/docs/openwhisk?topic=cloud-functions-services) novamente.
    {: tip}

3. Verifique se os parâmetros foram ligados à ação.
    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name", "value": "World" }
    ]
    ```
    {: screen}

Opcional: para limpar os parâmetros que foram ligados anteriormente, atualize a ação sem incluir nenhum parâmetro.
```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## Encadeando ações juntas como sequências de ações
{: #actions_seq}

É possível criar uma ação que encadeia uma sequência de ações juntas. O resultado de uma ação é passado
como um argumento para a próxima ação.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

Os parâmetros que são passados entre as ações na sequência são explícitos, exceto para os parâmetros padrão. Portanto, os parâmetros passados para a sequência de ações estão disponíveis somente para a primeira ação na sequência. O resultado da primeira ação na sequência se torna o objeto JSON de entrada para a segunda ação na sequência e assim por diante. Esse objeto não inclui nenhum dos parâmetros que são originalmente passados para a sequência, a menos que a primeira ação os inclua em seu resultado. Os parâmetros de entrada para uma ação são mesclados com os parâmetros padrão da ação, com o antigo tendo precedência e substituindo qualquer parâmetro padrão correspondente.

Uma sequência não tem um tempo limite geral separado dos tempos limite de cada ação dentro da sequência. Como uma sequência é um pipeline de operações, uma falha em uma ação quebra o pipeline. Se uma ação atingir o tempo limite, a sequência inteira será encerrada com essa falha.

Em seguida, quando você criar uma regra ou chamar as ações, use o nome da sequência.




## Empacotando ações
{: #actions_pkgs}

No {{site.data.keyword.openwhisk}}, é possível usar pacotes para empacotar um conjunto de ações e feeds relacionados juntos e compartilhá-los com outros. Os pacotes também permitem que os parâmetros sejam compartilhados entre todas as entidades no pacote.
{: shortdesc}

Um pacote pode incluir *ações* e *feeds*.
- Uma ação é uma parte do código executada no {{site.data.keyword.openwhisk_short}}. Por exemplo, o pacote {{site.data.keyword.cloudant}}inclui ações para ler e gravar registros em um banco de dados {{site.data.keyword.cloudant_short_notm}}.
- Um feed é usado para configurar uma origem de eventos externos para disparar eventos acionadores. Por exemplo, o pacote Alarme inclui um feed que pode disparar um acionador a uma frequência especificada.



1. Crie um pacote.
  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. Obtenha um resumo do pacote. Observe que o pacote está vazio.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /myNamespace/custom
  ```
  {: screen}

4. Crie uma ação e inclua-a no pacote. A criação de uma ação em um pacote requer que o nome da ação tenha como prefixo um nome de pacote. Aninhamento de pacote não é permitido. Um pacote pode conter apenas ações e não pode conter outro pacote.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. Obtenha um resumo do pacote.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /NAMESPACE/PACKAGE_NAME ação /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## Ligando parâmetros a pacotes
{: #actions_pkgs_params}

É possível configurar parâmetros padrão para todas as entidades em um pacote configurando os parâmetros no nível do pacote que são herdados por todas as ações no pacote.

Os parâmetros de limite servem como os parâmetros padrão para ações no pacote, a menos que:

- a ação em si tenha um parâmetro padrão
- a ação tenha um parâmetro fornecido no momento da chamada

Antes de iniciar, crie um pacote que inclua pelo menos uma ação.

1. Atualize um pacote e ligue o parâmetro padrão a ele.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    Exemplo:
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: updated package MyApp
    ```
    {: screen}

    Se você modificar seus parâmetros de credencial que não são de serviço, a execução de um comando `package update` com novos parâmetros removerá quaisquer parâmetros que existirem atualmente, mas que não forem especificados no comando `package update`. Por exemplo, se você executar `package update -p key1 new-value -p key2 new-value`, mas omitir quaisquer outros parâmetros que foram configurados, esses parâmetros não existirão mais depois que o pacote for atualizado. Todos os serviços que foram ligados ao pacote também serão removidos, portanto, depois de atualizar outros parâmetros, deve-se [ligar os serviços ao seu pacote](/docs/openwhisk?topic=cloud-functions-services) novamente.
    {: tip}

3. Verifique se os parâmetros foram ligados ao pacote.
    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name", "value": "World" }
    ]
    ```
    {: screen}

4. Verifique se os parâmetros foram herdados pelo pacote.
    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name", "value": "World" }
    ]
    ```
    {: screen}



## Compartilhando pacotes de ações
{: #actions_pkgs_share}

Após as ações e os feeds que formam um pacote serem depuradas e testadas, o pacote pode ser compartilhado com todos os usuários do {{site.data.keyword.openwhisk_short}}. Compartilhar o pacote possibilita que os usuários liguem o pacote, chamem ações no pacote e criem regras do {{site.data.keyword.openwhisk_short}} e ações de sequência. Ações e feeds dentro de um pacote compartilhado são _public_. Se
o pacote for privado, então, todo o seu conteúdo também será privado.
{: shortdesc}

1. Compartilhe o pacote com todos os usuários:
  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. Exiba a propriedade `publish` do pacote para verificar se agora é true.
  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. Obtenha uma descrição do pacote para fornecer a outros o nome completo do pacote para que eles possam ligá-lo ou chamar ações nele. O nome completo inclui o namespace, que neste exemplo é o namespace `myNamespace`.
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /NAMESPACE/PACKAGE_NAME ação /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## Variáveis de ambiente para ações
{: #actions_envvars}

O ambiente de ação contém várias variáveis de ambiente que são específicas para a ação em execução. As propriedades são acessíveis no ambiente do sistema para todos os tempos de execução suportados. Essas propriedades permitem que as ações funcionem programaticamente com ativos por meio da API de REST ou configurem um alarme interno quando a ação está prestes a usar seu orçamento de tempo atribuído.
{: shortdesc}

| Propriedade | Descrição |
| -------- | ----------- |
| `__OW_API_HOST` | O host da API para a implementação que executa essa ação. |
| `__OW_API_KEY` | A chave de API para o assunto que chama a ação. Essa chave pode ser uma chave de API restrita e está ausente, a menos que seja solicitada explicitamente. Consulte [Anotações](/docs/openwhisk?topic=cloud-functions-annotations). |
| `__OW_NAMESPACE` | O namespace para a ativação. Esse namespace pode não ser o mesmo
namespace da ação. |
| `__OW_ACTION_NAME` | O nome completo da ação em execução. |
| `__OW_ACTIVATION_ID` | O ID de ativação para esta instância de ação em execução. |
| `__OW_DEADLINE` | O tempo aproximado, em milissegundos, em que essa ação consome sua cota de duração inteira. |

### Incorporando variáveis de ambiente de ação ao seu app
{: #actions_envvars_app}

Para visualizar os valores de uma ação, inclua a exibição deles em seu código de app e publique-os nos resultados.

Exemplo para Python:
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

Depois de atualizar e ativar o código em uma ação, o resultado incluirá o nome completo para a ação.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
