# 1 Installation of the PortSIP SBC

## 1.1 Downloading PortSIP SBC

The latest free edition of [PortSIP SBC](https://www.portsip.com/portsip-sbc/) can always be found and downloaded at [PortSIP Website](https://www.portsip.com/download-portsip-sbc/). It’s available for both 64-bit Windows and Linux, but not for the 32-bit version.

The free edition of [PortSIP SBC](https://www.portsip.com/portsip-sbc/) offers a maximum of 3 simultaneous calls. If you require more simultaneous calls, please refer to the [PortSIP SBC Pricing page](https://www.portsip.com/portsip-sbc-pricing/) to get more details.

You will get the installer after the download is completed.

## 1.2 Installing PortSIP SBC on Linux

### **Supported Linux OS**

* Ubuntu 20.04, 22.04, 24.04
* Debian 11.x, 12.x

It only supports 64-bit OS.

### **Preparing the Linux Host Machine for Installation**

Tasks that MUST be completed before installing PortSIP SBC.

* If the Linux on which SBC will be installed is located on a LAN, assign a**`static LAN IP address`**to the SBC server; if it's on a public network, assign a**`static private IP address`** and a **`static public IP address`** to the SBC server.
* Install all available updates and service packs before installing PortSIP SBC.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High Performance).
* Do not install TeamViewer, VPN, or other similar software on the host machine.
* The PortSIP SBC must not be installed on a host that is a DNS or DHCP server.
* The below ports must be permitted by your firewall.
  * UDP: 5066, 25000-34999
  * TCP:  5065, 5067, 8882, 8883, 10443. please also ensure the above ports have not been used by other applications.
* Ensure the server date-time is synced correctly.
* Must execute all Linux commands as the root user. please `su root` first.

{% hint style="danger" %}
If the SBC runs on a cloud platform such as AWS and the cloud platform has its own firewall, you must also open the ports on the cloud platform's firewall.
{% endhint %}

### **Step 1 Download the  Installation Scripts**

Execute the below commands to download the installation scripts.

{% hint style="danger" %}
You must use the su - rather than su root
{% endhint %}

```shell
su -
mkdir -p /opt/portsip
cd /opt/portsip
```

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

```shell
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh \
-o install_docker.sh
```

```sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/sbc_ctl.sh \
-o sbc_ctl.sh
```

### **Step 2 Setup the Docker Environment**

Execute the below command to install the `Docker-Compose` environment. If you get the prompt like`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```shell
/bin/sh install_docker.sh
```

### Step 3 Create and Run the PortSIP SBC Docker Container Instance

The below command is used to create and run the SBC on a server.

```shell
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

You can now access the SBC Web portal via [https://server-ip-or-domain:8883](https://server-ip-or-domain:8883), the default system administrator name and password are`admin`.

After successfully creating the SBC docker instance, you can use the below commands to manage it.

#### Display the SBC Docker Instance Status

```bash
/bin/sh sbc_ctl.sh status
```

#### Start the SBC Docker Instance

```bash
/bin/sh sbc_ctl.sh start
```

#### Stop the SBC Docker Instance

```bash
/bin/sh sbc_ctl.sh stop
```

#### Restart the SBC Docker Instance

```bash
/bin/sh sbc_ctl.sh restart
```

#### Delete the SBC Docker Instance

```bash
/bin/sh sbc_ctl.sh rm
```



## 1.3 Installing PortSIP SBC on Windows

### **Supported Windows OS**

* Windows 10, 11
* Windows Server 2016, 2019, 2022

It only supports 64-bit OS.

### **Preparing the Windows Host Machine for Installation**

Tasks that MUST be completed before installing PortSIP SBC.

* If the Windows PC / server on which SBC will be installed is located in LAN,assign a**`static LAN IP address`**to the SBC server; if it's on a public network, assign a**`static private IP address`** and a **`static public IP address`** to the SBC server.
* Install all available Windows updates & service packs before installing PortSIP SBC. The reboot after installing Windows updates may reveal additional updates. Pay particular attention to installing all updates for Microsoft .Net before running the PortSIP PBX installation.
* Anti-virus Software should not scan the following directories to avoid complications and write access delays: `C:\Program Files\PortSIP`; `C:\Programdata\PortSIP`
* Do not install VPN, TeamViewer software on your PortSIP SBC Server
* Ensure the **Windows Firewall** service has been started.
* Ensure that all power-saving options for your System and Network adapters are disabled (by setting the system to High Performance).
* Disable Bluetooth adapters if it is a Windows client PC.
* PortSIP SBC must not be installed on a host which is a DNS or DHCP server, or that has MS SharePoint or Exchange services installed.
* The below ports must be permitted by your firewall.
  * UDP: 5066, 25000-35000
  * TCP: 5065, 5067, 8882, 8883, 10443. Please also ensure the above ports have not been used by other applications.
* Ensure server date-time is synced correctly.
* Ensure your Windows firewall is enabled.

{% hint style="danger" %}
If the SBC runs on a cloud platform such as AWS and the cloud platform has its own firewall, you must also open the ports on the cloud platform's firewall.
{% endhint %}



### **Step 1 Installing a fresh PortSIP SBC for Windows**

The SBC installer can be downloaded at [PortSIP Website](https://www.portsip.com/download-portsip-sbc/).

To install PortSIP SBC, you only need to double-click the installer, which will guide you through the installation process.

PortSIP SBC services will automatically start after successful installation (and thereafter every time your computer starts up).

### **Step 2 Configuring Windows Firewall Rules**

After having successfully installed PortSIP SBC, the PortSIP SBCports have been opened with the Windows firewall.

If your server has a firewall that is blocking the ports, you must open the below ports in order to make the PortSIP SBC work properly.

* The below ports must be permitted by your firewall.
  * UDP: 5066, 25000-35000
  * TCP: 5065, 5067, 8882, 8883, 10443. Please also ensure the above ports have not been used by other applications.

{% hint style="danger" %}
If the SBC runs on a cloud platform such as AWS and the cloud platform has its own firewall, you must also open the ports on the cloud platform's firewall.
{% endhint %}

You can now access the SBC Web portal via [https://server-ip-or-domain:8883](https://server-ip-or-domain:8883), the default system administrator name and password are`admin`.

## 1.4 Uninstall PortSIP SBC

Please use the below steps to uninstall the PortSIP SBC for Linux.

### Uninstall the SBC

```
cd /opt/portsip
/bin/sh sbc_ctl.sh stop
/bin/sh sbc_ctl.sh rm
```

The below commands will delete the SBC data, be careful.

```
cd /var/lib/portsip/
rm -rf sbc
```

For the Windows installation, just uninstall it from the Windows control panel.

