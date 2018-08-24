---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gerenciando ações
{: #openwhisk_managing}

Gerencie ações monitorando a saída da ação, obtendo informações específicas sobre uma ação ou excluindo
ações.
{: shortdec}

## Obtendo ações
{: #getting-actions}

Depois de ter criado uma ação, é possível obter mais informações sobre os detalhes da ação e listar as
ações em seu namespace.
{: shortdesc}

Para listar todas as ações que você criou:
```
ibmcloud fn action list
```
{: pre}

À medida que você cria mais ações, pode ser útil agrupar ações relacionadas em
[pacotes](./openwhisk_packages.html). Para filtrar sua lista de ações para somente ações em
um pacote específico:
```
ibmcloud fn action list [ PACKAGE NAME ]
```
{: pre}

Para obter metadados que descrevem ações específicas:

```
ibmcloud fn action get hello
```
{: pre}

Exemplo de Saída:
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>Entendendo a Saída do Comando  <code> action get </code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo a saída do comando <code>action get</code></th>
</thead>
<tbody>
<tr>
<td><code> namespace </code></td>
<td>O namespace no qual esta ação está.</td>
</tr>
<tr>
<td><code> name </code></td>
<td>O nome da ação.</td>
</tr>
<tr>
<td><code> version </code></td>
<td>A versão de semântica da ação.</td>
</tr>
<tr>
<td><code> exec </code></td>
<td><ul><li><code> kind </code>: o tipo de ação. Os valores possíveis são nodejs:6, nodejs:8, php:7.1,
python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox e sequence.</li>
<li><code>code</code>: o código Javascript ou Swift a ser executado quando o tipo é nodejs ou swift.</li>
<li><code>components</code>: as ações na sequência quando o tipo é sequência. As ações são listadas em ordem.</li>
<li><code>image</code>: o nome da imagem do contêiner quando o tipo é blackbox.</li>
<li><code>init</code>: a referência de zipfile opcional quando o tipo é nodejs.</li>
<li><code>binary</code>: se a ação é compilada em um executável binário.</li></ul></td>
</tr>
<tr>
<td><code> annotations </code></td>
<td>Anotações sobre esta ação. Para obter uma lista de anotações possíveis, consulte os tópicos de referência
[Anotações de ação](openwhisk_annotations.html#action) e
[Anotações da web](openwhisk_annotations.html#annotations-specific-to-web-actions).</td>
</tr>
<tr>
<td><code> limites </code></td>
<td><ul><li><code>timeout</code>: o tempo limite, em milissegundos, configurado para a ação após o qual a
ação é finalizada. Padrão: 6000</li>
<li><code>memory</code>: o limite máximo de memória, em MB, configurado para a ação. Padrão: 256</li>
<li><code>logs</code>: o limite de tamanho máximo do log, em MB, configurado para a ação. Padrão: 10</li></ul></td>
</tr>
<tr>
<td><code> publicar </code></td>
<td>Se a ação é publicada publicamente.</td>
</tr>
</tbody></table>

## Visualizando detalhes de ativação
{: #activation}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários, em resposta a vários eventos ou como parte de uma sequência de ações. 
Sempre que uma ação é chamada, um registro de ativação é criado para essa chamada. Para obter informações
sobre o resultado da chamada de ação, é possível obter detalhes sobre ativações.

Para obter todos os IDs de registro de ativação em um namespace:
```
ibmcloud fn activation list
```
{: pre}

Para obter detalhes sobre um registro de ativação específico que resultou de uma chamada de ação:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Exemplo de Saída:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Entendendo a saída do comando <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo a saída do comando
<code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code> namespace </code></td>
<td>O namespace no qual essa ativação se encontra. Isso pode ser diferente do namespace no qual a ação se
encontra.</td>
</tr>
<tr>
<td><code> name </code></td>
<td>O nome da ação.</td>
</tr>
<tr>
<td><code> version </code></td>
<td>A versão de semântica da ação.</td>
</tr>
<tr>
<td><code> subject </code></td>
<td>A conta do usuário que ativou o item.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID desse registro de ativação.</td>
</tr>
<tr>
<td><code> iniciar </code></td>
<td>Horário em que a ativação foi iniciada.</td>
</tr>
<tr>
<td><code>termina
</code></td>
<td>Horário em que a ativação foi concluída.</td>
</tr>
<tr>
<td><code> duração </code></td>
<td>O tempo, em milissegundos, que a ativação levou para ser concluída.</td>
</tr>
<tr>
<td><code> response </code></td>
<td><ul><li><code>status</code>: o status de saída da ativação.</li>
<li><code> statusCode </code>: o código de status. Se a ação falhou, o código de erro HTTP.</li>
<li><code> success </code>: Se a ação foi concluída com êxito.</li>
<li><code>result</code>: o valor de retorno da ativação.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Logs para esta ativação.</td>
</tr>
<tr>
<td><code> annotations </code></td>
<td>Anotações sobre esta ação. Para obter uma lista de possíveis anotações de ativação, consulte o
[Tópico de referência de anotações](openwhisk_annotations.html#activation).</td>
</tr>
<tr>
<td><code> publicar </code></td>
<td>Se a ação é publicada publicamente.</td>
</tr>
</tbody></table>

## Acessando metadados de ação dentro do corpo de ação
{: #accessing-action-metadata-within-the-action-body}

O ambiente de ação contém várias propriedades que são específicas da ação em execução. Essas
propriedades permitem que a ação funcione programaticamente com os ativos do OpenWhisk por meio da API de REST ou
configure um alarme interno quando a ação estiver prestes a usar seu orçamento de tempo atribuído. As
propriedades são acessíveis no ambiente do sistema para todos os tempos de execução suportados: Node.js,
Python, Swift, Java e Docker ao usar a estrutura básica do OpenWhisk Docker.

| Propriedade | Descrição |
| -------- | ----------- |
| `__OW_API_HOST` | O host da API para a implementação do OpenWhisk que executa esta ação. |
| `__OW_API_KEY` | A chave API para o assunto que chama a ação, essa chave pode ser uma chave
de API restrita. |
| `__OW_NAMESPACE` | O namespace para a ativação. Esse namespace pode não ser o mesmo
namespace da ação. |
| `__OW_ACTION_NAME` |O nome completo da ação em execução. |
| `__OW_ACTIVATION_ID` | O ID de ativação para esta instância de ação em execução. |
| `__OW_DEADLINE` | O tempo aproximado, em milissegundos, em que essa ação consome sua cota de duração inteira. |

## Obtendo uma URL de ação
{: #get-action-url}

Uma ação pode ser chamada usando a interface REST por meio de uma solicitação de HTTPS.
{: shortdesc}

Para obter uma URL de ação:
```
ibmcloud fn action get actionName -- url
```
{: pre}

Exemplo de saída para ações padrão:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Exemplo de saída para  [ ações da web ](./openwhisk_webactions.html):
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Nota:** para ações padrão, a autenticação deve ser fornecida quando chamada por meio de uma solicitação de HTTPS. Para obter mais informações sobre as chamadas de ação usando a interface REST, veja a [Referência da API de REST](https://console.bluemix.net/apidocs/openwhisk).

## Salvando o código de ação
{: #save-action}

É possível obter e salvar localmente o código associado a uma ação existente. É possível salvar o código
para todas as ações, exceto para ações de sequências e do Docker.
{: shortdesc}

Salve o código de ação em um nome de arquivo que corresponda a um nome de ação existente no diretório atualmente em funcionamento.
```
ibmcloud fn action get actionName -- save
```
{: pre}

Uma extensão de arquivo que corresponde ao tipo de ação é usada. Para o código de ação que for um
arquivo zip, uma extensão de .zip será usada. Exemplo de Saída:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

Em vez disso, é possível fornecer um caminho de arquivo customizado, o nome do arquivo e a extensão
usando a sinalização `-- save-as`.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Exemplo de Saída:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Monitorando logs de ações
{: #monitor-action-output}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários, em resposta a vários eventos ou como parte de uma sequência de ações. 
Para obter informações sobre quando as ações foram chamadas e qual foi a saída, pode ser útil monitorar os
logs de ação.

É possível usar a CLI do {{site.data.keyword.openwhisk_short}} para observar a saída de ações à medida que são chamadas.

1. Inicie um loop de pesquisa que verifica continuamente os logs de ativações.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Alterne para outra janela e chame uma ação.
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: /whisk.system/samples/helloWorld chamada com id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Na janela de pesquisa, é possível ver o log de ativação.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
Também é possível ver os logs de qualquer ação que for executada em seu nome no
{{site.data.keyword.openwhisk_short}} em tempo real.

## Excluindo ações
{: #deleting-actions}

É possível limpar excluindo ações que você não deseja usar.

1. Excluir uma ação.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: deleted hello
    ```
    {: screen}

2. Verifique se a ação não aparece mais na lista de ações.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Exemplo de Saída:
    ```
    actions
    ```
    {: screen}
