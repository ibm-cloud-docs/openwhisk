---

copyright:
  years: 2021
lastupdated: "2021-01-29"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions, sysdig

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

# {{site.data.keyword.mon_full_notm}}
{: #monitor-sysdig}

Get insight into the performance of your actions that are deployed with {{site.data.keyword.openwhisk}}. Metrics can help you find bottlenecks or predict possible production problems based on action duration, results of action activations, or hitting action activation limits.
{: shortdesc}

Monitoring of your {{site.data.keyword.openwhisk_short}} actions is provided through the {{site.data.keyword.mon_full}} service. {{site.data.keyword.openwhisk_short}} service forwards selected information about your actions to {{site.data.keyword.mon_short}} so that you can monitor specific metrics such as status, duration, rate limits, and concurrency.

## Set up your {{site.data.keyword.mon_full_notm}} service instance
{: #setup-monitor}

To set up your {{site.data.keyword.openwhisk_short}} customer metrics dashboards in {{site.data.keyword.mon_short}}, you must create a service instance and then enable Platform Metrics in the same region as the {{site.data.keyword.openwhisk_short}} actions that you want to monitor. If you have deployments in more than one region, you must provision {{site.data.keyword.mon_short}} and enable platform metrics for each region.
{: shortdesc}

To set up {{site.data.keyword.mon_short}},

1. From the {{site.data.keyword.cloud_notm:}} navigation menu, select **Observability**.
2. Select **Monitoring**.
3. Either use an existing {{site.data.keyword.mon_short}} service instance or create a new one.
4. After the instance is ready, enable platform metrics by clicking **Configure platform metrics**.
5. Select a region and then a {{site.data.keyword.mon_short}} instance from that region. Note that if you have deployments in more than one region, you must provision {{site.data.keyword.mon_short}} and enable platform metrics for each region.

Provisioning your monitor might take a few minutes and that you must run an action or trigger in the same region as the monitor before metrics are displayed.
{: note}

You can also launch monitoring from your {{site.data.keyword.openwhisk_short}} dashboard by selecting **Launch Monitoring**.
{: tip}

## Accessing your {{site.data.keyword.mon_full_notm}} metrics
{: #access-monitor}

To see your {{site.data.keyword.openwhisk_short}} customer metrics dashboards in {{site.data.keyword.mon_short}}:

1. From the IBM Cloud navigation menu, select **Observability**.
2. Select **Monitoring**.
3. Select **View Sysdig** to view the dashboard.
4. From the navigation menu, select **Dashboards->IBM->Cloud Functions - Customer Metrics Dashboard**.

<br />
## Metrics available by Service Plan
{: #metrics-by-plan}

All metric names have the prefix `ibm_functions_`.

| Metric Name |
|-----------|
| [ibm_functions_activation](#ibm_functions_activation) | 
| [ibm_functions_concurrent-invocations](#ibm_functions_concurrent-invocations) | 
| [ibm_functions_concurrent-rate-limit](#ibm_functions_concurrent-rate-limit) | 
| [ibm_functions_duration](#ibm_functions_duration) | 
| [ibm_functions_init-time](#ibm_functions_init-time) | 
| [ibm_functions_status-error-application](#ibm_functions_status-error-application) | 
| [ibm_functions_status-error-developer](#ibm_functions_status-error-developer) | 
| [ibm_functions_status-error-internal](#ibm_functions_status-error-internal) | 
| [ibm_functions_status-success](#ibm_functions_status-success) | 
| [ibm_functions_timed-rate-limit](#ibm_functions_timed-rate-limit) | 
| [ibm_functions_wait-time](#ibm_functions_wait-time) | 
{: caption="Table 1: Metrics Available by Plan Names" caption-side="top"}

### ibm_functions_activation
{: #ibm_functions_activation}

The overall number of activations that were triggered on the system.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_activation`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 2: ibm_functions_activation metric metadata" caption-side="top"}

### ibm_functions_concurrent-invocations
{: #ibm_functions_concurrent-invocations}

The number of concurrent invocations on the system.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_concurrent-invocations`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 3: ibm_functions_concurrent-invocations metric metadata" caption-side="top"}

### ibm_functions_concurrent-rate-limit
{: #ibm_functions_concurrent-rate-limit}

The sum of activations that were throttled due to exceeding the concurrency rate limit.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_concurrent-rate-limit`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 4: ibm_functions_concurrent-rate-limit metric metadata" caption-side="top"}

### ibm_functions_duration
{: #ibm_functions_duration}

The average action duration, billed action execution time in milliseconds.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_duration`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 5: ibm_functions_duration metric metadata" caption-side="top"}

### ibm_functions_init-time
{: #ibm_functions_init-time}

The time that is spent to initialize the action container in milliseconds.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_init-time`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 6: ibm_functions_init-time metric metadata" caption-side="top"}

### ibm_functions_status-error-application
{: #ibm_functions_status-error-application}

The number of unsuccessful activations that are caused by application errors.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_status-error-application`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 7: ibm_functions_status-error-application metric metadata" caption-side="top"}

### ibm_functions_status-error-developer
{: #ibm_functions_status-error-developer}

The number of unsuccessful activations that are caused by the developer.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_status-error-developer`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 8: ibm_functions_status-error-developer metric metadata" caption-side="top"}

### ibm_functions_status-error-internal
{: #ibm_functions_status-error-internal}

The number of unsuccessful activations that are caused by {{site.data.keyword.openwhisk_short}} internal errors.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_status-error-internal`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 9: ibm_functions_status-error-internal metric metadata" caption-side="top"}

### ibm_functions_status-success
{: #ibm_functions_status-success}

The number of successful activations of action code.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_status-success`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 10: ibm_functions_status-success metric metadata" caption-side="top"}

### ibm_functions_timed-rate-limit
{: #ibm_functions_timed-rate-limit}

The sum of activations that were throttled due to exceeding the per-minute limit.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_timed-rate-limit`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 11: ibm_functions_timed-rate-limit metric metadata" caption-side="top"}

### ibm_functions_wait-time
{: #ibm_functions_wait-time}

The average time that is spent in a queue, waiting for an activation to be scheduled in milliseconds.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_functions_wait-time`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance, ibm_functions_namespace, ibm_functions_package, ibm_functions_action` |
{: caption="Table 12: ibm_functions_wait-time metric metadata" caption-side="top"}

## Attributes for Segmentation
{: attributes}

### Global Attributes
{: global-attributes}

The following attributes are available for segmenting all of the metrics previously listed.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type, which is always set to `bluemix`. |
| `Location` | `ibm_location` | The region of the monitored resource. Possible regions include `us-south`, `eu-de`, or `au-syd`. |
| `Scope` | `ibm_scope` | The scope is the account, organization, or space GUID associated with this metric. |
{: caption="Table 13: Global attributes" caption-side="top"}

### Additional Attributes
{: additional-attributes}

The following attributes are available for segmenting one or more attributes as described in the previous reference. See the [individual metrics](#metrics-by-plan) for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Service instance` | `ibm_service_instance` | The service instance segment identifies the instance that the metric is associated with. |
| `ibm_functions_action` | `ibm_functions_action` | The action name. |
| `ibm_functions_namespace` | `ibm_functions_namespace` | The namespace that contains the action. |
| `ibm_functions_package` | `ibm_functions_package` | The package that contains the action. |
{: caption="Table 14: Additional attributes" caption-side="top"}
