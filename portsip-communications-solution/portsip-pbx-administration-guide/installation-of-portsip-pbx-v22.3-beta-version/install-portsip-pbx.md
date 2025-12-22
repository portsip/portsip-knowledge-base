# Install PortSIP PBX

### Upgrading

This guide applies **only to fresh installations** of the **latest PortSIP PBX v22.x**.

If you are **upgrading from an earlier version**, such as **v16.x**, **v22.1.x**, or **v22.2.x**, please follow the appropriate upgrade guide below:

* [Upgrade to the Latest Version Within v22.x](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/upgrade-to-the-latest-version-within-v22.x-on-linux.md)
* [Upgrade v16.x to the Latest v22.x](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/upgrade-v16.x-to-the-latest-v22.x-on-linux.md)

***

### Minimal Hardware Requirements

The following minimum hardware configuration is required for a basic deployment:

* **CPU:** 2 cores
* **Memory:** 4 GB
* **Disk:** 50 GB

With this configuration, the PBX can support:

* Up to **1,000 online (registered) users**
* Approximately **300–500 simultaneous calls**

***

### Supported Operating Systems

PortSIP PBX v22.x supports the following Linux operating systems:

* **Ubuntu:** 22.04, 24.04
* **Debian:** 12

***

### Preparing the Server for Installation

The following tasks **must be completed before installing PortSIP PBX**.

#### System Preparation

* Ensure the system date and time are correctly synchronized.
* Performing the installation using `sudo` is recommended.
* Assign static IP addresses:
  * If the PBX server is on a LAN, assign a static private IP address.
  * If the PBX server is on a public network, assign both a static public IP and a static private IP.
* Install all available OS updates and service packs before installing the PBX.
* Do not install PostgreSQL on the PortSIP PBX server.
* Disable all power-saving options for the system and network adapters (set the system to High-Performance mode).
* Do not install TeamViewer, VPN software, or similar tools on the host machine.
* The PortSIP PBX must not be installed on a server acting as a DNS or DHCP server.

***

### Firewall and Network Requirements

The following ports **must be allowed by the firewall** and **must not be used by other applications**.

* **UDP:** 5060, 5066, 25000–34999, 45000–65000
* **TCP:** 5061, 5063, 5065, 5067, 8882, 8883, 8887, 8888, 10443

> ❗**Important**\
> If the PBX is deployed on a **cloud platform** such as AWS, Azure, or other providers, you must also open these ports in the **cloud platform’s firewall or security group**.

***

### Installing PortSIP PBX

#### Step 1: Download Installation Scripts

Run the following commands to download the installation scripts and initialize the environment:

```bash
sudo mkdir -p /opt/portsip
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh \
-o init.sh
sudo /bin/sh init.sh
```

***

#### Step 2: Set Up the Docker Environment

All commands must be executed in the `/opt/portsip` directory.

Run the following command to install the **Docker and Docker-Compose environment**:

```bash
cd /opt/portsip
sudo /bin/sh install_docker.sh
```

If you are prompted with:

```
cloud.cfg (Y/I/N/O/D/Z) [default=N] ?
```

Enter **Y** and press **Enter** to continue.

***

#### Step 3: Create and Run the PBX Docker Instance

The following command creates and runs the PortSIP PBX Docker instance on a server with the public IP address `66.175.221.120`. If the PBX is deployed on a **LAN without a public IP**, replace `66.175.221.120` with the PBX server’s **private LAN IP address**.

```bash
sudo /bin/sh pbx_ctl.sh run \
-p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22
```

**Command Parameters Explained**

* **`-p`**: Specifies the directory for storing PBX data.
* **`-a`**: Specifies the PBX server IP address (public or private, depending on deployment).
* **`-i`**: Specifies the PBX Docker image version.
* **`-f`** _: (Optional)_ Specifies a separate directory for storing call recording files.

The **-f** parameter is optional and allows you to specify a separate path for storing recording files. If this parameter is not specified, the recording files will be stored in the path defined by the **-p** parameter.

For example, if you mount a device or an external NAS device to **`/pbx/recordings`** and want to store the recording files there, you can create and run the PBX Docker instance using the following command:

If you have mounted a local disk or an external NAS device at `/pbx/recordings`, you can store recording files there by running:

```bash
sudo /bin/sh pbx_ctl.sh run \
-p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22 \
-f /pbx/recordings
```

