---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} 대시보드로 활동 모니터링

[{{site.data.keyword.openwhisk}} 대시보드](https://{DomainName}/openwhisk/dashboard/)는 사용자의 활동에 대한 그래픽 요약을 제공합니다. 대시보드를 사용하여 {{site.data.keyword.openwhisk_short}} 조치의 성능과 상태를 판별할 수 있습니다.
{:shortdesc}

언제든지 **다시 로드**를 클릭하여 최신 활성화 로그 데이터로 대시보드를 업데이트할 수 있습니다. 

## 활동 요약
{: #summary}

이 보기는 사용자의 {{site.data.keyword.openwhisk_short}} 환경에 대한 상위 레벨 요약을 제공합니다. **활동 요약** 보기를 사용하면 {{site.data.keyword.openwhisk_short}} 사용 서비스의 전체 상태와 성능을 모니터할 수 있습니다. 이 보기의 메트릭에서 다음을 수행할 수 있습니다.
* 호출된 횟수를 보고 서비스의 {{site.data.keyword.openwhisk_short}} 사용 조치의 사용률을 판별합니다. 
* 모든 조치에서 전체 실패율을 판별합니다. 오류가 감지되면 **활동 히스토그램** 보기를 보고 오류가 발생한 서비스나 조치를 격리시킬 수 있습니다. **활동 로그**를 보고 오류 자체를 격리하십시오.
* 각 조치와 연관된 평균 완료 시간을 보고 조치가 얼마나 잘 수행되는지를 판별합니다. 

<!-- For tips on improving performance, see troubleshooting? -->

## 활동 타임라인
{: #timeline}

**활동 타임라인** 보기는 과거와 현재 조치의 활동을 보기 위한 세로 막대 그래프를 표시합니다. 빨간색은 특정 조치 내의 오류를 표시합니다. 이 보기를 **활동 로그**와 연관시키면 오류에 대한 상세 정보를 찾을 수 있습니다. 

## 활동 히스토그램
{: #histogram}

**활동 히스토그램** 보기는 과거와 현재 조치의 활동을 보기 위한 가로 막대 그래프를 표시합니다. 빨간색은 특정 조치 내의 오류를 표시합니다. 이 보기를 **활동 로그**와 연관시키면 오류에 대한 상세 정보를 찾을 수 있습니다. 

## 활동 로그
{: #log}

이 보기는 활성화 로그의 형식화된 버전을 표시합니다. 이는 모든 활성화의 세부사항을 표시하지만 새 활성화에 대해 1분마다 한 번씩 폴링합니다. 조치를 클릭하여 자세한 로그를 표시할 수 있습니다. 

CLI를 사용하여 활동 로그에 표시된 출력을 가져오려면 다음 명령을 사용하십시오.
{: tip}

  ```
  wsk activation poll
  ```
  {: pre}

## 필터 옵션
{: #filtering}

보고자 하는 조치 로그를 선택하고 로깅된 활동의 시간 범위를 선택하십시오. 

이러한 필터는 대시보드의 모든 보기에 적용됩니다.
{: tip}
