---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-30"

keywords: web actions, serverless, functions, sitemap, actions

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

# Sitemap for {{site.data.keyword.openwhisk_short}}
{: #sitemap}

Find what you are looking for in the compilation of {{site.data.keyword.openwhisk}} topics.
{: shortdesc}







## Getting started with IBM {{site.data.keyword.openwhisk_short}}
{: #sitemap_getting_started_with_ibm_}


[Getting started with IBM {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-getting-started)

* [Working with actions](/docs/openwhisk?topic=openwhisk-getting-started#gs_actions)

* [Creating an action in the console](/docs/openwhisk?topic=openwhisk-getting-started#gs_hello_world)

* [Creating an action in the CLI](/docs/openwhisk?topic=openwhisk-getting-started#gs_hello_world_cli)

* [What's next in {{site.data.keyword.openwhisk_short}}?](/docs/openwhisk?topic=openwhisk-getting-started#gs_next_steps)


## What is FaaS (Function-as-a-Service)?

[What is FaaS (Function-as-a-Service)?](https://www.ibm.com/cloud/learn/faas){: new_window}{: external}


## About
{: #sitemap_about}


[Common use cases](/docs/openwhisk?topic=openwhisk-use_cases)
* [Microservices](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_microservices)
* [Web apps](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_webapps)
* [IoT](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_iot)
* [API backend](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_backend)
* [Mobile back end](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_mobile)
* [Data processing](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_data)
* [Cognitive](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_cognitive)
* [Event processing with Kafka or {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=openwhisk-use_cases#use_cases_events)

[How {{site.data.keyword.openwhisk_short}} works](/docs/openwhisk?topic=openwhisk-about)
* [{{site.data.keyword.openwhisk_short}} terminology](/docs/openwhisk?topic=openwhisk-about#functions-terminology)
* [What happens behind the scenes in {{site.data.keyword.openwhisk_short}}?](/docs/openwhisk?topic=openwhisk-about#about_scenes)
  * [1. Entering the system: NGINX](/docs/openwhisk?topic=openwhisk-about#about_ngnix)
  * [2. Entering the system: Controller](/docs/openwhisk?topic=openwhisk-about#about_controller)
  * [3. Authentication and Authorization: CouchDB](/docs/openwhisk?topic=openwhisk-about#about_auth)
  * [4. Getting the action: CouchDB… again](/docs/openwhisk?topic=openwhisk-about#about_couchdb)
  * [5. Who’s there to invoke the action: Load Balancer](/docs/openwhisk?topic=openwhisk-about#about_lb)
  * [6. Please form a line: Kafka](/docs/openwhisk?topic=openwhisk-about#about_kafka)
  * [7. Invoking the code: Invoker](/docs/openwhisk?topic=openwhisk-about#about_invoker)
  * [8. Storing the results: CouchDB again](/docs/openwhisk?topic=openwhisk-about#about_storing)

[Function as a Service architecture](/docs/openwhisk?topic=openwhisk-faas)
* [Comparison of OpenWhisk architectures](/docs/openwhisk?topic=openwhisk-faas#faas_architecture)
* [Cost considerations for {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-faas#faas_cost)
  * [Limitless capacity](/docs/openwhisk?topic=openwhisk-faas#faas_capacity)
  * [Run actions in any region](/docs/openwhisk?topic=openwhisk-faas#faas_region)
  * [Redundancy by design](/docs/openwhisk?topic=openwhisk-faas#faas_redundancy)


## Tutorials
{: #sitemap_tutorials}


[Calling an action from another action](/docs/openwhisk?topic=openwhisk-tutorial_action)
* [Create the `cos-access` action](/docs/openwhisk?topic=openwhisk-tutorial_action#tutorial_action_cos-action)
* [Create the `db-access` action](/docs/openwhisk?topic=openwhisk-tutorial_action#tutorial_action_db-access)
* [Create the `ow-sdk-action` action](/docs/openwhisk?topic=openwhisk-tutorial_action#tutorial_action_ow-sdk-action)

[Build a database driven Slackbot](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-slack-chatbot-database-watson#slack-chatbot-database-watson){: new_window}{: external}

[Combining serverless and Cloud Foundry for data retrieval and analytics](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-serverless-github-traffic-analytics#serverless-github-traffic-analytics){: new_window}{: external}

[Mobile application with serverless backend](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-serverless-mobile-backend#serverless-mobile-backend){: new_window}{: external}

[Serverless web application and API](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-serverless-api-webapp#serverless-api-webapp){: new_window}{: external}


## Installing the CLI and plug-in
{: #sitemap_installing_the_cli_and_plug-in}


[Installing the CLI and plug-in](/docs/openwhisk?topic=openwhisk-cli_install)

* [Setting up the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install#cli_setup)

* [Setting up the {{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=openwhisk-cli_install#cli_plugin_setup)

* [Next steps for {{site.data.keyword.openwhisk}} CLI](/docs/openwhisk?topic=openwhisk-cli_install#install_next)

* [Updating the {{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=openwhisk-cli_install#cli_update)

* [Configuring the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy](/docs/openwhisk?topic=openwhisk-cli_install#cli_proxy)

* [Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=openwhisk-cli_install#cli_migrate)
    * [Command syntax](/docs/openwhisk?topic=openwhisk-cli_install#cli_syntax)
    * [API Authentication and Host](/docs/openwhisk?topic=openwhisk-cli_install#cli_api_auth)
    * [API Gateway authentication](/docs/openwhisk?topic=openwhisk-cli_install#cli_apigw_authentication)
    * [Migrating deployment scripts](/docs/openwhisk?topic=openwhisk-cli_install#cli_migrating_deploy_scripts)


## Managing namespaces
{: #sitemap_managing_namespaces}


[Managing namespaces](/docs/openwhisk?topic=openwhisk-namespaces)

* [What is a namespace?](/docs/openwhisk?topic=openwhisk-namespaces#what_is_namespace)
    * [What happens when I create a namespace?](/docs/openwhisk?topic=openwhisk-namespaces#what_happens_create_namespace)
    * [Are there any limitations for namespaces?](/docs/openwhisk?topic=openwhisk-namespaces#namespace_limitations)
    * [What do I do if I have a Cloud Foundry-based namespace?](/docs/openwhisk?topic=openwhisk-namespaces#cf_namespace)
    * [How do I see a list of my {{site.data.keyword.openwhisk_short}} namespaces?](/docs/openwhisk?topic=openwhisk-namespaces#how_list_namespace)
    * [How do I see the {{site.data.keyword.openwhisk_short}} entities in my namespace?](/docs/openwhisk?topic=openwhisk-namespaces#how_find_iam_namespace)

* [Creating an IAM-based namespace](/docs/openwhisk?topic=openwhisk-namespaces#create_iam_namespace)
    * [Creating an IAM-based namespace from the console](/docs/openwhisk?topic=openwhisk-namespaces#create_iam_ui)
    * [Creating an IAM-based namespace with the CLI](/docs/openwhisk?topic=openwhisk-namespaces#namespaces_create)
    * [Creating a namespace with the API](/docs/openwhisk?topic=openwhisk-namespaces#namespaces_create_api)

* [Targeting namespaces](/docs/openwhisk?topic=openwhisk-namespaces#targeting-namespaces)
    * [Accessing other resources from a namespace](/docs/openwhisk?topic=openwhisk-namespaces#namespace-access)

* [Next steps for namespaces](/docs/openwhisk?topic=openwhisk-namespaces#namespaces_next)


## Deploying quickstart templates
{: #sitemap_deploying_quickstart_templates}


[Deploying quickstart templates](/docs/openwhisk?topic=openwhisk-templates)

* [Available quickstart templates](/docs/openwhisk?topic=openwhisk-templates#available-templates)

* [Deploying the {{site.data.keyword.cloudant_short_notm}} Events template](/docs/openwhisk?topic=openwhisk-templates#cloudant-template)
    * [Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the console](/docs/openwhisk?topic=openwhisk-templates#cloudant-template-ui)
    * [Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the CLI](/docs/openwhisk?topic=openwhisk-templates#cloudant-template-cli)
    * [Testing your {{site.data.keyword.cloudant_short_notm}} Events package](/docs/openwhisk?topic=openwhisk-templates#cloudant-template_test)

* [Deploying the Get HTTP Resource template](/docs/openwhisk?topic=openwhisk-templates#get-http-resource-template)
    * [Deploying the Get HTTP Resource template from the console](/docs/openwhisk?topic=openwhisk-templates#get-http-resource-template-ui)
    * [Deploying the Get HTTP Resource template from the CLI](/docs/openwhisk?topic=openwhisk-templates#get-http-resource-template-cli)
    * [Invoking the **Get HTTP Resource** action](/docs/openwhisk?topic=openwhisk-templates#get-http-resource-template-invoke)

* [Deploying the Hello World template](/docs/openwhisk?topic=openwhisk-templates#hello-world-template)
    * [Deploying the Hello World template from the console](/docs/openwhisk?topic=openwhisk-templates#hello-world-template-ui)
    * [Deploying the Hello World template from the CLI](/docs/openwhisk?topic=openwhisk-templates#hello-world-template-cli)
    * [Testing the Hello World action](/docs/openwhisk?topic=openwhisk-templates#hello-world-template-test)

* [Deploying the {{site.data.keyword.messagehub_full}} template](/docs/openwhisk?topic=openwhisk-templates#messagehub-events-template)
    * [Deploying the {{site.data.keyword.messagehub}} template from the console](/docs/openwhisk?topic=openwhisk-templates#messagehub-events-template-ui)
    * [Deploying the {{site.data.keyword.messagehub}} Events template from the CLI](/docs/openwhisk?topic=openwhisk-templates#messagehub-events-template-cli)

* [Deploying the Periodic Slack Reminder template](/docs/openwhisk?topic=openwhisk-templates#slack-reminder-template)
    * [Deploying the Periodic Slack Reminder template from the console](/docs/openwhisk?topic=openwhisk-templates#slack-reminder-template-ui)
    * [Deploying the Periodic Slack Reminder template from the CLI](/docs/openwhisk?topic=openwhisk-templates#slack-reminder-template-cli)
    * [Testing the Periodic Slack Reminder template](/docs/openwhisk?topic=openwhisk-templates#periodic-slack-template-test)
    * [Pausing or stopping your Periodic Slack Reminder trigger](/docs/openwhisk?topic=openwhisk-templates#periodic-slack-stop-trigger)


## Developing actions
{: #sitemap_developing_actions}


[Action overview](/docs/openwhisk?topic=openwhisk-actions_over)

[Planning serverless apps](/docs/openwhisk?topic=openwhisk-plan)
* [Review the runtime support](/docs/openwhisk?topic=openwhisk-plan#plan_runtime)
* [Architect your code](/docs/openwhisk?topic=openwhisk-plan#plan_architect)
* [Determine your event source](/docs/openwhisk?topic=openwhisk-plan#plan_source)
* [Organize deployments across environments](/docs/openwhisk?topic=openwhisk-plan#plan_environments)

[Creating actions](/docs/openwhisk?topic=openwhisk-actions)
* [Creating actions from the CLI](/docs/openwhisk?topic=openwhisk-actions#actions_cli)
  * [Combining app files and Docker images to create actions](/docs/openwhisk?topic=openwhisk-actions#actions_combine_app)
* [Creating actions from the console](/docs/openwhisk?topic=openwhisk-actions#actions_create_ui)
* [Creating actions from binaries](/docs/openwhisk?topic=openwhisk-actions#actions_create_binaries)
* [Updating code or runtimes in actions](/docs/openwhisk?topic=openwhisk-actions#actions_update)
  * [Updating actions from the CLI](/docs/openwhisk?topic=openwhisk-actions#actions_update_cli)
  * [Updating actions from the console](/docs/openwhisk?topic=openwhisk-actions#actions_update_console)
* [Binding parameters to actions](/docs/openwhisk?topic=openwhisk-actions#actions_params)
* [Packaging actions](/docs/openwhisk?topic=openwhisk-actions#actions_pkgs)
* [Binding parameters to packages](/docs/openwhisk?topic=openwhisk-actions#actions_pkgs_params)
* [Sharing packages of actions](/docs/openwhisk?topic=openwhisk-actions#actions_pkgs_share)
* [Environment variables for actions](/docs/openwhisk?topic=openwhisk-actions#actions_envvars)
  * [Incorporating action environment variables in your app](/docs/openwhisk?topic=openwhisk-actions#actions_envvars_app)
* [Versioning your actions](/docs/openwhisk?topic=openwhisk-actions#actions-version)
  * [Version control systems](/docs/openwhisk?topic=openwhisk-actions#version-control-systems)
  * [Naming conventions](/docs/openwhisk?topic=openwhisk-actions#naming-conventions)

[Creating web actions](/docs/openwhisk?topic=openwhisk-actions_web)
* [Why use web actions instead of standard actions?](/docs/openwhisk?topic=openwhisk-actions_web#why_actions_web)
* [How do web actions work?](/docs/openwhisk?topic=openwhisk-actions_web#how_actions_web_work)
* [Packaging code for web actions](/docs/openwhisk?topic=openwhisk-actions_web#packaging_actions_web)
* [Available features of web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_extra)
* [Creating a web action](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_example)
  * [Web action JSON properties](/docs/openwhisk?topic=openwhisk-actions_web#web_action_properties)
  * [Performing an HTTP redirect by using a web action](/docs/openwhisk?topic=openwhisk-actions_web#http_redirect)
  * [Setting cookies by using a web action](/docs/openwhisk?topic=openwhisk-actions_web#multiple_cookie)
  * [Returning an image by using a web action](/docs/openwhisk?topic=openwhisk-actions_web#return_image)
  * [Returning JSON by using a web action](/docs/openwhisk?topic=openwhisk-actions_web#return_json)
  * [HTTP Context](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_context)
  * [HTTPS Endpoint support](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_endpoint)
  * [Altering the response content of web action](/docs/openwhisk?topic=openwhisk-actions_web#extra_features)
* [Securing web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure)
  * [Creating a secure web action](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure_create)
  * [Testing a secure web action with randomly generated value of `X-Require-Whisk-Auth`](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure_test_random)
  * [Testing a secure web action with custom value of `X-Require-Whisk-Auth`](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure_test_custom)
  * [Testing to see if your web action is secure](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure_test)
* [Raw HTTP handling](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_raw)
  * [Enabling raw HTTP handling](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_raw_enable)
  * [Decoding binary body content from Base64](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_decode)
* [Options requests](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_options)
* [Error handling](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_errors)
* [Disabling web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_disable)
* [Web action limits](/docs/openwhisk?topic=openwhisk-actions_web#web-action-limits)

[Creating sequences](/docs/openwhisk?topic=openwhisk-sequences)
* [Creating a sequence from the console](/docs/openwhisk?topic=openwhisk-sequences#actions_seq_console)
* [Creating a sequence from the CLI](/docs/openwhisk?topic=openwhisk-sequences#actions_seq_cli)

[Preparing apps for actions](/docs/openwhisk?topic=openwhisk-prep)
* [Preparing JavaScript apps](/docs/openwhisk?topic=openwhisk-prep#prep_js)
  * [Structuring JavaScript code](/docs/openwhisk?topic=openwhisk-prep#prep_js_struct)
  * [Structuring JavaScript code with synchronous behavior](/docs/openwhisk?topic=openwhisk-prep#prep_js_sync)
  * [Structuring JavaScript code with asynchronous behavior](/docs/openwhisk?topic=openwhisk-prep#prep_js_async)
  * [Structuring JavaScript code with synchronous and asynchronous behavior](/docs/openwhisk?topic=openwhisk-prep#prep_js_both)
  * [Example: Calling an external API with JavaScript](/docs/openwhisk?topic=openwhisk-prep#prep_js_api)
  * [Packaging JavaScript code with the `webpack` module](/docs/openwhisk?topic=openwhisk-prep#prep_js_pkg)
  * [Packaging JavaScript code as NPM files](/docs/openwhisk?topic=openwhisk-prep#prep_js_npm)
  * [NPM libraries with native dependencies](/docs/openwhisk?topic=openwhisk-prep#npm-libraries-with-native-dependencies)
* [How do I package my Python app for deployment in {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-prep#how_to_package_python)
  * [Structuring Python code](/docs/openwhisk?topic=openwhisk-prep#prep_python_struct)
  * [Packaging multiple Python files into an archive](/docs/openwhisk?topic=openwhisk-prep#prep_python_multiple_zip)
  * [Packaging Python code with a local virtual environment in a compressed file](/docs/openwhisk?topic=openwhisk-prep#prep_python_local_virt_zip)
  * [Packaging Python code with a Docker virtual environment in a compressed file](/docs/openwhisk?topic=openwhisk-prep#prep_python_docker_virt_zip)
  * [Packaging large Python dependencies in a custom Docker image](/docs/openwhisk?topic=openwhisk-prep#packaging_large_python_docker)
  * [Packaging your app within a custom Docker image](/docs/openwhisk?topic=openwhisk-prep#packaging_docker_skeleton)
* [Packaging Python code](/docs/openwhisk?topic=openwhisk-prep#prep_python)
  * [Packaging multiple Python files into a compressed file](/docs/openwhisk?topic=openwhisk-prep#prep_python_pkg)
  * [Packaging Python code with a local virtual environment in a compressed file](/docs/openwhisk?topic=openwhisk-prep#prep_python_local_virtenv)
  * [Packaging Python code with a Docker virtual environment in an archive](/docs/openwhisk?topic=openwhisk-prep#prep_python_virtenv)
  * [Packaging large Python dependencies in a custom Docker image](/docs/openwhisk?topic=openwhisk-prep#prep_python_docker)
* [Preparing apps in Docker images](/docs/openwhisk?topic=openwhisk-prep#prep_docker)
  * [Creating a custom Docker image for your action](/docs/openwhisk?topic=openwhisk-prep#prep_create_custom_docker_action)
  * [Deploying an action with a custom Docker image](/docs/openwhisk?topic=openwhisk-prep#prep_deploy_action_custom_docker_image)
* [Preparing Go Apps](/docs/openwhisk?topic=openwhisk-prep#prep_go)
  * [Structuring Go Apps](/docs/openwhisk?topic=openwhisk-prep#prep_go_struct)
  * [Creating a simple Golang Action](/docs/openwhisk?topic=openwhisk-prep#prep_go_simple)
  * [Create a Golang action made up of multiple packages](/docs/openwhisk?topic=openwhisk-prep#prep_go_multi_packages)
  * [Create an action by using external libraries with Go modules](/docs/openwhisk?topic=openwhisk-prep#prep_go_external_libraries)
* [Preparing Swift apps](/docs/openwhisk?topic=openwhisk-prep#prep_swift)
  * [Structuring Swift code](/docs/openwhisk?topic=openwhisk-prep#prep_swift_struc)
  * [Packaging a Swift 4.2 file](/docs/openwhisk?topic=openwhisk-prep#prep_swift42_single)
  * [Packaging Swift 4.2 multi-file projects and dependencies](/docs/openwhisk?topic=openwhisk-prep#prep_swift42_multi)
* [Preparing Ruby apps](/docs/openwhisk?topic=openwhisk-prep#prep_ruby)
  * [Structuring Ruby code](/docs/openwhisk?topic=openwhisk-prep#prep_ruby_struct)
  * [Packaging Ruby code](/docs/openwhisk?topic=openwhisk-prep#prep_ruby_pkg)
* [Preparing PHP apps](/docs/openwhisk?topic=openwhisk-prep#prep_php)
  * [Structuring PHP code](/docs/openwhisk?topic=openwhisk-prep#prep_php_struct)
  * [Packaging PHP code](/docs/openwhisk?topic=openwhisk-prep#prep_php_pkg)
* [Preparing Java apps](/docs/openwhisk?topic=openwhisk-prep#prep_java)
  * [Structuring Java code](/docs/openwhisk?topic=openwhisk-prep#prep_java_struct)
  * [Packaging Java code](/docs/openwhisk?topic=openwhisk-prep#prep_java_pkg)
  * [Packaging Java code with Gradle](/docs/openwhisk?topic=openwhisk-prep#prep_java_gradle)
* [Preparing .NET Core apps](/docs/openwhisk?topic=openwhisk-prep#prep_dotnet)
  * [Structuring .NET Core code](/docs/openwhisk?topic=openwhisk-prep#prep_dotnet_struct)
  * [Packaging .NET Core code](/docs/openwhisk?topic=openwhisk-prep#prep_dotnet_pkg)

[Creating serverless REST APIs](/docs/openwhisk?topic=openwhisk-apigateway)
* [Creating your first API](/docs/openwhisk?topic=openwhisk-apigateway#api_create)
  * [Creating your first API by using the CLI](/docs/openwhisk?topic=openwhisk-apigateway#api_create_cli)
  * [Creating an API from the console](/docs/openwhisk?topic=openwhisk-apigateway#api_create_ui)
* [Using full control over the HTTP response](/docs/openwhisk?topic=openwhisk-apigateway#api_control)
* [Securing your API web action](/docs/openwhisk?topic=openwhisk-apigateway#api_secure)
* [Modifying the API configuration](/docs/openwhisk?topic=openwhisk-apigateway#api_modify_config)


## Managing triggers and rules
{: #sitemap_managing_triggers_and_rules}


[Creating triggers for events](/docs/openwhisk?topic=openwhisk-triggers)
* [Creating triggers from the CLI](/docs/openwhisk?topic=openwhisk-triggers#triggers_create)
* [Difference between feed and trigger](/docs/openwhisk?topic=openwhisk-triggers#triggers_difference)
* [Creating a trigger for a feed](/docs/openwhisk?topic=openwhisk-triggers#triggers_feeds)

[Associating triggers and actions through rules](/docs/openwhisk?topic=openwhisk-rules)
* [Creating a rule from the console](/docs/openwhisk?topic=openwhisk-rules#rules_ui)
* [Creating rules from the CLI](/docs/openwhisk?topic=openwhisk-rules#rules_create)
* [Creating rules for action sequences](/docs/openwhisk?topic=openwhisk-rules#rules_seq)
* [Associating multiple triggers and actions](/docs/openwhisk?topic=openwhisk-rules#rules_assoc)


## Testing serverless apps
{: #sitemap_testing_serverless_apps}


[Testing serverless apps](/docs/openwhisk?topic=openwhisk-test)

* [Testing actions from the console](/docs/openwhisk?topic=openwhisk-test#test-js-console)

* [Testing actions from the CLI](/docs/openwhisk?topic=openwhisk-test#test-js)
    * [Testing parameters stored in JSON files](/docs/openwhisk?topic=openwhisk-test#test_json_file)
    * [Testing parameters entered in JSON format](/docs/openwhisk?topic=openwhisk-test#test_json)
    * [Testing blocking actions](/docs/openwhisk?topic=openwhisk-test#test-block)

* [Testing triggers](/docs/openwhisk?topic=openwhisk-test#test_triggers)

* [Testing duration of activations](/docs/openwhisk?topic=openwhisk-test#test_time)

* [Testing memory usage](/docs/openwhisk?topic=openwhisk-test#test_memory)
    * [Special considerations for memory usage with Node.js runtime actions](/docs/openwhisk?topic=openwhisk-test#memory_usage)


## Setting access policies
{: #sitemap_setting_access_policies}


[Setting access policies](/docs/openwhisk?topic=openwhisk-iam)

* [How do I set IAM policies so that others can work with my namespace?](/docs/openwhisk?topic=openwhisk-iam#iam_namespace_policies)

* [How do I set IAM policies so that others can create namespaces in my account?](/docs/openwhisk?topic=openwhisk-iam#iam_namespace_create)

* [How do I know which access policies have set for me?](/docs/openwhisk?topic=openwhisk-iam#iam_set_policies_me)

* [Platform management roles](/docs/openwhisk?topic=openwhisk-iam#iam_platform_roles)

* [Service-specific roles](/docs/openwhisk?topic=openwhisk-iam#service_specific_roles)

* [Setting access policies for a service ID](/docs/openwhisk?topic=openwhisk-iam#service-id-set-policy)
    * [Setting access policies for a service ID in the console](/docs/openwhisk?topic=openwhisk-iam#service-id-set-ui)
    * [Setting an access policy for your {{site.data.keyword.openwhisk_short}} service ID through the CLI](/docs/openwhisk?topic=openwhisk-iam#cli-set)


## Integrating serverless apps
{: #sitemap_integrating_serverless_apps}


[Binding {{site.data.keyword.cloud_notm}} services to {{site.data.keyword.openwhisk_short}} entities](/docs/openwhisk?topic=openwhisk-services)
* [Binding a service to an action or package](/docs/openwhisk?topic=openwhisk-services#services_bind)
* [Binding Cloud Foundry based services](/docs/openwhisk?topic=openwhisk-services#services_bind_cf)
* [Unbinding services from actions](/docs/openwhisk?topic=openwhisk-services#services_unbind)

[Incorporating packages](/docs/openwhisk?topic=openwhisk-pkg_ov)
* [Overview for packages](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_overview)
* [Browsing pre-installed packages](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_browse)
* [Binding parameters to pre-installed packages](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_bind)
* [Adding your own packages](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_add)
  * [{{site.data.keyword.cos_full_notm}} package example](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_ex)

[Alarms](/docs/openwhisk?topic=openwhisk-pkg_alarms)
* [Firing a trigger event one time](/docs/openwhisk?topic=openwhisk-pkg_alarms#pkg_alarms_one)
* [Firing a trigger event periodically on an interval-based schedule](/docs/openwhisk?topic=openwhisk-pkg_alarms#pkg_alarms_int)
* [Firing a trigger on a time-based schedule by using cron](/docs/openwhisk?topic=openwhisk-pkg_alarms#pkg_alarms_cron)

[Cloudant](/docs/openwhisk?topic=openwhisk-pkg_cloudant)
* [Available entities for Cloudant](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_available)
* [Binding the `/whisk.system/cloudant` package to your {{site.data.keyword.cloudant_short_notm}} database.](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_bind)
* [Working with documents in an {{site.data.keyword.cloudant_short_notm}} database](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_read)
  * [Reading a document](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_read_doc)
  * [Writing a document to an {{site.data.keyword.cloudant_short_notm}} database](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_write)
  * [Updating a document](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_update)
* [Create a trigger by using the filter function](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_trigger)
  * [Data structure of a trigger activation](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_struct)
* [Filter database change events](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_filter)
* [Processing an individual document by using an action sequence](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_seq)
  * [Creating an action to process an individual document](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_action_individual_doc)
  * [Create a sequence with the `read` action](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_sequence_read)
  * [Create a trigger with `changes` feed](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_trigger_change)
  * [Create a rule to associate the trigger with the sequence](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_rule_trigger)
  * [Test the sequence](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_test_sequence)
  * [Next steps with {{site.data.keyword.cloudant_short_notm}} database](/docs/openwhisk?topic=openwhisk-pkg_cloudant#cloudant_next)

[Composer](/docs/openwhisk?topic=openwhisk-pkg_composer)
* [Installing the Composer library for JavaScript or Python](/docs/openwhisk?topic=openwhisk-pkg_composer#install_composer)
* [Configuring and running compositions in {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-pkg_composer#run)
* [Extending sequences with Composer](/docs/openwhisk?topic=openwhisk-pkg_composer#extending)
  * [Sequences without Composer](/docs/openwhisk?topic=openwhisk-pkg_composer#sequences-without-composer)
  * [Sequences with Composer](/docs/openwhisk?topic=openwhisk-pkg_composer#sequences-with-composer)
  * [Error handling for a sequence](/docs/openwhisk?topic=openwhisk-pkg_composer#error-handling)
  * [Conditional branching for a sequence](/docs/openwhisk?topic=openwhisk-pkg_composer#conditional-branch)
  * [Loops in Composer](/docs/openwhisk?topic=openwhisk-pkg_composer#loop)
  * [Inline definition of Actions within a composition](/docs/openwhisk?topic=openwhisk-pkg_composer#inline-def)
* [Using other combinator definitions](/docs/openwhisk?topic=openwhisk-pkg_composer#combinator-def)

[{{site.data.keyword.messagehub}}](/docs/openwhisk?topic=openwhisk-pkg_event_streams)
* [Package entities](/docs/openwhisk?topic=openwhisk-pkg_event_streams#pkg_event_streams_options)
* [Binding the `/whisk.system/messaging` package to your {{site.data.keyword.messagehub}} instance](/docs/openwhisk?topic=openwhisk-pkg_event_streams#event_streams_binding)
  * [Setting up an {{site.data.keyword.messagehub}} package outside {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=openwhisk-pkg_event_streams#eventstreams_outside)
* [Creating a trigger that listens to an {{site.data.keyword.messagehub}} instance](/docs/openwhisk?topic=openwhisk-pkg_event_streams#eventstreams_trigger)
  * [Creating a trigger for an {{site.data.keyword.messagehub}} package outside {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=openwhisk-pkg_event_streams#eventstreams_trigger_outside)
  * [Listening for messages](/docs/openwhisk?topic=openwhisk-pkg_event_streams#eventstreams_listen_messages)
  * [Messages are batched](/docs/openwhisk?topic=openwhisk-pkg_event_streams#eventstreams_batched)
* [References for {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=openwhisk-pkg_event_streams#message_references)

[Mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk)
* [Add the SDK to your app](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#add-mobile-sdk-app)
  * [Install mobile SDK with CocoaPods](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#install-mobile-sdk-cocoapods)
  * [Install mobile SDK with Carthage](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#install-mobile-sdk-carthage)
  * [Install mobile SDK from source code](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#install-mobile-sdk-source-code)
* [Install the starter app example for mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#install-mobile-sdk-starter-app)
* [Getting started with mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#get-started-mobile-sdk)
* [Invoke a mobile SDK action](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#invoke-mobile-sdk-action)
* [Fire a mobile SDK trigger](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#fire-mobile-sdk-trigger)
* [Use mobile SDK actions that return a result](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#mobile-sdk-actions-results)
* [Configuring the mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#configure-mobile-sdk)
  * [Support for qualified names with mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#qualified-names-mobile-sdk)
  * [Invoking actions with mobile SDK from `WhiskButton`](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk#invoke-mobile-sdk-actions-whiskbutton)

[Object Storage](/docs/openwhisk?topic=openwhisk-pkg_obstorage)
* [Packages for {{site.data.keyword.cos_short}}](/docs/openwhisk?topic=openwhisk-pkg_obstorage#obstorage_packages)
* [Setting up the {{site.data.keyword.cos_full_notm}} trigger](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev)
  * [Prerequisites for working with the {{site.data.keyword.cos_full_notm}} trigger](/docs/openwhisk?topic=openwhisk-pkg_obstorage#cos_changes_pre)
  * [1. Assigning the Notifications Manager role to your {{site.data.keyword.openwhisk_short}} namespace](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_auth)
  * [2. Determining your trigger parameters](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev_trig_param)
  * [3. Creating a trigger to listen for bucket changes](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev_trig_ui)
* [Connecting a {{site.data.keyword.openwhisk_short}} action to the trigger](/docs/openwhisk?topic=openwhisk-pkg_obstorage#cos_feed_action_connect)
  * [1. Creating an action to process the trigger results](/docs/openwhisk?topic=openwhisk-pkg_obstorage#cos_feed_action)
  * [2. Testing the trigger and action](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev_test)
  * [Next steps for {{site.data.keyword.cos_short}} and {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_next)
  * [Reference for {{site.data.keyword.cos_short}} and {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_ev_ch_ref)
* [Configuring the {{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_configure)
  * [1. Installing the {{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_install)
  * [2. Setting an IAM access policy for your {{site.data.keyword.openwhisk_short}} namespace](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_rw_auth)
  * [Setting default parameters for a package or action](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_param_bind)
* [Writing an object to a bucket](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_write)
* [Reading objects from a bucket](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_read)
  * [Reference for {{site.data.keyword.cos_short}} and {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-pkg_obstorage#pkg_obstorage_actions)

[Slack](/docs/openwhisk?topic=openwhisk-pkg_slack)
* [Posting a message to a Slack channel](/docs/openwhisk?topic=openwhisk-pkg_slack#posting-a-message-to-a-slack-channel)
* [Using the Slack token-based API](/docs/openwhisk?topic=openwhisk-pkg_slack#using-the-slack-token-based-api)

[Utilities](/docs/openwhisk?topic=openwhisk-pkg_utils)

[WebSocket](/docs/openwhisk?topic=openwhisk-pkg_websocket)
* [Send a message to a WebSocket](/docs/openwhisk?topic=openwhisk-pkg_websocket#send-websocket)

[Creating custom event provider feeds](/docs/openwhisk?topic=openwhisk-feeds_custom)
* [Feed architecture](/docs/openwhisk?topic=openwhisk-feeds_custom#feeds_arch)
  * [Hooks](/docs/openwhisk?topic=openwhisk-feeds_custom#fds_hooks)
  * [Polling](/docs/openwhisk?topic=openwhisk-feeds_custom#fds_polling)
  * [Connections](/docs/openwhisk?topic=openwhisk-feeds_custom#fds_connections)
* [ Implementing feed actions](/docs/openwhisk?topic=openwhisk-feeds_custom#feeds_actions)
* [Implementing feeds with hooks](/docs/openwhisk?topic=openwhisk-feeds_custom#feeds_hooks)
* [Implementing feeds with polling](/docs/openwhisk?topic=openwhisk-feeds_custom#feeds_polling)
* [Implementing feeds by using connections](/docs/openwhisk?topic=openwhisk-feeds_custom#feeds_connections)

[{{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-alerts-notify)

[{{site.data.keyword.visualrecognitionshort}}](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition)
* [Creating a {{site.data.keyword.visualrecognitionshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition#service_instance_recognition)
* [Installing the {{site.data.keyword.visualrecognitionshort}} package](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition#install_recognition)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition#visualrecognition_cli)
  * [Installing from the {{site.data.keyword.visualrecognitionshort}} console](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition#visualrecognition_ui)
* [Using the {{site.data.keyword.visualrecognitionshort}} package](/docs/openwhisk?topic=openwhisk-pkg_visual_recognition#usage_recognition)

[{{site.data.keyword.conversationshort}}](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant)
* [Creating a {{site.data.keyword.conversationshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant#service_instance_conversation)
* [Installing the {{site.data.keyword.conversationshort}} package](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant#install_conversation)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant#conversation_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant#conversation_ui)
* [Using the {{site.data.keyword.conversationshort}} package](/docs/openwhisk?topic=openwhisk-pkg_watson_assistant#usage_conversation)

[{{site.data.keyword.nlclassifiershort}}](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier)
* [Creating a {{site.data.keyword.nlclassifiershort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier#service_instance_classifier)
* [Installing the {{site.data.keyword.nlclassifiershort}} package](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier#install_classifier)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier#nlclassifier_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier#nlclassifier_ui)
* [Using the {{site.data.keyword.nlclassifiershort}} package](/docs/openwhisk?topic=openwhisk-pkg_natlang_classifier#usage_classifier)

[{{site.data.keyword.nlushort}}](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding)
* [Creating a {{site.data.keyword.nlushort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding#service_instance_understanding)
* [Installing the {{site.data.keyword.nlushort}} package](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding#install_understanding)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding#nlus_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding#nlus_ui)
* [Using the {{site.data.keyword.nlushort}} package](/docs/openwhisk?topic=openwhisk-pkg_natlang_understanding#usage_understanding)

[{{site.data.keyword.toneanalyzershort}}](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer)
* [Creating a {{site.data.keyword.toneanalyzershort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer#service_instance_tone)
* [Installing the {{site.data.keyword.toneanalyzershort}} package](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer#install_tone)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer#toneanalyzer_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer#toneanalyzer_ui)
* [Using the {{site.data.keyword.toneanalyzershort}} package](/docs/openwhisk?topic=openwhisk-pkg_tone_analyzer#usage_tone)

[{{site.data.keyword.languagetranslatorshort}}](/docs/openwhisk?topic=openwhisk-pkg_translator)
* [Creating a {{site.data.keyword.languagetranslatorshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_translator#service_instance_translator)
* [Installing the {{site.data.keyword.languagetranslatorshort}} package](/docs/openwhisk?topic=openwhisk-pkg_translator#install_translator)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_translator#languagetranslator_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_translator#languagetranslator_ui)
* [Using the {{site.data.keyword.languagetranslatorshort}} package](/docs/openwhisk?topic=openwhisk-pkg_translator#usage_translator)

[{{site.data.keyword.discoveryshort}}](/docs/openwhisk?topic=openwhisk-pkg_discovery)
* [Creating a {{site.data.keyword.discoveryshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_discovery#service_instance_discovery)
* [Installing the {{site.data.keyword.discoveryshort}} package](/docs/openwhisk?topic=openwhisk-pkg_discovery#install_discovery)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_discovery#discovery_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_discovery#discovery_ui)
* [Using the {{site.data.keyword.discoveryshort}} package](/docs/openwhisk?topic=openwhisk-pkg_discovery#usage_discovery)

[{{site.data.keyword.personalityinsightsshort}}](/docs/openwhisk?topic=openwhisk-pkg_person_insights)
* [Creating a {{site.data.keyword.personalityinsightsshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_person_insights#service_instance_insights)
* [Installing the {{site.data.keyword.personalityinsightsshort}} package](/docs/openwhisk?topic=openwhisk-pkg_person_insights#install_insights)
  * [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_person_insights#personalityinsights_cli)
  * [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_person_insights#personalityinsights_ui)
* [Using the {{site.data.keyword.personalityinsightsshort}} package](/docs/openwhisk?topic=openwhisk-pkg_person_insights#usage_insights)

[{{site.data.keyword.speechtotextshort}}](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text)
* [Creating a {{site.data.keyword.speechtotextshort}} service instance](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text#service_instance_speechtotext)
* [Installing the {{site.data.keyword.speechtotextshort}} package](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text#install_speechtotext)
* [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text#speechtotext_cli)
* [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text#speechtotext_ui)
* [Using the {{site.data.keyword.speechtotextshort}} package](/docs/openwhisk?topic=openwhisk-pkg_speech_to_text#usage_speechtotext)

[{{site.data.keyword.texttospeechshort}}](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech)
* [Creating a {{site.data.keyword.texttospeechshort}} Service instance](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech#service_instance_texttospeech)
* [Installing the {{site.data.keyword.texttospeechshort}} package](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech#install_texttospeech)
* [Installing from the {{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech#texttospeech_cli)
* [Installing from the {{site.data.keyword.openwhisk_short}} console](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech#texttospeech_ui)
* [Using the {{site.data.keyword.texttospeechshort}} package](/docs/openwhisk?topic=openwhisk-pkg_text_to_speech#usage_texttospeech)


## Setting up an automated tool chain
{: #sitemap_setting_up_an_automated_tool_chain}


[Deploying entities with a manifest file](/docs/openwhisk?topic=openwhisk-deploy)
* [Creating the Hello World API example](/docs/openwhisk?topic=openwhisk-deploy#deploy_helloworld_example)
* [More OpenWhisk deployment examples](/docs/openwhisk?topic=openwhisk-deploy#more-openwhisk-deployment-examples)
* [Deployment manifest specification](/docs/openwhisk?topic=openwhisk-deploy#deployment-manifest-specification)


## Logging and monitoring events 
{: #sitemap_logging_and_monitoring_events }


[Viewing logs](/docs/openwhisk?topic=openwhisk-logs)
* [Viewing action logs as they occur](/docs/openwhisk?topic=openwhisk-logs#logs_poll)
* [Viewing activation details](/docs/openwhisk?topic=openwhisk-logs#activation_details)
* [Viewing logs in {{site.data.keyword.la_full_notm}}](/docs/openwhisk?topic=openwhisk-logs#logs_console)
  * [Configure {{site.data.keyword.la_full_notm}}](/docs/openwhisk?topic=openwhisk-logs#logs_configure_log)

[Auditing events for {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-at_events)
* [List of events](/docs/openwhisk?topic=openwhisk-at_events#events)
  * [Action events](/docs/openwhisk?topic=openwhisk-at_events#action-events)
  * [Namespace events](/docs/openwhisk?topic=openwhisk-at_events#namespace-events)
  * [Package events](/docs/openwhisk?topic=openwhisk-at_events#package-events)
  * [Trigger events](/docs/openwhisk?topic=openwhisk-at_events#trigger-events)
  * [Rule events](/docs/openwhisk?topic=openwhisk-at_events#rule-events)
* [Viewing events](/docs/openwhisk?topic=openwhisk-at_events#view)
* [Analyzing events](/docs/openwhisk?topic=openwhisk-at_events#at_events_analyze)

[Monitoring {{site.data.keyword.openwhisk_short}} entities with {{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-monitor-functions)
* [Set up your {{site.data.keyword.mon_full_notm}} service instance](/docs/openwhisk?topic=openwhisk-monitor-functions#setup-monitor)
* [Accessing your {{site.data.keyword.mon_full_notm}} metrics](/docs/openwhisk?topic=openwhisk-monitor-functions#access-monitor)
* [Metrics available by Service Plan](/docs/openwhisk?topic=openwhisk-monitor-functions#metrics-by-plan)
  * [ibm_functions_activation](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_activation)
  * [ibm_functions_concurrent-invocations](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_concurrent-invocations)
  * [ibm_functions_concurrent-rate-limit](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_concurrent-rate-limit)
  * [ibm_functions_duration](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_duration)
  * [ibm_functions_init-time](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_init-time)
  * [ibm_functions_status-error-application](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_status-error-application)
  * [ibm_functions_status-error-developer](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_status-error-developer)
  * [ibm_functions_status-error-internal](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_status-error-internal)
  * [ibm_functions_status-success](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_status-success)
  * [ibm_functions_timed-rate-limit](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_timed-rate-limit)
  * [ibm_functions_wait-time](/docs/openwhisk?topic=openwhisk-monitor-functions#ibm_functions_wait-time)
* [Attributes for segmentation](/docs/openwhisk?topic=openwhisk-monitor-functions#attributes-for-segmentation)
  * [Global attributes](/docs/openwhisk?topic=openwhisk-monitor-functions#global-attributes)
  * [Additional attributes](/docs/openwhisk?topic=openwhisk-monitor-functions#additional-attributes)


## Annotations
{: #sitemap_annotations}


[Annotations](/docs/openwhisk?topic=openwhisk-annotations)

* [Action annotations](/docs/openwhisk?topic=openwhisk-annotations#annotations_action)

* [Web action annotations](/docs/openwhisk?topic=openwhisk-annotations#annotations-specific-to-web-actions)

* [Package annotations](/docs/openwhisk?topic=openwhisk-annotations#annotations_package)

* [Parameter annotations](/docs/openwhisk?topic=openwhisk-annotations#annotations_parameter)

* [Activation annotations](/docs/openwhisk?topic=openwhisk-annotations#annotations_activation)


## Runtimes
{: #sitemap_runtimes}


[Runtimes](/docs/openwhisk?topic=openwhisk-runtimes)

* [Available images and runtimes](/docs/openwhisk?topic=openwhisk-runtimes#runtimes_available)

* [JavaScript runtimes](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_javascript_environments)
    * [Migrating from Node.js 10 to Node.js 12](/docs/openwhisk?topic=openwhisk-runtimes#migrate_javascript_environments_1012)
    * [Migrating from Node.js 8 to Node.js 10](/docs/openwhisk?topic=openwhisk-runtimes#migrate_javascript_environments_810)
    * [Node.js packages](/docs/openwhisk?topic=openwhisk-runtimes#javascript_packages)

* [Python runtimes](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments)
    * [Python packages](/docs/openwhisk?topic=openwhisk-runtimes#python_packages)

* [Swift runtime](/docs/openwhisk?topic=openwhisk-runtimes#swift-actions)

* [PHP runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_php)

* [Docker runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_docker)
    * [Docker skeleton](/docs/openwhisk?topic=openwhisk-runtimes#docker_skeleton)

* [More runtime support](/docs/openwhisk?topic=openwhisk-runtimes#more_runtime_support)

* [Available images and runtimes per region](/docs/openwhisk?topic=openwhisk-runtimes#runtimes_per_region)


## System details and limits
{: #sitemap_system_details_and_limits}


[System details and limits](/docs/openwhisk?topic=openwhisk-limits)

* [Action limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions)

* [Web action limits](/docs/openwhisk?topic=openwhisk-limits#web_action_limits)

* [Sequence limits](/docs/openwhisk?topic=openwhisk-limits#limits_sequence)

* [Trigger rate limits](/docs/openwhisk?topic=openwhisk-limits#limits_triggers)

* [Increasing fixed limits](/docs/openwhisk?topic=openwhisk-limits#limits_fixed)

* [{{site.data.keyword.openwhisk_short}} entities](/docs/openwhisk?topic=openwhisk-limits#limits_entities_ov)
    * [Namespaces and packages](/docs/openwhisk?topic=openwhisk-limits#limits_namespaces)
    * [Fully qualified names in {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-limits#limits_fullnames)

* [Action semantics](/docs/openwhisk?topic=openwhisk-limits#limits_semantics)
    * [Statelessness](/docs/openwhisk?topic=openwhisk-limits#limits_stateless)
    * [Invocation input and output](/docs/openwhisk?topic=openwhisk-limits#limits_invocationio)
    * [Invocation ordering of actions](/docs/openwhisk?topic=openwhisk-limits#limits_ordering)
    * [Action executions](/docs/openwhisk?topic=openwhisk-limits#limits_exec)

* [Activation record](/docs/openwhisk?topic=openwhisk-limits#limits_activation)


## Error handing for {{site.data.keyword.openwhisk_short}}
{: #sitemap_error_handing_for_}


[Error handing for {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-error-handing-functions)


## CLI reference
{: #sitemap_cli_reference}


[{{site.data.keyword.openwhisk_short}} CLI](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli)
* [Action commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action)
  * [`ibmcloud fn action create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create)
  * [`ibmcloud fn action delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_delete)
  * [`ibmcloud fn action get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_get)
  * [`ibmcloud fn action invoke`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_invoke)
  * [`ibmcloud fn action list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_list)
  * [`ibmcloud fn action update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_update)
* [Activation commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation)
  * [`ibmcloud fn activation get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_get)
  * [`ibmcloud fn activation list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_list)
  * [`ibmcloud fn activation logs`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_logs)
  * [`ibmcloud fn activation poll`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_poll)
  * [`ibmcloud fn activation result`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_result)
* [API commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_api)
  * [`ibmcloud fn api create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_api_create)
  * [`ibmcloud fn api delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_api_delete)
  * [`ibmcloud fn api get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_api_get)
  * [`ibmcloud fn api list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_api_list)
* [Deployment commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_deploy_cmds)
  * [`ibmcloud fn deploy`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_deploy)
  * [`ibmcloud fn undeploy`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_undeploy)
* [List command](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_list_cmd)
  * [`ibmcloud fn list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_list)
* [Namespace commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace)
  * [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create)
  * [`ibmcloud fn namespace delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_delete)
  * [`ibmcloud fn namespace get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_get)
  * [`ibmcloud fn namespace list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_list)
  * [`ibmcloud fn namespace target`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_target)
  * [`ibmcloud fn namespace update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_update)
* [Package commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg)
  * [`ibmcloud fn package bind`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind)
  * [`ibmcloud fn package create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_create)
  * [`ibmcloud fn package delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_delete)
  * [`ibmcloud fn package get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_get)
  * [`ibmcloud fn package list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_list)
  * [`ibmcloud fn package refresh`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_refresh)
  * [`ibmcloud fn package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_update)
* [Property commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_prop)
  * [`ibmcloud fn property get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_prop_get)
  * [`ibmcloud fn property set`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_prop_set)
  * [`ibmcloud fn property unset`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_prop_unset)
* [Rule commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule)
  * [`ibmcloud fn rule create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_create)
  * [`ibmcloud fn rule delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_delete)
  * [`ibmcloud fn rule disable`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_disable)
  * [`ibmcloud fn rule enable`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_enable)
  * [`ibmcloud fn rule get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_get)
  * [`ibmcloud fn rule list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_list)
  * [`ibmcloud fn rule status`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_status)
  * [`ibmcloud fn rule update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_update)
* [SDK command](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_sdk)
  * [`ibmcloud fn sdk install`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_sdk_install)
* [Service commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_service)
  * [`ibmcloud fn service bind`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_service_bind)
  * [`ibmcloud fn service unbind`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_service_unbind)
* [Trigger commands](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger)
  * [`ibmcloud fn trigger create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_create)
  * [`ibmcloud fn trigger delete`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_delete)
  * [`ibmcloud fn trigger fire`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_fire)
  * [`ibmcloud fn trigger get`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_get)
  * [`ibmcloud fn trigger list`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_list)
  * [`ibmcloud fn trigger update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_update)

[CLI version history](/docs/openwhisk?topic=openwhisk-cli_versions)


## Regions
{: #sitemap_regions}


[Regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions)

* [{{site.data.keyword.openwhisk_short}} endpoints](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions#cloudfunctions_endpoints)

* [{{site.data.keyword.openwhisk_short}} web action endpoints](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions#cloudfunctions_webaction_endpoints)


## Understanding high availability and disaster recovery for {{site.data.keyword.openwhisk_short}}
{: #sitemap_understanding_high_availability_and_disaster_recovery_for_}


[Understanding high availability and disaster recovery for {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=openwhisk-ha_dr)

* [High availability](/docs/openwhisk?topic=openwhisk-ha_dr#high-availability)

* [Disaster recovery](/docs/openwhisk?topic=openwhisk-ha_dr#disaster-recovery)

* [HIPAA](/docs/openwhisk?topic=openwhisk-ha_dr#hippa)


## API reference

[API reference](https://cloud.ibm.com/apidocs/functions){: new_window}{: external}


## Related links
{: #sitemap_related_links}


[Discover: IBM Cloud Functions](https://www.ibm.com/cloud/functions){: new_window}{: external}

[Pricing](https://cloud.ibm.com/functions/learn/pricing){: new_window}{: external}

[Apache OpenWhisk](http://openwhisk.apache.org/){: new_window}{: external}

[Tutorials and samples](https://github.com/apache/openwhisk-external-resources){: new_window}{: external}


## FAQ
{: #sitemap_faq}


[FAQ](/docs/openwhisk?topic=openwhisk-faq)

* [What language runtimes are supported in {{site.data.keyword.openwhisk_short}}?](/docs/openwhisk?topic=openwhisk-faq#supported-runtimes)

* [What's the maximum time or maximum memory that my function can run?](/docs/openwhisk?topic=openwhisk-faq#max-runtime)

* [What's the difference between an action and a web action?](/docs/openwhisk?topic=openwhisk-faq#difference)

* [How can I see my action logs?](/docs/openwhisk?topic=openwhisk-faq#logs_faq)

* [How does monitoring work?](/docs/openwhisk?topic=openwhisk-faq#monitor_faq)


## Troubleshooting
{: #sitemap_troubleshooting}


[Troubleshooting overview](/docs/openwhisk?topic=openwhisk-troubleshooting)
* [General ways to resolve issues](/docs/openwhisk?topic=openwhisk-troubleshooting#help-general)
* [Reviewing cloud issues and status](/docs/openwhisk?topic=openwhisk-troubleshooting#help-cloud-status)
* [Getting help](/docs/openwhisk?topic=openwhisk-troubleshooting#help-functions)


## General troubleshooting
{: #sitemap_general_troubleshooting}


[Action is failing](/docs/openwhisk?topic=openwhisk-ts_action_fails)

[Service configuration properties are incorrect](/docs/openwhisk?topic=openwhisk-ts_property)

[System limits were reached](/docs/openwhisk?topic=openwhisk-ts_limit_reached)

[Action terminates after one minute](/docs/openwhisk?topic=openwhisk-ts_action_terminated)

[{{site.data.keyword.apigw_short}} fails with HTTP status code 504](/docs/openwhisk?topic=openwhisk-ts_api_gateway_504)

[Can't access private endpoint from action](/docs/openwhisk?topic=openwhisk-ts_private_endpoint)

[Node.js action is failing](/docs/openwhisk?topic=openwhisk-ts_action_nodejs_fails)

[Memory usage with Node.js runtime actions](/docs/openwhisk?topic=openwhisk-memory_usage_ts)


## Release notes
{: #sitemap_release_notes}


[Release notes](/docs/openwhisk?topic=openwhisk-release-notes)

* [June 2021](/docs/openwhisk?topic=openwhisk-release-notes#june21)

* [May 2021](/docs/openwhisk?topic=openwhisk-release-notes#may21)

* [April 2021](/docs/openwhisk?topic=openwhisk-release-notes#april21)

* [March 2021](/docs/openwhisk?topic=openwhisk-release-notes#march21)

* [January 2021](/docs/openwhisk?topic=openwhisk-release-notes#jan21)

* [December 2020](/docs/openwhisk?topic=openwhisk-release-notes#dec20)

* [October 2020](/docs/openwhisk?topic=openwhisk-release-notes#oct20)

* [September 2020](/docs/openwhisk?topic=openwhisk-release-notes#sept20)

* [August 2020](/docs/openwhisk?topic=openwhisk-release-notes#august20)

* [July 2020](/docs/openwhisk?topic=openwhisk-release-notes#july20)

* [June 2020](/docs/openwhisk?topic=openwhisk-release-notes#june20)

* [May 2020](/docs/openwhisk?topic=openwhisk-release-notes#may20)

* [April 2020](/docs/openwhisk?topic=openwhisk-release-notes#april20)

* [March 2020](/docs/openwhisk?topic=openwhisk-release-notes#mar20)

* [December 2019](/docs/openwhisk?topic=openwhisk-release-notes#dec19)

* [November 2019](/docs/openwhisk?topic=openwhisk-release-notes#nov19)

* [October 2019](/docs/openwhisk?topic=openwhisk-release-notes#oct19)

* [September 2019](/docs/openwhisk?topic=openwhisk-release-notes#sept19)


## Getting help and support
{: #sitemap_getting_help_and_support}


[Getting help and support](/docs/openwhisk?topic=openwhisk-gettinghelp)