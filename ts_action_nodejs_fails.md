---

copyright:
  years: 2022
lastupdated: "2022-02-03"

keywords: troubleshooting actions, functions, help, support, node.js, node, javascript

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Node.js action is failing
{: #ts_action_nodejs_fails}

A Node.js action does not seem to run to completion reliably.
{: tsSymptoms}

If your Node.js action involves asynchronous calls to other services, your code might return from the `main()` function prematurely, which causes the action runtime to be stopped before your action is completed.
{: tsCauses}

The `main()` function of a Node.js action is a synchronous function, which expects your action to be complete when you return from the `main()` function. In case you are using asynchronous calls, for example, when fetching data from Cloud Object Storage or Cloudant instances, it is unpredictable as to when the `main()` function fully completes its work. To ensure that the action runs to completion, use a *Promise*. A Promise in Node.js is a stand-in for an object. Using a promise allows you to return the object from a synchronous function immediately, but the actual result becomes available at a later point in time, when the promise is fulfilled.
{: tsResolve}

The {{site.data.keyword.openwhisk}} Node.js runtime container supports promises that are returned from the `main()` function. If you prefer working in a more synchronous fashion, you can also declare your `main()` function as an `async` function, which allows you to wait for asynchronous calls to complete before the function continues. 

**Examples:**

```javascript
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

```javascript
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

```javascript
async function main() {
	...some synchronous code...
	await doGet(callback => {
		...log result...  // <-- this line gets now executed BEFORE the below synchronous code block is reached
	});
	...some synchronous code...  // <-- the whole function now behaves as if it was only using synchronous code
}
```
{: pre}


