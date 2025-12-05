# Preparing TLS Certificates

Please take the steps listed below to get trusted certificates.

1. Purchase a Domain (for example, `portsip.cc`) from the domain provider (for example, [Digicert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/)) for your PBX and SBC.&#x20;
2. Add an A record in the Domain DNS zone, and resolve the Domain to your PBX IP, for example: point the `uc.portsip.cc` to PBX server IP.
3. If your SBC is deployed separately from the PBX server, add an A record DNS record and resolve to the SBC server IP, for example, point `sbc.portsip.cc` point to the SBC server IP.
4. Purchase a TLS certificate from the trust certificate provider for your domain, for example, [Digicert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/); If your SBC is deployed separately from the PBX server, you must purchase the **`Wildcard Certificate`**.
5.  Generate a **Certificate Signing Request (CSR)** and a **private key** following your certificate provider’s instructions(Please choose the certificate type for Nginx).

    > ⚠️ When generating the private key, **do not set a password** — a password-protected key will prevent the PBX service from starting automatically.

    \
    After completing this step, you should have the following two files stored locally:

* **CSR file** – Used to request the certificate from your provider.
* **Private key file** – Rename it to `portsip.key`. Must be securely stored and never shared with anyone.

6. Submit the CRS file to the certificate provider. Once your certificate provider issues the SSL certificate, you will receive **certificate files** corresponding to your CSR. This step will end up with two files: `Intermediate CA certificate` and `TLS certificate` . Assume the file names are:
   * The  **TLS certificate** file: `cert.pem`
   * The  **Intermediate CA certificate** file: `intermediate.pem`&#x20;
7. If your certificate provider did not include the **Intermediate CA certificate**, please request it from them. Without the **Intermediate CA certificate**, your SSL certificate chain will be incomplete (not fully chained).   \
   An incomplete certificate chain can cause browsers and third-party services (such as SMS or WhatsApp providers) to reject or mistrust the certificate. This may result in failures when receiving **inbound SMS or WhatsApp messages** through the PBX.
8. To create a full-chain certificate:
   1.  In a Windows environment, open both the **TLS certificate file** and the **Intermediate CA certificate file** using a **plain text editor** — for example, Windows Notepad.

       ⚠️ Do not use Microsoft Word or any rich-text editor.

       Copy the entire contents of the **Intermediate CA certificate** and **append** it to the end of the **TLS certificate file.**

       Save the combined file and rename it as **portsip.pem**.
   2. In Linux environments, you can use the following command to combine the certificate files:

```
// Append intermediate file to TLS certificate file
cat intermediate.pem >> cert.pem

// Rename certificate file to portsip.pem
mv cert.pem portsip.pem
```

10. Now you will have two certificate files:

* Certificate File: **portsip.pem**
* Private Key file: **portsip.key**

After successfully creating the certificate files, please follow the article [Update Certificates](update-certificates.md) to update the certificates.&#x20;

