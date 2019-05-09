, select the namespace that you want to install the package into. Namespaces are formed from the combined org and space names.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Natural Language Classifier** Package.

5. Click **Install**.

6. Once the package has been installed you will be redirected to the actions page and can search for your new package, which is named **natural-language-classifier-v1**.

7. To use the actions in the **natural-language-classifier-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click an action from the **natural-language-classifier-v1** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.nlclassifiershort}} package
{: #usage_classifier}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-classifiers` action.
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}
</staging>
