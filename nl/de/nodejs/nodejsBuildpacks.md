---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"
subcollection: "Nodejs"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Node.js-Buildpacks

{{site.data.keyword.Bluemix}} stellt mehrere Versionen des Node.js-Buildpacks bereit.
{: shortdesc}

* Das von {{site.data.keyword.IBM_notm}} erstellte Buildpack **sdk-for-nodejs** ist das für Node.js-Anwendungen in {{site.data.keyword.Bluemix_notm}} standardmäßig verwendete Buildpack.
* Das **nodejs_buildpack** ist ein Community-Buildpack, das von der Cloud Foundry-Community zur Verfügung gestellt wird.

Das Buildpack **sdk-for-nodejs** hat in {{site.data.keyword.Bluemix_notm}} Vorrang vor dem Buildpack **nodejs_buildpack**. Wenn Sie mit Ihrer Anwendung das Buildpack **nodejs_buildpack** statt des Buildpacks **sdk-for-nodejs** verwenden möchten, müssen Sie das Buildpack angeben, beispielsweise über Option `-b` mit dem Befehl `ibmcloud cf push`.

In der Regel stehen das aktuelle Buildpack **sdk-for-nodejs** und eine frühere Version zur Verfügung.  Mithilfe des Befehls `ibmcloud cf buildpacks` können Sie alle verfügbaren Buildpacks anzeigen.  Beispiel:

```
   ibmcloud cf buildpacks
   Abrufen von Buildpacks...

   Buildpack                                 Position   aktiviert gesperrt Dateiname   

   sdk_for_nodejs                            2          true      false    buildpack_sdk-for-nodejs_v2.8-20151209-1403.zip   
   nodejs_buildpack                          5          true      false    nodejs_buildpack-cached-v1.5.0.zip   
   sdk-for-nodejs_v2_7-20151118-1003         17         true      false    buildpack_sdk-for-nodejs_v2.7-20151118-1003.zip
```
{: codeblock}
