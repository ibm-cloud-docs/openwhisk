---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-21"

keywords: actions, serverless, functions, utilities, split, sort, hosturl, date, head, echo, cat, smash

subcollection: openwhisk

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

# Utilities
{: #pkg_utils}

Several utility actions are provided in the `/whisk.system/utils` package for {{site.data.keyword.openwhisk}}. This example creates a sequence with several actions from the `utils` package.
{: shortdesc}

1. List the actions in the `/whisk.system/utils` package.

   ```
   ibmcloud fn package get --summary /whisk.system/utils
   ```
   {: pre}

   **Example output**

   ```
   package /whisk.system/utils: Building blocks that format and assemble data
    action /whisk.system/utils/smash: Nests all properties under given property
    action /whisk.system/utils/date: Current date and time
    action /whisk.system/utils/hosturl: Returns the URL to activation an action or trigger
    action /whisk.system/utils/head: Extract prefix of an array
    action /whisk.system/utils/cat: Concatenates input into a string
    action /whisk.system/utils/namespace: Returns namespace for the authorization key used to invoke this action
    action /whisk.system/utils/split: Split a string into an array
    action /whisk.system/utils/sort: Sorts an array
    action /whisk.system/utils/echo: Returns the input
   ```
   {: screen}

2. Using the `split` and `sort` actions, create an action sequence so that the result of `split` is passed as an argument to `sort`. This action sequence converts some lines of text to an array, and sorts the lines.

   ```
   ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
   ```
   {: pre}

3. Invoke the action.

   ```
   ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
   ```
   {: pre}
   
   **Example output**

   In the output, the split lines are sorted alphabetically.
   
   ```
   {
       "length": 3,
       "lines": [
           "Is full of regret.",
           "Over-ripe sushi,",
           "The Master"
       ]
   }
   ```
   {: screen}
