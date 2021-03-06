---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Getting started tutorial
{: #getting-started}

* {: download} Congratulations, you deployed a Hello World sample application on {{site.data.keyword.Bluemix}}!  To get started, follow this step-by-step guide. Or, download the sample code and explore on your own.

<a class="xref" href="https://github.com/IBM-Cloud/get-started-swift" target="_blank" title="(Download sample code)"><img class="w3-image" src="docs/runtimes/images/btn_starter-code.svg" alt="Download application code" /></a> 

By following this tutorial, you'll set up a development environment, deploy an app locally and on {{site.data.keyword.Bluemix}}, and integrate an {{site.data.keyword.Bluemix_notm}} database service in your app.

Throughout these docs, references to the Cloud Foundry CLI are now updated to the {{site.data.keyword.Bluemix_notm}} CLI! The {{site.data.keyword.Bluemix_notm}} CLI has the same familiar Cloud Foundry commands, but with better integration with {{site.data.keyword.Bluemix_notm}} accounts and other services. Learn more about getting started with the {{site.data.keyword.Bluemix_notm}} CLI in this tutorial.
{: tip}

## Before you begin
{: #prereqs}
* [Git ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}
* [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud/download_cli.html)
* [Swift compiler ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://swift.org/download/) for your platform.

## Step 1: Clone the sample app
{: #clone}

First, clone the repository and change to the directory to where the sample app is located.
  ```
git clone https://github.com/IBM-Cloud/get-started-swift
  ```
  {: codeblock}

  ```
cd get-started-swift
  ```
  {: codeblock}

  Peruse the files in the *get-started-swift* directory to familiarize yourself with the contents.

## Step 2: Run the app locally
{: #run_locally}

Once you have installed the Swift compiler and cloned this Git repository, you can now compile and run the application.

1. Go to the root directory of this repository on your system and issue the following command:

  ```
swift build
  ```
  {: codeblock}

  This command might take a few minutes to run.

1. When the application is successfully compiled, you can run the executable that was generated by the Swift compiler:
```
swift run
```
  {: codeblock}

  or
  ```
.build/debug/get-started-swift
  ```
  {: codeblock}

  You should see an output similar to the following:

  ```
Server is listening on port: 8080
  ```
  {: pre}

1. View your app at the following URL: http://localhost:8080

## Step 3: Prepare the app for deployment
{: #prepare}

To deploy to {{site.data.keyword.Bluemix_short}}, it can be helpful to set up a manifest.yml file. The manifest.yml includes basic information about your app, such as the name, how much memory to allocate for each instance and the route. We've provided a sample manifest.yml file in the `get-started-swift` directory.

Open the manifest.yml file, and change the `name` from `GetStartedSwift` to your app name, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
  applications:
   - name: Get-Started-Swift
     random-route: true
     memory: 256M
     command: get-started-swift
     buildpack: swift_buildpack
  ```
  {: codeblock}

In this manifest.yml file, **random-route: true** generates a random route for your app to prevent your route from colliding with others.  If you choose to, you can replace **random-route: true** with **host: myChosenHostName**, supplying a host name of your choice.
{: tip}

## Step 4: Deploy the app
{: #deploy}

You can use the {{site.data.keyword.Bluemix_short}} CLI to deploy apps.

1. Log in to your {{site.data.keyword.Bluemix_notm}} account, and select an API endpoint.

  ```
ibmcloud login
  ```
  {: codeblock}

  If you have a federated user ID, instead use the following command to log in with your single sign-on ID. See [Logging in with a federated ID](/docs/cli/login_federated_id.html) to learn more.

  ```
ibmcloud login --sso
  ```
  {: codeblock}

1. Target a Cloud Foundry org and space:
  ```	  
ibmcloud target --cf
  ```
  {: codeblock}

  If you don't have an org or a space set up, see [Adding orgs and spaces](/docs/account/orgs_spaces.html).
  {: tip}

1. From within the *get-started-swift* directory, push your app to {{site.data.keyword.Bluemix_notm}}

  ```
ibmcloud cf push
  ```
  {: codeblock}

  This can take a minute. If there is an error in the deployment process you can use the command `ibmcloud cf logs <Your-App-Name> --recent` to troubleshoot.

When deployment completes you should see a message indicating that your app is running.  View your app at the URL listed in the output of the push command.  You can also issue the following command to view your apps status and see the URL.

  ```
ibmcloud cf apps
  ```
  {: codeblock}

You can also go to the {{site.data.keyword.Bluemix_notm}} [Resource List](https://cloud.ibm.com/resources) to view your app.

## Step 5: Add a database
{: #add_database}

Next, we'll add an {{site.data.keyword.cloudant_short_notm}} NoSQL database to this application and set up the application so that it can run locally and on {{site.data.keyword.Bluemix_notm}}.

1. In your browser, log in to {{site.data.keyword.Bluemix_notm}} and go to the Dashboard. Select **Create resource**.
1. Search for **{{site.data.keyword.cloudant_short_notm}}**, and select the service.
1. For **Available authentication methods**, select **Use both legacy credentials and IAM**. You can leave the default settings for the other fields. Click **Create** to create the service.
1. In the navigation, go to **Connections**, then click **Create connection**. Select your application, and click **Connect**.
1. Using the default values, click **Connect & restage app** to connect the database to your application. Click **Restage** when prompted.

   {{site.data.keyword.Bluemix_notm}} will restart your application and provide the database credentials to your application using the `VCAP_SERVICES` environment variable. This environment variable is available to the application only when it is running on {{site.data.keyword.Bluemix_notm}}.

Environment variables enable you to separate deployment settings from your source code. For example, instead of hardcoding a database password, you can store it in an environment variable that you reference in your source code.
{: tip}

## Step 6: Use the database
{: #use_database}

We're now going to update your local code to point to this database. Create a JSON file that will store the credentials for the services the application will use. This file will get used ONLY when the application is running locally. When running in {{site.data.keyword.Bluemix_notm}}, the credentials will be read from the VCAP_SERVICES environment variable.

Create a file called `my-cloudant-credentials.json` in the `config` directory with the following content (as reference, see `config/my-cloudant-credentials.json.example`):

 ```
 {
   "password": "<password>",
   "url": "<url>",
   "username": "<username>"
 }
 ```
 {: codeblock}

Update the `mappings.json` file in the `config` directory by replacing the `cloudant` placeholder with the **name** of your DB instance:

  ```
{
  "MyCloudantDB": {
    "searchPatterns": [
      "cloudfoundry:cloudant",
      "env:kube-cloudant-credentials",
      "file:config/my-cloudant-credentials.json"
    ]
  }
}
  ```
  {: codeblock}

This sample application uses the `CloudEnvironment` package to interact with {{site.data.keyword.Bluemix_notm}} to parse environment variables. [Learn more...](https://github.com/IBM-Swift/CloudEnvironment)

The `cloudant` placeholder in the `cloudfoundry:cloudant` configuration makes it easier to bind a user-provided Cloudant service to your application. With the `cloudfoundry:cloudant` configuration, you can create a Cloudant service that includes the string, `cloudant` somewhere in the service name and bind it to your application, without editing the `config.json` file. If you modify this configuration and later want to use a user-provided Cloudant service, you either need to edit the configuration to `cloudfoundry:cloudant` or define `cloudfoundry:` with the name of your user-provided service.
{: tip}

Find your app in the {{site.data.keyword.Bluemix_notm}} [Resource List](https://cloud.ibm.com/resources). On the Service Details page for your app, click **Connections** in the sidebar. Click the {{site.data.keyword.cloudant_short_notm}} menu icon (**&hellip;**) and select **View credentials**.

Copy and paste just the credentials to the corresponding fields in your local config.json file.

Build and run your application locally.
 ```
swift build  
 ```
 {: codeblock}

  ```
.build/debug/kitura-helloworld
  ```
 {: codeblock}

 View your app at: http://localhost:8080. Any names you enter into the app will now get added to the database.

 This sample application uses the `Kitura-CouchDB` package to interact with Cloudant. [Learn more...](https://github.com/IBM-Swift/Kitura-CouchDB)

 Make any changes you want and re-deploy to {{site.data.keyword.Bluemix_notm}}!

  ```
ibmcloud cf app push
  ```

View your app at the URL listed in the output of the push command, for example, *myUrl.mybluemix.net*.

Remember, if you don't need your app live, stop it so you don't incur any unexpected charges.
{: tip}

## Next Steps

* [Kitura & Server-side Swift Tutorials ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.kitura.io/learn.html){: new_window}
* [Samples ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://ibm-cloud.github.io){: new_window}
* [Architecture Center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
