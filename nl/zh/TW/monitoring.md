---

copyright:
  years: 2018
lastupdated: "2019-04-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 監視 
{: #monitoring}

包含 Prometheus 及 Grafana 的開放程式碼工具集，能支援監視 {{site.data.keyword.cfee_full}} 實例及其支援的基礎架構。此解決方案可讓您分析、視覺化及管理 Cloud Foundry 環境中的度量值警示。有 3 個可從中進行監視的 Web 主控台：Grafana 主控台、Prometheus 主控台及 Prometheus AlertManager 主控台。

從 CFEE 2.2.0 版開始，在建立環境時，依預設不會啟用監視工具。管理者可以在建立環境之後啟用監視。在 CFEE 使用者介面中，移至左導覽窗格中的_監視_ 頁面，然後按一下**啟用**。啟用監視會自動佈建其他 Kubernetes 工作者節點（在相同的 IBM Cloud 帳戶中），並將監視工具安裝至這些工作者節點。

**附註：**存取 {{site.data.keyword.cfee_full}} 實例中的監視功能需要 CFEE 中的_管理者_ 或_編輯者_ 角色，以及 Kubernetes 叢集中支援 CFEE 實例的_操作員_ 角色（在其下分組 CFEE 之資源群組中的_檢視者_ 角色之外）。支援 CFEE 實例的預設 Kubernetes 叢集名稱是 _`<CFEEname>`-cluster_。 

## Prometheus
{: #prometheus}

Prometheus 是開放程式碼的系統監視及警示工具箱。從 2012 年開始，許多公司及組織都採用 Prometheus，而且該專案具有非常活躍的開發人員及使用者社群。它是獨立式的開放程式碼專案，且獨立於任何公司進行維護。為了強調這點，並釐清專案控管結構，Prometheus 繼 Kubernetes 之後，於 2016 年加入 Cloud Native Computing Foundation 成為第二個管理的專案。如需相關資訊，請參閱 [Prometheus 文件](https://prometheus.io/docs/introduction/overview/)。

Prometheus 生態系統由多個元件組成，其中許多元件為選用項目：

* 可提取及儲存時間序列資料的主要 Prometheus 伺服器。</li>
* 可處理警示的 [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/)。</li>
* 各種特殊用途的輸出器，例如節點輸出器、黑盒輸出器等。</li>
* 支援短期工作的推送閘道。</li>

Prometheus 會直接或透過短期工作的中間推送閘道，從受檢測的工作收集度量值。它會在本端儲存所有收集到的樣本，並對此資料執行規則，以聚集並記錄現有資料的新時間序列，或是產生警示。 

## Grafana
{: #grafana}

Grafana 是 Prometheus 所收集之所有度量值的開放程式碼分析平台。您叢集上部署的 Grafana 版本，已經配置成使用基礎 Prometheus 資料庫。它也包含一些寶貴的 Grafana 儀表板。如需相關資訊，請參閱 [Grafana 文件](http://docs.grafana.org/guides/getting_started/)。

## 開始使用監視
{: #gettingStarted_monitor}

包含監視解決方案的 Prometheus 及 Grafana 元件，會預先安裝至支援 CFEE 實例的 Kubernetes 基礎架構中。若要存取監視工具，需要轉遞 Prometheus、Prometheus AlertManager 及 Grafana 伺服器的埠。這是透過 Kubernetes CLI 所完成。
以下會引導您完成安裝必要 CLI、轉遞伺服器埠以及啟動主控台的步驟。

**附註：**在 {{site.data.keyword.cfee_full}} 使用者介面中，也提供下列指示。開啟 CFEE 實例使用者介面，按一下左導覽窗格中的**監視**，然後移至**存取**標籤，以查看顯示的指示。

### 必要條件

1. 檢查[存取原則](https://console.bluemix.net/iam/#/users)，以確保您在支援環境的 Kubernetes 叢集上，至少具有「檢視者」角色。
2. 安裝 [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/ibmcloud/download_cli.html#install_use)。
3. 安裝 [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。如果您有現有的 Kubernetes CLI，則建議您安裝最新版本。
4. 安裝容器服務外掛程式：
```
ibmcloud plugin install container-service -r Bluemix
```
 
### 自訂 Alertmanager 配置

Alertmanager 具有預設 [Alertmanager 配置](https://prometheus.io/docs/alerting/configuration/)檔案，可定義 Alertmanager 警示處理、分組及通知遞送的原則。您可以下載預設配置檔，並在_監視_ 頁面的**配置**標籤中上傳自訂配置。
 
### 存取 Kubernetes 叢集

1. 登入 IBM Cloud 帳戶：

  ```
  ibmcloud login -a https://api.ng.bluemix.net
  ```
  {: pre}
  
  如果您有聯合 ID，請使用 __ibmcloud login -sso__ 來登入 IBM Cloud CLI。

2. 將目標設為您要在其中工作的 IBM Cloud Container Service 地區（例如，us-south）：

  ```
  ibmcloud cs region-set us-south
  ```
  {: pre}
    
3. 在 CLI 中設定叢集的環境定義： 

  a. 讓指令設定環境變數，並下載 Kubernetes 配置檔：

  ```
  ibmcloud cs cluster-config <CFEE_instance_name>-cluster
  ```
  {: pre}
    
  b. 設定 KUBECONFIG 環境變數。複製先前指令的輸出，並貼到終端機中。指令輸出應該類似如下：__export KUBECONFIG=/Users/$USER/.bluemix/plugins/container-service/clusters/cf-admin-0703-cluster/kube-config-dal10-cf-admin-0703-cluster.yml__

### 轉遞伺服器埠
4. 針對執行 Prometheus、AlertManager 及 Grafana 的 Pod，設定 Kubernetes 叢集中的埠轉遞。這可讓您在本端機器 (localhost) 上透過 Proxy 來管理監視度量值。

  ```
  sh -c 'kubectl -n monitoring port-forward deployment/prometheus-server 9090 & kubectl -n monitoring port-forward deployment/prometheus-alertmanager 9093 & kubectl -n monitoring port-forward deployment/grafana 3000 &''
  ```
  {: pre}
  
### 在本端 Web Proxy 上啟動監視主控台

5. 啟動 Grafana 主控台，以查看針對所選取度量值的分析。CFEE 實例中包含一些預設 Grafana 儀表板。這些預設儀表板是互動式的，可讓您檢視用來管理 CFEE 實例的基礎架構（Kubernetes 叢集）。在您啟動 Grafana 主控台之後，請按一下 Grafana 主控台頂端的**首頁**按鈕，以選取其中一個預先部署的儀表板（請參閱下面的清單），它會繪製對應度量值的圖形：

   對於 CFEE 2.1.0 版或更早版本，Grafana 中有一個預設 `admin` 使用者，而其預設密碼設為 `admin`。建議您以使用者 ID/密碼 `admin/admin` 登入，並將它們變更為新的認證。對於 CFEE 2.2.0 版或更新版本，會自動以使用者的 IBM ID 認證來鑑別使用者。

     [啟動 Grafana 主控台](https://localhost:3000)

   下列預設儀表板是與 CFEE 實例一起提供，並且可以從_首頁_ 下拉清單中取得。

    Cloud Foundry 儀表板：
   - _CF：Cell 容量_ 
        - 顯示 Cloud Foundry 應用程式部署所在之 Cloud Foundry Cell 的一般狀態。
   - _CF：Diego Cell 儀表板_ 
        - 顯示 Cloud Foundry Cell 及 Diego 元件的狀態。
   - _CF：路由器_ 
        - 顯示 CFEE 環境上執行的 Cloud Foundry 路由器狀態。
  
   支援 CFEE 環境之 Kubernetes 基礎架構的儀表板：
   - _部署_ 
        - 顯示 Kubernetes 部署的狀態。
   - _Kubernetes 叢集性能_ 
        - 顯示 Kubernetes 叢集的性能。
   - _Kubernetes 叢集狀態_ 
        - 顯示 Kubernetes 叢集的狀態。
   - _Kubernetes 資源要求_ 
        - 顯示 Kubernetes 叢集的已使用 CPU、記憶體及其他參數。
   - _Pod_ 
        - 顯示 Kubernetes 叢集上執行之每個 Pod 的詳細資料。
   - _抄本集_ 
        - 顯示 Kubernetes 抄本集的狀態。       
   - _工作者節點_ 
        - 顯示 Kubernetes 叢集之每個工作者節點的詳細資料。
   - _工作者節點概觀_ 
        - 顯示 Kubernetes 基礎架構的 CPU 及記憶體用量，以及它的網路資料流量。
        
6. （選用）您也可以啟動 Prometheus 主控台來查看 Prometheus 伺服器所收集的原始資料，以及啟動 Prometheus Alertmanager 來管理 Prometheus 伺服器所傳送的警示：

     [啟動 Prometheus 伺服器](https://localhost:9090)

     [啟動 Prometheus Alertmanager 伺服器](https://localhost:9093)

