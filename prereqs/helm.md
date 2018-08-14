# Helm

{% include "/partials/helm/description.md" %}

以下假設已經安裝了 *Kubernetes* 且 *kubectl* 可以問 POD 。 
CORD使用 helm 在 Kubernetes 上部署 containers，因此，應該在嘗試部署任何 CORD container 之前安裝它。

Helm 文件可以在 <https://docs.helm.sh/> 找到，他有兩個 components:

* `helm`: helm  client 是一個基本的 CLI utility。
* `tiller`: 是 server side 的 component，是在 k8s 的 cluster 上執行 client commands。

Helm 可以安裝在任何能夠訪問 Kubernetes POD 的設備上（如開發者筆電，網路中的另一台 server）。
Tiller 應該安裝在 Kubernetes 集群上。

> *注意*：如果你安裝了 Minikube ，你可能還需要在繼續之前安裝 `socat` 套件，否則會引發錯誤。
例如，在 Ubuntu 上執行 `sudo apt-get install socat` 。

## Install Helm Client

Follow the instructions at <https://github.com/kubernetes/helm/blob/master/docs/install.md#installing-the-helm-client>

## Install Tiller

Enter the following commands from any device thsat is already
able to access the Kubernetes cluster.

```shell
helm init
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm init --service-account tiller --upgrade
```

Once *helm* and *tiller* are installed you should be able to run the
command *helm ls* without errors.

## Next Step

Once you are done, you are ready to deploy CORD components using their
helm charts! See [Bringing Up CORD](../profiles/intro.md). For more detailed
information, see the [helm chart reference guide](../charts/helm.md).
