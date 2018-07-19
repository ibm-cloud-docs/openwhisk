# Cloud Functions - Internal/Staging Docs Repo

Welcome to the internal/staging docs repo for IBM Cloud Functions!

To suggest doc changes:
1. Make a PR in this repo and tag `Rachael-Graham` for review.
2. Once reviewed and merged, the doc will take about an hour or less to build into the docs stage framework at https://console.stage1.bluemix.net/docs/openwhisk/index.html
    * Note: Viewing the stage frameowrk requires a federated IBM ID, so external customers can't see what is built there.
3. Verify your changes in the staging framework.
4. Once you are ready for the doc changes to go out to production/become available to external customers, ping `rachael.graham` on slack.
5. Rachael will mirror the changes for you in the [production repo](https://github.com/IBM-Bluemix-Docs/openwhisk).
6. Once merged in the production repo, the doc will take about an hour or more to build into the docs production framework at https://console.bluemix.net/docs/openwhisk/index.html

Note that conrefs are being used in these files. Variables like `{{site.data.keyword.openwhisk_short}}` in the markdown call the master list of IBM-approved product names. For example, `openwhisk_short` renders as `Cloud Functions` (no IBM or trademark character) in the staging and production doc frameworks. 
If you want to use conrefs for product names, you can check them in [this file](https://github.ibm.com/Bluemix-Docs/docs-build/blob/master/markdown/cloudoeconrefs.yml#L796). Otherwise, Rachael will replace product names during the review of your PR!
