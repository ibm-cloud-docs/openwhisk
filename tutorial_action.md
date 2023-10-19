---

copyright:
  years: 2023
lastupdated: "2023-10-19"

keywords: actions, functions, serverless, javascript, node, node.js

subcollection: openwhisk

content-type: tutorial
completion-time: 10m 

---

{{site.data.keyword.attribute-definition-list}}

# Calling an action from another action
{: #tutorial_action}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

In this tutorial, learn how to call an action from another action by creating a custom sequence in {{site.data.keyword.openwhisk}}. This tutorial uses Node.js 10, but the general principles can be applied to other runtimes. 
{: shortdesc}

This tutorial uses {{site.data.keyword.openwhisk_short}} to build a custom sequence within another Node.js action to model a typical scenario, such as getting data from {{site.data.keyword.cos_full_notm}}, retrieving related data from {{site.data.keyword.cloudant}} database, and then putting the results back into {{site.data.keyword.cos_full_notm}}. To accomplish this task, first create an {{site.data.keyword.cos_full_notm}} action, then an {{site.data.keyword.cloudant}} database action, and finally a main action that calls both of them.

Note that this tutorial is a simulation; it does not connect to {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.cloudant}}. This tutorial does, however, run successfully to show you how to call an action from another action. You can use this tutorial as a building block to create something new. Because access to both {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.cloudant}} is asynchronous, to simulate asynchronous code, the actions use a timeout to complete these actions at a later point in time. This simulation is important to verify whether the custom sequence action code, which uses the `openwhisk` SDK, is using promises properly.

The code for the actions also includes some basic error handling code, including explanations. Note that, when you run these actions, you might not ever see the error handling code in this example. However, you might find this code useful if you choose to adapt the code examples.

Before you begin

