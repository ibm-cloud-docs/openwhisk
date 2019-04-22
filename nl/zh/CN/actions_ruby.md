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

# 创建 Ruby 操作
{: #creating-ruby-actions}

以下各部分将指导您创建并调用单个 Ruby 操作，然后向该操作添加参数。 

Ruby 操作在 Ruby 2.5 中执行。要使用此运行时，请在创建或更新操作时，指定 `ibmcloud fn` CLI 参数 `--kind ruby:2.5`。

## 创建并调用 Ruby 操作
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

操作仅仅是顶级 Ruby 方法。

例如，创建名为 `hello.rb` 的文件。

1. 将以下代码保存在名为 `hello.rb` 的文件中。

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby 操作始终使用散列（类似于字典的集合）并返回散列。
    * 缺省情况下，操作的入口方法为 `main`，但使用 `ibmcloud fn` CLI 通过 `--main` 标志来创建操作时，可指定入口方法。

2. 创建名为 `helloRuby` 的操作。

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    示例输出：

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 对 Ruby 操作打包。
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

可以将 PHP 操作以及其他文件或从属包打包成 .zip 文件。例如，可以打包具有名为 `helper.rb` 的第二个文件的操作。

1. 创建包含源文件的归档。**注**：包含入口点的源文件必须命名为 `main.rb`。

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. 创建操作。

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

除了缺省和捆绑的 gem 外，还提供了 gem `mechanize` 和 `jwt`。只要使用压缩的操作来打包所有依赖项，就可以使用任意 gem。
