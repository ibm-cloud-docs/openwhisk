---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# Tempos de execução
{: #runtimes}
Seus apps podem ser codificados e executados em linguagens de programação, como JavaScript ou Python. Muitos tempos de execução estão disponíveis por padrão com o {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Visualize os tempos de execução disponíveis para o IBM Cloud Functions em cada região. Os links a seguir retornam uma resposta JSON.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

A seção `runtimes` da resposta contém o conjunto de tempos de execução disponíveis.

A seção `image` contém o nome da imagem de tempo de execução no [Docker Hub](https://hub.docker.com/) e a tag que é usada.


Os exemplos a seguir apontam para as imagens `ibmfunctions/action-nodejs-v10` e `openwhisk/nodejs8action`. As tags podem ser números de versão como `1.9.0`ou o formato abreviado de um hash de confirmação de git, como `b99d71e`.

Campos de imagem de exemplo.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Os tempos de execução são atualizados regularmente. Essas atualizações incluem correções de segurança e atualizações de versão secundária para os pacotes dentro dos tempos de execução. Atualizações de versão secundária podem apresentar quebras de compatibilidade com versões anteriores. As atualizações de tempo de execução podem afetar suas ações. Deve-se migrar ações que estejam executando um tempo de execução para uma versão mais recente, atualizando-a.

Os apps que são executados em tempos de execução descontinuados não podem ser concluídos com êxito até que o tempo de execução seja atualizado para um suportado. Ao resolver problemas de uma ação com falha, para identificar se um tempo de execução foi descontinuado, verifique `deprecated=true` na resposta da consulta. Para atualizar o tempo de execução, consulte [mudando o tempo de execução da ação](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

Esses tempos de execução foram descontinuados:
<ul>
  <li><code>nodejs:6</code> (descontinuado)</li>
  <li><code>php:7.1</code> (descontinuado)</li>
  <li><code>php:7.2</code> (descontinuado)</li>
  <li><code>swift:3</code> (descontinuado)</li>
  <li><code>swift:3.1.1</code> (descontinuado)</li>
  <li><code>swift:4.1</code> (descontinuado)</li>
  <li><code>ballerina:0.990</code> (descontinuado)</li>
</ul>






## Tempos de execução de JavaScript
{: #openwhisk_ref_javascript_environments}

As ações do JavaScript podem ser executadas no Node.js versão 8 ou 10. Por padrão, todas as ações do Node.js são executadas em um ambiente da versão 10.



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

#### Pacotes do Node.js 10.15

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 Pacotes NPM:
   - [amqplib](https://www.npmjs.com/package/amqplib) - Uma biblioteca para criar clientes AMQP 0-9-1 para o Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - Um módulo Node.js para fazer interface com o serviço Apple Push Notification.
   - [async](https://www.npmjs.com/package/async) - Fornece funções para trabalhar com funções assíncronas.
   - [bent](https://www.npmjs.com/package/bent) - Cliente HTTP funcional para Node.js com async e await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Analise corpos de solicitações recebidas em um middleware antes de seus manipuladores, disponíveis na propriedade req.body.
   - [btoa](https://www.npmjs.com/package/btoa) - Uma porta da função btoa do navegador.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - Esta é a biblioteca oficial Cloudant para Node.js.
   - [commander](https://www.npmjs.com/package/commander) - A solução completa para interfaces da linha de comandos node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Conversor de endereço da IU ou da API do Compose para bancos de dados Scylla.
   - [consul](https://www.npmjs.com/package/consul) - Um cliente para Consul, envolvendo descoberta e configuração de serviço.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Analise o cabeçalho de Cookie e preencha req.cookies com um objeto chaveado pelos nomes de cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - O cliente Elasticsearch oficial de baixo nível para o Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Middleware do manipulador de erros somente para desenvolvimento.
   - [etcd3](https://www.npmjs.com/package/etcd3) - Um cliente de alta qualidade e pronto para produção para a API etcdv3 baseada em Buffer de protocolo.
   - [formidable](https://www.npmjs.com/package/formidable) - Um módulo Node.js para análise de dados de formulário, especialmente uploads de arquivo.
   - [glob](https://www.npmjs.com/package/glob) - Corresponda arquivos usando os padrões usados pelo shell, como estrelas e coisas.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick for Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - Uma interface assíncrona/síncrona para node.js para o IBM DB2 e o IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - O cliente node.js é usado para simplificar a interação com o IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Conversão de codificação de caracteres JS puro
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom é uma implementação JavaScript pura de muitos padrões da web, especialmente os Padrões WHATWG DOM e os HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce) - Biblioteca API do Salesforce para aplicativos JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - Uma implementação de JSON Web Tokens.
   - [lodash](https://www.npmjs.com/package/lodash) - A biblioteca Lodash exportada como módulos Node.js.
   - [marked](https://www.npmjs.com/package/marked) - Um analisador e compilador de redução de preço com todos os recursos, escrito em JavaScript. Construído para ser veloz.
   - [merge](https://www.npmjs.com/package/merge) - Mescle vários objetos em um, criando, opcionalmente, um novo objeto clonado.
   - [moment](https://www.npmjs.com/package/moment) - Uma biblioteca de data JavaScript leve para analisar, validar, manipular e formatar datas.
   - [mongodb](https://www.npmjs.com/package/mongodb) - O driver MongoDB oficial para Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - Esse é um driver node.js para mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - O mustache.js é uma implementação do sistema de modelo mustache em JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - driver couchdb minimalista para Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Envie e-mails por meio do Node.js – muito fácil!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Módulo completo, compatível e bem testado para implementar um Servidor/Provedor OAuth2 com o express no Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - Biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Transforme uma sequência de caminho como /user/:name em uma expressão regular que possa, então, ser usada para corresponder aos caminhos de URL.
   - [pg](https://www.npmjs.com/package/pg) - Cliente PostgreSQL sem bloqueio para node.js. JavaScript puro e ligações opcionais de libpq nativo.
   - [process](https://www.npmjs.com/package/process) - require('process'); como qualquer outro módulo.
   - [pug](https://www.npmjs.com/package/pug) - Implementa a linguagem de modelagem Pug.
   - [redis](https://www.npmjs.com/package/redis) - Esse é um cliente Redis completo e rico em recursos para o Node.js.
   - [request](https://www.npmjs.com/package/request) - A solicitação é projetada para ser a maneira mais simples possível de fazer chamadas HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise) - A 'solicitação' simplificada do cliente de solicitação de HTTP com suporte do Promise. Desenvolvido com Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - O comando UNIX rm -rf para o nó.
   - [semver](https://www.npmjs.com/package/semver) - Versão semântica para Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Fornece suporte de e-mail por meio da API do SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Serialize um erro em um objeto simples.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Middleware do Node.js para atender um favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io) - O Socket.IO permite a comunicação baseada em evento bidirecional em tempo real.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Estrutura de aplicativo em tempo real para o socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - O SuperAgent é uma pequena biblioteca de solicitação de HTTP progressiva do lado do cliente e um módulo Node.js com a mesma API, com muitos recursos clientes HTTP de alto nível.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Pacote que fornece várias ferramentas para integrar e interagir com o Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - Um wrapper para a API do Twilio, relacionado a voz, vídeo e sistema de mensagens.
   - [underscore](https://www.npmjs.com/package/underscore) - O Underscore.js é uma biblioteca de utilitários para JavaScript que fornece suporte para os suspeitos funcionais usuais (cada, mapa, redução, filtro...) sem estender nenhum objeto JavaScript principal.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Analise URLs para parâmetros de caminho mais facilmente do que usando um correspondente de sequência de expressão regular.
   - [uuid](https://www.npmjs.com/package/uuid) - Geração simples e rápida de UUIDS RFC4122.
   - [validator](https://www.npmjs.com/package/validator) - Uma biblioteca de validadores e sanitizantes de sequência.
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - Analise e retorne credenciais de serviço da variável de ambiente VCAP_SERVICES fornecida pelo IBM Cloud.
   - [when](https://www.npmjs.com/package/when) - O When.js é uma implementação promissora e testada de Promises/A+ e when(), incluindo um shim completo do ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston) - Uma biblioteca de criação de log assíncrona de transporte múltiplo para node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs."
   - [ws](https://www.npmjs.com/package/ws) - O ws é uma implementação de cliente e servidor WebSocket simples de usar, extremamente rápida e testada.
   - [xlsx](https://www.npmjs.com/package/xlsx) - Analisador e gravador para vários formatos de planilha.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Conversor de objeto simples XML em JavaScript. Ele suporta conversão bidirecional.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - O node-XMLHttpRequest é um wrapper para o cliente http integrado para emular o objeto XMLHttpRequest do navegador.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Outra biblioteca de extração para o nó.

### Ambiente do Node.js versão 8 com SDKs da IBM
{: #openwhisk_ref_javascript_environments_8}
O ambiente do Node.js versão 8 será usado se o sinalizador `--kind` for especificado explicitamente com um valor de `nodejs:8` ao criar ou atualizar uma ação.

O Node.js versão 8 está no modo de manutenção e está disponível até dezembro de 2019. Consulte o [Planejamento de liberação do Node.js](https://github.com/nodejs/Release).
{: deprecated}

#### Pacotes do Node.js 8.15

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - Uma biblioteca para criar clientes AMQP 0-9-1 para o Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - Um módulo Node.js para fazer interface com o serviço Apple Push Notification.
   - [async](https://www.npmjs.com/package/async) - Fornece funções para trabalhar com funções assíncronas.
   - [bent](https://www.npmjs.com/package/bent) - Cliente HTTP funcional para Node.js com async/await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Analise corpos de solicitações recebidas em um middleware antes de seus manipuladores, disponíveis na propriedade req.body.
   - [btoa](https://www.npmjs.com/package/btoa) - Uma porta da função btoa do navegador.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [cloudant](https://www.npmjs.com/package/cloudant) - Esta é a biblioteca oficial Cloudant para Node.js.
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - Esta é a biblioteca oficial Cloudant para Node.js.
   - [commander](https://www.npmjs.com/package/commander) - A solução completa para interfaces da linha de comandos node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Conversor de endereço da IU ou da API do Compose para bancos de dados Scylla.
   - [consul](https://www.npmjs.com/package/consul) - Um cliente para Consul, envolvendo descoberta e configuração de serviço.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Analise o cabeçalho de Cookie e preencha req.cookies com um objeto chaveado pelos nomes de cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - O cliente Elasticsearch oficial de baixo nível para o Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Middleware do manipulador de erros somente para desenvolvimento.
   - [etcd3](https://www.npmjs.com/package/etcd3) - Um cliente de alta qualidade e pronto para produção para a API etcdv3 baseada em Buffer de protocolo.
   - [formidable](https://www.npmjs.com/package/formidable) - Um módulo Node.js para análise de dados de formulário, especialmente uploads de arquivo.
   - [glob](https://www.npmjs.com/package/glob) - Corresponda arquivos usando os padrões usados pelo shell, como estrelas e coisas.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick for Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - Uma interface assíncrona/síncrona para node.js para o IBM DB2 e o IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - O cliente node.js é usado para simplificar a interação com o IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Conversão de codificação de caracteres JS puro
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom é uma implementação JavaScript pura de muitos padrões da web, especialmente os Padrões WHATWG DOM e os HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce) - Biblioteca API do Salesforce para aplicativos JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - Uma implementação de JSON Web Tokens.
   - [lodash](https://www.npmjs.com/package/lodash) - A biblioteca Lodash exportada como módulos Node.js.
   - [log4js](https://www.npmjs.com/package/log4js) - Esta é uma conversão da estrutura log4js para trabalhar com o Node.
   - [marked](https://www.npmjs.com/package/marked) - Um analisador e compilador de redução de preço com todos os recursos, escrito em JavaScript. Construído para ser veloz.
   - [merge](https://www.npmjs.com/package/merge) - Mescle vários objetos em um, criando, opcionalmente, um novo objeto clonado.
   - [moment](https://www.npmjs.com/package/moment) - Uma biblioteca de data JavaScript leve para analisar, validar, manipular e formatar datas.
   - [mongodb](https://www.npmjs.com/package/mongodb) - O driver MongoDB oficial para Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - Esse é um driver node.js para mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - O mustache.js é uma implementação do sistema de modelo mustache em JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - driver couchdb minimalista para Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Envie e-mails por meio do Node.js – muito fácil!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Módulo completo, compatível e bem testado para implementar um Servidor/Provedor OAuth2 com o express no Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - Biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Transforme uma sequência de caminho como /user/:name em uma expressão regular que possa, então, ser usada para corresponder aos caminhos de URL.
   - [pg](https://www.npmjs.com/package/pg) - Cliente PostgreSQL sem bloqueio para node.js. JavaScript puro e ligações opcionais de libpq nativo.
   - [process](https://www.npmjs.com/package/process) - require('process'); como qualquer outro módulo.
   - [pug](https://www.npmjs.com/package/pug) - Implementa a linguagem de modelagem Pug.
   - [redis](https://www.npmjs.com/package/redis) - Esse é um cliente Redis completo e rico em recursos para o Node.js.
   - [request](https://www.npmjs.com/package/request) - A solicitação é projetada para ser a maneira mais simples possível de fazer chamadas HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise) - A 'solicitação' simplificada do cliente de solicitação de HTTP com suporte do Promise. Desenvolvido com Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - O comando UNIX rm -rf para o nó.
   - [semver](https://www.npmjs.com/package/semver) - Versão semântica para Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Fornece suporte de e-mail por meio da API do SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Serialize um erro em um objeto simples.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Middleware do Node.js para atender um favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io) - O Socket.IO permite a comunicação baseada em evento bidirecional em tempo real.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Estrutura de aplicativo em tempo real para o socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - O SuperAgent é uma pequena biblioteca de solicitação de HTTP progressiva do lado do cliente e um módulo Node.js com a mesma API, com muitos recursos clientes HTTP de alto nível.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Pacote que fornece várias ferramentas para integrar e interagir com o Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - Um wrapper para a API do Twilio, relacionado a voz, vídeo e sistema de mensagens.
   - [underscore](https://www.npmjs.com/package/underscore) - O Underscore.js é uma biblioteca de utilitários para JavaScript que fornece suporte para os suspeitos funcionais usuais (cada, mapa, redução, filtro...) sem estender nenhum objeto JavaScript principal.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Analise URLs para parâmetros de caminho mais facilmente do que usando um correspondente de sequência de expressão regular.
   - [uuid](https://www.npmjs.com/package/uuid) - Geração simples e rápida de UUIDS RFC4122.
   - [validator](https://www.npmjs.com/package/validator) - Uma biblioteca de validadores e sanitizantes de sequência.
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - Analise e retorne credenciais de serviço da variável de ambiente VCAP_SERVICES fornecida pelo IBM Cloud.
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - Biblioteca do cliente Node.js para usar os serviços Watson Developer Cloud, uma coleção de APIs que usam computação cognitiva para resolver problemas complexos.
   - [when](https://www.npmjs.com/package/when) - O When.js é uma implementação promissora e testada de Promises/A+ e when(), incluindo um shim completo do ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston) - Uma biblioteca de criação de log assíncrona de transporte múltiplo para node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs."
   - [ws](https://www.npmjs.com/package/ws) - O ws é uma implementação de cliente e servidor WebSocket simples de usar, extremamente rápida e testada.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Conversor de objeto simples XML em JavaScript. Ele suporta conversão bidirecional.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - O node-XMLHttpRequest é um wrapper para o cliente http integrado para emular o objeto XMLHttpRequest do navegador.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Outra biblioteca de extração para o nó.

As informações detalhadas sobre o ambiente de tempo de execução do Node.js versão 8 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).


## Tempos de execução do Python
{: #openwhisk_ref_python_environments}

É possível escolher entre duas versões de tempo de execução diferentes para ações Python. Por padrão, todas as ações do Python são executadas em um ambiente da versão 2.


### Ações do Python 3.7 (com base no Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

As ações do Python 3.7 são executadas com o Python 3.7.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `--kind python:3.7` ao criar ou atualizar uma ação.

O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso pelas ações Python, além das bibliotecas padrão do Python 3.7.

#### Pacotes Python 3.7.2

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Pacotes do Python:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Clique
 - cloudant
 - constantly
 - Criptografia
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - solicitações
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - Tornado
 - Torcido
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

As informações detalhadas sobre o ambiente de tempo de execução do Python 3.7 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Ações Python 3.6.8 (Baseado em Jessie Debian)
{: #openwhisk_ref_python_environments_3.6}

As ações do Python 3 são executadas com o Python 3.6.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `--kind python:3.6` ao criar ou atualizar uma ação.

O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso por ações do Python, além das bibliotecas padrão do Python 3.6.

#### Pacotes Python 3.6.8

Versão do Python:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Pacotes do Python:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Clique
 - cloudant
 - constantly
 - Criptografia
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - solicitações
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - Tornado
 - Torcido
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

As informações detalhadas sobre o ambiente de tempo de execução do Python 3.6 podem ser localizadas no [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Ações do Python 2

As ações do Python 2 são executadas com Python 2.7.15, a menos que você especifique o sinalizador `--kind` ao criar ou atualizar uma ação.

Ao criar ações do Python usando virtualenv, use a imagem do docker `openwhisk/python2action`.
Os pacotes a seguir estão disponíveis para uso por ações do
Python 2, além da biblioteca padrão do Python 2.7.

#### Pacotes do Python 2

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Clique
 - constantly
 - Criptografia
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - solicitações
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Torcido
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

As informações detalhadas sobre o ambiente de tempo de execução do Python 2 podem ser localizadas no [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).


## Tempo de execução do Swift
{: #swift-actions}

Por padrão, todas as ações Swift são executadas em um ambiente da versão 4.2.

Os tempos de execução de ação do Swift 4.x não incorporam nenhum pacote, siga as instruções para [ações swift empacotadas](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) para incluir dependências usando um Package.swift.

As ações do Swift 4.2 podem usar os pacotes a seguir ao usar o arquivo de origem Swift único:
- Watson Developer Cloud SDK versão 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### SwiftyJSON usando um único arquivo de ação de origem
Se você tiver uma ação que não esteja compilada e usar o pacote **SwiftyJSON**, será necessário pré-compilar sua ação e especificar a versão do SwiftyJSON que você desejar usar para a ação de tipo `swift:4.2`.


## Tempo de execução PHP
{: #openwhisk_ref_php}

Por padrão, todas as ações do PHP são executadas em um ambiente da versão 7.3.

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

## Tempo de execução do Docker
{: #openwhisk_ref_docker}

As ações do Docker executam binário fornecido pelo usuário em um contêiner do Docker. O binário é executado em uma imagem do Docker com base em [python:3.6-alpine](https://hub.docker.com/r/library/python), portanto, o binário deve ser compatível com essa distribuição.

A estrutura básica do Docker é uma maneira conveniente de construir imagens do Docker compatíveis com o OpenWhisk. É possível instalar a estrutura básica com o comando de plug-in da CLI `ibmcloud fn sdk install docker`.

O programa binário principal deve estar localizado em `/action/exec` dentro do contêiner. O executável recebe os argumentos de entrada de uma única sequência de argumentos de linha de comandos, que pode ser desserializada como um objeto `JSON`. Ele deve retornar um resultado usando `stdout` como uma sequência de linha única de `JSON` serializado.

É possível incluir qualquer etapa ou dependência de compilação modificando o `Dockerfile` incluído no `dockerSkeleton`.



## Tempo de execução
{: #runtimes_go}

Por padrão, todas as ações do Go são executadas em um ambiente da versão 1.11.



## Tempo de execução Java
{: #runtimes_java}

Por padrão, todas as ações Java são executadas em um ambiente da versão 8.



## Tempo de execução Ruby
{: #runtimes_ruby}

Por padrão, todas as ações do Ruby são executadas em um ambiente da versão 2.5.



## Tempo de execução do .NET Core
{: #runtimes_dotnet}

Por padrão, todas as ações .NET Core são executadas em um ambiente da versão 2.2.
