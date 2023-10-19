---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: actions, serverless, functions, utilities, split, sort, hosturl, date, head, echo, cat, smash

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Utilities
{: #pkg_utils}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Several utility actions are provided in the `/whisk.system/utils` package for {{site.data.keyword.openwhisk}}. This example creates a sequence with several actions from the `utils` package.
{: shortdesc}

1. List the actions in the `/whisk.system/utils` package.

    ```sh
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **Example output**

    ```sh
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

    ```sh
    ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
    ```
    {: pre}

3. Invoke the action.

    ```sh
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    **Example output**

    In the output, the split lines are sorted alphabetically.

    ```sh
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


