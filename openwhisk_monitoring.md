---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Monitor activity with Dashboard
{: #openwhisk_monitoring}

The [{{site.data.keyword.openwhisk}} Dashboard](https://console.bluemix.net/openwhisk/dashboard/) provides a graphical summary of your activity. Use the dashboard to determine the performance and health of your {{site.data.keyword.openwhisk_short}} actions.
{:shortdesc}

Click **Reload** at any time to update the dashboard with the latest activation log data.

## Activity Summary
{: #summary}

This view provides a high-level summary of your {{site.data.keyword.openwhisk_short}} environment. Use the **Activity Summary** view to monitor the overall health and performance of your {{site.data.keyword.openwhisk_short}} enabled service. From the metrics in this view, you can do the following:
* Determine the usage rate of your service's {{site.data.keyword.openwhisk_short}} enabled actions by viewing the number of times that they were invoked.
* Determine the overall rate of failure across all actions. If you spot an error, you can isolate which services or actions had errors by viewing the **Activity Histogram** view. Isolate the errors themselves by viewing the **Activity Log**.
* Determine how well your actions are performing by viewing the average completion time that is associated with each action.

<!-- For tips on improving performance, see troubleshooting? -->

## Activity Timeline
{: #timeline}

The **Activity Timeline** view displays a vertical bar graph for viewing the activity of past and present actions. Red indicates errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.

## Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red indicates errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.

## Activity Log
{: #log}

This view displays a formatted version of the activation log. It shows the details of every activation, but polls once a minute for new activations. Click an action to display a detailed log.

To get the output displayed in the Activity Log by using CLI, use the following command:
```
ibmcloud wsk activation poll
```
{: pre}

## Filter options
{: #filtering}

Select which action logs you want to view, and select the time frame of the activity logged.

These filters are applied to all views on the dashboard.
{: tip}
