---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Getting started
{: #index}

With {{site.data.keyword.openwhisk}} you can use your favorite programming language to write lightweight code that runs app logic in a scalable way. You can run code on-demand with HTTP-based API requests from applications or automatically in response to {{site.data.keyword.Bluemix_notm}} services and third-party events. The Function-as-a-Service (Faas) programming platform is based on the open source project Apache OpenWhisk.
{: shortdesc}

Because {{site.data.keyword.openwhisk_short}} is serverless, you're not limited in the languages that you can use and you don't have to spend time explicitly provisioning backend infrastructure. You can focus on writing app logic instead of worrying about auto-scaling, high availability, updates, or maintenance. Out of the box, IBM provides the hardware, networking, software administration, load balancing, plug-ins, and so on. You just have to bring the code!

Click an option to get started:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Click an icon to get started quickly with {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="/docs/openwhisk/index.html#openwhisk_start_hello_world" alt="Create an action" title="Create an action" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk/bluemix_cli.html" alt="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" title="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk/openwhisk_about.html" alt="See the platform architecture" title="See the platform architecture" shape="rect" coords="326, -10, 448, 218" />
</map>


## Creating an action in the GUI
{: #openwhisk_start_hello_world}

To get started with {{site.data.keyword.openwhisk_short}}, try using the HelloWorld quickstart template.

1.  From the [{{site.data.keyword.Bluemix_notm}} **Catalog** ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/?category=whisk), select Functions.

2. Click **Start Creating** > **Quickstart Templates** and select the **Hello World** template.

3. Create a package for your actions by inputting a unique name in the **Package Name** field.

4. Select a runtime from the drop down in the **Action helloworld** section. You can preview the code for the sample action in each available runtime before you deploy the template.

5. Click **Deploy**. You've created an action. Great job!

6. Run the action by clicking **Invoke**. Invoking an action manually runs the app logic that the action defines. In the **Activations** panel, you can see the "Hello stranger!" greeting produced by the action.

7. Optional: Click **Change Input** to change the action or try your own. Then, click **Invoke** to run the action with your updates. The result is shown in the **Activations** panel. You can repeat this process as many times as you like.

Good work! You created your first action. To clean up this action, click the overflow menu and select **Delete Action**.

## Creating an action in the CLI
{: #openwhisk_start_hello_world_cli}

Get up and running quickly with the HelloWorld JavaScript example code. This example creates a basic `hello` action, which you can manually invoke to run its app logic.

1. [Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk/bluemix_cli.html).

2. Save the following code to a file named **hello.js**.

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
    function main(params) {
        var name = params.name || 'World';
        return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Create the `hello` action.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Invoke the action without passing any parameters.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Invoke the action again, passing the name parameter, to test the app logic.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Good work! You created your first action. To clean up this action, run `ibmcloud fn action delete hello`.

## What's next?
{: #next-steps}

So you've completed your first template deploy, where do you go from here? You can:

* Become familiar with the [terminology](/docs/openwhisk/openwhisk_about.html#technology).
* Get started with [your own actions](/docs/openwhisk/openwhisk_actions.html).
* Learn about organizing actions in [packages](/docs/openwhisk/openwhisk_packages.html).
* Advanced option: create a [serverless REST API](/docs/openwhisk/openwhisk_apigateway.html).
