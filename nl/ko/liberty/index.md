---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-04"
subcollection: "liberty"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Liberty for Java
{: #liberty_runtime}

{{site.data.keyword.Bluemix}}의 Liberty for Java 애플리케이션은 liberty-for-java 빌드팩을 통해 제공됩니다. liberty-for-java 빌드팩은 WebSphere Liberty 위에서 Java EE 7 및 OSGi 애플리케이션을 실행하기 위한 완벽한 런타임 환경을 제공합니다. 이는 Spring과 같은 인기 있는 프레임워크를 지원하며 {{site.data.keyword.IBM_notm}} JRE를 포함합니다. Liberty를 통해 클라우드에 적합한 애플리케이션을 신속하게 개발할 수 있습니다.
{: shortdesc}

## 발견
{: #detection}
다음과 같은 종류의 애플리케이션이 배치된 경우 Liberty 빌드팩이 사용됩니다.
* [WAR 파일](/docs/runtimes/liberty/optionsForPushing.html#stand_alone_apps)
* [EAR 파일](/docs/runtimes/liberty/optionsForPushing.html#stand_alone_apps)
* [Liberty 서버 디렉토리](/docs/runtimes/liberty/optionsForPushing.html#server_directory)
* [Liberty 패키지된 서버](/docs/runtimes/liberty/optionsForPushing.html#packaged_server)
* Java 기본
* [Distzip](https://github.com/cloudfoundry/ibm-websphere-liberty-buildpack/blob/master/docs/container-distZip.md)

## 스타터 애플리케이션
{: #starter_application}
{{site.data.keyword.Bluemix_notm}}에서는 몇 가지 Liberty 스타터 애플리케이션을 제공합니다.  Liberty 스타터 애플리케이션은 사용할 수 있는 템플리트를 제공하는 단순한 Liberty 앱입니다. 스타터 앱을 사용하여 시험해 볼 수 있으며 {{site.data.keyword.Bluemix_notm}} 환경을 변경하고 변경사항을 푸시할 수 있습니다.  스타터 애플리케이션 사용에 대한 도움말은 [스타터 애플리케이션 사용](/docs/runtimes-common/starter_app_usage.html)을 참조하십시오.
