---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: serverless, framework, getting started

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 無伺服器架構
{: #openwhisk_goserverless}

[無伺服器架構](https://serverless.com/)是用於建置無伺服器應用程式的開放程式碼架構。開發人員可以使用簡單的資訊清單檔來定義無伺服器函數，並將其連接至事件來源，然後宣告其應用程式所需的雲端服務。此架構會處理如何將這些無伺服器應用程式部署至雲端提供者。它也容許開發人員監視正式作業環境中的服務、實施更新項目，以及協助除錯問題。它也會有協力廠商外掛程式的活躍生態系統，以擴充架構的功能。OpenWhisk 是驅動「無伺服器架構」的技術。
{:shortdesc}

OpenWhisk 具有[無伺服器架構的專屬提供者外掛程式](https://github.com/serverless/serverless-openwhisk)。使用「無伺服器架構」的開發人員可以選擇將其應用程式部署至任何 OpenWhisk 平台實例（在 {{site.data.keyword.Bluemix_notm}} 上或者其他雲端或專用環境中進行管理）。多提供者支援也表示在平台之間移動應用程式較為簡單，讓開發人員可以開發多雲端無伺服器應用程式。

## 開始使用
{: #openwhisk_goserverless_starting}

官方無伺服器架構 [OpenWhisk 入門手冊](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/)。
* 此手冊涵蓋安裝、開發工作流程、最佳作法、建置及部署工作中 OpenWhisk 應用程式的逐步指南等等。

觀賞[無伺服器架構及 OpenWhisk](https://youtu.be/GJY10W98Itc)，此視訊說明如何搭配使用「無伺服器架構」與 OpenWhisk 提供者外掛程式。

## 文件
{: #openwhisk_goserverless_docs}

[在這裡可以找到](https://serverless.com/framework/docs/providers/openwhisk/)如何搭配使用 OpenWhisk 與「無伺服器架構」的最新文件。

## 範例
{: #openwhisk_goserverless_samples}
[「無伺服器架構」範例 GitHub 儲存庫](https://github.com/serverless/examples)現在具備 OpenWhisk，可顯示如何建置 HTTP API、以 Cron 為基礎的排程器、鏈結函數等等作業。

