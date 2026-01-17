# PBX and SIP Trunk using PortSIP SBC

This topic describes how to configure **PortSIP SBC** to enable interoperability between a **generic SIP trunk** and **PortSIP PBX**.\
The SBC acts as a secure SIP signaling and media boundary, interconnecting the enterprise PBX deployed in a private network with a SIP trunk service hosted on a public network.

***

### 1. Interoperability Topology

The interoperability testing between PortSIP SBC, a generic SIP trunk, and PortSIP PBX was performed using the following deployment model.

#### Deployment Scenario

* The enterprise deploys **PortSIP PBX** within its private LAN to provide internal voice communications.
* The enterprise requires PSTN connectivity and enterprise calling services through a **SIP Trunk** provided over the public Internet.
* **PortSIP SBC** is deployed at the network edge to interconnect the enterprise LAN with the SIP trunk provider and to protect the PBX from direct exposure to the public network.

#### Key Concepts

* **Session**\
  A real-time voice communication session established using the Session Initiation Protocol (SIP).
* **Border**\
  The IP-to-IP network boundary between the enterprise LAN (where PortSIP PBX resides) and the public network hosting the SIP trunk.

<figure><img src="../../.gitbook/assets/enterprise_pbx_sbc_trunk.png" alt=""><figcaption></figcaption></figure>

#### Call Flow Overview

The high-level call flow for this topology is:

```
Extension <-> PortSIP PBX <-> PortSIP SBC <-> SIP Trunk
```

This architecture ensures secure signaling, controlled media traversal, and protocol normalization between the PBX and the SIP trunk provider.

***

### 2. Configuring PortSIP PBX

Before configuring the SBC and SIP trunk, ensure that the PortSIP PBX is installed and operational.

#### Prerequisites

Complete the following PBX setup steps:

1. [Install PortSIP PBX](installation-of-portsip-pbx-v22.3-beta-version/install-portsip-pbx.md)
2. Perform basic [PBX configuration, including system and tenant setup](2-portsip-pbx-management/)

Refer to the corresponding PBX installation and configuration guides for detailed instructions.

***

### 3. Configuring PortSIP SBC

Install and [configure the PortSIP SBC](9-configuring-portsip-sbc/) according to your deployment requirements.

> ❗**Note**\
> If WebRTC endpoints are used, ensure the SBC is configured for WebRTC support as described in the [_Configuring SBC for WebRTC_ guide](9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md).

***

### 4. Adding the SIP Trunk to PortSIP PBX

This section describes how to add the SIP trunk to PortSIP PBX when the SBC is used as an outbound proxy.

#### Assumed SIP Trunk Parameters

* SIP trunk is reachable over the public Internet
* SIP trunk IP address: `52.214.181.141`
* SIP signaling port: `5060`
* Transport protocol: `UDP`
* Authentication mode:
  * IP-based authentication **or**
  * Register-based authentication

***

#### Step-by-Step Configuration

1. **Sign in to the PortSIP PBX Web Portal**
   * Use **System Administrator** credentials.
   * Navigate to **Call Manager > Trunks**.
2. **Add a New Trunk**
   * Click the arrow button and select:
     * **IP-based Trunk** or
     * **Register-based Trunk**, depending on the SIP trunk type.
3. **Configure Basic Trunk Settings**
   * Enter a **friendly name** for the trunk.
   *   Set **Host Domain or IP** to the SIP trunk IP address:

       ```
       52.214.181.141
       ```
   * Set **Port** to `5060`, or to the actual port provided by the SIP trunk provider if different.
4. **Configure the Outbound Proxy (SBC)**
   *   Set **Outbound Proxy Server** to the **private IP address of the SBC**, for example:

       ```
       192.168.1.73
       ```
   *   Set **Outbound Proxy Server Port** to:

       ```
       5069
       ```

       > By default, PortSIP SBC listens on TCP port 5069 for SIP traffic from the PBX.
5.  **Select Transport Protocol**

    * Select **TCP** as the transport.
    * Click **Next**.

    > **Note**\
    > PortSIP PBX communicates with the SBC using TCP by default. Ensure the selected transport matches the SBC configuration.
6. **Configure Authentication (Register-Based Trunk Only)**
   * Enter the **Authorization Name** and **Password** provided by the SIP trunk service provider.
   * Click **Next**.
7. **Advanced Options**
   * Enable **Trunk is located in same LAN with PBX**.
   * Disable **Rewrite the host IP of Via header by public IP when sending the request to trunk**.
   * Click **Next**.
8. **Assign Trunk to Tenants**
   * Since the trunk is created by the **System Administrator**, select one or more tenants that are allowed to use this trunk.
   * For details, refer to the _Add the Trunk by System Admin_ section in [Trunk Management](7-trunk-management/).<br>

Now you are ready to [create the inbound & outbound rules for the call routing](8-call-route-management/).





