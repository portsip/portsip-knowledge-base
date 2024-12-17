# Preparing TLS Certificates

Please take the steps listed below to get trusted certificates.

1. Purchase a Domain (for example, `portsip.cc`) from the domain provider (for example, [Godaddy](https://www.godaddy.com/).) for your PBX and SBC.&#x20;
2. Add an A record in the Domain DNS zone, and resolve the Domain to your PBX IP, for example: point the `uc.portsip.cc` to PBX server IP.
3. If your SBC is deployed separately from the PBX server, add an A record DNS record and resolve to the SBC server IP, for example, point `sbc.portsip.cc` point to the SBC server IP.
4. Purchase a TLS certificate from the trust certificate provider for your domain, for example, [Digicert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/); If your SBC is deployed separately from the PBX server, you must purchase the **`Wildcard Certificate`**.
5. Generate the CSR file and private key file according to the certificate provider’s guide, and keep the files. Please **don't set the password when generating the private key file**; usually, you will have two files: the `certificate` and the `private key`.&#x20;

{% hint style="warning" %}
**Note**: Please choose the certificate type for Nginx.
{% endhint %}

6. Rename the private key file as `portsip.key`.&#x20;
7.  Submit the CRS file to the certificate provider, and download the certificate files after your certificates are approved. This step will end up with two files: `Intermediate CA certificate` and `TLS certificate` . Assume the file names are:

    * The  `TLS certificate` file: `cert.pem`
    * The  `Intermediate CA certificate` file: `intermediate.pem`&#x20;

    **Note** that some providers don't have the `Intermediate CA certificate`.&#x20;
8. Please ignore this step if your provider doesn't provide the `Intermediate CA certificate` . Use a plain text editor for example Windows Notepad (do not use MS Word) to open the `Intermediate CA` file and `TLS certificate` file, copy the `Intermediate CA` contents to append to the `TLS certificate file`, and rename the TLS certificate file as `portsip.pem`. In the Linux environment, you can use the below commands to combine the certificate files.&#x20;

```
// Append intermediate file to TLS certificate file
cat intermediate.pem >> cert.pem
```

9. Rename the certificate file:

```
// Rename certificate file to portsip.pem
mv cert.pem portsip.pem
```

10. Now you will have two certificate files:

* Certificate File: portsip.pem,
* Private Key file: portsip.key

Now please follow the article [Update Certificates](update-certificates.md) to update the certificates.&#x20;

