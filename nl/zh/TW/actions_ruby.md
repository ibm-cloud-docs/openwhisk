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

# 建立 Ruby 動作
{: #creating-ruby-actions}

下列各節會引導您建立及呼叫單一 Ruby 動作，並且將參數新增至該動作。 

Ruby 動作是在 Ruby 2.5 中執行。若要使用此運行環境，請在建立或更新動作時指定 `ibmcloud fn` CLI 參數 `--kind ruby:2.5`。

## 建立及呼叫 Ruby 動作
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

動作就只是最上層 Ruby 方法。

例如，建立稱為 `hello.rb` 的檔案。

1. 將下列程式碼儲存至稱為 `hello.rb` 的檔案中。

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby 動作一律使用「雜湊」（字典式集合），並傳回「雜湊」。
    * 動作的進入方法依預設是 `main`，但可以在您使用 `--main` 旗標，以 `ibmcloud fn` CLI 建立動作時指定。

2. 建立稱為 `helloRuby` 的動作。

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    輸出範例：

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 包裝 Ruby 動作。
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

您可以將 PHP 動作及其他檔案或相依套件包裝在 .zip 檔案中。例如，您可以使用第二個稱為 `helper.rb` 的檔案來包裝動作。

1. 建立包含原始檔的保存檔。**附註**：包含進入點的原始檔必須命名為 `main.rb`。

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. 建立動作。
    

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

除了預設及組合 Gem 之外，還提供 Gem `mechanize` 及 `jwt`。
只要使用壓縮動作來包裝所有相依關係，您就可以使用任意 Gem。
