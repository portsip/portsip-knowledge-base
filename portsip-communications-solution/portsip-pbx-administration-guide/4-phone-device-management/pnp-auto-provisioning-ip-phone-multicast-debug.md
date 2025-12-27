# PnP Auto Provisioning IP Phone Multicast Debug

To enable **PortSIP PBX** to answer PnP provisioning requests, **all** of the following requirements must be met:

* The **PortSIP PBX must be able to join the multicast group**
* The **IP phone and PBX must be on the same local LAN subnet**
* The **network switch/router must support multicast**
* The **IP phone must support PnP provisioning**

> ❗ **Important**\
> PnP provisioning relies on **SIP multicast discovery**. If any requirement above is not satisfied, automatic discovery will fail and the phone must be provisioned manually.

***

### PortSIP PBX Must Be Able to Join the Multicast Group

Depending on the **network interface order**, PortSIP PBX may not be able to listen for multicast events.

> ❗ **Important**\
> **Unused LAN adapters, Wi-Fi, and Bluetooth interfaces must be disabled.**\
> These interfaces cannot be used to join multicast events and may prevent the PBX from listening on the correct network interface.

***

#### Verify Multicast Membership on Windows

1. Open the **Command Prompt** as Administrator.
2.  Run the following command:

    ```cmd
    netsh interface ipv4 show join
    ```
3. Verify that the multicast address **224.0.1.75** appears on the correct network interface.

If your interface does **not** list this address, the PBX is **not ready** for PnP provisioning.

<figure><img src="../../../.gitbook/assets/phone_pnp_multicast.png" alt=""><figcaption></figcaption></figure>

***

#### Fix Multicast Issues on Windows (Interface Metric Adjustment)

1. Open **Control Panel > Network and Internet > Network Connections**.
2. Right-click the active network adapter and select **Properties**.
3. Select **Internet Protocol Version 4 (TCP/IPv4)** → **Properties** → **Advanced**.
4. Edit the **Default Gateway Metric** and try the following values **one at a time**:
   * `1`
   * `10`
   * `12`
   * `15`
   * `20`
5. After each change:
   * Reboot the server
   * Re-run `netsh interface ipv4 show join`
   * Confirm the multicast join appears

<figure><img src="../../../.gitbook/assets/phone_pnp_multicast1.png" alt="" width="308"><figcaption></figcaption></figure>

***

#### Verify Multicast Membership on Linux

1. Connect to the PBX server via **SSH**.
2. Switch to the root user.
3.  Run the following command:

    ```bash
    netstat -g
    ```
4. Verify that **`sip.mcast.net`** is listed on the active application interfaces.

If it is not listed, the PBX is not receiving multicast traffic and cannot support PnP discovery.

<figure><img src="../../../.gitbook/assets/phone_pnp_multicast2.png" alt=""><figcaption></figcaption></figure>

***

### IP Phone and PBX Must Be in the Same Local LAN Subnet

PnP provisioning **requires multicast message exchange**, which does **not** cross subnets.

#### Supported Example (Will Work)

* PBX: `192.168.1.1` / `255.255.255.0`
* Phone: `192.168.1.28` / `255.255.255.0`

#### Unsupported Example (Will Not Work)

* PBX: `192.168.0.1` / `255.255.255.0`
* Phone: `192.168.1.28` / `255.255.255.0` _(different subnet)_

> ❗ **Important**\
> VLANs, routed networks, or NAT boundaries will prevent multicast discovery.\
> Phones in these environments must be provisioned using **manual provisioning links**.

***

### Network Infrastructure Must Support Multicast

* Switches and routers must allow **multicast traffic**
* IGMP snooping or multicast filtering must be configured correctly
* Multicast must not be blocked by firewall rules

> ❗ **Best Practice**\
> In enterprise environments, verify multicast behavior with your network team before deploying PnP at scale.

***

### IP Phone Must Support PnP Provisioning

The IP phone firmware must support **PnP multicast announcements**.

> ❗ **Important**\
> Some older phone firmware versions do not fully implement PnP discovery.\
> If discovery fails and all other requirements are met, **upgrade the phone to the latest supported firmware** and retry.

***

### Summary

For successful PnP provisioning:

* The PBX must correctly **join the multicast group**
* Phones and PBX must share the **same LAN subnet**
* The network must **permit multicast traffic**
* IP phones must support **PnP and run compatible firmware**

When these conditions are met, IP phones will be discovered automatically and can be provisioned quickly through the **PortSIP PBX Web Portal**.





