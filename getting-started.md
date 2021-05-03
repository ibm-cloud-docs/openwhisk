---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-03"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions, actions, OpenWhisk

subcollection: openwhisk

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

# Getting started with IBM {{site.data.keyword.openwhisk_short}}
{: #getting-started}

With {{site.data.keyword.openwhisk}}, you can use your favorite programming language to write lightweight code that runs app logic in a scalable way. You can run code on-demand with HTTP-based API requests from applications or run code in response to {{site.data.keyword.cloud_notm}} services and third-party events. The Function-as-a-Service (FaaS) programming platform is based on the open source project Apache OpenWhisk.
{: shortdesc}

## Working with actions
{: #gs_actions}

With {{site.data.keyword.openwhisk}}, you can create stateless code snippets, called actions, that are set to perform one specific task. To learn more about actions and other Functions terms, see [terminology](/docs/openwhisk?topic=openwhisk-about).
{:shortdesc}

Click an option to get started.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Click an icon to get started quickly with {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Create an action" title="Create an action" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=openwhisk-cli_install" alt="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" title="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=openwhisk-about" alt="See the platform architecture" title="See the platform architecture" shape="rect" coords="326, -10, 448, 218" />
</map>

## Creating an action in the console
{: #gs_hello_world}

To get started with {{site.data.keyword.openwhisk_short}}, try creating the Hello World quickstart template.

1. Create an [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external} account or log in to an existing account.

2. Navigate to the [{{site.data.keyword.openwhisk_short}} dashboard](https://cloud.ibm.com/functions){: external}.

3. Select a [namespace](/docs/openwhisk?topic=openwhisk-namespaces).

4. Click **Start Creating** > **Quickstart Templates** and select the **Hello World** template.

5. Create a package for your action by entering a unique name in the **Package Name** field.

6. Select a runtime for your action. You can preview the code for the sample action in each available runtime before you deploy the template.

7. Click **Deploy**. You created an action. Great job!

8. Run the action by clicking **Invoke**. Invoking an action manually runs the app logic that the action defines. In the **Activations** panel, you can see the "Hello stranger!" greeting produced by the action.

9. Optional: Click **Change Input** to change the action or try your own.

  a. Paste the following code into the **Change Input** box and change the name value.
  ```json
  { "name": "xxxx" }
  ```
  {: codeblock}
  
  b. Then, click **Invoke** to run the action with your updates. The result is shown in the **Activations** panel. You can repeat this process as many times as you like.

Good work! You created your first action. To clean up this action, click the overflow menu and select **Delete Action**.

## Creating an action in the CLI
{: #gs_hello_world_cli}

Get up and running quickly with the [Hello World JavaScript](/docs/openwhisk?topic=openwhisk-prep#prep_js) example code. This example creates a basic `hello` action, which you can manually invoke to run its app logic.

## What's next in {{site.data.keyword.openwhisk_short}}?
{: #gs_next_steps}

So you completed your first template deployment, where do you go from here?

* Become familiar with the [terminology](/docs/openwhisk?topic=openwhisk-about).
* Get started with [your own actions](/docs/openwhisk?topic=openwhisk-actions).
* Learn about organizing actions in [packages](/docs/openwhisk?topic=openwhisk-pkg_ov).
* Advanced option - Create a [serverless REST API](/docs/openwhisk?topic=openwhisk-apigateway).
