 After logging in, all commands begin with `ibmcloud fn`.


If you need to use the authentication API key for {{site.data.keyword.openwhisk_short}} in an external HTTP client such as cURL or Postman, you can retrieve it with the following commands:

To get the current API key:
```
ibmcloud fn property get --auth
```
{: pre}

To get the current API host:
```
ibmcloud fn property get --apihost
```
{: pre}

The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}

### API Gateway authentication
{: #cli_apigw_authentication}

The OpenWhisk CLI required you to run the `wsk bluemix login` to be able to configure the API Gateway authorization for management of your APIs by using the `wsk api` command. With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to run `wsk bluemix login`. Instead, when you use the `ibmcloud login` command to log in to {{site.data.keyword.Bluemix_notm}}, the {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information. Now you can manage your APIs by using the `ibmcloud fn api` command.

### Migrating deployment scripts
{: #cli_migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` binary, all commands work the same way by using the command `ibmcloud fn`. You can modify your scripts to use the {{site.data.keyword.Bluemix_notm}} CLI plug-in, or create an alias or wrapper so that current commands using `wsk` are translated to `ibmcloud fn`. The `ibmcloud login` and `ibmcloud target` commands in the {{site.data.keyword.Bluemix_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before you run `ibmcloud fn` commands to deploy and manage your {{site.data.keyword.openwhisk_short}} entities.





## CLI version history
{: #cli_versions}

A historical record of versions that show highlights and bug fixes.

v1.0.30 (2019-04-03)
* Improved the `service bind` handling of IAM and org and space-based services.
* Added a fix for handling API endpoint https://cloud.ibm.com.

v1.0.29 (2019-02-06)
* Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, refer to the [Deployment](/docs/openwhisk?topic=cloud-functions-deploy#deploy) documentation.

v1.0.28 (2019-01-21)
* Added an error message when `update|delete|get namespace name` exists multiple times.

v1.0.27 (2018-12-11)
* Added `namespace get` fixes.
* Added a fix for `--save-as` when an action is a black box action.
* Added `--concurrency` flag for action create and action update commands.

v1.0.26 (2018-11-30)
* Enabled `fn property get --auth` to correctly return the auth key in a new environment.

v1.0.25 (2018-11-23)
* Improved error message result display.
* Added a `fn namespace get` fix to correctly display namespace properties.

1.0.23 (2018-10-15)
* Added support for ruby (.rb) action code recognition.

1.0.22 (2018-08-20)
* Added us-east region support.

1.0.21 (2018-08-01)
* Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: `ibmcloud fn <command>` and `ibmcloud fn <command>`. You can also still use `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Minor bug fixes and improvements.

1.0.18 (2018-06-20)
* Added a fix for unbinding user-provided service instances.
* Performance improvements.

1.0.17 (2018-06-12)
* Added support for binding (`ibmcloud wsk service bind`) and unbinding (`ibmcloud wsk service unbind`) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command.

1.0.16 (2018-05-24)
* Minor bug fixes and improvements.

1.0.15 (2018-05-21)
* Minor bug fixes and improvements.

1.0.14 (2018-05-17)
* Enabled support for the `&` character in org and space names.

1.0.13 (2018-05-07)
* Minor bug fixes and error handling improvements.

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK updates to maintain `bx` CLI compatibility.

1.0.11 (2018-04-23)
* Minor bug fixes and improvements.

1.0.10 (2018-04-09)
* Added new `--web-secure` option to the `ibmcloud wsk action create|update` commands to secure web action endpoints.
* Fixed back-to-back path parameter [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237).

1.0.9 (2018-03-16)
* Enabled support for service bind at the package level.

1.0.8 (2018-02-22)
* Enabled support for IAM service bind.

1.0.7 (2018-02-02)
* Updated `ibmcloud wsk api` to accept path parameters such as `/api/{id}`. For info, see [API Gateway](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
* Restored proxy support.
* Removed `swift:3`.

1.0.6 (2018-01-30)
* Fixed a bug with the command `ibmcloud wsk service bind` for actions inside a package.
</staging>
