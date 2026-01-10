# Trace Server – A Better Way to Monitor SIP Messages and QoS for PortSIP PBX

PortSIP has built the **PortSIP Trace Server** on top of the open-source **HOMER** project to provide a powerful, centralized solution for troubleshooting SIP signaling, SIP trunks, endpoints, and media quality issues.

The Trace Server enables you to:

* Access SIP and RTCP (QoS) captures through a web-based UI
* Centrally store SIP capture data from multiple hosts
* Filter SIP messages intuitively and correlate them to dialogs and transactions
* Automatically age out capture data after a configurable retention period
* Visualize calls and RTCP QoS metrics using clear, interactive charts

***

### Supported Operating Systems

* **Ubuntu**: 20.04, 22.04, 24.04
* **Debian**: 11.x, 12.x
* **Architecture**: 64-bit only

***

### Preparing the Linux Host

Because the Trace Server processes a high volume of SIP and RTCP data, it requires sufficient CPU and memory resources.

#### Recommended Hardware Specifications

| Concurrent Calls | CPU Cores | Memory |
| ---------------- | --------- | ------ |
| Up to 200        | 2–4       | 2–4 GB |
| Up to 1,000      | 4         | 4 GB   |
| Up to 5,000      | 8–10      | 8 GB   |
| Up to 10,000     | 16–20     | 16 GB  |

> **Important**\
> The PortSIP Trace Server **must not** be installed on the same server as the PortSIP PBX. Running both services on one host may lead to performance degradation or undefined behavior.

***

### Firewall Requirements

Ensure that the server firewall and any cloud security groups allow the following ports:

* **TCP 9061** – Receiving SIP messages from PortSIP PBX
* **UDP 9060** – RTCP/QoS data
* **TCP 9080** – Trace Server Web Portal

***

### Installing PortSIP Trace Server

#### Prerequisites

* System date and time must be correctly synchronized
* All commands must be executed as **root**

```bash
su root
```

***

#### Download Installation Scripts

```bash
mkdir -p /opt/portsip-trace && cd /opt/portsip-trace
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/install_docker.sh -o install_docker.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/trace_ctl.sh -o trace_ctl.sh
```

***

#### Install the Docker Environment

```bash
cd /opt/portsip-trace && /bin/sh install_docker.sh
```

***

#### Run the Trace Server (Default Settings)

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh run
```

***

#### Optional Runtime Parameters

You can customize the Trace Server using the following options:

* `-p` – Data storage path (default: `/var/lib/portsip`)
* `-k` – Retention period in days (default: `5`)
* `-l` – Web portal listening port (default: `9080`)
* `-z` – SIP message receiving port (default: `9061`)

> **Note**\
> Ensure the specified data path contains **no spaces**.

***

#### Example: Custom Configuration

```bash
cd /opt/portsip-trace && \
/bin/sh trace_ctl.sh run \
-p /opt/portsip/trace \
-k 30 \
-l 12345 \
-z 23456
```

* Data path: `/opt/portsip/trace`
* Retention: 30 days
* Web portal port: 12345
* SIP receiving port: 23456

***

### Accessing the Trace Server Web Portal

*   Default:

    ```
    http://trace-server-ip:9080
    ```
*   Custom port:

    ```
    http://trace-server-ip:12345
    ```

> If deployed on a cloud platform, ensure the web portal port is allowed in the cloud firewall or security group.

#### Default Credentials

* **Username**: `admin`
* **Password**: `admin`

> **Security Recommendation**\
> Change the default password immediately after first login.

***

### Enabling SIP Trace in PortSIP PBX

1. Sign in to the **PortSIP PBX Web Portal**
2. Go to: **Advanced > Settings**
3. On the **General** page, enter the Trace Server details:
   * Trace Server IP
   * Trace Server Port (default: 9061 or the value specified with `-z`)
4. Click **OK**

After configuration, the PBX will forward **all SIP messages** to the Trace Server.

> **Important**\
> Always enter the **actual Trace Server IP and port** used during installation.

<figure><img src="../../.gitbook/assets/portsip_trance_server.png" alt=""><figcaption></figcaption></figure>

***

### Disabling SIP Trace in PortSIP PBX

1. Go to: **Advanced > Settings**
2. Clear the Trace Server IP and Port fields
3. Click **OK**

> **Best Practice**\
> SIP tracing should remain **disabled during normal operation** and enabled **only for troubleshooting**, as continuous tracing consumes CPU, memory, and disk resources and may affect PBX performance.

***

### Troubleshooting SIP Messages

In the Trace Server Web Portal:

* Select **Home** to view call-related SIP messages
* Select **REGISTRATION** to view REGISTER traffic

![](<../../.gitbook/assets/image (2).png>)

Click a SIP **method name** to view detailed message contents.

![](<../../.gitbook/assets/image (19).png>)

#### Advanced Searching

You can search SIP messages using:

* `Call-ID`
* `X-Session-Id`
* `X-CID`

![](<../../.gitbook/assets/image (21).png>)

These identifiers allow precise tracking of calls across multiple legs and systems.

Clicking a **Session ID** displays the complete SIP call flow for that session.

![](<../../.gitbook/assets/image (10).png>)

![](<../../.gitbook/assets/image (7).png>)

![](<../../.gitbook/assets/image (6).png>)

![](<../../.gitbook/assets/image (28).png>)

***

### Troubleshooting RTP and QoS

The Trace Server supports monitoring call quality using **RTCP** data.

1. Open the **QoS** tab
2. Analyze metrics such as:
   * Packets
   * Octets
   * Jitter
   * Packet loss
   * Other RTP quality indicators

These charts help identify media issues such as network congestion, packet loss, or jitter spikes.

<figure><img src="../../.gitbook/assets/portsip_trace_qos.png" alt=""><figcaption></figcaption></figure>

***

### Managing the Trace Server

Use the following commands to manage the service:

#### Start

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh start
```

#### Check Status

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh status
```

#### Restart

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh restart
```

#### Stop

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh stop
```

#### Remove

```bash
cd /opt/portsip-trace && /bin/sh trace_ctl.sh rm
```

***

### Upgrading the Trace Server

To upgrade to the latest version:

1.  Stop the service:

    ```bash
    cd /opt/portsip-trace && /bin/sh trace_ctl.sh stop
    ```
2.  Remove the service:

    ```bash
    cd /opt/portsip-trace && /bin/sh trace_ctl.sh rm
    ```
3. Reinstall by following the [Installing PortSIP Trace Server](debug-sip-message.md#installing-portsip-trace-server) section above.





