---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"
subcollection: "Nodejs"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Node.js buildpack

{{site.data.keyword.Bluemix}} 提供多个版本的 Node.js buildpack。
{: shortdesc}

* {{site.data.keyword.IBM_notm}} 创建的 **sdk-for-nodejs** buildpack 是 {{site.data.keyword.Bluemix_notm}} 中用于 Node.js 应用程序的缺省 buildpack。
* **nodejs_buildpack** 是 Cloud Foundry 社区提供的社区 buildpack。

在 {{site.data.keyword.Bluemix_notm}} 中，**sdk-for-nodejs** buildpack 优先于 **nodejs_buildpack**。如果想要将 **nodejs_buildpack** 用于您的应用程序，而不是 **sdk-for-nodejs** buildpack，那么必须指定该 buildpack，例如，使用带 `-b` 选项的 `ibmcloud cf push` 命令来进行指定。

通常会提供 **sdk-for-nodejs** buildpack 的当前版本和低版本。要查看所有可用的 buildpack，请使用 `ibmcloud cf buildpacks` 命令。例如：

```
   ibmcloud cf buildpacks
   Getting buildpacks...

   buildpack                                 position   enabled   locked   filename   

   sdk_for_nodejs                            2          true      false    buildpack_sdk-for-nodejs_v2.8-20151209-1403.zip   
   nodejs_buildpack                          5          true      false    nodejs_buildpack-cached-v1.5.0.zip   
   sdk-for-nodejs_v2_7-20151118-1003         17         true      false    buildpack_sdk-for-nodejs_v2.7-20151118-1003.zip
```
{: codeblock}
