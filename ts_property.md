---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-01"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Service configuration properties are incorrect
{: #ts_property}

Your properties are not being set correctly. For example, the incorrect namespace is targeted.
{: tsSymptoms}

When a property is set, it is retained on your workstation at `<home_dir>/.bluemix/plugins/cloud-functions/config.json`.
{: tsCauses}

You might review your properties by using the `ibmcloud fn property get --all` command. You might need to run `ibmcloud fn property unset --<property>` to unset a specific property, then reset it. Or you might unset all properties by deleting the `config.json` file. This config file is re-created when you work with the service, for example, when you run `ibmcloud fn property get --all`.
{: tsResolve}

