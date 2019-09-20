---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-19"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions

subcollection: cloud-functions

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

# Monitoring activity
{: #monitor}

Get insight into the performance of your actions deployed with {{site.data.keyword.openwhisk}}. Metrics can help you find bottlenecks or predict possible production problems based on action duration, results of action activations, or when you hit action activation limits. 
{: shortdesc}

Metrics are collected automatically for all entities. Depending on whether your actions are in an IAM-based or a Cloud Foundry-based namespace, metrics are located in the IBM Cloud account or space. These metrics are sent to {{site.data.keyword.monitoringlong}} and are made available through Grafana, where you can configure your dashboards, create alerts based on the metrics event values, and more. For more information about metrics, see the [{{site.data.keyword.monitoringlong_notm}} documentation](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started).

## Creating a dashboard
{: #monitor_dash}

Get started by creating a Grafana monitoring dashboard.
{: shortdesc}

1. Go to one of the following URLs.
  <table>
    <thead>
      <tr>
        <th>{{site.data.keyword.openwhisk_short}} region</th>
        <th>Monitoring address</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Germany (EU Central)</td>
        <td>[`metrics.eu-de.bluemix.net`](https://metrics.eu-de.bluemix.net)</td>
      </tr>
      <tr>
        <td>United Kingdom (UK South)</td>
        <td>[`metrics.eu-gb.bluemix.net`](https://metrics.eu-gb.bluemix.net)</td>
      </tr>
      <tr>
        <td>Dallas (US South)</td>
        <td>[`metrics.ng.bluemix.net`](	https://metrics.ng.bluemix.net)</td>
      </tr>
      <tr>
        <td>AP Sydney</td>
        <td>[`metrics.au-syd.bluemix.net](https://metrics.au-syd.bluemix.net)</td>
      </tr>
    </tbody>
  </table>
2. Select the metrics domain.
    * IAM-based namespaces:
        1. Click your user name.
        2. In the **Domain** drop-down list, select **account**.
        3. In the **Account** drop-down list, select the IBM Cloud account where your IAM-based namespace is located.
    * Cloud Foundry-based namespaces:
        1. Click your user name.
        2. In the **Domain** drop-down list, select **space**.
        3. Use the **Organization** and **Space** drop-down lists to select your Cloud Foundry-based namespace.
3. Create a dashboard.
    * To use the pre-made {{site.data.keyword.openwhisk_short}} dashboard:
        1. Navigate to **Home > Import**.
        2. Enter the ID for the pre-made {{site.data.keyword.openwhisk_short}} dashboard, `8124`, into the **Grafana.net Dashboard** field.
        3. Click **Import**.
    * To create a custom dashboard, navigate to **Home > Create New**.

After an action is executed, new metrics are generated and are searchable in Grafana. Note: It can take up to 10 minutes for the executed action to display in Grafana.

## Using the dashboards
{: #monitor_dash_use}

The [{{site.data.keyword.openwhisk_short}} Dashboard](https://cloud.ibm.com/openwhisk/dashboard){: external} provides a graphical summary of your activity. Use the dashboard to determine the performance and health of your {{site.data.keyword.openwhisk_short}} actions.
{:shortdesc}

You can filter logs by selecting which action logs you want to view, and select the time frame of the activity logged. These filters are applied to all views on the dashboard. Click **Reload** at any time to update the dashboard with the latest activation log data.

### Activity Summary
{: #monitor_dash_sum}

The **Activity Summary** view provides a high-level summary of your {{site.data.keyword.openwhisk_short}} environment. Use this view to monitor the overall health and performance of your {{site.data.keyword.openwhisk_short}}-enabled service.
{:shortdesc}

From the metrics in this view, you can do the following:
* Determine the usage rate of your service's {{site.data.keyword.openwhisk_short}}-enabled actions by viewing the number of times that they were invoked.
* Determine the overall rate of failure across all actions. If you see an error, you can isolate which services or actions had errors by viewing the **Activity Histogram** view. Isolate the errors themselves by viewing the **Activity Log**.
* Determine how well your actions are performing by viewing the average completion time that is associated with each action.

### Activity Timeline
{: #monitor_dash_time}

The **Activity Timeline** view displays a vertical bar graph for viewing the activity of past and present actions. Red indicates errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
{:shortdesc}


### Activity Log
{: #monitor_dash_log}

This **Activity Log** view displays a formatted version of the activation log. This view shows the details of every activation, but polls once a minute for new activations. Click an action to display a detailed log.
{:shortdesc}

To get the output displayed in the Activity Log by using the CLI, use the following command:
```
ibmcloud fn activation poll
```
{: pre}

## Metric format
{: #monitor_metric}

The metrics reflect data that is collected from your action activations that is aggregated on a per-minute basis. Metrics are searchable on the action performance or action concurrency level.

### Action performance metrics
{: #monitor_metric_perf}

Action performance metrics are values that are calculated for a single action. Action performance metrics encompass both the timing characteristics of the executions and the status of the activations. Note: If you don't specify the name of a package during creation, then the default package name is used. These metrics take the following format:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.<package>.<action>.<metric_name>
```
{: codeblock}

The following characters are converted to dashes (`-`): period (.), an at sign (@), whitespace ( ), ampersand (&), underscore (_), colon (:)
{: tip}

Example: If you have an action that is named `hello-world` in the Cloud Foundry-based namespace `user@email.com_dev` in the `us-south` region, an action performance metric would look similar to the following:

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### Action concurrency metrics
{: #monitor_metric_con}

Action concurrency metrics are calculated based on the data from all active actions in a namespace. Action concurrency includes the number of concurrent invocations and the system throttles that could potentially occur when the concurrency limits are exceeded. These metrics take the following format:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.action-performance.<package>.<action>.<metric_name>
```
{: codeblock}

Example: If you have an IAM-based namespace that is named `myNamespace` in the `us-south` region, an action concurrency metric would look similar to the following:

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### Available metrics
{: #monitor_metric_av}

Because you might have thousands or millions of action activations, the metric values are represented as an aggregation of events produced by many activations. The values are aggregated in the following ways:

* Sum: All metric values are added.
* Average: An arithmetical mean is calculated.
* Summed average: An arithmetical mean is calculated based on components and adding different components together.

Check out the following table to see the metrics that are available to you.

<table>
  <thead>
    <tr>
      <th>Metric name</th>
      <th>Description</th>
      <th>Type</th>
      <th>Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>The average action duration, billed action execution time.</td>
      <td>Average</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>The time that is spent to initialize the action container.</td>
      <td>Average</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>The average time that is spent in a queue waiting for an activation to be scheduled.</td>
      <td>Average</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>The overall number of activations that were triggered in the system.</td>
      <td>Sum</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>The number of successful activations of action code.</td>
      <td>Sum</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>The number of unsuccessful activations that are caused by application errors. For example, graceful errors from the actions. For more information about how the action-performance metrics are derived, check out [Understanding the activation record](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record){: external}.</td>
      <td>Sum</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>The number of unsuccessful activations that are caused by the developer. For example, the violation of the [action proxy interface](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface){: external} by unhandled exceptions in the action code.</td>
      <td>Sum</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>The number of unsuccessful activations caused by {{site.data.keyword.openwhisk_short}} internal errors.</td>
      <td>Sum</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>The sum of activations that were throttled due to exceeding the concurrency rate limit. No metric is emitted if the limit is not reached.</td>
      <td>Sum</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>The sum of activations that were throttled due to exceeding the per minute limit. No metric is emitted if the limit is not reached.</td>
      <td>Sum</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>The number of concurrent invocations in the system.</td>
      <td>Summed average</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

Metrics for actions that exist as part of a default namespace are available in the default category.
{: tip}
