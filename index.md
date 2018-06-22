---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Getting started

{{site.data.keyword.openwhisk}} is a polyglot Functions-as-a-Service (FaaS) programming platform based on Apache OpenWhisk. {{site.data.keyword.openwhisk_short}} enables developers to write lightweight code called Actions that scalably execute app logic. You can set up Actions to occur on-demand based on HTTP-based API requests from web apps or mobile apps, or automatically in response to event-based requests from {{site.data.keyword.Bluemix_notm}} services and third-party events.
{: shortdesc}

Because {{site.data.keyword.openwhisk_short}} is a serverless, event-driven platform, you don't need to explicitly provision servers. Developers working with microservices, mobile, IoT, and many other apps can focus on writing app logic instead of worrying about auto-scaling, high availability, updates, and maintenance. Out of the box auto-scaling and load balancing means that you don't have to manually configure clusters, http plugins, and so on! IBM takes care of all of the hardware, networking, and software administration. All you have to do is provide the code.

Click an option to get started:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Click an icon to get started quickly with {{site.data.keyword.openswhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Create an Action" title="Create an Action" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" title="Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="See the platform architecture" title="See the platform architecture" shape="rect" coords="326, -10, 448, 218" />
</map>


## Creating an Action in the GUI
{: #openwhisk_start_hello_world}

To get started with {{site.data.keyword.openwhisk_short}}, try using the HelloWorld Quickstart Template.

1.  From the [{{site.data.keyword.Bluemix_notm}} **Catalog** ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/?category=whisk) **Functions** category, click Functions.

2. Click **Start Creating** > **Quickstart Templates** and select the **Hello World** template.

5. Check out the code for the Action, and then create the Action by clicking **Deploy**. You now have an Action called `hello`.

6. Run the Action by clicking **Invoke**. Invoking an Action manually executes the app logic that the Action defines. In the **Activations** panel, you can see the "Hello stranger!" greeting produced by the Action.

Good work! You created your first Action. To clean up this Action, click on the overflow menu and select **Delete Action**.

## Creating an Action in the CLI
{: #openwhisk_start_hello_world_cli}

Get up and running quickly with the HelloWorld JavaScript example code. This example creates a basic `hello` Action which you can manually invoke to run its app logic.

1. [Set up the {{site.data.keyword.openwhisk_short}} CLI plug-in](bluemix_cli.html).

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

3. Create the `hello` Action.
    ```
    ibmcloud wsk action create hello hello.js
    ```
    {: pre}

4. Invoke the Action without passing any parameters.
    ```
    ibmcloud wsk action invoke hello --blocking --result
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Invoke the Action again, passing the name parameter, to test the app logic.
    ```
    ibmcloud wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Good work! You created your first Action. To clean up this Action, run `ibmcloud wsk action delete hello`.

**What's next?**
* [Try out the alarm service example to invoke the **hello** Action every time a periodic event is generated.](./openwhisk_packages.html#openwhisk_package_trigger)
* [Create a serverless REST API.](openwhisk_apigateway.html)
* [Check out pre-installed Action packages for {{site.data.keyword.Bluemix_notm}} services like Cloudant.](cloudant_actions.html)
