---

copyright:
  years: 2023, 2023
lastupdated: "2023-11-02"

keywords: functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Deprecation overview 
{: #dep-overview}
  
{{site.data.keyword.cloud}} is announcing the full deprecation of {{site.data.keyword.openwhisk}}. See the deprecation dates, details, and specific implications.
{: shortdesc}
  
## Which services are included in this change?
{: #dep-over-service}
  
{{site.data.keyword.openwhisk}}

## Why are we making this change?
{: #dep-over-why}
  
With the successful release and adoption of [{{site.data.keyword.codeenginefull}}](/docs/codeengine) in 2021, IBM created its next generation serverless compute platform. Functions is based on the Apache OpenWhisk project and is limited in its ability to support a varied set of non-transactional, stateless workloads. {{site.data.keyword.codeengineshort}} runs the same kind of workloads as Functions and, in addition, supports the following enhanced capabilities.
  
- Support for the latest generation of VPC networking
- Choice of public/internet-facing workloads or private ones
- Support for serverless, batch, and long running jobs
- Build container images via Dockerfiles or Paketo buildpacks
- Store source code in public or private git repos
- Deploy container images from public or private image registries
- Automatically secure apps with TLS
- Strategy for regulatory compliance for CaaS/FaaS solutions
  
## What are the implications for current users of Functions?
{: #dep-over-current-users}
  
Clients will need to migrate their Functions actions and triggers over to {{site.data.keyword.codeengineshort}}. You can expect a level of source code changes to be necessary with the migration, but for most users  and use cases, these changes will be minimal. {{site.data.keyword.codeengineshort}} is based on the mature, opensource, Knative serverless containers project that is used by Google Cloud and others. IBM and others created documentation about migrating your workloads to {{site.data.keyword.codeengineshort}} and to Knative more broadly. IBM support and lab services are available for customers with large Functions environments that need assistance.
  
## Deprecation schedule
{: #dep-over-schedule}
  
IBM Cloud for Education is withdrawn from marketing effective immediately. The service is deprecated in the {{site.data.keyword.cloud}} catalog, and no new orders or renewals are possible. Customers with existing deployments will still be able to use them through November 30th, 2023, or the expiry of their {{site.data.keyword.cloud}} order, whichever is first.

{{site.data.keyword.openwhisk_short}} will be withdrawn from marketing effective December 28, 2023. The service will be deprecated in the {{site.data.keyword.cloud}} catalog and at this time, no new orders will be taken. {{site.data.keyword.openwhisk_short}} will remains up and running for existing clients, but no new entities can be created.
  
Formal end of support for {{site.data.keyword.openwhisk}} will be in June, 2024. This is the last date {{site.data.keyword.cloud}} will deliver standard support, imaging, or reload services for a specific version or release.
  
{{site.data.keyword.openwhisk}} will be completely end of life and shut down in all regions by October 2024.
  
| Stage | Date | Description |
| ---------------- | ----------------- | ------------------------------------------------------------ |
| Deprecation announcement | 26 October 2023  | Announcement of the {{site.data.keyword.openwhisk_short}} deprecation. Existing entities will continue to run. |
| End of marketing | 28 December 2023 | No new entities of {{site.data.keyword.openwhisk_short}} can be created or purchased. Existing entities will continue to run. |
| End of support   | June 2024 | You can continue to use any existing entities of {{site.data.keyword.openwhisk_short}}, but support is no longer available.  |
| End of life | October 2024   | Running entities of {{site.data.keyword.openwhisk_short}} are permanently disabled and deprovisioned. |
{: caption="Table 1. Deprecation timeline" caption-side="bottom"}

## What if I'm still working with the service?
{: #dep-over-schedule}
  
Clients are encourages to migrate their Functions actions and triggers to {{site.data.keyword.codeenginefull}}. Alternatively, clients might decide to host their code on VPC virtual instances or in IBM Cloud Kubernetes or OpenShift for {{site.data.keyword.cloud}} clusters.

See the following topics for documentation about migrating from {{site.data.keyword.openwhisk}} to {{site.data.keyword.codeengineshort}}. 

- [Migrating Cloud Functions Code to Code Engine](https://www.ibm.com/blog/migrating-cloud-functions-code-to-code-engine){: external}
- [Migrating IBM Cloud Functions to Code Engine](/docs/codeengine?topic=codeengine-fun-migrate).
  
IBM representatives are reaching out to help customers plan for this change and decide on the best course of action. If you are a current {{site.data.keyword.openwhisk}} customer and have not heard from IBM, reach out to your account representative or contact [IBM Cloud Support](https://iam.bluemix.net/identity/mirror?state=initiating_transaction_id_djU0OHg-a5e8453733154995b0fabc7acac407e0){: external}.
  
## Have questions?
{: #dep-over-questions}
  
If you have questions, comments, or concerns, reach out to your {{site.data.keyword.cloud}} representative or [IBM Cloud Support](https://iam.bluemix.net/identity/mirror?state=initiating_transaction_id_djU0OHg-a5e8453733154995b0fabc7acac407e0){: external}.


  


