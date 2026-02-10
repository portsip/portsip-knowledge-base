# Using a Free Let’s Encrypt SSL Certificate

Let’s Encrypt is a free, automated, and open Certificate Authority (CA) that enables HTTPS servers to obtain browser-trusted TLS certificates automatically, without manual intervention.

With Let’s Encrypt, the certificate needs to be installed only once. After installation, the certificate is automatically renewed before it expires, ensuring continuous trust without service interruption.

When a certificate used by PortSIP PBX or PortSIP SBC is nearing expiration, the renewal process is handled automatically by the Let’s Encrypt service. No user action is required, and existing services continue to operate securely during the renewal process.

***

### Prerequisites

Before using a Let’s Encrypt SSL certificate, ensure the following requirements are met:

* A static public IP address assigned to your PortSIP PBX server
* A domain name that resolves to the PBX server’s static public IP address
* A static public IP address assigned to your PortSIP SBC server
* A domain name that resolves to the SBC server’s static public IP address

Correct DNS resolution is required so that Let’s Encrypt can validate domain ownership and issue trusted certificates.

> **Note**: If PortSIP PBX and PortSIP SBC are installed on the same server, you may use the same static public IP address and domain name for both services.

