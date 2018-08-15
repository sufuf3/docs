# Fabric Software Setup

CORD 使用 Trellis fabric 來和 data plane components 互相溝通。
這個 section 是描述如何為這些 switch  setup 。

最新的 [Trellis Fabric](https://wiki.opencord.org/display/CORD/Trellis%3A+CORD+Network+Infrastructure) 文件可以在 CORD wiki 上查看。

## Supported Switches

可以支援的硬體請參考 [hardware requirements page](prereqs/hardware.md).

## Operating System

All CORD-compatible switches use
[Open Networking Linux (ONL)](https://opennetlinux.org/) as the operating system.
The [latest compatible ONL image](https://github.com/opencord/OpenNetworkLinux/releases/download/2017-10-19.2200-1211610/ONL-2.0.0_ONL-OS_2017-10-19.2200-1211610_AMD64_INSTALLED_INSTALLER) can be downloaded from [here](https://github.com/opencord/OpenNetworkLinux/releases/download/2017-10-19.2200-1211610/ONL-2.0.0_ONL-OS_2017-10-19.2200-1211610_AMD64_INSTALLED_INSTALLER).

**Checksum**: *sha256:2db316ea83f5dc761b9b11cc8542f153f092f3b49d82ffc0a36a2c41290f5421*

Guidelines on how to install ONL on top of an ONIE compatible device can be found directly on the [ONL website](https://opennetlinux.org/docs/deploy).

This specific version of ONL has been customized to accept an IP address through DHCP on the management interface, *ma0*. If you'd like to use a static IP, first give
it an IP address through DHCP, then login and change the configuration in
*/etc/network/interfaces*.

The default *username* and *password* are *root* / *onl*.

## OFDPA Drivers

Once ONL is installed, OFDPA drivers will need to be installed as well.
Each switch model requires a specific version of OFDPA. All driver packages are distributed as DEB packages, which makes the installation process straightforward.

First, copy the package to the switch. For example

```shell
scp your-ofdpa.deb root@fabric-switch-ip:
```

Then, install the DEB package

```shell
dpkg -i your-ofdpa.deb
```

Three OFDPA drivers are available:

* [EdgeCore 5712-54X / 5812-54X / 6712-32X](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/builds/ofdpa_3.0.5.5%2Baccton1.7-1_amd64.deb?raw=true) - *checksum: sha256:db228b6e79fb15f77497b59689235606b60abc157e72fc3356071bcc8dc4c01f*
* [EdgeCore 7712-32X](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/builds/ofdpa_3.0.5.5%2Baccton1.7-1_amd64.deb) - *checksum: sha256:4f78e8f43976dc86ab1cdc2f98afa743ce2e0cc5923e429c91f96b0edc3ddf4b*
* [QuantaMesh T3048-LY8](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/builds/ofdpa-ly8_0.3.0.5.0-EA5-qct-01.01_amd64.deb?raw=true) - *checksum: sha256:f8201530b1452145c1a0956ea1d3c0402c3568d090553d0d7b3c91a79137da9e*
* [QuantaMesh BMS T7032-IX1/IX1B](https://github.com/onfsdn/atrium-docs/blob/master/16A/ONOS/builds/ofdpa-ix1_0.3.0.5.0-EA5-qct-01.00_amd64.deb?raw=true) *checksum: sha256:278b8ffed8a8fc705a1b60d16f8e70377e78342a27a11568a1d80b1efd706a46*

## Connect the Fabric Switches to ONOS

將 Fabric Switches 連結到 ONOS
如果 switches 還沒有連接，那就 ssh 到每台 switch 中在 */etc/ofagent/ofagent.conf* 設定，uncommenting 並編輯如下：

```shell
OPT_ARGS="-d 2 -c 2 -c 4 -t K8S_NODE_IP:31653 -i $DPID"
```

Then start ofagent by running

```shell
service ofagentd start
```

You can verify ONOS has recognized the devices using the following command:

> NOTE: When prompted, use password `rocks`.

```shell
ssh -p 31101 onos@K8S_NODE_IP devices
```

> NOTE: It may take a few seconds for the switches to initialize and connect to ONOS
