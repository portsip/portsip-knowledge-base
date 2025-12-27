# 6 Transport Management

After completing the **Setup Wizard**, you can manage your **PortSIP PBX** through the **Web Portal**.

PortSIP PBX supports multiple SIP signaling transports, including **UDP**, **TCP**, and **TLS**. You must configure which transports and ports the PBX listens on to accept SIP registrations and calls.

> ❗ **Important**\
> Only **System Administrators** are allowed to create or delete SIP transports.\
> When deleting transports, **at least one transport must always remain configured**.

The Setup Wizard configures a **default transport** automatically. To modify or add transports, sign in to the PBX Web Portal as a **System Administrator** and navigate to: **Call Manager > Transports**

Click **Add** to create a new transport.

***

### Firewall Considerations

When you add a new SIP transport, you **must** update firewall rules to allow traffic on the selected port.

* IP phones and client applications connect to the PBX using the configured **transport protocol and port**
* If the PBX is deployed on a cloud platform such as **Amazon Web Services (AWS)**, you must:
  * Open the port on the **OS-level firewall**
  * Open the same port in the **cloud platform firewall** (for example, AWS Security Groups)

Failure to do so will result in **registration failures and call setup issues**.

***

### Adding UDP, TCP, or TLS Transports

To add a new SIP transport:

1. Navigate to **Call Manager > Transports**.
2. Click **Add**.
3. In the **Transport protocol** field, select **UDP**, **TCP**, or **TLS**.
4. Specify a listening port:
   * Default ports:
     * UDP: `5060`
     * TLS: `5061`
     * TCP: `5063`
   * You may choose a different port if required, provided it is **not already in use**.
5. Click **OK** to add the transport.

***

### Adding a TLS Transport (Secure SIP)

Before adding a TLS transport, you must prepare **TLS certificate files**.

Refer to [Preparing TLS Certificates](certificates-for-tls-https-webrtc/preparing-tls-certificates.md) to obtain a certificate from a trusted third-party provider for your PBX Web Domain (for example: `uc.portsip.cc`).

To add the TLS transport:

1. Navigate to **Call Manager > Transports**.
2. Click **Add**.
3. Select **TLS** as the transport protocol.
4. Specify the listening port (default: `5061`).
5. Click **OK** to save the configuration.

> ❗ **Best Practice**\
> TLS is strongly recommended for deployments exposed to the public internet to protect SIP credentials and signaling traffic.

***

### Configuring Firewall Rules (Linux / firewalld)

If you create **custom transport ports** instead of using the default ones, you must explicitly open those ports on the firewall.

#### Example Scenario

You created the following transports:

* UDP on port `5066`
* TCP on port `5071`
* TLS on port `5072`
* WSS on port `5075`

Run the following commands on the PBX server:

```bash
firewall-cmd --permanent --service=portsip-pbx --add-port=5066/udp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5071/tcp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5072/tcp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5075/tcp --set-description="PortSIP PBX"
firewall-cmd --reload
```

> ❗ **Important**\
> Opening the OS firewall alone is **not sufficient** for cloud deployments.\
> You must also open the same ports in the **cloud platform firewall**, such as **AWS Security Groups**.

***

### Summary

* SIP transports define **how** and **where** the PBX listens for signaling traffic
* Only **System Administrators** can manage transports
* Always ensure:
  * At least **one transport** remains configured
  * Firewall rules are updated at both the **OS** and **cloud** levels
* Use **TLS** wherever possible for secure, internet-facing deployments



