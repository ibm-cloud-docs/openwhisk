---

copyright:
  years: 2021
lastupdated: "2021-06-18"

keywords: troubleshooting actions, functions, help, support, node.js, node, javascript

subcollection: openwhisk

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Node.js action is failing
{: #ts_action_nodejs_fails}

{: tsSymptoms}
A Node.js action does not seem to run to completion reliably.

{: tsCauses}
If your Node.js action involves asynchronous calls to other services, your code might return from the `main()` function prematurely, which causes the action runtime to be stopped before your action is completed.

{: tsResolve}
The `main()` function of a Node.js action is a synchronous function, which expects your action to be complete when you return from the `main()` function. In case you are using asynchronous calls, for example, when fetching data from Cloud Object Storage or Cloudant instances, it is unpredictable as to when the `main()` function fully completes its work. To ensure that the action runs to completion, use a *Promise*. A Promise in Node.js is a stand-in for an object. Using a promise allows you to return the object from a synchronous function immediately, but the actual result becomes available at a later point in time, when the promise is fulfilled.

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
