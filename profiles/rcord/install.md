# R-CORD Profile

最新版本的 R-CORD 與早期版本中包含的版本不同，它不包括 vSG 服務。
在程式碼中，這個新配置稱為 `rcord-lite` ，但由於它是目前支持的唯一版本的 Residential CORD ，我們簡稱為 *R-CORD profile*。

## Prerequisites

- Kubernetes: 請遵循這兩個的其中一個安裝指南，a [single
   node](../../prereqs/k8s-single-node.md) or a [multi
   node](../../prereqs/k8s-multi-node.md) cluster.
- Helm: 請遵循著個 [guide](../../prereqs/helm.md).

## Install VOLTHA

在執行包含 OLT/ONU 硬體的實體 POD，啟動 R-CORD 的第一步是需要先安裝 [VOLTHA helm chart](../../charts/voltha.md)。

## Install CORD Platform

R-CORD profile 依賴於以下平台 charts ，因此需要安裝它們：

- [xos-core](../../charts/xos-core.md)
- [onos-fabric](../../charts/onos.md#onos-fabric)
- [onos-voltha](../../charts/onos.md#onos-voltha)

## Install R-CORD Profile

安裝 R-CORD profile:

```shell 
helm dep update xos-profiles/rcord-lite
helm install -n rcord-lite xos-profiles/rcord-lite
```

（可選）如果要使用 [操作指南](configuration.md) 中描述的"bottom up"訂戶配置工作流(subscriber provisioning workflow)，則還需要安裝以下兩個 charts：

- [cord-kafka](../../charts/kafka.md)
- [hippie-oss](../../charts/hippie-oss.md)

> **注意**：如果同時安裝 VOLTHA 和可選的 Kafka ，最終會有兩個Kafka instantiations：`kafka-voltha` 和 `kafka-cord`。

完成 R-CORD 部署後，請閱讀以下指南以了解如何配置它：
[Configure R-CORD](configuration.md)

## Customize an R-CORD Install

Define a `my-rcord-values.yaml` that looks like:

```yaml
# in service charts
addressmanager:
  imagePullPolicy: 'Always'
fabric:
  imagePullPolicy: 'Always'
onos-service:
  imagePullPolicy: 'Always'
volt:
  imagePullPolicy: 'Always'
vsg-hw:
  imagePullPolicy: 'Always'
kubernetes:
  imagePullPolicy: 'Always'
vrouter:
  imagePullPolicy: 'Always'
xos-gui:
  imagePullPolicy: 'Always'
simpleexampleservice:
  imagePullPolicy: 'Always'
```

and use it during the installation with:

```shell
helm install -n rcord-lite xos-profiles/rcord-lite -f my-rcord-values.yaml
```
