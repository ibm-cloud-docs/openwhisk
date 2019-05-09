, select the namespace that you want to install the package into. Namespaces are formed from the combined org and space names.

3. Click **Install Packages**.

4. Click on the **Watson** Package group.

5. Click on the **Watson Assistant** Package.

5. Click **Install**.

6. Once the Package has been installed you will be redirected to the Actions page and can search for your new Package, which is named **assistant-v1**.

7. To use the Actions in the **assistant-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click on an action from the **assistant-v1** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click on the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.conversationshort}} package
{: #usage_conversation}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions will require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning).

This package's functions use the current version of Watson Assistant, 2018-07-10. Try out the `list-workspaces` action.
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}
</staging>
