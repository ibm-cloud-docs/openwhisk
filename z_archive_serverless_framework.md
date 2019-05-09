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

# Serverless Framework
{: #openwhisk_goserverless}

The [Serverless Framework](https://serverless.com/) is an open-source framework for building serverless applications. Using a simple manifest file, developers can define serverless functions, connect them to event sources and declare cloud services that are needed by their application. The framework handles deploying these serverless applications to the cloud providers. It also allows developers to monitor services in production, roll-out updates, and assist debugging issues. It also has a vibrant ecosystem of third-party plugins to extend the functionality of the framework. OpenWhisk is the technology that drives the Serverless Framework.
{:shortdesc}

OpenWhisk has [its own provider plugin for Serverless Framework](https://github.com/serverless/serverless-openwhisk). Developers that use the Serverless Framework can choose to deploy their applications to any OpenWhisk platform instance (hosted on {{site.data.keyword.Bluemix_notm}}, or other cloud or private). Multi-provider support also that means moving an application between platforms is much easier, allowing developers to develop multi-cloud serverless applications.

## Getting Started
{: #openwhisk_goserverless_starting}

Official Serverless Framework [Getting Started Guide for OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/).
* A guide that covers installation, development workflow, best practices, step by step guide to building and deploying a working OpenWhisk application, and more.

Watch [The Serverless Framework and OpenWhisk](https://youtu.be/GJY10W98Itc), a video that explains how to use the Serverless Framework with the OpenWhisk provider plug-in.

## Documentation
{: #openwhisk_goserverless_docs}

Latest documentation on how to use OpenWhisk with the Serverless Framework [can be found here](https://serverless.com/framework/docs/providers/openwhisk/).

## Samples
{: #openwhisk_goserverless_samples}
[Serverless Framework examples GitHub repository](https://github.com/serverless/examples) now features OpenWhisk showing you how to build HTTP APIs, cron-based schedulers, chaining functions and much more.

