# CORD 指引


在 CORD 指引中，將幫助了解安裝、操作和開發 CORD 以及相應工具集雨每個階段的規範文件之間的關係。

* **Installation (Helm):**
安裝 CORD 意味著在 Kubernetes 集群中安裝一組 Docker containers。 使用 Helm 來進行安裝(使用 `helm-charts` 定義有效的 configurateions)。
這些 charts 指定每個要部署的 container 版本，在版本升級也會用到。更多關於 `helm-charts` 可以參考[這邊](charts/helm.md)。

* **Operations (TOSCA):** 
運行中的 CORD POD 支援多個北向 Interfaces(包含 GUI 和 REST API)，不過為了 config 和 provision 運行中的系統，通常我們使用 `TOSCA` 來定義 workflow。新的 CORD POD 有一組 control plane 和 platform level containers running (e.g., XOS, ONOS, OpenStack)。
不過在使用 `TOSCA` 前，並沒有 services 和 service graph。更多關於 `TOSCA` 可以參考[這邊](xos-tosca/README.md)。

* **Development (XOS):** 
service 通常使用 Docker container 部署，並指定 service 如何被加到 CORD 的對應 model。
model 是用 `xproto` modeling 語言撰寫的，並由 XOS tool-chain 處理。此外，tool-chain 還會產生 TOSCA-engine (用於處理維運 CORD 的 configuration 和 provisioning 的 workflow)。
更多關於 `xproto` 可以參考[這邊](xos/dev/xproto.md)。

這些 tools 和 containers 是相互關聯的:

* 初始安裝會帶出一組已經被被配置好的基本模型在 XOS-related containers (例如，xos-core、xos-gui、xos-tosca)中。 其中，xos-tosca container 實現了 TOSCA engine，而該 engine 將 TOSCA workflows 作為輸入與 configures/provisions CORD。

* 即便安裝和維運是不同的，為了方便起見，一些 helm-charts 選擇啟動 tosca-loader container (在 k8s 是 job， 在 CORD 是 service)來載入 initial TOSCA workflow 到新部署的 service set 中。
這是通常是 service graph 的 instantiated.。

* 雖然使用 Docker containers 部署 CORD 的 control plane，但並不是所有的服務都是 container 。有些服務是在 OpenStack 的 VM 中(M-CORD 就是這樣)。
還有些服務是用 Maven 包在 ONOS APP 中。在這種情況下，仍然在 TOSCA 的 workflow 中指定 VM  image 和 Maven package。

* 每個 service (無論是在 Docker、OpenStack、ONOS 中實現) 都有一個 counter-part *synchronizer* container 作為 CORD control plane 的一部分運行(例如，vOLT service 的`volt-synchronizer`)。 通常，service 的 helm-chart 會啟動這個 synchronizer container，此外，TOSCA worflow 會創建，配置和初始化 backend container, VM 或 ONOS APP。

* 在正在運行的 POD 中加入其他 service 涉及執行 helm-charts 以安裝新 service 的synchronizer container。synchronizer container 會將相應的新 models 載到 XOS 中。 這個 load 會 triggers, upgrade 和重新啟動 TOSCA engine （和其他NBI），TOSCA engine 是配置和配置新 service 的先決條件。

* 依據 Kubernetes 的功能進行 現有的 service upgrade 或 rollback。我們依靠XOS從舊模型遷移到新模型 （並在過渡期間支持新舊API）。 升級現有服務尚未經過全面測試。
