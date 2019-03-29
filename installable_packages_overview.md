---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: packages, installable packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using installable packages
{: #installable-packages-overview}

Installable packages represent a new design for managing and interacting with packages within {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Overview
{: #overview}

**What are installable packages?**

Packages are bundled sets of related actions and feeds. Each package is designed for specific interaction with services and event providers. Installable packages are packages that are available for you to choose, install, and edit based on your needs.

**How are installable packages different from pre-installed packages?**

[Pre-installed packages](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#browse-packages) are automatically registered within {{site.data.keyword.openwhisk_short}} in the `/whisk.system` namespace. To store credentials or other parameters in a pre-installed package, you must create [package bindings](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_package_bind).

Installable packages do not reside within the {{site.data.keyword.openwhisk_short}} system. Instead, installable packages are externally housed in individual Github repositories. You can install these packages directly into your own namespace by using the `ibmcloud fn deploy` command, and can give a package a custom name. Because the package is installed into your own namespace, you can modify the actions and feeds in the package as needed.

## Installing packages using wskDeploy
{: #installing}

Before you begin:

  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

To install a package:

1. Clone the package repo. Package repos can be found on the individual pages for each package in this documentation set.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Navigate to the package directory that contains a `manifest.yaml` or `manifest.yml` file. The `manifest.yaml` file specifies the overall structure of the package, including the package and actions to be installed into your namespace and any metadata that must be included with the `ibmcloud fn deploy` command. To learn more about `manifest.yaml` files, see the [wskdeploy documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Deploy the package. Some packages require certain environment variables to enable the package to function properly.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Example using the {{site.data.keyword.cos_full_notm}} package
{: #example}

To see an example of how to install a package, check out the [{{site.data.keyword.cos_short}} package](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions). {{site.data.keyword.cos_full}} is a service that allows users to store all types of files, such as images, videos, music, and text. To interact with the files, a Cloud-based datastore of key/value pairs are stored in a bucket. So, to use the [{{site.data.keyword.cos_short}} package](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions), you must first create an {{site.data.keyword.cos_short}} service instance, and then create a bucket. The bucket is used as an environment variable that is required to install this package.

After creating the service instance and bucket, installing the package requires the following commands:

1. Clone the package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to the package directory that contains the `manifest.yaml`. In this example, the Node.js runtime version of the {{site.data.keyword.cos_short}} package is used.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package, using your bucket as an environment variable. The dependency on the `PACKAGE_NAME` environment variable allows you to give this package a custom name.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}
