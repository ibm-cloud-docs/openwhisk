---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Tempos de execução

## Tempos de execução de ação
As ações podem ser codificadas e executadas em várias linguagens de programação (por exemplo, JavaScript, Python, etc.). Os ambientes de tempo de execução disponíveis são mostrados nas seções a seguir.

Os links a seguir retornam uma resposta JSON que mostra os tempos de execução disponíveis para o IBM Cloud Functions em cada região.

A seção `runtimes` da resposta contém o conjunto de tempos de execução disponíveis.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

A seção `image` contém o nome da imagem de tempo de execução no [DockerHub](https://hub.docker.com/) e a tag que é usada. 

Os exemplos a seguir apontam para as imagens `ibmfunctions / action-nodejs-v10`e `openwhisk/nodejs6action`.
As tags podem ser números de versão como `1.9.0`ou o formato abreviado de um hash de confirmação de git, como `b99d71e`.

Campos de imagem de exemplo.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- Os tempos de execução de ação são atualizados regularmente. Essas atualizações incluem correções de segurança e atualizações de versão secundária para os pacotes dentro dos tempos de execução. Atualizações de versão secundária podem apresentar quebras de compatibilidade com versões anteriores. As atualizações de tempo de execução podem afetar suas ações. Não há migração automática para um tempo de execução mais novo do mesmo tipo.
- As ações que nos tempos de execução descontinuados não podem ser concluídas com sucesso até que o tempo de execução seja atualizado para um suportado. Ao resolver problemas de uma ação com falha, para identificar se um tempo de execução foi descontinuado, verifique `deprecated=true` na resposta da consulta. Para atualizar o tempo de execução, consulte [mudando o tempo de execução da ação](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)

## Ambientes de tempo de execução do JavaScript
{: #openwhisk_ref_javascript_environments}

As ações do JavaScript podem ser executadas no Node.js versão 8 ou 10. 

O Node.js versão 6 é a versão padrão, mas é descontinuada a partir de 6 de dezembro de 2018. Para continuar usando uma ação JavaScript, atualize para o Node.js versão 8 ou 10.
{: deprecated}

### Ambiente do Node.js versão 10 com SDKs IBM
{: #openwhisk_ref_javascript_environments_10}
O ambiente do Node.js versão 10 será usado se o sinalizador `--kind` for especificado explicitamente com um valor de `nodejs:10` ao criar ou atualizar uma ação.

#### Migrando de `nodejs:8` para `nodejs:10`
- O pacote npm `ibm_db` não está disponível em `nodejs:10`. O pacote `ibm_db` não suporta Node.js 10. É possível rastrear o progresso neste problema [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- O pacote npm `cloudant` não está disponível em `nodejs:10`, o pacote foi descontinuado; é necessário usar o pacote npm oficial [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 ao importar o módulo nodejs (ex., `require('@cloudant/cloudant')`), além disso [v3.x só retorna Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- O pacote npm `cradle` não está disponível em `nodejs:10`.
- O pacote npm `log4js` não está disponível em `nodejs:10`. É possível rastrear o problema em [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- O pacote `Watson-developer-cloud`npm não está disponível em `nodejs: 10`. É possível rastrear o progresso na nova versão no problema [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

As informações detalhadas sobre o ambiente de tempo de execução do nodejs versão 10 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

### Ambiente do Node.js versão 8 com SDKs da IBM
{: #openwhisk_ref_javascript_environments_8}
O ambiente do Node.js versão 8 será usado se o sinalizador `--kind` for especificado explicitamente com um valor de `nodejs:8` ao criar ou atualizar uma ação.

O Node.js versão 8 está no modo de manutenção e está disponível até dezembro de 2019. Consulte o [Planejamento de liberação do Node.js](https://github.com/nodejs/Release).
{: deprecated}
 
As informações detalhadas sobre o ambiente de tempo de execução do Node.js versão 8 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).

### Ambiente do Node.js versão 6 (descontinuado)
{: #openwhisk_ref_javascript_environments_6}
O Node.js versão 6 é a versão padrão, mas foi descontinuada. Para continuar usando uma ação JavaScript, atualize para o Node.js versão 8 ou 10.
{: deprecated}

As informações detalhadas sobre o ambiente de tempo de execução do nodejs versão 6 podem ser localizadas no [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md).

## Ambientes de tempo de execução do Python
{: #openwhisk_ref_python_environments}

O OpenWhisk suporta a execução de ações Python usando duas versões de runtime diferentes.

### Ações do Python 3.7 (com base no Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

As ações do Python 3.7 são executadas com o Python 3.7.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind python:3.7` quando você criar ou atualizar uma ação.

O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso pelas ações Python, além das bibliotecas padrão do Python 3.7.

As informações detalhadas sobre o ambiente de tempo de execução do Python 3.7 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Ações do Python 3.6 (baseado em Jessie Debian)
{: #openwhisk_ref_python_environments_3.6}

As ações do Python 3 são executadas com o Python 3.6.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind python:3.6` quando você criar ou atualizar uma ação.

O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso por ações do Python, além das bibliotecas padrão do Python 3.6.

As informações detalhadas sobre o ambiente de tempo de execução do Python 3.6 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Ações do Python 2

As ações do Python 2 são executadas com Python 2.7.15, a menos que você especifique o sinalizador `--kind` ao criar ou atualizar uma ação. Para selecionar explicitamente esse tempo de
execução, use `--kind python:2`.

Ao criar ações do Python usando virtualenv, use a imagem do docker `openwhisk/python2action`.
Os pacotes a seguir estão disponíveis para uso por ações do
Python 2, além da biblioteca padrão do Python 2.7.

As informações detalhadas sobre o ambiente de tempo de execução do Python 2 podem ser localizadas no [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).

## Ações do Swift
{: #swift-actions}

Os tempos de execução do Swift 3.1.1 e 4.1 estão descontinuados e estão disponíveis até 28 de fevereiro de 2019. Inicie quaisquer ações novas ou migre quaisquer ações existentes para o tempo de execução do Swift 4.2 usando o tipo `swift:4.2` e o novo processo de compilação.
{: tip}

### Swift 3
As ações do Swift 3 são executadas com o Swift 3.1.1 `--kind swift:3.1.1`. Sempre especifique o tipo `swift:3.1.1` porque versões anteriores do Swift não são suportadas.

Deve-se migrar todas as ações do Swift para usar o tipo `swift:3.1.1`. Como uma melhor prática, forneça sempre o tipo específico ao criar ou atualizar ações.
{: tip}

As ações do Swift 3.1.1 podem usar os pacotes a seguir ao usar um único arquivo de origem do Swift:
- KituraNet versão 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON versão 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK versão 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
As ações do Swift 4 podem ser executadas usando o Swift 4.1 ou 4.2 usando `--kind swift:4.1` ou `--kind swift:4.2`, respectivamente.
O padrão `--kind swift:default` é Swift 4.2.

Os tempos de execução de ação do Swift 4.x não incorporam nenhum pacote, siga as instruções para [ações swift empacotadas](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable) para incluir dependências usando um Package.swift.

As ações do Swift 4.1 podem usar os pacotes a seguir ao usar o único arquivo de origem do Swift:
- Watson Developer Cloud SDK versão 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

As ações do Swift 4.2 podem usar os pacotes a seguir ao usar o arquivo de origem Swift único:
- Watson Developer Cloud SDK versão 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Migrando o Swift 3.1.1 para o Swift 4.1

#### SwiftyJSON usando um único arquivo de ação de origem
Se você tiver uma ação `swift:3.1.1` que não for compilada e usar o pacote **SwiftyJSON**, será necessário pré-compilar sua ação e especificar a versão do SwiftyJSON que você desejar usar para a ação de tipo `swift:4.2`. Leve em conta que, a partir do Swift 4.1, há melhorias para gerenciar dados JSON.

## Ações PHP
{: #openwhisk_ref_php}

Os PHP 7.1 e 7.2 foram descontinuados a partir de 11 de janeiro de 2019. Para continuar utilizando uma ação PHP, atualize para PHP 7.3.
{: deprecated}

As ações de PHP são executadas com o PHP 7.3.0. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind php:7.3` quando você criar ou atualizar uma ação. Esse comportamento é o padrão quando você cria uma ação com um arquivo que tem uma extensão `.php`.

O tempo de execução do PHP 7.1 e 7.2 foi descontinuado. Migre todas as ações para o PHP 7.3 para melhor latência e execuções mais rápidas de ponta a ponta.

As extensões PHP a seguir estão disponíveis além daquelas padrão:

- bcmath
- ondulação
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Ações do Docker
{: #openwhisk_ref_docker}

As ações do Docker executam binário fornecido pelo usuário em um contêiner do Docker. O binário é executado em uma imagem do Docker com base em [python:3.6-alpine](https://hub.docker.com/r/library/python), portanto, o binário deve ser compatível com essa distribuição.

A estrutura básica do Docker é uma maneira conveniente de construir imagens do Docker compatíveis com o OpenWhisk. É possível instalar a estrutura básica com o comando de plug-in da CLI `ibmcloud fn sdk install docker`.

O programa binário principal deve estar localizado em `/action/exec` dentro do contêiner. O executável recebe os argumentos de entrada de uma única sequência de argumentos de linha de comandos, que pode ser desserializada como um objeto `JSON`. Ele deve retornar um resultado usando `stdout` como uma sequência de linha única de `JSON` serializado.

É possível incluir qualquer etapa ou dependência de compilação modificando o `Dockerfile` incluído no `dockerSkeleton`.
