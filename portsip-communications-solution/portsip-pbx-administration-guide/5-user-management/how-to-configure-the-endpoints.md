# How to Configure the Endpoints?

After successfully [configuring the PortSIP PBX](../2-portsip-pbx-management/) and [SBC](../9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md), and creating the required [tenants ](../3-tenant-management/)and [extensions](users.md), you can register endpoints with the PBX.\
Once registered, endpoints can make and receive calls.

Supported endpoints include:

* WebRTC clients
* Mobile apps
* Windows Desktop apps
* IP phones
* Any standard SIP-based device

***

### Configuring the PortSIP ONE App

If you are using **PortSIP PBX v22.0 or later**, follow the appropriate guide below to configure the PortSIP ONE application:

* [PortSIP ONE for Desktop and WebRTC](../../../apps-guides/portsip-one-app/portsip-one-desktop-app.md)
* [PortSIP ONE for Mobile](../../../apps-guides/portsip-one-mobile-app/)

***

### Configuring IP Phones

The following sections describe how to configure IP phones based on a sample deployment scenario.

***

#### Assumed Environment and Configuration

In this article, the following environment and configuration are assumed:

* **PBX and SBC deployment**
  * Public IP address: `66.175.221.120`
  * Private IP address: `192.168.1.72`
* **PBX Web Domain**
  * Domain name: `uc.portsip.cc`
  * Resolved to public IP: `66.175.221.120`
* **SBC Transport Configuration**
  * UDP: Port `5060`
  * TLS: Port `5061`
  * TCP: Port `5063`
  * WSS: Port `5065`
* **Tenant Configuration**
  * Tenant SIP domain: `test.io`

<figure><img src="../../../.gitbook/assets/portsip-pbx-home-1.png" alt=""><figcaption></figcaption></figure>

***

### Essential Information

The following rules apply when configuring **client endpoints** for the deployment scenario described above.

***

#### Transport

**Transport** defines the network protocol used to send and receive SIP messages between the endpoint and the PBX (for example, UDP, TCP, TLS, or WSS).

For more information, refer to [Transport Management](../6-transport-management.md).

***

#### Outbound Proxy Server

In the client endpoint settings, the **Outbound Proxy Server** must be set to the **PBX server address**.

* When registering an endpoint **over the Internet**:
  * Use the PBX **public IP address**, or
  * Use the PBX domain name (for example, `uc.portsip.cc`), provided it resolves to the public IP address.
* When registering an endpoint **from the local LAN**:
  * Use the PBX **private IP address**.

Selecting the correct outbound proxy is critical for proper SIP routing, especially in NAT or multi-interface environments.

***

#### Outbound Proxy Server Port

The **Outbound Proxy Server Port** must match the port of the transport configured on the **PBX or SBC**.

For example:

* If the endpoint registers using **TCP**, set the Outbound Proxy Server Port to **5063**.
* If the endpoint registers using **TLS**, set the port to **5061**.
* If the endpoint registers using **WSS**, set the port to **5065**.

***

#### Domain (SIP Domain / SIP Server)

The **Domain** (also referred to as **SIP Domain** or **SIP Server** in endpoint settings) must be set to the **tenant SIP domain** created in PortSIP PBX.

For example:

* Tenant SIP domain: `test.io`
* Domain / SIP Server value in the endpoint: `test.io`

> **Important**
>
> * Configure the **transport port only** in the **Outbound Proxy Server Port** field.
> * Do **not** configure a port for the Domain / SIP Server.
> * If the endpoint requires a port value for the Domain, enter **`0`**.

***

### Auto-Provisioning IP Phones

Many popular IP phone models can be **auto-provisioned** to register with the PortSIP PBX, eliminating the need for manual phone configuration.

Supported vendors include:

* Fanvil
* Yealink
* SNOM
* Grandstream
* DinStar
* ALE
* Htek

For detailed instructions, refer to [Phone Device Management](../4-phone-device-management/).

***

### Manually Registering an IP Phone to the PBX

In addition to auto-provisioning, you can manually register an **IP phone** or other **SIP-based device or application** by entering the SIP extension credentials directly in the device’s web management interface.

This method is commonly used when:

* Auto-provisioning is not available or not preferred
* You are using a third-party SIP device
* You need to perform testing or troubleshooting

***

#### Example: Fanvil IP Phone

<figure><img src="../../../.gitbook/assets/fanvil_phone_sip1.png" alt="" width="563"><figcaption></figcaption></figure>

***

#### Example: Yealink IP Phone

<figure><img src="../../../.gitbook/assets/yealink_phone_sip1.png" alt="" width="563"><figcaption></figcaption></figure>

***

#### Configuration Reminder

When manually configuring an IP phone or SIP device, ensure that:

* The **Outbound Proxy Server**, **Port**, **Transport**, and **Domain** values match the PBX configuration
* The **extension number** and **password** are entered exactly as configured in PortSIP PBX
* The selected transport (UDP/TCP/TLS) is supported by both the device and the PBX

For detailed parameter definitions and best practices, refer to [Essential Information](how-to-configure-the-endpoints.md#essential-information) earlier in this guide.





