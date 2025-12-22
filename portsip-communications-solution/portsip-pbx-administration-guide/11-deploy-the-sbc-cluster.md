# 11 Deploy the SBC Cluster

If your deployment handles a high volume of **WebRTC** or **Microsoft Teams** calls, or if you use a Session Border Controller (SBC) to isolate the PBX from external networks, all signaling and media traffic will be routed through the SBC layer.

In these scenarios, it is strongly recommended to deploy the SBC in a **clustered configuration**. This approach improves scalability, enhances reliability, and ensures efficient load distribution across SBC nodes.

***

### Deployment Architecture

In a typical architecture, the **PortSIP PBX** is deployed within a protected environment, such as a private VLAN or cloud network. One or more SBC servers are positioned in front of the PBX to:

* Prevent direct user access to the PBX
* Secure SIP, WebRTC, and Teams traffic
* Handle NAT traversal, TLS termination, and media anchoring
* Provide horizontal scalability and high availability

<figure><img src="../../.gitbook/assets/sbc_cluster.png" alt=""><figcaption></figcaption></figure>

All external client traffic (WebRTC browsers, Microsoft Teams, SIP endpoints) is terminated on the SBC cluster before being forwarded to the PBX.

***

### DNS Configuration

You must create DNS records for each SBC node. The following DNS record types are supported:

* **A records**
* **DNS SRV records**

#### Example SBC IP Addresses

* SBC 1: `72.247.113.11`
* SBC 2: `72.247.113.12`
* SBC 3: `72.247.113.13`

#### Individual A Records

Create individual DNS records for each SBC:

* `sbc1.sbc.com` → `72.247.113.11`
* `sbc2.sbc.com` → `72.247.113.12`
* `sbc3.sbc.com` → `72.247.113.13`

#### Shared DNS Record (Load Distribution)

You may also configure a shared DNS name that resolves to all SBC IP addresses:

* `sbc.com` → `72.247.113.11`
* `sbc.com` → `72.247.113.12`
* `sbc.com` → `72.247.113.13`

This allows clients to distribute connections across multiple SBC servers using DNS-based load balancing.

***

### TLS Certificates

Purchase and install a **wildcard TLS certificate** for the domain:

```
*.sbc.com
```

Refer to the [Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/) guide for detailed instructions on certificate requirements and installation.

Using a wildcard certificate ensures:

* Seamless HTTPS and WebRTC access
* Consistent TLS configuration across all SBC nodes
* Simplified certificate management in clustered environments

***

### Configuring the SBC

Follow the instructions in [Configuring PortSIP SBC for WebRTC](9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md) to configure each SBC server. In addition, ensure the following settings are applied consistently across all nodes:

#### Certificate Settings

* Set **TLS Domain** to: `sbc.com`
* Enable **This is SBC Web Domain Certificate**

#### Web Domain Configuration

* Set **Web Domain** to: `sbc.com`

***

### Accessing the SBC

After completing the SBC configuration, you can access each SBC’s web management portal using its dedicated hostname:

* **SBC 1:** `https://sbc1.sbc.com:8883`
* **SBC 2:** `https://sbc2.sbc.com:8883`
* **SBC 3:** `https://sbc3.sbc.com:8883`

#### Accessing the WebRTC Client

End users can access the WebRTC client using the shared SBC domain: https://sbc.com:10443/webrtc

This URL automatically leverages DNS-based distribution to route users to available SBC nodes.

***

### Best Practice Summary

* Always deploy SBCs in a clustered configuration for high-volume WebRTC or Microsoft Teams traffic
* Use DNS-based load distribution with a shared SBC domain
* Protect the PBX by placing SBCs in front of it
* Use a wildcard TLS certificate for consistent WebRTC and HTTPS access
* Keep all SBC nodes configured identically to ensure predictable behavior



