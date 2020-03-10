---

copyright:
  years: 2020
lastupdated: "2020-2-23"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Troubleshooting Node.js issues
{: #troubleshooting-nodejs}

If you have problems while you're working with {{site.data.keyword.openwhisk}} and Node.js, consider these techniques for troubleshooting. 
{: shortdesc}

## Node.js action is failing
{: #ts_action_nodejs_fails}

{: tsSymptoms}
A Node.js action does not seem to run to completion reliably.

{: tsCauses}
If your Node.js action involves asynchronous calls to other services, your code might return from the `main()` function prematurely, which causes the action runtime to be stopped before your action is completed.

{: tsResolve}
The `main()` function of a Node.js action is a synchronous function, which expects your action to be complete when you return from the `main()` function. In case you are using asynchronous calls, for example, when fetching data from Cloud Object Storage or Cloudant instances, it is unpredictable as to when the `main()` function fully completes its work. To ensure that the action runs to completion, use a *Promise*. A Promise in Node.js is a stand-in for an object. Using a promise allows you to return the object from a synchronous function immediately, but the actual result becomes available at a later point in time, when the promise is fulfilled.

The {{site.data.keyword.openwhisk}} Node.js runtime container supports promises that are returned from the `main()` function. If you prefer working in a more synchronous fashion, you can also declare your `main()` function as an `async` function, which allows you to wait for asynchronous calls to complete before the function continues. 

**Examples:**

```
function main() {
	return new Promise((resolve, reject) => {
		...doGet(callback => {
			resolve();  // <-- this terminates action execution, as the Promise got resolved
		});
		...
		try {
			...some synchronous code...
		} catch(e) {
			...log error...
			reject(e);  // <-- this also terminates the execution, but reports the failure back to the runtime
		}
	});
}

async function main() {
	...some synchronous code...
	await doGet(....)   // <-- function continues synchronous execution, once this async call returned
	...some synchronous code...
}
```
{: pre}

Note that a function declared as `async` automatically returns a promise, so you don't have to explicitly return one. 

The following example does not necessarily work as expected, but might work intermittently, depending on the runtime of asynchronous code.  Do not use this type of pattern.

```
function main() {
	...some synchronous code...
	doGet(callback => {
		...log result...  // <-- this line is never reached, if the doGet is not complete before the action container gets stopped
	});
	...some synchronous code...  // <-- this code is usually executed BEFORE the doGet callback got processed. Make sure that this is what you expect to happen!
}
```
{: pre}

Instead, either use promises as part of the function or use `async/await`.

The following example performs the same type of function as in the previous example, but instead uses `async/await`. This type of function code runs reliably.

```
async function main() {
	...some synchronous code...
	await doGet(callback => {
		...log result...  // <-- this line gets now executed BEFORE the below synchronous code block is reached
	});
	...some synchronous code...  // <-- the whole function now behaves as if it was only using synchronous code
}
```
{: pre}

## Memory usage with Node.js runtime actions
{: #memory_usage_ts}


{: tsSymptoms}
In the case where an action consumes more memory than requested, the action is terminated and the following log information is displayed:

```
2019-10-22T10:00:50.509Z  stderr: Killed
2019-10-22T10:00:50.510Z  stderr: The action did not initialize or run as expected. Log data might be missing.
```
{: screen}

{: tsCauses}
When {{site.data.keyword.openwhisk_short}} runs successive invocations of the same action, the optimizations that are performed by {{site.data.keyword.openwhisk_short}} might consume more memory than expected in order to improve run times. 

For example, when {{site.data.keyword.openwhisk_short}} runs actions, Linux containers are used for the processes. To speed up the process, new containers are not created each time that your action runs ("cold"), but instead, existing containers that ran your action before ("warm") are reused. So when your action completes, the container "freezes" to pause all processes and then "wakes" when your action is rerun.

This approach affects garbage collection. When you run a Node.js action, garbage is created on the heap. This garbage is accumulating over each warm reuse of the action process. However, because the Node.js process pauses between invocations, garbage collection does not run reliably.

{: tsResolve}
Invoke Node.js garbage collection explicitly from within your action code by adding the option `--expose-gc`.

For example, use code similar to the following example code in your action:

```
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

