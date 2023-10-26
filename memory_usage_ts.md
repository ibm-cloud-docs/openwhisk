---

copyright:
  years: 2023
lastupdated: "2023-10-19"

keywords: troubleshooting actions, functions, help, support, node.js, node, javascript

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Memory usage with Node.js runtime actions
{: #memory_usage_ts}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}


In the case where an action consumes more memory than requested, the action is terminated and the following log information is displayed:
{: tsSymptoms}

```sh
2019-10-22T10:00:50.509Z  stderr: Killed
2019-10-22T10:00:50.510Z  stderr: The action did not initialize or run as expected. Log data might be missing.
```
{: screen}

When {{site.data.keyword.openwhisk_short}} runs successive invocations of the same action, the optimizations that are performed by {{site.data.keyword.openwhisk_short}} might consume more memory than expected to improve run times. 
{: tsCauses}

For example, when {{site.data.keyword.openwhisk_short}} runs actions, Linux containers are used for the processes. To speed up the process, new containers are not created each time that your action runs ("cold"), but instead, existing containers that ran your action before ("warm") are reused. So when your action completes, the container "freezes" to pause all processes and then "wakes" when your action is rerun.

This approach affects garbage collection. When you run a Node.js action, garbage is created on the heap. This garbage is accumulating over each warm reuse of the action process. However, because the Node.js process pauses between invocations, garbage collection does not run reliably.

Invoke Node.js garbage collection explicitly from within your action code by adding the option `--expose-gc`.
{: tsResolve}

For example, use code similar to the following example code in your action:

```javascript
try {
    if (global.gc) {
    console.log("About to run garbage collection.");
    global.gc();
    console.log("Completed running garbage collection.");
    } else {
    console.log("Garbage collection not available.");
    }
} catch (e) {
    console.log("Garbage collection cannot be started: " + e);
}
```
{: pre}


