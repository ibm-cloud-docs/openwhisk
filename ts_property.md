---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Service configuration properties are incorrect
{: #ts_property}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Your properties are not being set correctly. For example, the incorrect namespace is targeted.
{: tsSymptoms}

When a property is set, it is retained on your workstation at `<home_dir>/.bluemix/plugins/cloud-functions/config.json`.
{: tsCauses}

You might review your properties by using the `ibmcloud fn property get --all` command. You might need to run `ibmcloud fn property unset --<property>` to unset a specific property, then reset it. Or you might unset all properties by deleting the `config.json` file. This config file is re-created when you work with the service, for example, when you run `ibmcloud fn property get --all`.
{: tsResolve}

