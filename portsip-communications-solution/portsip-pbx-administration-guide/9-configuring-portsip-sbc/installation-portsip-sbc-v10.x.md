# Installation PortSIP SBC v10.x

{% hint style="warning" %}
If your PortSIP PBX has already been upgraded to version 22.x, you will also need to upgrade your PortSIP SBC from version 10.x to version 11.x in order to use the latest WebRTC app.
{% endhint %}

Before proceeding, please review the following sections carefully:

## Supported OS

* Debian 11.x, 12.x
* Ubuntu 22.04, 24.04
* Windows 10 1903/19H1 or higher, Windows 11
* Windows Server 2022 or higher

## Preparing the Server for Installation <a href="#preparing-the-server-for-installation" id="preparing-the-server-for-installation"></a>

Tasks that MUST be completed before installing PortSIP SBC.

* **Ensure the server date-time is synced correctly**.
* For the Linux, use the `sudo` to perform the installation is recommended. For Windows, it requires the Administrator user.
* If the server on which SBC will be installed is located on a LAN, assign &#x61;**`static private IP address`**&#x74;o the PBX server; if it's on a public network, assign &#x61;**`static public IP address`** and a **`static private IP`** to the PBX server.
* Install all available updates and service packs before installing PortSIP SBC.
* Do not install **PostgreSQL** on your PortSIP SBC Server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or other similar software on the host machine.
* The PortSIP SBC must not be installed on a host that is a DNS or DHCP server.
* The below ports must be permitted by your firewall(these ports are required by the PortSIP SBC).
  * UDP: 5066, 25000-34999
  * TCP: 5065, 5067, 10443. please also ensure the above ports have not been used by other applications.
* If installed on Windows, ensure the **`Windows Firewall`** service has been started.

{% hint style="warning" %}
If the PBX runs on a cloud platform such as AWS and the cloud platform has its own firewall, you **must** also open the ports on the cloud platform's firewall.
{% endhint %}

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Prerequisites

Assume that you have successfully installed the PortSIP PBX following the instructions in the [Installation of the PortSIP PBX](../1-installation-of-the-portsip-pbx-1/) guide.

## Install the PortSIP SBC and PBX on the Same Server

The PortSIP SBC can be deployed with the PortSIP PBX on the same server. In this configuration, the PBX handles SIP calling directly, while the SBC provides WebRTC services and enables interworking for Microsoft Teams Direct Routing.

For this example, assume the following server configuration:

* **Private IP**: 192.168.1.72
* **Public IP**: 66.175.221.120
* The domain **uc.portsip.cc** is resolved to the public IP address **66.175.221.120**.
* A trusted SSL certificate(not self-signed) is installed for the domain **uc.portsip.cc**. Please follow the article [Certificates for TLS/HTTPS/WebRTC](../certificates-for-tls-https-webrtc/) to prepare the certificates.

### Install PortSIP SBC for Linux

To install the SBC, please perform the below commands:

```shell
sudo cd /opt/portsip
```

```bash
sudo curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/sbc_ctl.sh \
-o sbc_ctl.sh
```

```shell
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

### Install PortSIP SBC for Windows

You can download the PortSIP SBC installer at [PortSIP Website](https://www.portsip.com/download-portsip-sbc/), just double click the installer and follow the instructions to install it.

### Configuring PortSIP SBC

Now follow the guide [Configure PortSIP SBC on the Same Server as PortSIP PBX](configuring-sbc-for-webrtc.md#configure-portsip-sbc-on-the-same-server-as-portsip-pbx) to complete the SBC configuration.

## Install the PortSIP SBC on a Separate Server

Typically, the PortSIP SBC is deployed on a separate server from the PortSIP PBX. In this configuration, the SBC acts as a front-end component, while the PBX remains transparent to the end users.

Assuming the following server configuration for installation:

* **PBX Server (Private IP)**: 192.168.1.72
* **SBC Server (Private IP)**: 192.168.1.73
* **SBC Server (Public IP)**: 66.175.221.120
* The domain **sbc.portsip.cc** is resolved to the SBC server's public IP, **66.175.221.120**.
* The domain **uc.portsip.cc** is resolved to the PBX server's private IP, **192.168.1.72**. (Note: This step is not necessary for the SBC deployment.)
* A trusted **Wildcard SSL certificate**(not self-signed) is installed for the domain **portsip.cc**. Please follow the article [Certificates for TLS/HTTPS/WebRTC](../certificates-for-tls-https-webrtc/) to prepare the certificates.

### Install PortSIP SBC for Linux

Please follow the below steps to install the PortSIP SBC on that separate server.

```shell
mkdir -p /opt/portsip
cd /opt/portsip
```

```bash
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh \
-o install_docker.sh
```

```bash
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/sbc_ctl.sh \
-o sbc_ctl.sh
```

Execute the below command to install the `Docker-Compose` environment.  If get the prompt like`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```bash
sudo /bin/sh install_docker.sh
```

Now, execute the following command to create and run the PortSIP SBC Docker instance.

```sh
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

### Install PortSIP SBC for Windows

You can download the PortSIP SBC installer from the [PortSIP Website](https://www.portsip.com/download-portsip-sbc/). Simply double-click the installer and follow the on-screen instructions to complete the installation.

### Configuring PortSIP SBC

Now follow the guide [Configure PortSIP SBC on a Separate Server](configuring-sbc-for-webrtc.md#configure-portsip-sbc-on-a-separate-server) to complete the SBC configuration.

## Managing PortSIP SBC Docker Instance

After successfully installing the SBC, you can use the following commands to manage the PortSIP SBC docker instance.

```sh
cd /opt/portsip
```

### Show the SBC Docker Instance Status

<pre class="language-sh"><code class="lang-sh"><strong>sudo /bin/sh sbc_ctl.sh status
</strong></code></pre>

### Start the SBC Docker Instance

```bash
sudo /bin/sh sbc_ctl.sh start
```

### Stop the SBC Docker Instance

```bash
sudo /bin/sh sbc_ctl.sh stop
```

### Restart the SBC Docker Instance

```bash
sudo /bin/sh sbc_ctl.sh restart
```

### Delete the SBC Docker Instance

This command will not delete the data of the SBC.

<pre class="language-bash"><code class="lang-bash"><strong>sudo /bin/sh sbc_ctl.sh rm
</strong></code></pre>



