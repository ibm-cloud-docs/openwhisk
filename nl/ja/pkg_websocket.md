---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: websocket, functions, actions, package

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

# WebSocket
{: #pkg_websocket}

事前インストール済みの `/whisk.system/websocket` パッケージを利用して、WebSocket にメッセージを簡単に送ることができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | パッケージ | uri | WebSocket との通信のためのユーティリティー |
| `/whisk.system/websocket/send` | アクション | uri、payload | ペイロードを WebSocket URI に送信 |

多数のメッセージを同じ WebSocket URI に送信する予定の場合、`uri` 値を指定してパッケージ・バインディングを作成することをお勧めします。 バインディングを使用すると、`send` アクションを使用するたびに値を指定する必要がありません。

## WebSocket にメッセージを送信

`/whisk.system/websocket/send` アクションは、WebSocket URI にペイロードを送信します。 パラメーターは次のとおりです。

- `uri`: WebSocket サーバーの URI (例: ws://mywebsockethost:80)。
- `payload`: WebSocket に送信したいメッセージ。

