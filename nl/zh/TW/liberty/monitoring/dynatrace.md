---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-27"
subcollection: "liberty"

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 使用 Dynatrace 在 {{site.data.keyword.cloud_notm}} 中監視 Liberty
{: #using_dynatrace}

Dynatrace 是協力廠商服務，可為您的應用程式提供監視功能。您可以整合 Dynatrace 與 Liberty 應用程式，但 IBM 不提供對協力廠商服務的支援。如需相關資訊，請參閱[協力廠商服務](/docs/runtimes-common/buildpackSupport.html#third-party)。

如需 Dynatrace 及其授權的相關資訊，請參閱 [Dynatrace Application Monitoring ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.dynatrace.com/en/products/application-monitoring.html)。

Liberty 應用程式配置成使用 Dynatrace 時，預設行為是 Liberty 運行環境將從 Dynatrace 網站獲得 Dynatrace 代理程式 `.jar` 檔案，並且搭配執行該 Dynatrace 代理程式與您的應用程式。運用該預設行為，使用 Dynatrace 的最小必要配置是建立可指向 Dynatrace 收集器的使用者提供服務。

## 建立可指向 Dynatrace 收集器的使用者提供服務

首先，您需要設定 Dynatrace 收集器。接下來，您必須建立一個使用者提供的服務來傳遞 Dynatrace 代理程式的資訊，以便與 Dynatrace 收集器進行連線。請參閱 [Dynatrace 架構 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://community.dynatrace.com/community/display/DOCDT65/Architecture)，以更充分地瞭解 Dynatrace 元件之間的關係。

1. 設定 Dynatrace 收集器。
  * 請參閱 [Dynatrace 社群網站 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://community.dynatrace.com/community/display/EVAL/Step+3+-+Connect+Agent+to+Dynatrace)，以取得下載和設定 Dynatrace 收集器的指示。
  * 請確定收集器是設定在 Dynatrace 代理程式（它與您的應用程式一起在 {{site.data.keyword.Bluemix_notm}} 中執行）可存取的位置中。
2. 建立一個使用者提供的服務來指向執行中的 Dynatrace 收集器。

  **附註：**使用者所提供服務的名稱必須包含字串 **dynatrace**。忽略大小寫。例如，使用下列指令，其中 **my-dynatrace-collector** 包含 **dynatrace**：
  ```
  ibmcloud cf cups my-dynatrace-collector -p '{"server":"DynatraceCollectorIPaddress","profile":"Monitoring"}'
  ```
  {: codeblock}

    在這個範例中，my-dynatrace-collector 是提供給服務的名稱，DynatraceCollectorIPaddress 是您已配置的 Dynatrace 收集器的 IP 位址，而 profile 是與此受監視應用程式相關聯的選用 Dynatrace 設定檔名稱。預設設定檔值是 Monitoring。您可以指定選用的參數，如下列範例所示。

    ```
ibmcloud cf cups my-dynatrace-collector -p '{"server":"DynatraceCollectorIPaddress","profile":"Monitoring",
                      "options" : {"dynatrace-parameter-1": "value",
                                   "dynatrace-parameter-2": "value"}}'
    ```
    {: codeblock}

    如需可用選項的相關資訊，請參閱 Dynatrace 社群網站上 [Agent Configuration 的 _Agent Settings_ 一節 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://community.dynatrace.com/community/display/DOCDT65/Set+up+Agents)。例如，使用 exclude 選項，您可以排除類別使其不受 Dynatrace 監視。如需配置使用者所提供服務的詳細資料，請參閱 [Dynatrace Agent Framework ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/ibm-websphere-liberty-buildpack/blob/master/docs/framework-dynatrace-agent.md)。

3. 將您的應用程式推送到 {{site.data.keyword.Bluemix_notm}} 之後，請將您建立的使用者所提供服務連結到該應用程式。例如，使用下列指令：
  ```
  ibmcloud cf bs myApp my-dynatrace-collector
  ```
  {: codeblock}

    **附註：**您必須在連結該服務之後，重新編譯打包應用程式。

## 選用配置
{: #optional_configuration}

您可能選擇自行獲得及管理 Dynatrace 代理程式 `.jar` 檔案。在該情況下，需要執行下列其他配置步驟。
1. 獲得及管理 Dynatrace 代理程式 `.jar` 檔案，讓 Liberty 建置套件能夠下載它。
2. 配置 Liberty 應用程式，讓它可以下載 Dynatrace 代理程式。

### 管理 Dynatrace 代理程式
{: #hosting_dynatrace_agent}
Dynatrace 代理程式必須在 Web 伺服器上進行管理，而 Liberty 建置套件必須能夠從該伺服器下載代理程式 `.jar` 檔案。必須以指定代理程式 `.jar` 檔案相關詳細資料的 `index.yml` 檔案來配置伺服器。請完成下列步驟，以設定 Dynatrace 代理程式：
  1. 下載 Dynatrace 代理程式 `.jar` 檔案。請參閱 Dynatrace 社群網站的 [Dynatrace Server Platform Installers ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://community.dynatrace.com/community/display/EVAL/Step+1+-+Download+and+install+Dynatrace)，以取得下載 Dynatrace 代理程式 `.jar` 檔案的指示。適合在 {{site.data.keyword.Bluemix_notm}} 上執行的代理程式 `.jar` 檔案是 **dynatrace-agent-unix.jar** **6.+** 版。
  2. 在可從中下載 Liberty 建置套件的位置管理代理程式 `.jar` 檔案。您可以使用任何可用的伺服器機能，在 {{site.data.keyword.Bluemix_notm}} 上管理它，也可以在某個公用的位置上管理它。
     * 確定您在該管理位置提供 `index.yml` 檔案。`index.yml` 檔案必須包含一個項目，其中包含代理程式 `.jar` 檔案的版本 ID，後面接著一個冒號，以及該代理程式 `.jar` 檔案位置的完整 URL。例如：

            ---
               6.3.0: https://my-dynatrace-agent.mybluemix.net/dynatrace-agent-6.3.0-unix.jar
            {: codeblock}

     * `index.yml` 檔案所指定的位置上必須提供 **dynatrace-agent-6.3.0-unix.jar** 檔案。`.jar` 檔案和 `index.yml` 的位置可以是相同的目錄。

### 配置 Liberty 應用程式
{: #configuring_liberty_app}

必須配置您要監視的 Liberty 應用程式，才能找出管理您先前設定之代理程式 `.jar` 檔案的伺服器。您可以使用 **JBP_CONFIG_DYNATRACEAPPMONAGENT** 環境變數配置應用程式。**JBP_CONFIG_DYNATRACEAPPMONAGENT** 環境變數告知建置套件從何處下載 Dynatrace 代理程式。若要設定此環境變數，請完成下列步驟：

1. 將 **JBP_CONFIG_DYNATRACEAPPMONAGENT** 變數的值設為 *"repository_root: URL_of_server_hosting_index.yml"*。例如，在推送您的應用程式之後發出下列指令：

        ibmcloud cf se myApp JBP_CONFIG_DYNATRACEAPPMONAGENT 'repository_root: https://my-dynatrace-agent-host.mybluemix.net'
        {: codeblock}

    在這個範例中，*my-dynatrace-agent-host.mybluemix.net* 是您先前配置的伺服器所管理 `index.yml` 檔案的 URL。

2. 在設定此環境變數之後，請重新編譯打包您的應用程式。請檢查編譯打包日誌來尋找一則訊息，該訊息指出已從代理程式管理伺服器成功下載 Dynatrace 代理程式。例如：
```
    Downloading dynatrace-agent-6.3.0-unix.jar 6.3.0 from https://my-dynatrace-agent-host.mybluemix.net/dynatrace-agent-6.3.0-unix.jar (17.8s)
```
{: codeblock}
