# 2 PortSIP PBX Management

If you have already completed the configurations in the [**Install PortSIP PBX**](../installation-of-portsip-pbx-v22.3-beta-version/install-portsip-pbx.md) step 4, please skip this article.

The default system administrator username and password are both **admin**.

### Configure the PortSIP PBX

Once the PortSIP PBX is successfully installed, you can access the web portal by opening your browser and navigating to [**https://66.175.221.120:8887**](https://66.175.221.120:8887). If your browser displays an SSL certificate warning, you can safely ignore it and continue. You will then be directed to the login page, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/login-1.png" alt="" width="321"><figcaption></figcaption></figure>

Click on **"Sign in as the administrator or dealer"** to navigate to the administrator login page, as shown in the screenshot below. Enter **admin** as both the username and password to log in to the web portal.

<figure><img src="../../../.gitbook/assets/login-2.png" alt="" width="320"><figcaption></figcaption></figure>

{% hint style="danger" %}
Please change the admin's default password after you log in.
{% endhint %}

After successfully logging into the PBX Web Portal with a new installation, the PBX will launch a setup wizard automatically to guide you through completing the mandatory settings.

#### **1. Network Environment**

* **Private IPv4 Address**\
  You must enter the server's private IPv4 address. If the server does not have a private IP, use the public IP address instead.
* **Public IPv4 Address**\
  If the PBX server has a static public IP address, enter it in the **Public IPv4** field. If the server does not have a static public IP, leave this field blank.

These IP addresses must be accessible to your SIP clients, as the IP entered here will function as the SIP server IP address for the PBX. This is crucial when a SIP client or IP phone registers to PortSIP PBX, and should be configured as the **Outbound Proxy Server**.

* **Cloud Deployment:**\
  If the PBX is deployed in the cloud, both **Private IPv4** and **Public IPv4** addresses must be entered.
* **LAN Deployment:**\
  If the PBX is on a local network (LAN), only the **Private IPv4** address is required.

{% hint style="danger" %}
The loopback interface (127.0.0.1) is unacceptable for the private IP. Only the static IP for the LAN where the PBX is located is allowed (do not use DHCP dynamic IP).&#x20;
{% endhint %}

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_1.png" alt=""><figcaption></figcaption></figure>

#### **2. SSL Certificate**

To enable **TLS** transport for SIP and secure **HTTPS** access to the Web Portal and REST API, a trusted SSL certificate must be uploaded during this step.

* **Domain Setup:**\
  You will need a web domain. For example, you can purchase a domain from providers like GoDaddy and point it to your PBX’s IP address.
* **SSL Certificate:**\
  A trusted SSL certificate is necessary to avoid browser warnings. Recommended certificate providers include **DigiCert**, **GeoTrust**, **GoDaddy**, and others.
  * If you do not have a domain or SSL certificate, you can use your PBX’s IP address as the **Web Domain** and proceed with the default certificate. However, please note that PortSIP PBX uses a self-signed certificate by default, which will trigger browsers to block the connection and display a security warning.
* **Certificate Providers:**\
  To purchase an SSL certificate, follow the guide: [Preparing TLS Certificates for TLS/HTTPS/WebRTC](../certificates-for-tls-https-webrtc/).

You will have two certificate files if you complete the steps in the guide:

* **portsip.key**
* **portsip.pem**

**Configuring the Certificates**

In this guide, we assume the use of the domain **uc.portsip.cc** for the PBX web domain.

1. In the **Web Domain** field, enter **uc.portsip.cc**.
2. Open the **portsip.pem** file in a text editor (such as Windows Notepad), and copy the entire contents into the **Certificate File** field.
3. Similarly, open the **portsip.key** file, and copy its entire contents into the **Private Key File** field.

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_2.png" alt=""><figcaption></figcaption></figure>

#### **3. Transport Protocol**

You can configure the transport layer protocol for SIP signalling by clicking the **Add** button. The default transport ports are as follows:

* **UDP**: 5060
* **TCP**: 5063
* **TLS**: 5061

You are free to change these default ports to any preferred value, but ensure that the new port is not already in use by other applications.

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_3.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
After adding a new transport protocol, be sure to update your firewall rules to allow traffic on the newly assigned transport port. The IP Phone and client app will use this transport and port to connect to the PBX.
{% endhint %}

### Reboot to Apply the Certificate

If you **uploaded** a trusted SSL certificate in the above steps instead of using the default self-signed certificate, you need to restart the PBX to apply the changes. Use the following commands to reboot the PBX:

```sh
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh restart
```

Now that the PortSIP PBX is successfully installed, you can use [https://uc.portsip.cc:8887](https://uc.portsip.cc:8887) to access the PortSIP PBX web portal.

### Managing PortSIP PBX Docker Instance

After successfully installing the PortSIP PBX, you can use the following commands to manage the PortSIP PBX Docker instance.

```sh
cd /opt/portsip
```

#### Show the PBX Docker Instance Status

<pre class="language-sh"><code class="lang-sh"><strong>sudo /bin/sh pbx_ctl.sh status
</strong></code></pre>

#### Start the PBX Docker Instance

```bash
sudo /bin/sh pbx_ctl.sh start
```

#### Stop the PBX Docker Instance

```bash
sudo /bin/sh pbx_ctl.sh stop
```

#### Restart the PBX Docker Instance

```bash
sudo /bin/sh pbx_ctl.sh restart
```

#### Delete the PBX Docker Instance

This command will not delete the data of the PBX.

<pre class="language-bash"><code class="lang-bash"><strong>sudo /bin/sh pbx_ctl.sh rm
</strong></code></pre>

### Managing PortSIP IM Service Docker Instance

First, you will need to at the `/opt/portsip` folder, then you can use the following commands to manage the PortSIP IM Service Docker instance.

```sh
cd /opt/portsip
```

#### Show the IM Service Docker Instance Status

<pre class="language-sh"><code class="lang-sh"><strong>sudo /bin/sh im_ctl.sh status
</strong></code></pre>

#### Start the IM Service Docker Instance

```bash
sudo /bin/sh im_ctl.sh start
```

#### Stop the IM Service Docker Instance

```bash
sudo /bin/sh im_ctl.sh stop
```

#### Restart the IM Service Docker Instance

```bash
sudo /bin/sh im_ctl.sh restart
```

#### Delete the IM Service Docker Instance

This command will not delete the data of the PBX.

<pre class="language-bash"><code class="lang-bash"><strong>sudo /bin/sh im_ctl.sh rm
</strong></code></pre>

### Managing PortSIP Data Flow Service Docker Instance

First, you will need to be at the `/opt/portsip` folder, then you can use the following commands to manage the PortSIP Data Flow Service Docker instance.

```sh
cd /opt/portsip
```

#### Show the Data Flow Service Docker Instance Status

<pre class="language-sh"><code class="lang-sh"><strong>sudo /bin/sh dataflow_ctl.sh status
</strong></code></pre>

#### Start the Data Flow Service Docker Instance

```bash
sudo /bin/sh dataflow_ctl.sh start
```

#### Stop the Data Flow Service Docker Instance

```bash
sudo /bin/sh dataflow_ctl.sh stop
```

#### Restart the Data Flow Service Docker Instance

```bash
sudo /bin/sh dataflow_ctl.sh restart
```

#### Delete the Data Flow Service Docker Instance

This command will not delete the data of the PBX.

<pre class="language-bash"><code class="lang-bash"><strong>sudo /bin/sh dataflow_ctl.sh rm
</strong></code></pre>

