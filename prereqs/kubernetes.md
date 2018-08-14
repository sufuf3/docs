# Kubernetes

CORD 運行的 k8s 版本事 v1.10 以上，並使用 Helm client-side tool。
如果你是 k8s 新手，請閱讀 <https://kubernetes.io/docs/tutorials/>。


注意：我們使用 kubernetes 1.10 中的允許本地持久性的 data 功能。在撰寫本文時，這是 K8S 1.10.x 中的測試版功能，默認情況下應啟用。
但是，如果不是，則在使用以下功能 gate 設置啟動 kubernetes 時，需要將其功能 gate 啟用：

```shell
PersistentLocalVolumes=true
VolumeScheduling=true
MountPropagation=true
```

More information about feature gates can be found [here](https://github.com/kubernetes-incubator/external-storage/tree/local-volume-provisioner-v2.0.0/local-volume#enabling-the-alpha-feature-gates).

雖然您可以以任何對部署有意義的方式自由設置 Kubernetes 和 Helm ，不過以下提供了可能有用的指南，指針和自動腳本。

## Install Kubernetes

The following sections, [單個 node 的 Cluster](k8s-single-node.md) and [多個 Node 的 Cluster](k8s-multi-node.md), offer pointers and scripts to install your favorite
version of Kubernetes. Start there, then come back here and follow the
steps in the following three subsections.

## Export KUBECONFIG

安裝 Kubernetes 後，您應該有一個 KUBECONFIG 配置文件，其中包含部署的所有詳細信息：
機器的地址，credentials 等。該文件可用於從能夠與 Kubernetes 安裝進行通信的任何客戶端訪問 Kubernetes 部署。要管理 pod ，請導出包含配置文件路徑的 KUBECONFIG 變量：

```shell
export KUBECONFIG=/path/to/your/kubeconfig/file
```

You can also permanently export this environment variable, so you don’t have to
export it every time you open a new window in your terminal. More info on this
topic at
<https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/>.

## Install Kubectl

Again assuming Kubernetes is already installed, the next step is to
install the CLI tools used to interact with it. *kubectl* is the basic tool
you need. It can be installed on any device able to reach the Kubernetes
just installed (i.e., the development laptop, another server, the same machine
where Kubernetes is installed).

To install kubectl, follow this step-by-step guide: <https://kubernetes.io/docs/tasks/tools/install-kubectl/>.

To test the kubectl installation run:

```shell
kubectl get pods
```

Kubernetes should reply to the request showing the pods already deployed.
If you've just installed Kubernetes, likely you won't see any pod, yet.
That's fine, as long as you don't see errors.

