# 11 Deploy the SBC Cluster

If your business handles a high volume of WebRTC and MS Teams calls, or if you use an SBC to isolate your PBX, all calls will be routed through the SBC. In such cases, it is recommended to deploy the SBC in a cluster configuration to efficiently manage the call traffic.

## Deployment Architecture

<figure><img src="../../.gitbook/assets/sbc_cluster.png" alt=""><figcaption></figcaption></figure>

As illustrated in the diagram, the PBX is deployed on either a VLAN or a cloud platform. Multiple SBC servers are positioned in front of the PBX to prevent direct access by users.

## DNS Configuration

You will need to create DNS records for each SBC server. The DNS records can be of the following types:

* **A Record**
* **DNS SRV Record**

For example, assume the SBC servers have the following IP addresses:

* SBC 1: 72.247.113.11
* SBC 2: 72.247.113.12
* SBC 3: 72.247.113.13

You can create the following **A Records** for the SBC servers:

* Resolve **sbc1.sbc.com** to **72.247.113.11**
* Resolve **sbc2.sbc.com** to **72.247.113.12**
* Resolve **sbc3.sbc.com** to **72.247.113.13**

Additionally, you may create records to resolve **sbc.com** to all three SBC IPs:

* Resolve **sbc.com** to **72.247.113.11**
* Resolve **sbc.com** to **72.247.113.12**
* Resolve **sbc.com** to **72.247.113.13**

## Certificates

Please purchase a wildcard TLS certificate for the domain **sbc.com** as per the article[ Certificates for TLS/HTTPS/WebRTC](../tutorials/certificates-for-tls-https-webrtc/) guide.

## Configuring the SBC

Please follow the instructions in the [**Configuring PortSIP SBC for WebRTC**](9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md) topic to set up each SBC server. However, keep the following points in mind:

* When adding certificates to the SBC, set the **TLS domain** to **sbc.com** and enable the **This is SBC Web Domain Certificate** option.
* When configuring the **Web Domain** for the SBC, enter **sbc.com** for the Web Domain field.

## Access the SBC

After completing the setup of the SBC servers, you can access the SBC Web Portal using the following URLs:

* To manage SBC 1, use [https://sbc1.sbc.com:8883](https://sbc1.sbc.com:8883)
* To manage SBC 2, use [https://sbc2.sbc.com:8883](https://sbc1.sbc.com:8883)
* To manage SBC 3, use [https://sbc3.sbc.com:8883](https://sbc1.sbc.com:8883)

You can access the WebRTC client using this URL: [https://sbc.com:10443/webrtc](https://sbc.com:10443/webrtc).

