---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: actions, serverless, ruby

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

# Ruby 액션 작성
{: #creating-ruby-actions}

다음 절에서는 단일 Ruby 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다.  

Ruby 액션은 Ruby 2.5에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성 또는 업데이트할 때 `ibmcloud fn` CLI 매개변수
`--kind ruby:2.5`를 지정하십시오. 

## Ruby 액션 작성 및 호출
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

액션은 최상위 레벨 Ruby 메소드입니다.

예를 들어 `hello.rb`라는 파일을 작성하십시오.

1. `hello.rb` 파일에 다음 코드를 저장하십시오.

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby 액션은 항상 Hash(디렉토리 같은 콜렉션)를 이용하고 Hash를 리턴합니다.
    * 액션에 대한 진입 메소드는 기본적으로 `main`이지만, `--main` 플래그를 사용하여 `ibmcloud fn` CLI에서 액션을 작성할 때 지정될 수 있습니다. 

2. `helloRuby`라고 하는 액션을 작성하십시오. 

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    출력 예:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Ruby 액션을 패키징하십시오.
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

PHP 액션 및 기타 파일 또는 종속 패키지를 Zip 파일에 패키징할 수 있습니다. 예를 들어 `helper.rb`라는 두 번째 파일로 액션을 패키징할 수 있습니다.

1. 소스 파일이 포함된 아카이브를 작성하십시오. **참고**: 시작점이 포함된 소스 파일의 이름은 `main.rb`여야 합니다.
    

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. 액션을 작성하십시오.

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

기본 및 번들링된 gems 외에도 gems `mechanize` 및 `jwt`를 사용 가능합니다.
압축된 액션을 사용하여 모든 종속 항목을 패키징할 때마다 임의의 gems를 사용할 수 있습니다.
