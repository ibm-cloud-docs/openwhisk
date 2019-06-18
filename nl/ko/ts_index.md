---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 문제점 해결
{: #troubleshooting}

{{site.data.keyword.openwhisk}}에 대해 작업하는 동안 문제가 발생하면 이러한 문제점을 해결하고 도움을 얻기 위해 다음 방법을 고려해 보십시오.
{: shortdesc}



## 액션이 실패함
{: #ts_action_fails}

{: tsSymptoms}
액션이 실패합니다. 

{: tsCauses}
앱이 실패하는 한 가지 이유는 더 이상 사용되지 않는 런타임이 사용 중이기 때문입니다. 액션은 런타임이 지원되는 런타임으로 업데이트될 때까지 성공적으로 완료할 수 없습니다. 

{: tsResolve}
액션에 사용되는 런타임을 확인하려면 `ibmcloud fn action get ACTION_NAME`을 실행하고 조회 응답에서 `deprecated=true`를 확인하십시오. 런타임이 더 이상 사용되지 않으면 [런타임을 업데이트](/docs/openwhisk?topic=cloud-functions-actions#actions_update)하십시오.


<br />


## 시스템 한계에 도달함
{: #ts_limit_reached}

{: tsSymptoms}
기능이 제대로 작동하지 않고 `{"error":"signal: killed"}`와 같은 메시지가 표시될 수 있습니다.

{: tsCauses}
모든 기능에는 최소 및 최대 [시스템 한계](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)(예: 최대 메모리 사용량 또는 제한시간)가 있습니다. 

{: tsResolve}
일부 한계는 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 한계 값을 늘리려면 IBM Cloud Functions 웹 콘솔에서 직접 티켓을 열어서 IBM 지원 센터에 문의하십시오.

1. **지원**을 선택하십시오.
2. 드롭 다운 메뉴에서 **티켓 추가**를 선택하십시오.
3. 티켓 유형에 대해 **기술적**을 선택하십시오.
4. 지원의 기술 영역에 대해 **함수**를 선택하십시오.


<br />


## 도움 및 지원 받기
{: #gettinghelp}

아직도 기능에 문제가 있습니까?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}}의 사용 가능 여부를 알아보려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지를 확인![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/status?selected=status)하십시오.
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생했는지 확인하십시오. 포럼을 사용하여 질문을 하는 경우 {{site.data.keyword.Bluemix_notm}} 개발 팀에서 확인하도록 질문에 태그를 지정하십시오.
    -   {{site.data.keyword.openwhisk}}에 대한 기술적인 질문이 있는 경우, 질문을 [스택 오버플로우 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/search?q=ibm-cloud-functions)에 게시하고 이 질문에 `ibm-cloud-functions` 태그를 지정하십시오.
    -   서비스 및 시작된 지시사항에 대한 질문은 [IBM Developer Answers ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `functions` 태그를 포함하십시오.
    포럼 사용에 대한 자세한 정보는 [도움 받기](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)를 참조하십시오. 
-   사례를 열어 IBM 지원 센터에 문의하십시오. IBM 지원 사례 열기 또는 레벨 및 사례 심각도에 대해 알아보려면 [지원에 문의](/docs/get-support?topic=get-support-getting-customer-support)를 참조하십시오.
문제를 보고할 때 활성화 ID를 포함하십시오. 활성화 ID를 얻으려면 `ibmcloud fn activation list`를 실행하십시오.
{: tip}