Select a namespace to contain your {{site.data.keyword.openwhisk_short}} entities from the [console](https://cloud.ibm.com/functions){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=openwhisk-namespaces).

## Create the `cos-access` action
{: #tutorial_action_cos-action}
{: step}

The `cos-access` action is a Node.js program that simulates code to access {{site.data.keyword.cos_full_notm}} by setting a timeout that resolves after 10 seconds.

1. Go to the [Create page](https://cloud.ibm.com/functions/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Select **Action**.

3. Create the `cos-access` action:

    1. Name your action `cos-access`.

    2. Click **Create Package**. Name your package `action-tutorial` and click **Create**.

    3. Select Node.js 10 for the runtime.

    4. Click **Create**.

    5. Paste in the following code example:

        ```javascript
        /**
        *
        * main() will be run when you invoke this action
        *
        * @param Cloud Functions actions accept a single parameter, which must be a JSON object.
        *
        * @return The output of this action, which must be a JSON object.
        *
        */
        function main(params) {
        	return new Promise((resolve, reject) => {
	            // simulate a COS access by resolving after 10s
	            setTimeout(() => {
	            console.log('fake COS bucket access done. Resolving Promise...');
	            resolve({ cos_message: 'SUCCESS'});
	            }, 10000);
	        });
        }
        ```
        {: codeblock}

    6. Click **Save**. 

    7. Test your action by clicking **Invoke** and waiting for the following output to display.

        **Example output**

        ```sh
        Results:
        {
            "cos_message": "SUCCESS"
        }

        Logs: 
        [
            "2020-04-21T01:51:49.464941Z    stdout: fake COS bucket access done. Resolving Promise..."
        ]
        ```
        {: screen}

Your `cos-access` action is ready!

## Create the `db-access` action
{: #tutorial_action_db-access}
{: step}

The `db-access` action is a Node.js program that simulates code to access {{site.data.keyword.cloudant}} database by setting a timeout that resolves after 5 seconds.

1. From the **Actions** page, create an action called `db-access`.

    1. Name your action `db-access`.

    2. Select the `action-tutorial` package.

    3. Select Node.js 10 for the runtime.

    4. Click **Create**.

    5. Paste in the following code example:

        ```javascript
        /**
        *
        * main() will be run when you invoke this action
        *
        * @param Cloud Functions actions accept a single parameter, which must be a JSON object.
        *
        * @return The output of this action, which must be a JSON object.
        *
        */
        function main(params) {
	      return new Promise((resolve, reject) => {
	         // simulate a db access by resolving after 5s
	         setTimeout(() => {
	             console.log('fake db access done. Resolving Promise...');
	             resolve({ cloudant_result: 'SUCCESS' });
	         }, 5000);
    	});
        }
        ```
        {: codeblock}

    6. Click **Save**. 

    7. Test your action by clicking **Invoke** and waiting for the following output to display.

        **Example output**

        ```sh
        Results:
        {
            "cloudant_result": "SUCCESS"
        }

        Logs:
        [
            "2020-04-21T01:53:36.739565Z    stdout: fake db access done. Resolving Promise..."
        ]
        ```
        {: screen}

Your `db-access` action is ready!

## Create the `ow-sdk-action` action
{: #tutorial_action_ow-sdk-action}
{: step}

The `ow-sdk-action` action is a Node.js program that calls the other two actions: `cos-access` and `db-access`. When invoked, the `ow-sdk-action` action code acts as a custom sequence, first calling `cos-access`, then `db-access`, and finally `cos-access` again. The results of each action are stored in a variable that is called `chained_action_results`, which is then returned at the end. When the action is invoked, follow the code comments to see what is happening. 

1. From the **Actions** page, create a third action called `ow-sdk-action`.

    1. Name your action `ow-sdk-action`.

    2. Select the `action-tutorial` package.

    3. Select Node.js 10 for the runtime.

    4. Click **Create**.

    5. Paste in the following code example:

        ```javascript
        /**
        *
        * main() will be run when you invoke this action
        *
        * @param Cloud Functions actions accept a single parameter, which must be a JSON object.
        *
        * @return The output of this action, which must be a JSON object.
        *
        */
        const openwhisk = require('openwhisk');
        const ow = openwhisk();

        function main(params) {
          // for demonstration purposes, we keep track of the individual results of each action that we invoke in our 
          // custom sequence and return it in the last step of the sequence as the overall action result.

          // Although the following .then() blocks are run asynchronously, the main() function acts as a closure that 
          // makes sure that the chained_action_results variable is accessible for all .then() blocks
          const chained_action_results = [];

          console.log('Building custom sequence, using openwhisk node-js SDK...');
        	return ow.actions.invoke({ 
	          name: 'action-tutorial/cos-access',
	          blocking: true,
	          result: true,
	          params: {}
	      })
	      .then((result) => {
	          console.log('Result from cos-access ' + JSON.stringify(result));
	
    	    // storing result
	          chained_action_results.push(result);
	    
	          console.log('Now invoking db-access...');
	
	          return ow.actions.invoke({
	              name: 'action-tutorial/db-access',
	              blocking: true,
	              result: true,
	              params: {}
	          });
    	})
        	.catch((error) => {
	          console.log('An error occurred! ' + JSON.stringify(error));
	          // IMPORTANT! Re-throw the error, to avoid following .then() block to be executed!
	          throw error;
    	})
        	.then((result) => {
	          console.log('Result from db-access ' + JSON.stringify(result));
	    
	          // storing result
	          chained_action_results.push(result);
	          
	          console.log('Now invoking cos-access...');
	    
	          return ow.actions.invoke({
	              name: 'action-tutorial/cos-access',
	              blocking: true,
	              result: true,
	              params: {}
	          });
	    })
	      .catch((error) => {
	          console.log('An error occurred! ' + JSON.stringify(error));
	          // IMPORTANT! Re-throw the error, to avoid following .then() block to be executed!
	          throw error;
	    })
	      .then((result) => {
	          console.log('Result from cos-access ' + JSON.stringify(result));
	    
	          // storing result
	          chained_action_results.push(result);
	    
	          console.log('Custom SEQUENCE with openwhisk-node-js-sdk completed.');
	    
	          // returning the result here, makes sure that the top-most promise, we returned in main() returns the combined results of all 
	          // chained action invokes
	    
	          // since the result needs to be a valid JSON object, we cannot directly return the array, so we wrap it
	          return {
	              action_results: chained_action_results
	          };
    	})
        	.catch((error) => {
	          console.log('An error occurred! ' + JSON.stringify(error));

              console.log('Custom SEQUENCE with openwhisk-node-js-sdk FAILED');

              // This last throw is absolutely important to make the top-most promise, which we initially returned at the 
              // top of the main() function REJECTS with the given error. If we did not throw here, it would still RESOLVE
              // even though the code herein failed.
              throw error;
	    });
        }
        ```
        {: codeblock}

    6. Click **Save**. 

    7. Test your action by clicking **Invoke** and waiting for the following output to display.

        **Example output**

        ```sh
        Results:
        {
        "action_results": [
          {
            "cos_message": "SUCCESS"
          },
          {
            "cloudant_result": "SUCCESS"
          },
          {
            "cos_message": "SUCCESS"
          }
        ]
        }

        Logs:
        [
        "2020-04-17T04:31:20.965176Z    stdout: Building custom sequence, using openwhisk node-js SDK...",
        "2020-04-17T04:31:31.670466Z    stdout: Result from cos-access {\"cos_message\":\"SUCCESS\"}",
        "2020-04-17T04:31:31.670501Z    stdout: Now invoking db-access...",
        "2020-04-17T04:31:36.847844Z    stdout: Result from db-access {\"cloudant_result\":\"SUCCESS\"}",
        "2020-04-17T04:31:36.847883Z    stdout: Now invoking cos-access...",
        "2020-04-17T04:31:47.230424Z    stdout: Result from cos-access {\"cos_message\":\"SUCCESS\"}",
        "2020-04-17T04:31:47.230466Z    stdout: Custom SEQUENCE with openwhisk-node-js-sdk completed."
        ]
        ```
        {: screen}

Great work! You created an action that calls two other actions. From these actions, you can now develop the code to call {{site.data.keyword.cos_full_notm}}, access your {{site.data.keyword.cloudant}} database, and then put the results back into {{site.data.keyword.cos_full_notm}}. You even have the basic building blocks for error handling.


