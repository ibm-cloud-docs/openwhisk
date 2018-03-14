---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-15"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Detalhes do sistema {{site.data.keyword.openwhisk_short}}
{: #openwhisk_reference}

As seções a seguir fornecem mais detalhes sobre o sistema {{site.data.keyword.openwhisk}}.
{: shortdesc}

## entidades do {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Namespaces e pacotes
{: #openwhisk_entities_namespaces}

As ações, os acionadores e as regras do {{site.data.keyword.openwhisk_short}} pertencem a um namespace e às vezes um pacote.

Os pacotes podem conter ações e feeds. Um pacote não pode conter outro pacote, portanto, o aninhamento de pacote não é permitido. Além disso, as entidades não precisam estar contidas em um pacote.

No {{site.data.keyword.Bluemix_notm}}, um par de organização + espaço corresponde a um namespace do {{site.data.keyword.openwhisk_short}}. Por exemplo, a organização `BobsOrg` e o espaço `dev` corresponderiam ao namespace do {{site.data.keyword.openwhisk_short}} `/BobsOrg_dev`.

É possível criar seus próprios namespaces se estiver autorizado a fazer isso. O namespace `/whisk.system` é reservado para entidades distribuídas com o sistema {{site.data.keyword.openwhisk_short}}.


### Nomes Completos
{: #openwhisk_entities_fullyqual}

O nome completo de uma entidade é `/namespaceName[/packageName]/entityName`. Observe que `/` é usado para delimitar namespaces, pacotes e entidades. Além disso, os namespaces devem ser prefixados com um `/`.

Por conveniência, o namespace poderá ser deixado desativado se for o *namespace padrão* do usuário.

Por exemplo, considere um usuário cujo namespace padrão seja `/myOrg`. Seguem
exemplos de nomes completos de diversas entidades e seus aliases.

| Nome Completo | Alias | Namespace | Pacote | Nome |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

É possível usar esse esquema de nomenclatura ao usar a CLI do {{site.data.keyword.openwhisk_short}}, entre outros locais.

### Nomes de entidades
{: #openwhisk_entities_names}

Os nomes de todas as entidades, incluindo ações, acionadores, regras, pacotes e namespaces, são uma sequência de caracteres que seguem o formato a seguir:

* O primeiro caractere deve ser um caractere alfanumérico ou um sublinhado.
* Os caracteres subsequentes podem ser alfanuméricos, espaços ou qualquer um dos valores a seguir: `_`, `@`, `.`, `-`.
* O último caractere não pode ser um espaço.

Mais precisamente, um nome deve corresponder à expressão regular a seguir (expressa com sintaxe Java metacaractere): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semântica de ação
{: #openwhisk_semantics}

As seções a seguir descrevem detalhes sobre ações do {{site.data.keyword.openwhisk_short}}.

### Statelessness
{: #openwhisk_semantics_stateless}

As implementações de ação são stateless ou *idempotente*. Embora o sistema não cumpra essa propriedade, não é garantido que qualquer estado mantido por uma ação esteja disponível ao longo de chamadas.

Além disso, múltiplas instanciações de uma ação podem existir, com cada instanciação com seu próprio estado. Uma chamada de ação pode ser despachada para qualquer uma dessas instanciações.

### Saída e entrada de chamada
{: #openwhisk_semantics_invocationio}

A entrada e a saída de uma ação é um dicionário de pares chave-valor. A chave é uma sequência e o valor um valor JSON válido.

### Ordenação de chamada de ações
{: #openwhisk_ordering}

As chamadas de uma ação não são ordenadas. Se o usuário chama uma ação duas vezes na linha de comandos ou na API de REST, a segunda chamada poderá ser executada antes da primeira. Se as ações têm efeitos colaterais, elas podem ser observadas em qualquer ordem.

Além disso, não é garantido que as Ações sejam executadas automaticamente. Duas ações podem ser executadas simultaneamente e seus efeitos secundários podem ser intercalados. O OpenWhisk não assegura qualquer
modelo de consistência simultâneo específico para efeitos colaterais. Quaisquer efeitos colaterais de simultaneidade são dependentes da implementação.

### Garantias de execução de ação
{: #openwhisk_atmostonce}

Quando uma solicitação de chamada é recebida, o sistema registra a solicitação e despacha uma ativação.

O sistema retorna um ID de ativação (com uma chamada sem bloqueio) que confirma que ele foi recebido.
Se uma falha de rede ou outra falha intervém antes de você receber uma resposta de HTTP, é possível que o {{site.data.keyword.openwhisk_short}} recebeu e processou a solicitação.

O sistema tenta chamar a ação uma vez, resultando em um dos quatro resultados a seguir:
- *sucesso*: a chamada de ação foi concluída com êxito.
- *erro de aplicativo*: a chamada de ação foi bem-sucedida, mas a ação retornou um valor de erro de propósito, por exemplo, porque uma condição prévia dos argumentos não foi atendida.
- *erro de desenvolvedor de ação*: a ação foi chamada, mas foi concluída de forma anormal, por exemplo a ação não detectou uma exceção ou houve um erro de sintaxe.
- *erro interno do whisk*: o sistema não pôde chamar a ação.
O resultado é registrado no campo `status` do registro de ativação, como documento em uma seção a seguir.

Para cada chamada recebida com sucesso e pela qual o usuário pode ser faturado tem um registro de ativação.

Quando o resultado é *erro de desenvolvedor de ação*, a ação pode ser parcialmente executada e gerar efeitos colaterais visíveis externos. É de responsabilidade do usuário verificar se tais efeitos colaterais aconteceram e emitir uma lógica de nova tentativa, se desejado. Alguns *erros internos do whisk* indicam que uma ação começa a ser executada, mas falha antes de a ação registrar a conclusão.

## Registro de ativação
{: #openwhisk_ref_activation}

Cada chamada de ação e disparo de acionador resulta em um registro de ativação.

Um registro de ativação contém os campos a seguir:

- *activationId*: o ID de ativação.
- *start* e *end*: registros de data e hora de início e término da ativação. Os valores estão no [formato de hora do UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* e `name`: o namespace e o nome da entidade.
- *logs*: uma matriz de sequências com os logs que são produzidos pela ação durante sua ativação. Cada elemento de matriz corresponde a uma saída de linha para `stdout` ou `stderr` pela ação e inclui o horário e o fluxo da saída do log. A estrutura é a seguinte: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: um dicionário que define as chaves `success`, `status` e `result`:
  - *status*: o resultado de ativação, que pode ser um dos valores a seguir: "sucesso", "erro de aplicativo", "erro de desenvolvedor da ação", "erro interno do whisk".
  - *success*: é `true` se e somente se o status for `"sucesso"`
- *result*: um dicionário que contém o resultado da ativação. Se a ativação foi bem-sucedida, o resultado conterá o valor que é retornado pela ação. Se a ativação foi mal
sucedida, `result` terá a chave `error`, geralmente com
uma explicação da falha.


## Ações do JavaScript
{: #openwhisk_ref_javascript}

### Protótipo de função
{: #openwhisk_ref_javascript_fnproto}

As ações do JavaScript do {{site.data.keyword.openwhisk_short}} são executadas em um tempo de execução do Node.js.

As ações que são gravadas em JavaScript devem ser confinadas a um único arquivo. O arquivo pode conter múltiplas funções, mas por convenção, uma função chamada `main` deve existir e é aquela chamada quando a ação é chamada. Por exemplo, o exemplo a seguir mostra uma ação com múltiplas funções.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

Os parâmetros de entrada de ação são passados como um objeto JSON como um parâmetro para a função `main`. O resultado de uma ativação bem-sucedida também é um objeto JSON, mas é retornado de forma diferente dependendo de se a ação é síncrona ou assíncrona conforme descrito na seção a seguir.


### Comportamento síncrono e assíncrono
{: #openwhisk_ref_javascript_synchasynch}

É comum que as funções JavaScript continuem a execução em uma função de retorno de
chamada mesmo após um retorno. Para acomodar esse comportamento, uma ativação de uma ação do JavaScript pode ser *síncrona* ou *assíncrona*.

Uma ativação da ação do JavaScript será **síncrona** se a função principal sair sob uma das condições a seguir:

- A função principal sai sem executar uma instrução `return`.
- A função principal sai executando uma instrução `return` que retorna qualquer valor *exceto* uma Promessa.

Veja o exemplo a seguir de uma ação síncrona:

```javascript
// an Action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

A ativação de uma ação do JavaScript será **assíncrona** se a função principal sair retornando uma promessa. Nesse caso, o sistema assume que a ação ainda está em execução até que a promessa será cumprida ou rejeitada.
Comece
instanciando um novo objeto Promessa e passando a ele uma função de retorno de chamada. O
retorno de chamada aceita dois argumentos, resolver e rejeitar, que são ambos funções. Todos
os códigos assíncronos vão dentro desse retorno de chamada.

No exemplo a seguir, é possível ver como cumprir uma promessa chamando a função de resolução.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

Este exemplo mostra como rejeitar uma promessa chamando a função de rejeição.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

É possível que uma ação seja síncrona em algumas entradas e assíncrona em outras, conforme mostrado no exemplo a seguir. 
```javascript
  function main(params) {
      if (params.payload) {
         // asynchronous activation
         return new Promise(function(resolve, reject) {
                setTimeout(function() {
                  resolve({ done: true });
                }, 100);
             })
      } else {
         // synchronous activation
         return {done: true};
      }
  }
```
{: codeblock}

Independentemente se uma ativação é síncrona ou assíncrona, a chamada da ação pode ser bloqueio ou sem bloqueio.

### Objeto whisk global JavaScript global removido

O objeto global `whisk` foi removido; migre suas ações do nodejs para usar métodos alternativos.
Para as funções `whisk.invoke()` e `whisk.trigger()`, use a biblioteca do cliente já instalada [openwhisk](https://www.npmjs.com/package/openwhisk).
Para o `whisk.getAuthKey()`, é possível obter o valor da chave API da variável de ambiente `__OW_API_KEY`.
Para o `whisk.error()`, é possível retornar uma promessa rejeitada (ou seja, Promise.reject).

### Ambientes de tempo de execução do JavaScript
{: #openwhisk_ref_javascript_environments}

As ações do JavaScript podem ser executadas no Node.js versão 6 ou Node.js versão 8.
Atualmente, as ações são executadas por padrão em um ambiente do Node.js versão 6.11.4.  

### Ambiente do Node.js versão 6
{: #openwhisk_ref_javascript_environments_6}
O ambiente do Node.js 6.12.2 será usado se a sinalização `--kind` for explicitamente especificada com um valor de `nodejs:6` ao criar ou atualizar uma ação.

Os pacotes a seguir estão disponíveis para serem usados no ambiente do Node.js 6.12.2:

- [apn v2.1.2](https://www.npmjs.com/package/apn) - um módulo do Node.js para fazer interface com o serviço Apple Push Notification.
- [async v2.1.4](https://www.npmjs.com/package/async) - fornece funções para trabalhar com funções assíncronas.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - uma porta da função btoa do navegador.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - implementação rápida, flexível e lean do jQuery principal projetado especificamente para o servidor.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - a biblioteca oficial do Cloudant para o Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - a solução completa para interfaces da linha de comandos do Node.js.
- [consul v0.27.0](https://www.npmjs.com/package/consul) - um cliente para Consul, envolvendo descoberta de serviço e configuração.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - analisar o cabeçalho de cookie e preencher o req.cookies com um objeto chaveado pelos nomes dos cookies.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - um cliente CouchDB de armazenamento em cache de alto nível para Node.js.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - middleware de manipulador de erros somente para desenvolvimento.
- [glob v7.1.1](https://www.npmjs.com/package/glob) - corresponder arquivos usando padrões que o shell usa, como estrelas e tal.
- [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick para o Nó.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - a biblioteca Lodash que é exportada como módulos Node.js.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - uma conversão da estrutura log4js projetada para funcionar com o Nó. 
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - conversão de codificação de caracteres do Pure JS
- [marked v0.3.6](https://www.npmjs.com/package/marked) - um analisador e compilador de redução de preço com recursos integrais, que é gravado em JavaScript. Construído para ser veloz.
- [merge v1.2.0](https://www.npmjs.com/package/merge) - mesclar múltiplos objetos em um para criar um novo objeto clonado. 
- [moment v2.17.0](https://www.npmjs.com/package/moment) - uma biblioteca de data JavaScript leve para análise, validação, manipulação e formatação de datas.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - o driver MongoDB oficial para Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js é uma implementação do sistema de template mustache no JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - driver couchdb minimalista para Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - UUID descontinuado empacotado. 
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - enviar e-mails do Node.js – simples e fácil!
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - módulo completo, compatível e bem testado para implementar um Servidor/Provedor OAuth2 com Express no Node.js.
- [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud é uma biblioteca padrão para Node.js que abstrai as diferenças entre múltiplos provedores em nuvem.
- [process v0.11.9](https://www.npmjs.com/package/process) - requer('processo'); apenas como qualquer outro módulo.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - implementa a linguagem de modelagem Pug.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - um cliente Redis completo e rico em recursos para Node.js. 
- [request v2.79.0](https://www.npmjs.com/package/request) a solicitação é a maneira mais simples possível de fazer chamadas HTTP.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - a 'solicitação' do cliente de solicitação de HTTP simplificada com suporte de Promessa. Desenvolvido com Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - o comando UNIX rm -rf para o nó.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - suporta a versão semântica.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - fornece suporte de e-mail por meio da API SendGrid.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - o middleware do Node.js para entregar uma favicon.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - o Socket.IO permite comunicação baseada em evento bidirecional em tempo real.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - suporte do lado do cliente para Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - o SuperAgent é uma biblioteca pequena e progressiva de solicitação de HTTP do lado do cliente e um módulo do Node.js com a mesma API, usando muitos recursos clientes HTTP de alto nível.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - ferramentas que estão relacionadas ao trabalho com o Swagger, uma maneira de documentar APIs.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - um criador de arquivo e diretório temporário simples para node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - um wrapper para a API do Twilio, relacionado à voz, vídeo e sistema de mensagens.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js é uma biblioteca de múltiplas utilidades para JavaScript que suporta os suspeitos funcionais habituais (cada, mapa, reduzir, filtro...) sem estender nenhum objeto JavaScript principal.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - geração simples e rápida de UUIDS de RFC4122.
- [validator v6.1.0](https://www.npmjs.com/package/validator) - uma biblioteca de validadores e limpadores de sequência.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - a biblioteca do cliente Node.js para usar os serviços do Watson Developer Cloud, uma coleção de APIs que usam computação cognitiva para resolver problemas complexos.
- [when v3.7.7](https://www.npmjs.com/package/when) - o When.js é uma implementação sólida e testada em batalha do Promises/A+ e when(), incluindo um shim ES6 Promise completo.
- [winston v2.3.0](https://www.npmjs.com/package/winston) - uma biblioteca de criação de log assíncrono multitransporte para node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - o ws é uma implementação de cliente e servidor do WebSocket simples de usar, rápida e completamente testada.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - conversor simples de objeto XML para JavaScript. Ele suporta conversão bidirecional.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest é um wrapper para o cliente HTTP integrado a fim de emular o objeto XMLHttpRequest do navegador.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - outra biblioteca de descompactação de arquivo ZIP para o nó. Para compactação.

### Ambiente do Node.js versão 8
{: #openwhisk_ref_javascript_environments_8}
O ambiente do Node.js versão 8.9.3 será usado se a sinalização `--kind` for explicitamente especificada com um valor de `nodejs:8` ao criar ou atualizar uma ação.

Os pacotes a seguir são pré-instalados no ambiente do Node.js versão 8.9.3:

  - [apn v2.1.5](https://www.npmjs.com/package/apn) - um módulo do Node.js para fazer interface com o serviço Apple Push Notification.
  - [async v2.6.0](https://www.npmjs.com/package/async) - fornece funções para trabalhar com funções assíncronas.
  - [bent v1.1.0](https://www.npmjs.com/package/btoa) - uma porta da função btoa do navegador.
  - [btoa v1.1.2](https://www.npmjs.com/package/btoa) - uma porta da função btoa do navegador.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - esta é a biblioteca oficial do Cloudant para Node.js.
  - [commander v2.12.2](https://www.npmjs.com/package/commander) - a solução completa para interfaces da linha de comandos do Node.js.
  - [consul v0.30.0](https://www.npmjs.com/package/consul) - um cliente para Consul, envolvendo descoberta de serviço e configuração.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - analisar o cabeçalho de cookie e preencher o req.cookies com um objeto chaveado pelos nomes dos cookies.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - um cliente CouchDB de armazenamento em cache de alto nível para Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - middleware de manipulador de erros somente para desenvolvimento.
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - corresponder arquivos usando padrões que o shell usa, como estrelas e tal.
  - [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick para o Nó.
  - [ibm-cos-sdk v1.1.1](https://www.npmjs.com/package/ibm-cos-sdk) - SDK do IBM Cloud Object Storage para Node.js
  - [ibm_db v2.2.1](https://www.npmjs.com/package/ibm_db) - uma interface assíncrono/síncrona para node.js para o IBM DB2 e o IBM Informix. 
  - [lodash v4.17.4](https://www.npmjs.com/package/lodash) - a biblioteca Lodash exportada como módulos Node.js.
  - [log4js v2.3.12](https://www.npmjs.com/package/log4js) - uma conversão da estrutura log4js projetada para funcionar com o Nó.
  - [iconv-lite v0.4.19](https://www.npmjs.com/package/iconv-lite) - conversão de codificação de caracteres do Pure JS
  - [marked v0.3.7](https://www.npmjs.com/package/marked) - um analisador e compilador de redução de preço com recursos integrais, que é gravado em JavaScript. Construído para ser veloz.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - mesclar múltiplos objetos em um, criando opcionalmente um novo objeto clonado.
  - [moment v2.19.3](https://www.npmjs.com/package/moment) - uma biblioteca de data JavaScript leve para análise, validação, manipulação e formatação de datas.
  - [mongodb v2.2.33](https://www.npmjs.com/package/mongodb) - o driver MongoDB oficial para Node.js.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js é uma implementação do sistema de template mustache no JavaScript.
  - [nano v6.4.2](https://www.npmjs.com/package/nano) - driver couchdb minimalista para Node.js.
  - [nodemailer v4.4.1](https://www.npmjs.com/package/nodemailer) - enviar e-mails do Node.js – simples e fácil!
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - módulo completo, compatível e bem testado para implementar um Servidor/Provedor OAuth2 com Express no Node.js.
  - [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - biblioteca do cliente JavaScript para a plataforma OpenWhisk. Fornece um wrapper em torno das APIs do OpenWhisk.
  - [process v0.11.10](https://www.npmjs.com/package/process) - requer('processo'); apenas como qualquer outro módulo.
  - [pug v2.0.0-rc.4](https://www.npmjs.com/package/pug) - implementa a linguagem de modelagem Pug.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - este é um cliente Redis completo e rico em recursos para Node.js.
  - [request v2.83.0](https://www.npmjs.com/package/request) a solicitação é a maneira mais simples possível de fazer chamadas HTTP.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - a 'solicitação' do cliente de solicitação de HTTP simplificada com suporte de Promessa. Desenvolvido com Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - o comando UNIX rm -rf para o nó.
  - [semver v5.4.1](https://www.npmjs.com/package/semver) - suporta a versão semântica.
  - [@sendgrid/mail@6.1.4](https://www.npmjs.com/package/@sendgrid/mail) - fornece suporte de e-mail por meio da API SendGrid.
  - [serve-favicon v2.4.5](https://www.npmjs.com/package/serve-favicon) - o middleware do Node.js para entregar uma favicon.
  - [socket.io v2.0.4](https://www.npmjs.com/package/socket.io) - o Socket.IO permite comunicação baseada em evento bidirecional em tempo real.
  - [socket.io-client v2.0.4](https://www.npmjs.com/package/socket.io-client) - suporte do lado do cliente para Socket.IO.
  - [superagent v3.8.2](https://www.npmjs.com/package/superagent) - o SuperAgent é uma biblioteca pequena e progressiva de solicitação de HTTP do lado do cliente e um módulo do Node.js com a mesma API, usando muitos recursos clientes HTTP de alto nível.
  - [swagger-tools v0.10.3](https://www.npmjs.com/package/swagger-tools) - ferramentas que estão relacionadas ao trabalho com o Swagger, uma maneira de documentar APIs.
  - [tmp v0.0.33](https://www.npmjs.com/package/tmp) - um criador de arquivo e diretório temporário simples para node.js.
  - [twilio v3.10.1](https://www.npmjs.com/package/twilio) - um wrapper para a API do Twilio, relacionado à voz, vídeo e sistema de mensagens.
  - [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js é uma biblioteca de múltiplas utilidades para JavaScript que suporta os suspeitos funcionais habituais (cada, mapa, reduzir, filtro...) sem estender nenhum objeto JavaScript principal.
  - [uuid v3.1.0](https://www.npmjs.com/package/uuid) - geração simples e rápida de UUIDS de RFC4122.
  - [validator v9.2.0](https://www.npmjs.com/package/validator) - uma biblioteca de validadores e limpadores de sequência.
  - [watson-developer-cloud v3.0.2](https://www.npmjs.com/package/watson-developer-cloud) - a biblioteca do cliente Node.js para usar os serviços do Watson Developer Cloud, uma coleção de APIs que usam computação cognitiva para resolver problemas complexos.
  - [when v3.7.8](https://www.npmjs.com/package/when) - o When.js é uma implementação sólida e testada em batalha do Promises/A+ e when(), incluindo um shim ES6 Promise completo.
  - [winston v2.4.0](https://www.npmjs.com/package/winston) - uma biblioteca de criação de log assíncrono multitransporte para node.js. "CALMA, WINSTON! ... Eu coloco isso nos logs."
  - [ws v3.3.1](https://www.npmjs.com/package/ws) - o ws é uma implementação de cliente e servidor do WebSocket simples de usar, rápida e completamente testada.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - conversor simples de objeto XML para JavaScript. Ele suporta conversão bidirecional.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest é um wrapper para o cliente HTTP integrado a fim de emular o objeto XMLHttpRequest do navegador.
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - outra biblioteca de descompactação de arquivo ZIP para o nó. Para compactação

### Empacotando pacotes npm com suas ações
Para quaisquer pacotes `npm` que não estão pré-instalados no ambiente do Node.js, é possível empacotá-los como dependências ao criar ou atualizar a sua ação.

Para obter mais informações, veja [Empacotando uma ação como um módulo do Node.js](./openwhisk_actions.html#openwhisk_js_packaged_action) ou [Empacotando uma ação como um único pacote configurável](./openwhisk_actions.html#openwhisk_js_webpack_action).


## Ambientes de tempo de execução do Python
{: #openwhisk_ref_python_environments}

O OpenWhisk suporta a execução de ações do Python usando duas versões de tempo de execução diferentes.


### Ações do Python 3 (com base em Jessie)
{: #openwhisk_ref_python_environments_jessie}

As ações do Python 3 são executadas com o Python 3.6.4. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind python-jessie:3` quando criar ou atualizar uma ação.
Ao criar ações do Python usando virtualenv, use a imagem do docker `ibmfunctions/action-python-v3`.
O tempo de execução contém pacotes SDK para serviços do IBM Cloud disponíveis para uso por ações do Python, além das bibliotecas padrão do Python 3.6.

Versão do Python:
- [3.6.4](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Pacotes Python:
- asn1crypto (0.24.0)
- attrs (17.4.0)
- Automat (0.6.0)
- beautifulsoup4 (4.6.0)
- certifi (2017.11.5)
- cffi (1.11.4)
- chardet (3.0.4)
- click (6.7)
- cloudant (2.7.0)
- constantly (15.1.0)
- cryptography (2.1.4)
- cssselect (1.0.3)
- docutils (0.14)
- Flask (0.12.2)
- gevent (1.2.2)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- hyperlink (17.3.1)
- ibm-cos-sdk (2.0.0)
- ibm-db (2.0.8a0)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.10)
- jmespath (0.9.3)
- kafka-python (1.3.5)
- lxml (4.1.1)
- MarkupSafe (1.0)
- numpy (1.14.0)
- pandas (0.22.0)
- parsel (1.3.1)
- pip (9.0.1)
- pyasn1 (0.4.2)
- pyasn1-modules (0.2.1)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.5.0)
- pysolr (3.7.0)
- python-dateutil (2.6.1)
- pytz (2017.3)
- queuelib (1.4.2)
- requests (2.18.4)
- scikit-learn (0.19.1)
- scipy (1.0.0)
- Scrapy (1.5.0)
- service-identity (17.0.0)
- setuptools (38.4.0)
- simplejson (3.13.2)
- six (1.11.0)
- Twisted (17.9.0)
- urllib3 (1.22)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- watson-developer-cloud (1.0.2)
- Werkzeug (0.14.1)
- wheel (0.30.0)
- zope.interface (4.4.3)


### Ações do Python 3 (com base em Alpine)
{: #openwhisk_ref_python_environments_alpine}

As ações do Python 3 são executadas com o Python 3.6.1. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind python:3` quando criar ou atualizar uma ação.
Ao criar ações do Python usando virtualenv, use a imagem do docker `openwhisk/python3action`.
Os pacotes a seguir estão disponíveis para uso por ações do Python, além das bibliotecas padrão do Python 3.6.

- asn1crypto (0.23.0)
- attrs (17.3.0)
- Automat (0.6.0)
- beautifulsoup4 (4.5.3)
- cffi (1.11.2)
- click (6.7)
- constantly (15.1.0)
- cryptography (2.1.3)
- cssselect (1.0.1)
- Flask (0.12)
- gevent (1.2.1)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.4)
- lxml (3.7.3)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.5)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.6.0)
- queuelib (1.4.2)
- requests (2.13.0)
- Scrapy (1.3.3)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.10.0)
- six (1.11.0)
- Twisted (17.1.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)


### Ações do Python 2

As ações do Python 2 são executadas com o Python 2.7.12, que é o tempo de execução padrão para ações do Python. A menos que você especifique a sinalização `--kind` ao criar ou atualizar uma ação. Para selecionar explicitamente esse tempo de
execução, use `--kind python:2`. 
Ao criar ações do Python usando virtualenv, use a imagem do docker `openwhisk/python2action`.
Os pacotes a seguir estão disponíveis para uso por ações do Python 2, além da biblioteca padrão do Python 2.7.

- asn1crypto (0.23.0)
- attrs (17.2.0)
- beautifulsoup4 (4.5.1)
- cffi (1.11.1)
- click (6.7)
- cryptography (2.0.3)
- cssselect (1.0.1)
- enum34 (1.1.6)
- Flask (0.11.1)
- gevent (1.1.2)
- greenlet (0.4.12)
- httplib2 (0.9.2)
- idna (2.6)
- ipaddress (1.0.18)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.1)
- lxml (3.6.4)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.4)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.5.3)
- queuelib (1.4.2)
- requests (2.11.1)
- Scrapy (1.1.2)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.8.2)
- six (1.11.0)
- Twisted (16.4.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)

## Ações do Swift
{: #openwhisk_ref_swift3}

### Swift 3
As ações do Swift 3 são executadas com o Swift 3.1.1 `--kind swift:3.1.1`. Sempre especifique o tipo `swift:3.1.1` porque versões anteriores do Swift não são suportadas.

Deve-se migrar todas as ações do Swift para usar o tipo `swift:3.1.1`. Como uma melhor prática, forneça sempre o tipo específico ao criar ou atualizar ações.
{: tip}

As ações do Swift 3.1.1 podem usar os pacotes a seguir:
- KituraNet versão 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON versão 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK versão 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

## Ações do PHP
{: #openwhisk_ref_php}

As ações do PHP são executadas com o PHP 7.1. Para usar esse tempo de execução, especifique o parâmetro da CLI `wsk` `--kind php:7.1` ao criar ou atualizar uma ação. Esse comportamento é o padrão quando você cria uma ação com um arquivo que tenha uma extensão `.php`.

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

### Pacotes do Composer
Os pacotes do Composer a seguir também estão disponíveis:

- guzzlehttp/guzzle       v6.3.0
- ramsey/uuid             v3.6.1

## Ações do Docker
{: #openwhisk_ref_docker}

As ações do Docker executam um binário fornecido pelo usuário em um contêiner do Docker. O binário é executado em uma imagem do Docker com base no
[python:2.7.12-alpine](https://hub.docker.com/r/library/python), de maneira que o binário deve ser compatível com essa distribuição.

A estrutura básica do Docker é uma maneira conveniente de construir imagens do Docker compatíveis com o OpenWhisk. É possível instalar a estrutura básica com o comando da CLI `wsk sdk install docker`.

O programa binário principal deve estar localizado em `/action/exec` dentro do contêiner. O executável recebe os argumentos de entrada por meio de uma única sequência de argumentos de linha de comandos, que pode ser desserializada como um objeto `JSON`. Ele deve retornar um resultado por meio de `stdout` como sequência de linha única de `JSON` serializado.

É possível incluir qualquer etapa ou dependência de compilação modificando o `Dockerfile` incluído no `dockerSkeleton`.

## API da REST
{: #openwhisk_ref_restapi}
Informações sobre a API de REST podem ser localizadas [aqui](openwhisk_rest_api.html)

## Limites do sistema
{: #openwhisk_syslimits}

### Ações
O {{site.data.keyword.openwhisk_short}} tem alguns limites do sistema, incluindo o quanto de memória uma ação pode usar e quantas chamadas de ação são permitidas por minuto.

A tabela a seguir lista os limites padrão para ações.

| Limit | Descrição | Padrão | Mín. | Máxima | 
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | O tamanho máximo do código de ação em MB. | 48 | 1 | 48 |
| [simultâneo](openwhisk_reference.html#openwhisk_syslimits_concurrent) | Não mais que N ativações podem ser enviadas por namespace em execução ou enfileiradas para execução. | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | Um contêiner não tem permissão para gravar mais de N MB para stdout. | 10 | 0 | 10 |
| [memória](openwhisk_reference.html#openwhisk_syslimits_memory) | Um contêiner não tem permissão para alocar mais de N MB de memória. | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | Não mais que N ativações podem ser enviadas por namespace por minuto. | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | O número máximo de arquivos abertos para uma ação do Docker. | 64 | 0 | 64 |
| [parâmetros](openwhisk_reference.html#openwhisk_syslimits_parameters) | O tamanho máximo dos parâmetros que podem ser anexados em MB. | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | O número máximo de processos disponíveis para uma ação do Docker. | 512 | 0 | 512 |
| [resultado](openwhisk_reference.html#openwhisk_syslimits_result) | O tamanho máximo do resultado da chamada de ação em MB. | 1 | 0 | 1 |
| [tempo limite](openwhisk_reference.html#openwhisk_syslimits_timeout) | Um contêiner não tem permissão para executar por mais de N milissegundos. | 60000 | 100 | 300000 |

### Aumentando limites fixos
{: #increase_fixed_limit}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados se um business case puder justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://console.bluemix.net/openwhisk/) da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Funções** para a Área de suporte técnico.

#### codeSize (MB) (Fixo: 48 MB)
{: #openwhisk_syslimits_codesize}
* O tamanho máximo do código para a ação é 48 MB.
* É recomendado que uma ação do JavaScript use uma ferramenta para concatenar todos os códigos-fonte, que incluem dependências, em um único arquivo em pacote configurável.
* Esse limite é fixo e não pode ser mudado.

#### simultâneo (Fixo: 1000*)
{: #openwhisk_syslimits_concurrent}
* O número de ativações que estão em execução ou enfileiradas para execução para um namespace não pode exceder 1000.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](openwhisk_reference.html#increase_fixed_limit) para obter instruções detalhadas sobre como aumentar esse limite.

#### logs (MB) (Padrão: 10 MB)
{: #openwhisk_syslimits_logs}
* O limite de log N está no intervalo [0 MB..10 MB] e é configurado por ação.
* Um usuário pode mudar o limite de log de ações quando uma ação é criada ou atualizada.
* Os logs que excedem o limite configurado são truncados, assim quaisquer novas entradas de log são ignoradas e um aviso é incluído como a última saída da ativação para indicar que a ativação excedeu o limite de log configurado.

#### memória (MB) (Padrão: 256 MB)
{: #openwhisk_syslimits_memory}
* O limite de memória M está no intervalo de [128 MB..512 MB ] e é configurado por ação em MB.
* Um usuário pode mudar o limite de memória quando uma ação é criada.
* Um contêiner não pode usar mais memória do que está alocada pelo limite.

#### minuteRate (Fixo: 5000*)
{: #openwhisk_syslimits_minuterate}
* O limite de taxa N é configurado para 5000 e limita o número de chamadas de ação em janelas de 1 minuto.
* Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](openwhisk_reference.html#increase_fixed_limit) para obter instruções detalhadas sobre como aumentar esse limite.

#### openulimit (Fixo: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* O número máximo de arquivos abertos para uma ação do Docker é 1024 (para os limites máximo e flexível).
* Esse limite é fixo e não pode ser mudado.
* O comando de execução do docker usa o argumento `--ulimit nofile=1024:1024`.
* Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parâmetros (Fixo: 1 MB)
{: #openwhisk_syslimits_parameters}
* O limite de tamanho para o total de parâmetros na criação ou atualização de uma Ação/Pacote/Acionador é 1 MB.
* Uma entidade com parâmetros muito grandes é rejeitada em tentar criar ou atualizar isso.
* Esse limite é fixo e não pode ser mudado.

#### proculimit (Fixo: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* O número máximo de processos disponíveis para o contêiner de ação do Docker é 1024.
* Esse limite é fixo e não pode ser mudado.
* O comando de execução do docker usa o argumento `--pids-limit 1024`.
* Para obter mais informações, consulte a documentação de referência da linha de comandos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### resultado (Fixo: 1 MB)
{: #openwhisk_syslimits_result}
* O tamanho de saída máximo do resultado da chamada de ação em MB.
* Esse limite é fixo e não pode ser mudado.

#### tempo limite (ms) (Padrão: 60s)
{: #openwhisk_syslimits_timeout}
* O limite do tempo limite N está no intervalo [100 ms..300000 ms] e é configurado por ação em milissegundos.
* Um usuário pode mudar o limite do tempo limite quando uma ação é criada.
* Um contêiner executado mais de N milissegundos é finalizado.

### disparos

Os acionadores estão sujeitos a uma taxa de disparo por minuto, conforme documentado na tabela a seguir.

| Limit | Descrição | Padrão | Mín. | Máxima | 
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | Não mais que N acionadores podem ser disparados por namespace por minuto. | 5000* | 5000* | 5000* |

### Aumentando limites fixos
{: #increase_fixed_tlimit}

Os valores limite que terminam com um (*) são fixos, mas podem ser aumentados se um business case puder justificar valores limite de segurança mais altos. Se você desejar aumentar o valor limite, entre em contato com o suporte IBM abrindo um chamado diretamente do [{{site.data.keyword.openwhisk_short}}console da web](https://console.bluemix.net/openwhisk/) da IBM.
  1. Selecione **Support**
  2. Selecione **Incluir chamado** no menu suspenso.
  3. Selecione **Técnico** para o tipo de chamado.
  4. Selecione **Funções** para a Área de suporte técnico.

#### minuteRate (Fixo: 5000*)
{: #openwhisk_syslimits_tminuterate}

* O limite de taxa N é configurado para 5000 e limita o número de acionadores que um usuário pode disparar em janelas de 1 minuto.
* Um usuário não pode mudar o limite do acionador quando um acionador é criado.
* Uma CLI ou chamada API que exceder esse limite receberá um código de erro correspondente ao código de status de HTTP `429: TOO MANY REQUESTS`.
* Esse valor limite é fixo, mas pode ser aumentado se um business case puder justificar valores limite de segurança mais altos. Verifique a seção [Aumentando limites fixos](openwhisk_reference.html#increase_fixed_tlimit) para obter instruções detalhadas sobre como aumentar esse limite.
