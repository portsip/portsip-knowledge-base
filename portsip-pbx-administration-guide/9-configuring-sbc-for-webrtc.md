# 9 Configuring SBC for WebRTC

The PortSIP PBX introduced PortSIP SBC which can work as a component with the PortSIP PBX that provides the WebRTC service; Support Microsoft Teams Direct Routing.

The PortSIP SBC provides flexibility and scalability for the PortSIP PBX, allowing us to support large numbers of WebRTC customers.

Please read "[What's an SBC?](../faq/what-is-the-sbc.md)" before starting. Typically, the SBC concept is shown below.

<figure><img src="../.gitbook/assets/sbc_arch.png" alt=""><figcaption></figcaption></figure>

## Deployment Scenarios

### Enterprise IP PBX with SIP Trunk and Internet Users

The example describes how to configure the SBC when interworking between an IP PBX, a SIP Trunk, and nomadic Internet users (apps, WebRTC, IP Phone). The example scenario includes the following topology architecture:

* Enterprise LAN IP PBX at IP address, 192.168.1.72
* An internet SIP Trunk
* Nomadic internet users
* Microsoft Teams

<figure><img src="../.gitbook/assets/enterprise_pbx_sbc1.drawio.png" alt=""><figcaption></figcaption></figure>

SBC Logical Network Interface Connections:

* One logical network interfacing with the LAN, using IP address 192.168.1.73. The interface is also used for management.
* One logical network interfacing with the DMZ / WAN, using IP address 66.175.221.120.

SBC Physical LAN Port Connections:

* One Ethernet port is connected to the LAN.

In the above topology, the SBC only has one Physical Network Interface that offers two Logical Network Interfaces; for the DMZ/WAN interface, it's a virtual mapping for the IP 66.175.221.120.

The PortSIP SBC can also deploy with the two Physical Network Interfaces.

<figure><img src="../.gitbook/assets/enterprise_pbx_sbc.drawio.png" alt=""><figcaption></figcaption></figure>

SBC Logical Network Interface Connections:

* One logical network interfacing with the LAN, using IP address 192.168.1.73. The interface is also used for management.
* One logical network interfacing with the DMZ / WAN, using IP address 66.175.221.120.

SBC Physical LAN Port Connections:

* One Ethernet port is connected to the LAN.
* One Ethernet port is connected to the DMZ / WAN.

### Hosted Cloud PBX

The example describes how to configure the PortSIP SBC when interworking between LAN IP phones/apps/WebRTC clients and a hosted cloud IP PBX. The example scenario includes the following topology architecture:

<figure><img src="../.gitbook/assets/hosted_pbx_sbc.drawio.png" alt=""><figcaption></figcaption></figure>

SBC Logical Network Interface Connection:

* The example employs two logical network interfaces, one uses IP address 172.31.3.190. The interface is used for communicating with the PBX (172.31.3.192) which sits on the same LAN. One is using IP address 185.53.179.172, provided the SIP service, WebRTC Service, and Microsoft Teams Direct Routing. In the LAN, the IP phones/apps/WebRTC clients communicate with the SBC using SIP and RTP/SRTP.

## Install the PortSIP SBC and PBX on the same server

The PortSIP SBC can be deployed with the PortSIP PBX on the same server; in this case, the PBX provides SIP calling directly, and the SBC provides WebRTC service and also interworking for Microsoft Teams Direct Routing.

Assume we want to deploy the PortSIP PBX and SBC on the following server configuration:

* The server's private IP is **192.168.1.72**, and the public IP is **66.175.221.120**.
* The domain `uc.portsip.cc`has been resolved to the public IP of **66.175.221.120**.
* A trusted SSL certificate for the domain `uc.portsip.cc`

