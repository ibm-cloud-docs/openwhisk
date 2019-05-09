, select the namespace that you want to install the package into. Namespaces are formed from the combined org and space names.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Personality Insights** Package.

5. Click **Install**.

6. Once the package has been installed you will be redirected to the actions page and can search for your new package, which is named **personality-insights-v3**.

7. To use the actions in the **personality-insights-v3** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click an action from the **personality-insights-v3** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.personalityinsightsshort}} package
{: #usage_insights}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions will require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

This package's functions use the current version of Personality Insights, 2017-10-13. Try out the `profile` action.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. This excerpt is just some filler text and probably won't return anything very interesting from the personality insights service. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts. The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and, for JSON content that is timestamped, can report temporal behavior."
```
{: pre}
</taging>
