---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: runtimes, support, functions

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


# Tempos de execução
{: #runtimes}
Seus apps podem ser codificados e executados em linguagens de programação, como JavaScript ou Python. Muitos tempos de execução estão disponíveis por padrão com o {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Visualize os tempos de execução disponíveis para o IBM Cloud Functions em cada região. Os links a seguir retornam uma resposta JSON. A seção `runtimes` da resposta contém o conjunto de tempos de execução disponíveis. A seção `image` contém o nome da imagem de tempo de execução no [Docker Hub](https://hub.docker.com/){: external} e a tag que é usada.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}


Os exemplos a seguir apontam para as imagens `ibmfunctions/action-nodejs-v10` e `openwhisk/nodejs8action`.
As tags podem ser números de versão como `1.9.0`ou o formato abreviado de um hash de confirmação de git, como `b99d71e`.

Campos de imagem de exemplo.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Os tempos de execução são atualizados regularmente. Essas atualizações incluem correções de segurança e atualizações de versão secundária para os pacotes dentro dos tempos de execução. Atualizações de versão secundária podem apresentar quebras de compatibilidade com versões anteriores. As atualizações de tempo de execução podem afetar suas ações. Deve-se migrar ações que estejam executando um tempo de execução para uma versão mais recente, atualizando-a.

Os apps que são executados em tempos de execução descontinuados não podem ser concluídos com êxito até que o tempo de execução seja atualizado para um suportado. Ao resolver problemas de uma ação com falha para identificar se um tempo de execução foi descontinuado, verifique `deprecated=true` na resposta da consulta. Para atualizar o tempo de execução, consulte [mudando o tempo de execução da ação](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

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

Por padrão, todas as ações do Node.js são executadas em um ambiente da versão 10.
{: note}

As ações do JavaScript podem ser executadas no Node.js versão 8 ou 10. O Node.js versão 8 está no modo de manutenção e está disponível até dezembro de 2019. Consulte o [Planejamento de liberação do Node.js](https://github.com/nodejs/Release){: external}. 
{: deprecated}

| Tempo de execução | Descrição | Log de mudanças |
| --- | --- | --- |
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} | Por padrão, todas as ações do Node.js são executadas em um ambiente da versão 10. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} | O Node.js versão 8.15.0 será usado se o sinalizador `--kind` for especificado explicitamente com um valor de `nodejs:8` ao criar ou atualizar uma ação. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |


### Migrando do nodeJS 8 para nodeJS 10

| Pacote | Detalhes |
| --- | --- |
| `ibm_db` | O pacote NPM `ibm_db` não está disponível em `nodejs:10`. O pacote `ibm_db` não suporta Node.js 10. É possível rastrear o progresso [neste problema](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external}. |
| `cloudant` | O pacote NPM `cloudant` não está disponível em `nodejs:10`. O pacote foi descontinuado. É necessário usar o pacote NPM oficial [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} v3.0.0 ao importar o módulo Node.js (isto é, `require('@cloudant/cloudant')`), além disso [v3.x só retorna Promessas](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | O pacote NPM `cradle` não está disponível em `nodejs:10`. |
| `log4js` | O pacote NPM `log4js` não está disponível em `nodejs:10`. É possível rastrear [esse problema](https://github.com/log4js-node/log4js-node/issues/805){: external}. |
| `watson-developer-cloud` | O pacote NPM `watson-developer-cloud` não está disponível em `nodejs:10`. É possível rastrear o progresso na nova versão [neste problema](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external}. |


### Pacotes do Node.js

| Pacotes do Node.js 10.15 | Descrição |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Uma biblioteca para fazer os clientes do Advanced Message Queuing Protocol 0-9-1 para o Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Um módulo Node.js para fazer interface com o serviço Apple Push Notification. |
| [`async`](https://www.npmjs.com/package/async){: external} | Fornece funções para trabalhar com funções assíncronas. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Cliente HTTP funcional para Node.js com async e await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Analise os corpos da solicitação recebida em um middleware antes de seus manipuladores, disponíveis sob a propriedade req.body. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Uma porta da função `btoa` do navegador. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | Driver DataStax Node.js para Apache Cassandra. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external} | A biblioteca oficial Cloudant para Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | A solução completa para interfaces da linha de comandos Node.js. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Tradutor de endereço da IU do Compose ou API para bancos de dados Scylla. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Um cliente para o Consul, envolvendo a descoberta de serviço e a configuração. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Analisar cabeçalho de Cookie e preencher o req.cookies com um objeto com chave pelos nomes de cookie. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | O cliente ElasticSearch oficial de nível baixo para Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware do manipulador de erros somente de desenvolvimento. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Um cliente de alta qualidade e pronto para produção para a API etcdv3 baseada em Buffer de protocolo. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Um módulo Node.js para análise sintática de dados de formulário, especialmente uploads de arquivo. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Corresponder arquivos usando os padrões que o shell usa, como estrelas e tal. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick e ImageMagick para o Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Uma interface assíncrona/síncrona para Node.js para o IBM DB2 e o IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | O cliente Node.js que é usado para simplificar a interação com o IBM Watson Internet of Things Platform. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Conversão de codificação de caracteres de Pure JS. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` é uma implementação puramente JavaScript de muitos padrões da web, especialmente os Padrões DOM e HTML do Grupo de Trabalho de Tecnologia de Aplicativo de Hipertexto da Web. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Biblioteca da API Salesforce para aplicativos JavaScript. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Uma implementação de Tokens da Web JSON. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | A biblioteca `lodash` é exportada como módulos Node.js. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Um analisador de redução de preço com todos os recursos e o compilador que é gravado em JavaScript. Construído para ser veloz. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Mescle vários objetos em um, criando, opcionalmente, um novo objeto clonado.
| [`moment`](https://www.npmjs.com/package/moment){: external} | Uma biblioteca de data JavaScript leve para analisar, validar, manipular e formatar datas. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | O driver MongoDB oficial para Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Um driver Node.js para MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | Mustache.js é uma implementação do sistema de modelo mustache em JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Driver CouchDB minimalista para Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Enviar e-mails do Node.js - fácil como bolo! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Módulo concluído, em conformidade e testado para implementar um OAuth2 Server/Provider com o expresso em Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | Biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Transforme uma sequência de caminho como `/user/:name` em uma expressão regular, que pode então ser usada para corresponder aos caminhos de URL. |
| [`pg`](https://www.npmjs.com/package/pg){: external} | Cliente PostgreSQL sem bloqueio para Node.js. Pure JavaScript e ligações nativas `libpq` opcionais. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`; como qualquer outro módulo. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implementa a linguagem de modelagem Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Um cliente Redis completo e rico em recursos para Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Fazer chamadas HTTP. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | A 'solicitação' do cliente de solicitação de HTTP simplificado com suporte de Promessa. Desenvolvido com Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | O comando do UNIX rm -rf para o node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Versão semântica para nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Fornece suporte por e-mail por meio da API SendGrid. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Serializar um erro em um objeto simples. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Middleware do Node.js para atender a um favicon.
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` permite a comunicação baseada em evento bidirecional em tempo real. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Estrutura de aplicativo em tempo real para `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` é uma pequena biblioteca progressiva de solicitação de HTTP do lado do cliente e o módulo Node.js com a mesma API, com muitos recursos cliente HTTP de alto nível. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Pacote que fornece várias ferramentas para integrar e interagir com o Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Um wrapper para a API do Twilio, relacionado à voz, a um vídeo e a um sistema de mensagens. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js é uma biblioteca de utilitários para JavaScript que suporta os suspeitos funcionais comuns (cada um, mapa, redução, filtro...) sem estender nenhum objeto JavaScript principal. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Analisar URLs para parâmetros de caminho mais facilmente do que o uso de um correspondente de sequência de expressão regular. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Geração simples e rápida de UUIDS RFC4122. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Uma biblioteca de validadores de sequência e sanitizadores. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Analisar e retornar credenciais de serviço da variável de ambiente VCAP_SERVICES que o IBM Cloud fornece. |
| [`when`](https://www.npmjs.com/package/when){: external} | O When.js é uma implementação de `Promises/A+` e `when()` testado e comprovado, incluindo um shim completo do ES6 Promise. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Uma biblioteca de criação de log assíncrona de vários transportes para o Node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` é uma implementação do cliente e do servidor WebSocket simples de usar, intensamente rápida e completamente testada. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Analisador e gravador para vários formatos de planilha. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | XML simples para conversor de objeto JavaScript. Ele suporta conversão bidirecional. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | node-XMLHttpRequest é um wrapper para o cliente http integrado para emular o objeto XMLHttpRequest do navegador. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Outra biblioteca de extração para o node. |
{: caption="Tabela 1. Pacotes do Node.js 10.15." caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

| Pacotes do Node.js 8.15 | Descrição |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Uma biblioteca para fazer os clientes do Advanced Message Queuing Protocol 0-9-1 para o Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Um módulo Node.js para fazer interface com o serviço Apple Push Notification.
| [`async`](https://www.npmjs.com/package/async){: external} | Fornece funções para trabalhar com funções assíncronas. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Cliente HTTP funcional para Node.js com async-await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Analise os corpos da solicitação recebida em um middleware antes de seus manipuladores, disponíveis sob a propriedade req.body. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Uma porta da função `btoa` do navegador. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | Driver DataStax Node.js para Apache Cassandra. |
| [`cloudant`](https://www.npmjs.com/package/cloudant){: external} | A biblioteca oficial Cloudant para Node.js. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external} | A biblioteca oficial Cloudant para Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | A solução completa para interfaces da linha de comandos Node.js. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Tradutor de endereço da IU do Compose ou API para bancos de dados Scylla. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Um cliente para o Consul, envolvendo a descoberta de serviço e a configuração. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Analisar cabeçalho de Cookie e preencher o req.cookies com um objeto com chave pelos nomes de cookie. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | O cliente ElasticSearch oficial de nível baixo para Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware do manipulador de erros somente de desenvolvimento. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Um cliente de alta qualidade e pronto para produção para a API etcdv3 baseada em Buffer de protocolo. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Um módulo Node.js para análise sintática de dados de formulário, especialmente uploads de arquivo. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Corresponder arquivos usando os padrões que o shell usa, como estrelas e tal. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick e ImageMagick para o Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js. |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Uma interface assíncrona-síncrona para Node.js para o IBM DB2 e IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | O cliente Node.js é usado para simplificar a interação com o IBM Watson Internet of Things Platform. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Conversão de codificação de caracteres de Pure JS. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` é uma implementação puramente JavaScript de muitos padrões da web, especialmente os Padrões DOM e HTML do Grupo de Trabalho de Tecnologia de Aplicativo de Hipertexto da Web. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Biblioteca da API Salesforce para aplicativos JavaScript. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Uma implementação de Tokens da Web JSON. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | A biblioteca `lodash` que é exportada como módulos Node.js. |
| [`log4js`](https://www.npmjs.com/package/log4js){: external} | Uma conversão da estrutura log4js para trabalhar com o Node. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Um analisador de redução de preço com todos os recursos e o compilador que é gravado em JavaScript. Construído para ser veloz. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Mescle vários objetos em um, criando, opcionalmente, um novo objeto clonado. |
| [`moment`](https://www.npmjs.com/package/moment){: external} | Uma biblioteca de data JavaScript leve para analisar, validar, manipular e formatar datas. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | O driver MongoDB oficial para Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Um driver Node.js para MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | mustache.js é uma implementação do sistema de modelo mustache em JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Driver CouchDB minimalista para Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Enviar e-mails do Node.js - fácil como bolo! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Módulo concluído, em conformidade e testado para implementar um OAuth2 Server-Provider com expresso em Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | Biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Transforme uma sequência de caminho como `/user/:name` em uma expressão regular, que pode então ser usada para corresponder aos caminhos de URL. | [`pg`](https://www.npmjs.com/package/pg){: external} | Cliente PostgreSQL sem bloqueio para Node.js. Pure JavaScript e ligações nativas `libpq` opcionais. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`; como qualquer outro módulo. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implementa a linguagem de modelagem Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Um cliente Redis completo e rico em recursos para Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Fazer chamadas HTTP. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | A 'solicitação' do cliente de solicitação de HTTP simplificado com suporte de Promessa. Desenvolvido com Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | O comando do UNIX rm -rf para o node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Versão semântica para nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Fornece suporte por e-mail por meio da API SendGrid. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Serializar um erro em um objeto simples. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Middleware do Node.js para atender a um favicon. |
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` permite a comunicação baseada em evento bidirecional em tempo real. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Estrutura de aplicativo em tempo real para `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` é uma pequena biblioteca progressiva de solicitação de HTTP do lado do cliente e o módulo Node.js com a mesma API, com muitos recursos cliente HTTP de alto nível. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Pacote que fornece várias ferramentas para integrar e interagir com o Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Um wrapper para a API do Twilio, relacionado à voz, a um vídeo e a um sistema de mensagens. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js é uma biblioteca de utilitários para JavaScript que suporta os suspeitos funcionais comuns (cada um, mapa, redução, filtro...) sem estender nenhum objeto JavaScript principal. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Analisar URLs para parâmetros de caminho mais facilmente do que o uso de um correspondente de sequência de expressão regular. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Geração simples e rápida de UUIDS RFC4122. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Uma biblioteca de validadores de sequência e sanitizadores. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Analisar e retornar credenciais de serviço da variável de ambiente VCAP_SERVICES que o IBM Cloud fornece. |
| [`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external} | A biblioteca do cliente Node.js para usar os serviços Watson Developer Cloud, uma coleção de APIs que usam computação cognitiva para resolver problemas complexos. |
| [`when`](https://www.npmjs.com/package/when){: external} | O When.js é uma implementação de `Promises/A+` e `when()` testado e comprovado, incluindo um shim completo do ES6 Promise. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Uma biblioteca de criação de log assíncrona de vários transportes para o Node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` é uma implementação do cliente e do servidor WebSocket simples de usar, intensamente rápida e completamente testada. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | XML simples para conversor de objeto JavaScript. Ele suporta conversão bidirecional. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` é um wrapper para o cliente http integrado para emular o objeto XMLHttpRequest do navegador. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Outra biblioteca de extração para o node. |
{: caption="Tabela 2. Pacotes do Node.js 8.15." caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Tempos de execução do Python
{: #openwhisk_ref_python_environments}

Por padrão, todas as ações do Python são executadas em um ambiente da versão 2.7.15.
{: note}

| Versão do Python | Descrição | Log de mudanças |
| --- | --- | --- |
| 2.7.15 | Por padrão, todas as ações do Python são executadas em um ambiente da versão 2.7.15, a menos que você especifique o sinalizador `--kind` quando criar ou atualizar uma ação. Ao criar ações python usando `virtualenv`, use a imagem do docker `openwhisk/python2action`. Os pacotes a seguir estão disponíveis para uso por ações do
Python 2, além da biblioteca padrão do Python 2.7. | [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}. |
| [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | As ações do Python 3 são executadas com o Python 3.6.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `--kind python:3.6` ao criar ou atualizar uma ação. O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso por ações do Python, além das bibliotecas padrão do Python 3.6. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | As ações de Python 3.7 (baseadas em Debian Stretch) são executadas com o Python 3.7.x. Para usar esse tempo de execução, especifique o parâmetro da CLI `--kind python:3.7` ao criar ou atualizar uma ação. O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso pelas ações Python, além das bibliotecas padrão do Python 3.7. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Pacotes Python

| Pacotes do Python 2.7.15 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `enum34` |
| `Flask` |
| `functools32` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `idna` |
| `incremental` |
| `ipaddress` |
| `itsdangerous` |
| `Jinja2` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `parsel` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pyOpenSSL` |
| `python-dateutil` |
| `queuelib` |
| `requests` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `Twisted` |
| `urllib3` |
| `virtualenv=` |
| `w3lib` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabela 1. Pacotes do Python 2.7.15." caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Pacotes Python 3.6.8 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `autobahn` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six=` |
| `soupsieve` |
| `tornado` |
| `Twisted` |
| `txaio` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabela 2. Pacotes do Python 3.6.8." caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Pacotes Python 3.7.2 | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `etcd3` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `grpcio` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `protobuf` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `soupsieve` |
| `tenacity` |
| `tornado` |
| `Twisted` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `websocket-client` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabela 3. Pacotes do Python 3.7.2." caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Tempo de execução do Swift
{: #swift-actions}

Por padrão, todas as ações Swift são executadas em um ambiente da versão 4.2.
{: note}

Os tempos de execução de ação do Swift 4.x não incorporam nenhum pacote. Siga as instruções para [ações swift empacotadas](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) para incluir dependências usando um Package.swift.

As ações do Swift 4.2 poderão usar os pacotes a seguir quando você estiver usando um único arquivo de origem Swift:
- Watson Developer Cloud SDK versão 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### <ph class="ignoreSpelling">SwiftyJSON</ph> usando um único arquivo de ação de origem
Se você tiver uma ação que não é compilada e usar o pacote `SwiftyJSON`, será necessário pré-compilar sua ação e especificar a versão de `SwiftyJSON` que você deseja usar para a ação de tipo `swift:4.2`.


## Tempo de execução PHP
{: #openwhisk_ref_php}

Por padrão, todas as ações do PHP são executadas em um ambiente da versão 7.3.
{: note}

As extensões PHP a seguir estão disponíveis além daquelas padrão:

- `bcmath`
- `ondulação`
- `gd`
- `intl`
- `mbstring`
- `mysqli`
- `pdo_mysql`
- `pdo_pgsql`
- `pdo_sqlite`
- `soap`
- `zip`

## Tempo de execução do Docker
{: #openwhisk_ref_docker}

As ações do Docker executam um executável fornecido pelo usuário em um contêiner do Docker. O executável é executado em uma imagem do Docker com base em [python:3.6-alpine](https://hub.docker.com/_/python){: external}, portanto, o executável deve ser compatível com essa distribuição.

A estrutura básica do Docker é uma maneira conveniente de construir imagens do Docker compatíveis com o OpenWhisk. É possível instalar a estrutura básica com o comando de plug-in da CLI `ibmcloud fn sdk install docker`.

O programa executável principal deve estar localizado em `/action/exec` dentro do contêiner. O executável recebe os argumentos de entrada de uma única sequência de argumentos de linha de comandos, que pode ser desserializada como um objeto `JSON`. Ele deve retornar um resultado usando `stdout` como uma sequência de linha única de `JSON` serializado.

É possível incluir qualquer etapa ou dependência de compilação modificando o `Dockerfile` incluído no `dockerSkeleton`.

## Mais suporte de tempo de execução

| Tempo de execução |
| --- | --- | 
| Por padrão, todas as ações do Go são executadas em um ambiente da versão 1.11. |
{: caption="Tabela 1. Go." caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Tempo de execução |
| --- | --- | 
| Por padrão, todas as ações Java são executadas em um ambiente da versão 8. |
{: caption="Tabela 2. Java." caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Tempo de execução |
| --- |
| Por padrão, todas as ações do Ruby são executadas em um ambiente da versão 2.5. |
{: caption="Tabela 3. Ruby." caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Tempo de execução |
| --- |
| Por padrão, todas as ações .NET Core são executadas em um ambiente da versão 2.2. |
{: caption="Tabela 4. .NET Core." caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}