Please read the [Supported Linux OS](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#supported-linux-os), [Preparing the Linux Host Machine for Installation](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#preparing-the-linux-host-machine-for-installation), and [Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/) carefully.

### Install PortSIP PBX and SBC

Assume the PBX has been installed as the [Installation of the PortSIP PBX](1-installation-of-the-portsip-pbx.md).

To install the SBC, please follow the below steps:

#### Install PortSIP SBC for Linux

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

* Execute the below commands to download the SBC installation scripts.

{% hint style="danger" %}
You must use the command `su -` rather than `su root`
{% endhint %}

```shell
su - 
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

* Execute the below command to install the `Docker-Compose` environment.  If get the prompt like`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```bash
/bin/sh install_docker.sh
```

* The below command is used to create and run the SBC on the server.

```shell
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

#### Install PortSIP SBC for Windows

You can download the PortSIP SBC installer at [PortSIP Website](https://www.portsip.com/download-portsip-sbc/), just double click the installer and follow the instructions to install it.

### Configure the PortSIP SBC

1. Prepare the SSL certificate as the guide [TLS Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/).
2. Go to [https://uc.portsip.cc:8883](https://uc.portsip.io:8883) in the browser and log in using the credentials`admin/admin`. If the browser displays the SSL certificate warning, ignore the warning and continue processing.
3. Choose **Settings > TLS Certificates** from the menu, click **Add** button, and enter "**SBC Host Name**" for the **Description** field as an example; enter`uc.portsip.cc` for the **TLS Domain**. Open the **portsip.pem** file in Windows Notepad and copy the contents into the **Certificate Context** field. Copy and paste the contents of the **portsip.key** file into the **Private Key Context** field, and turn on the option **This is SBC Web Domain Certificate**. Click the **OK** button to save the certificates.
4. Select **Settings > Network** from the menu, then fill in the field **Web Domain** with `uc.portsip.cc`, **Private IPv4** with`192.168.1.72`, and Public IPv4 with `66.175.221.120`. By default, the **Create default transports automatically** option is turned on, and the SBC will create the default transports after successfully setting up the SBC IP address. We suggest keeping this turned on to create the default transports.
5.  Default transports:

    * &#x20;`TCP on port 5069` : it's used to communicate with PBX
    * &#x20;`TLS on port 5067` : it's used to communicate with Microsoft Teams
    * &#x20;`WSS on the port 5065` : it's used to provide the WebRTC service.&#x20;
    * `UDP on the port 5066`: it's used to provide the normal SIP service.&#x20;

    You can turn off this option to prevent the SBC from automatically creating default transports, **but this is not recommended**.
6. When you click the **OK** button, SBC will restart automatically and immediately sign you out.
7.  Execute the below commands on the server. If the server is Windows, just restart the server directly.

    ```shell
    /bin/sh sbc_ctl.sh restart
    ```
8. Sign in to the PBX web portal [https://uc.portsip.cc:8887](https://uc.portsip.io:8887), and click the menu **Advanced > SBC**. Click the **Generate** button to generate the token for the SBC's access. Click the **Copy** button to copy the token.
9. Sign in to the PortSIP SBC Web Portal [https://uc.portsip.cc:8883](https://uc.portsip.io:8883). Choose **Settings > PBX** from the menu. You have to set up the PBX information here then the SBC will communicate with the PBX. Paste the copied token to the **PBX Access Token** field, and enter the PBX private IP `192.168.1.72` for the **PBX IPv4 Address** field. Since the TCP transport is created on port **5063** in the PBX, please choose the **TCP** for **Prefer to transport to communicate with PBX**, and enter "**5063**" for **PBX Port**.&#x20;
10. Choose **Settings > Transports** from the menu. The SBC requires adding three types of transport here. One is for the WebRTC clients, one for Microsoft Teams, and one for the SBC to communicate with the PBX. After the above **step 4** is completed with the **Create default transports automatically** option enabled, the SBC will create the default transport automatically. **These default transports aren't recommended to be changed.**
11. **This step is unnecessary if you keep using the default transport**. If you wish to create your own transport, you can delete the existing transport.
    * Add the **TCP** transport for SBC to communicate with the PBX, please refer to the below screenshot, and choose the **SBC private IP** for the **Network Interface**.
    * Add the **WSS** transport for WebRTC clients as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.
    * Add the **TLS** transport for the SBC to communicate with Microsoft Teams as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.
    * Add a **UDP** transport for the SBC to provide the normal SIP service as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.

<figure><img src="../.gitbook/assets/sbc_transports_1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/sbc_transports_2.png" alt=""><figcaption></figcaption></figure>

&#x20;12\. If you created your own transport in **step 11** rather than using the default transport, please open the ports of your transport on the firewall with the below commands. For example, if you create a **UDP** transport on port **5066**, **TCP** transport on port **5069**, **TLS** transport on port **5067**, and **WSS** transport on port **5065**, please execute the below commands.

```bash
firewall-cmd --permanent --service=portsip-sbc --add-port=5066/udp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5065/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5067/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5069/tcp --set-description="PortSIP SBC"
firewall-cmd --reload
```

&#x20;13\. Open the URL [https://uc.portsip.cc:10443/webrtc](https://uc.portsip.cc:10443/webrtc) in the browser, and the WebRTC client will be launched. Just enter the extension number, password, and the tenant SIP domain to register to PBX to make & receive calls.

{% hint style="danger" %}
Once added a new transport, you have to change the firewall rule to allow that transport port. The client app, IP Phone will reach PBX by transport and port.
{% endhint %}

## Install the PortSIP SBC and PBX on separate servers

Typically, PortSIP SBC is deployed on a separate server from the PortSIP PBX, the SBC sits in front and the PBX is transparent to the end users.

Assuming we want to install the PortSIP PBX and SBC with the following configurations:

* The PBX server's private IP is **192.168.1.72**.
* The SBC server's private IP is **192.168.1.73**, and the public IP is **66.175.221.120**
* The domain `sbc.portsip.cc` has been resolved to the SBC server public IP of **66.175.221.120**.
* The domain `uc.portsip.cc` has been resolved to the PBX server **192.168.1.72**, this step is not necessary.
* A trusted **Wildcard SSL certificate** for the domain `portsip.cc`

Please read the [Supported Linux OS](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#supported-linux-os), [Preparing the Linux Host Machine for Installation](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#preparing-the-linux-host-machine-for-installation), and [TLS Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/) carefully.

### Install PortSIP PBX and SBC

Assume the PBX has been installed as the [1 Installation of the PortSIP PBX](1-installation-of-the-portsip-pbx.md).

#### Install PortSIP SBC for Linux

In the SBC server, to install the SBC, please follow the below steps.

* Execute the below commands to download the SBC installation scripts.

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

{% hint style="danger" %}
You must use the su - rather than su root
{% endhint %}

```shell
sudo -
mkdir -p /opt/portsip
cd /opt/portsip
```

```bash
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/install_sbc_docker.sh \
-o install_sbc_docker.sh
```

```bash
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/sbc_ctl.sh \
-o sbc_ctl.sh
```

* Execute the below command to install the `Docker-Compose` environment.  If get the prompt like`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```bash
/bin/sh install_sbc_docker.sh
```

* The below command is used to create and run the SBC on the PBX server.

```shell
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

#### Install PortSIP SBC for Windows

You can download the PortSIP SBC installer at [PortSIP Website](https://www.portsip.com/download-portsip-sbc/), just double click the installer and follow the instructions to install it.

### Configure the PortSIP SBC

1. Prepare a **wildcard SSL certificate** as the guide [Preparing SSL Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/).
2. Go to [https://sbc.portsip.cc:8883](https://sbc.portsip.io:8883) in the browser and log in using the credentials`admin/admin`. If the browser displays the SSL certificate warning, ignore the warning and continue processing.
3. Choose **Settings > TLS Certificates** from the menu, click the **Add** button, and type "**SBC Host Name"** for the **Description** field as an example; enter `portsip.cc` for the **TLS Domain**. Open the **portsip.pem** file in Windows Notepad and copy the contents into the **Certificate Context** field. Copy and paste the contents of the **portsip.key** file into the **Private Key Context** field, and turn on the option **This is SBC Web Domain Certificate**. Click the **OK** button to save the certificates.
4. Select **Settings > Network** from the menu, then fill in the field **Web Domain** with `portsip.cc`, **Private IPv4** with`192.168.1.73`, and Public IPv4 with `66.175.221.120`. By default, the **Create default transports automatically** option is turned on, and the SBC will create the default transports after successfully setting up the SBC IP address.&#x20;
5.  Default transports:

    * &#x20;`TCP on port 5069` : it's used to communicate with PBX
    * &#x20;`TLS on port 5067` : it's used to communicate with Microsoft Teams
    * &#x20;`WSS on the port 5065` : it's used to provide the WebRTC service.&#x20;
    * `UDP on the port 5066`: it's used to provide the normal SIP service.&#x20;

    You can turn off this option to prevent the SBC from automatically creating default transports, **but this is not recommended**.
6. When you click the **OK** button, SBC will restart automatically and immediately sign you out.
7.  Execute the below commands on the SBC server.  If the server is Windows, just restart the server directly.

    ```shell
    /bin/sh sbc_ctl.sh restart
    ```
8. Sign in to the PBX web portal, [https://uc.portsip.cc:8887](https://uc.portsip.io:8887), and click the menu **Advanced > SBC**. Click the **Generate** button to generate the token for the SBC's access. Click the **Copy** button to copy the token.
9. Sign in to the PortSIP SBC Web Portal [https://uc.portsip.cc:8883](https://uc.portsip.io:8883). Choose **Settings > PBX** from the menu. You have to set up the PBX information here then the SBC will communicate with the PBX. Paste the copied token to the **PBX Access Token** field, and enter the PBX private IP `192.168.1.72` for the **PBX IPv4 Address** field. Since the TCP transport is created on port 5063 in the PBX, please choose the **TCP** for **Prefer to transport to communicate with PBX**, and enter **5063** for **PBX Port**.
10. **This step is unnecessary if you keep using the default transport**. If you wish to create your own transport, you can delete the existing transport.
    * Add the **WSS** transport for WebRTC clients as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.
    * Add the **TLS** transport for the SBC to communicate with Microsoft Teams as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.
    * Add a **UDP** transport for the SBC to provide the normal SIP service as in the below screenshot, please choose the **SBC public IP** for the **Network Interface**.
    * Add the **TCP** transport for SBC to communicate with the PBX, please refer to the below screenshot, and choose the **SBC private IP** for the **Network Interface**;&#x20;
11. Choose **Settings > Transports** from the menu. The SBC requires adding three types of transport here. One is for the WebRTC clients, one for Microsoft Teams, and one for the SBC to communicate with the PBX. After the above **step 4** is completed with the **Create default transports automatically** option on, the SBC will create the default transport automatically. **These default transports aren't recommended to be changed.**

<figure><img src="../.gitbook/assets/sbc_transports_1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/sbc_transports_2.png" alt=""><figcaption></figcaption></figure>

12. &#x20;If you created your own transport in **step 9** rather than using the default transport, please open the ports of your transport on the firewall with the below commands. For example, if you create a **UDP** transport on port **5066**, **TCP** transport on port **5069**, **TLS** transport on port **5067**, and **WSS** transport on port **5065**, please execute the below commands.&#x20;

```
firewall-cmd --permanent --service=portsip-sbc --add-port=5066/udp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5067/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5069/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5065/tcp --set-description="PortSIP SBC"
firewall-cmd --reload
```

13. &#x20;Open the URL [https://uc.portsip.cc:10443/webrtc](https://uc.portsip.cc:10443/webrtc) in the browser, and the WebRTC client will be launched. Just enter the extension number, password, and the tenant SIP domain to register to PBX to make & receive calls.

After successfully creating the SBC docker instance, you can use the below commands to manage it.

#### Display the SBC Docker Instance Status.

```bash
/bin/sh sbc_ctl.sh status
```

#### Start the SBC Docker Instance.

```bash
/bin/sh sbc_ctl.sh start
```

#### Stop the SBC Docker Instance.

```bash
/bin/sh sbc_ctl.sh stop
```

#### Restart the SBC Docker Instance.

```bash
/bin/sh sbc_ctl.sh restart
```

#### Delete the SBC Docker Instance.

```bash
/bin/sh sbc_ctl.sh rm
```

### Add the SBC IP address to the PBX whitelist

To prevent the PBX from limiting the request rate, we need to add the SBC IP to the whitelist in the PBX.&#x20;

To do this, sign in as the System Administrator and select the menu **IP Blacklist** > **Add**. Then enter the SBC IP as shown in the screenshot below and choose a long **expiration date.**

<figure><img src="../.gitbook/assets/sbc_whitelist.png" alt=""><figcaption></figcaption></figure>

### Check opened firewall ports

The below commands are used to check currently opened ports for PortSIP SBC.

```sh
firewall-cmd --info-service=portsip-sbc
```