***

#### Access the PBX Web Portal

After the installation completes successfully, access the PBX web portal at: https://66.175.221.120:8887

<figure><img src="../../../.gitbook/assets/login-1.png" alt="" width="321"><figcaption></figcaption></figure>

Click on **"Sign in as the administrator or dealer"** to navigate to the administrator login page, as shown in the screenshot below. Enter **admin** as both the username and password to log in to the web portal.

<figure><img src="../../../.gitbook/assets/login-2.png" alt="" width="320"><figcaption></figcaption></figure>

> ❗**Important**\
> For security reasons, change the default administrator password immediately after logging in.

***

### Step 4: Configure the PortSIP PBX

#### Initial Setup Wizard

For a new installation, the PBX automatically launches a **Setup Wizard** after the first successful login to the PBX web portal.\
This wizard guides you through the mandatory initial configuration steps.

***

#### 1. Network Environment

In the **Network Environment** step, configure the PBX IP addresses.

{% hint style="danger" %}
The loopback interface (127.0.0.1) is not suitable for a private IP address. Only the static IP for the LAN where the PBX is located is allowed (do not use a DHCP dynamic IP).&#x20;
{% endhint %}

**Private IPv4 Address**

* Enter the server’s **private IPv4 address**.
* If the server does **not** have a private IP address, enter the **public IP address** instead.

**Public IPv4 Address**

* If the PBX server has a **static public IP address**, enter it in the **Public IPv4** field.
* If the server does **not** have a static public IP address, leave this field **blank**.

***

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_1.png" alt=""><figcaption></figcaption></figure>

**Important Notes on IP Configuration**

* The IP addresses configured here are used by **SIP clients and IP phones** to register with the PBX.
* The configured IP address will act as the **SIP server address** and should be set as the **Outbound Proxy Server** on SIP endpoints.
* **Cloud Deployment:** Both Private IPv4 and Public IPv4 addresses must be entered.
* **LAN Deployment:** Only the Private IPv4 address is required.

***

### 2. SSL Certificate

To enable TLS transport for SIP and provide secure HTTPS access to the PBX Web Portal and REST API, a trusted SSL certificate must be configured during this step.

***

#### Domain Setup

You must have a **web domain** for the PBX.\
For example, you can purchase a domain from providers such as **GoDaddy** and point it to your PBX server’s IP address.

***

#### SSL Certificate Requirements

* A **trusted SSL certificate** is strongly recommended to avoid browser security warnings.
* Common certificate providers include:
  * DigiCert
  * GeoTrust
  * GoDaddy
  * Other trusted Certificate Authorities (CAs)

> **Note**\
> If you do not have a domain or SSL certificate, you may use the PBX server’s **IP address** as the Web Domain and proceed with the default certificate.\
> However, PortSIP PBX uses a **self-signed certificate by default**, which will cause browsers to block the connection and display security warnings.

***

#### Certificate Preparation

To purchase and prepare an SSL certificate, follow the guide: [Preparing TLS Certificates for TLS/HTTPS/WebRTC](../certificates-for-tls-https-webrtc/).

After completing the steps in that guide, you will have the following certificate files:

* `portsip.pem` — Certificate file
* `portsip.key` — Private key file

***

#### Configuring the Certificate

In this guide, we assume the PBX web domain is `uc.portsip.cc`.

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_2.png" alt=""><figcaption></figcaption></figure>

1. In the **Web Domain** field, enter: `uc.portsip.cc`
2. Open the `portsip.pem` file in a text editor (such as Windows Notepad), and copy **the entire contents** into the **Certificate File** field.
3. Open the `portsip.key` file and copy **the entire contents** into the **Private Key File** field.

***

### 3. Transport Protocol

#### Default Transport Ports

Click **Add** to create or review the transport protocols. By default, PortSIP PBX uses the following transport ports:

* **UDP:** `5060`
* **TCP:** `5063`
* **TLS:** `5061`

<figure><img src="../../../.gitbook/assets/pbx_setup_wizard_3.png" alt=""><figcaption></figcaption></figure>

***

#### Customizing Transport Ports

You may change the default transport ports to any values that suit your deployment requirements.

> ❗**Important**
>
> * Ensure the selected port is **not already in use** by another application.
> * Use **TLS** whenever possible to secure SIP signaling.

***

#### Firewall and Client Configuration

After adding or modifying a transport protocol:

