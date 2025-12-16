# Preparing TLS Certificates

To ensure secure and trusted communication between clients, the PBX, and SBC, you need to prepare and install valid TLS certificates.&#x20;

Follow the steps below to obtain and configure them properly.

***

### 1. Purchase a Domain

Purchase a domain (for example, **portsip.cc**) from a trusted domain registrar such as **DigiCert**, **Thawte**, or **GeoTrust**.\
This domain will be used to identify your PBX and SBC servers.

***

### 2. Configure DNS Records

Create an **A record** in your domain’s DNS zone and map the domain name to your PBX server’s public IP address.\
For example:

```
uc.portsip.cc → [PBX Server Public IP]
```

If your **SBC** (Session Border Controller) is deployed on a separate server, create another A record for it:

```
sbc.portsip.cc → [SBC Server Public IP]
```

***

### 3. Purchase a TLS Certificate

Purchase a **TLS/SSL certificate** for your domain from a trusted certificate authority such as DigiCert, Thawte, or GeoTrust.\
If your SBC is deployed on a separate server, it is recommended to purchase a **Wildcard Certificate** (for example, `*.portsip.cc`) so that both the PBX and SBC can share the same certificate.

***

### 4. Generate the CSR and Private Key

Generate a **Certificate Signing Request (CSR)** and **private key** according to your certificate provider’s instructions.

> 💡 When prompted for the certificate type, select **Nginx**.

⚠️ **Important:** Do **not** set a password when generating the private key — a password-protected key will prevent the PBX service from starting automatically.

After this step, you should have two files stored locally:

* **CSR file** – Used to request the certificate from your provider.
* **Private key file** – Rename it to **`portsip.key`**, store it securely, and never share it with anyone.

***

### 5. Obtain the Certificate Files

Submit the CSR file to your certificate provider. Once processed, you will receive the signed certificate files corresponding to your CSR. Typically, these include:

* **TLS certificate file** – `cert.pem`
* **Intermediate CA certificate file** – `intermediate.pem`

If your certificate provider did **not** include the Intermediate CA certificate, request it from them.\
Without the Intermediate CA certificate, your SSL certificate chain will be incomplete (_not fully chained_).\
An incomplete certificate chain can cause browsers and third-party services (such as SMS or WhatsApp providers) to mistrust the certificate, potentially preventing the PBX from receiving inbound SMS or WhatsApp messages.

***

### 6. Create a Full-Chain Certificate

#### **Windows Environment**

1.  Open both the **TLS certificate file** and the **Intermediate CA certificate file** using a plain text editor such as **Windows Notepad**.

    > ⚠️ Do not use Microsoft Word or any rich-text editor.
2. Copy the entire contents of the **Intermediate CA certificate** and append it to the end of the **TLS certificate file**.
3. Save the combined file and rename it as **`portsip.pem`**.

#### **Linux Environment**

Use the following commands to combine the certificate files:

```bash
# Append the intermediate certificate to the TLS certificate
cat intermediate.pem >> cert.pem

# Rename the combined file
mv cert.pem portsip.pem
```

After completing this step, you should have two final certificate files:

* **Certificate file:** `portsip.pem`
* **Private key file:** `portsip.key`&#x20;

You can verify whether your SSL certificate is trusted and includes the complete certificate chain using one of these tools:

* [SSL Checker](https://www.sslchecker.com/sslchecker)
* [SSL Shopper](https://www.sslshopper.com/ssl-checker.html)
* [Key CDN SSL Checker](https://tools.keycdn.com/ssl)

If your certificate is not a full-chain SSL certificate, contact your SSL certificate provider to have it corrected.

***

### 7. Update the Certificates

Once the certificate files are ready, follow the guide [**Update Certificates**](update-certificates.md) to apply the new TLS certificates to your PortSIP PBX system.



