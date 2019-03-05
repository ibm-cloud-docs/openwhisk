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

# Creating Ruby actions
{: #creating-ruby-actions}

The following sections guide you through creating and invoking a single Ruby action and adding parameters to that action. 

Ruby actions are executed in Ruby 2.5. To use this runtime, specify the `ibmcloud fn` CLI parameter
`--kind ruby:2.5` when creating or updating an action.

## Creating and invoking Ruby actions
{: #actions_ruby_invoke}
{: #openwhisk_actions_ruby_invoke}

An action is simply a top-level Ruby method.

For example, create a file called `hello.rb`.

1. Save the following code in a file called `hello.rb`.

    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby actions always consume a Hash (dictionary-like collection) and return a Hash.
    * The entry method for the action is `main` by default but can be specified when you create the action with the `ibmcloud fn` CLI by using the `--main` flag.

2. Create an action called `helloRuby`.

    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Example output:

    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Packaging Ruby actions.
{: #actions_ruby_zip}
{: #openwhisk_actions_ruby_zip}

You can package a PHP action and other files or dependent packages in a .zip file. For example, you can package an action with a second file called `helper.rb`.

1. Create an archive containing your source files. **Note**: The source file that contains the entry point must be named `main.rb`.

    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Create the action.

    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}

The gems `mechanize` and `jwt` are available in addition to the default and bundled gems.
You can use arbitrary gems so long as you use zipped actions to package all the dependencies.