* Update your **firewall rules** to allow traffic on the newly assigned port.
* Configure **IP phones and client applications** to use the same **transport protocol and port** when connecting to the PBX.

The transport protocol and port configured here are used by:

* SIP phones
* Desktop and mobile client apps
* Other SIP endpoints registering to the PBX

***

### Step 5: Install the Instant Messaging (IM) Service

Starting with **PortSIP PBX v22.0**, an integrated **Instant Messaging (IM) service** is available, providing modern collaboration features such as:

* One-to-one messaging
* Group chat

The IM service **requires a separate installation step**.

For high-concurrency or large-scale deployments, we recommend installing the IM service on a dedicated server to achieve better performance and scalability.

Please follow the guide below to install and configure the IM service: [Install IM Service](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/install-im-service.md)

***

### Step 6: Install the Data Flow Service

Starting with **PortSIP PBX v22.3.0**, PortSIP introduces the **Data Flow service**, which delivers:

* Advanced data analytics
* Real-time metrics
* Dashboards and wallboards

The Data Flow service is built on **ClickHouse**, a high-performance, column-oriented database optimized for analytics workloads.\
Due to its **resource-intensive nature**, this service **must be installed on a dedicated, high-performance server** and runs independently from the core PBX.

Please follow the guide below to install and configure the Data Flow service: [Install Data Flow Service](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/install-data-flow-service.md)

> **Note**\
> Installing the Data Flow service is **optional**.
>
> * Call processing is **not affected** if it is not installed.
> * However, **analytics, wallboards, and certain metrics features** will be unavailable.

***

### Step 7: Restart the PBX to Apply the SSL Certificate

If, in step [2. SSL Certificate](install-portsip-pbx.md#id-2.-ssl-certificate), you uploaded a **trusted SSL certificate** instead of using the default self-signed certificate, you must restart the PBX to apply the changes.

Run the following commands to restart the PBX:

```bash
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh restart
```

Now that the PortSIP PBX is successfully installed, you can use [https://uc.portsip.cc:8887](https://uc.portsip.cc:8887) to access the PortSIP PBX web portal.

***

### Managing PortSIP PBX Docker Instance

After successfully installing the **PortSIP PBX**, you can use the following commands to manage the PBX Docker instance.

All commands must be executed from the following directory:

```bash
cd /opt/portsip
```

#### Show PBX Status

```bash
sudo /bin/sh pbx_ctl.sh status
```

#### Start the PBX

```bash
sudo /bin/sh pbx_ctl.sh start
```

#### Stop the PBX

```bash
sudo /bin/sh pbx_ctl.sh stop
```

#### Restart the PBX

```bash
sudo /bin/sh pbx_ctl.sh restart
```

#### Remove the PBX Container

> This command **does not delete PBX data**.

```bash
sudo /bin/sh pbx_ctl.sh rm
```

***

### Managing PortSIP IM Service Docker Instance

To manage the **PortSIP Instant Messaging (IM) Service** Docker instance, first ensure you are in the `/opt/portsip` directory:

```bash
cd /opt/portsip
```

#### Show IM Service Status

```bash
sudo /bin/sh im_ctl.sh status
```

#### Start the IM Service

```bash
sudo /bin/sh im_ctl.sh start
```

#### Stop the IM Service

```bash
sudo /bin/sh im_ctl.sh stop
```

#### Restart the IM Service

```bash
sudo /bin/sh im_ctl.sh restart
```

#### Remove the IM Service Container

> This command **does not delete IM service data**.

```bash
sudo /bin/sh im_ctl.sh rm
```

***

### Managing PortSIP Data Flow Service Docker Instance

To manage the **PortSIP Data Flow Service** Docker instance, ensure you are in the `/opt/portsip` directory:

```bash
cd /opt/portsip
```

#### Show Data Flow Service Status

```bash
sudo /bin/sh dataflow_ctl.sh status
```

#### Start the Data Flow Service

```bash
sudo /bin/sh dataflow_ctl.sh start
```

#### Stop the Data Flow Service

```bash
sudo /bin/sh dataflow_ctl.sh stop
```

#### Restart the Data Flow Service

```bash
sudo /bin/sh dataflow_ctl.sh restart
```

#### Remove the Data Flow Service Container

> This command **does not delete Data Flow service data**.

```bash
sudo /bin/sh dataflow_ctl.sh rm
```

***



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



