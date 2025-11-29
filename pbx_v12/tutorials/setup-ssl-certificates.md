# Setup SSL Certificates for HTTPS/WebRTC

This guide for solving the below SSL certificates issues with PortSIP PBX.

* After you completed the PBX setting up, if get the self-signed certificates warning in the browser when you access PBX Web Portal by HTTPS or access the WebRTC client, please follow up on the below steps to solve it
* The SSL certificates are expired

Please follow up on the below steps.

1. Purchase a Domain from the domain provider for your PBX, for example, Godaddy.
2. Add the A record in the Domain DNS zone, resolve the Domain to your PBX IP.
3. Purchase a certificate from the trust certificate provider, for example, [Digicert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/), usually you will have two files, one is the certificate, another one is the private key. **Note**, please check the certificates for Nginx.
4. Sign in the PortSIP PBX Web Portal.
5. Click the left menu "**Home > Summary**", then click the "**Setup Wizard**", in step 1, enter your domain for the "**Web Domain**", click the **Next** button to complete the wizard.
6. Click the left menu "**Home > Call Manager > Domain and Transport**", if there has the TLS, WSS transport added, just delete the TLS and WSS transport.
7. Add the WSS and TLS transport with upload the purchased certificate files.
8. For Windows installation, restart the Windows Server directly.
9. For Linux installation, perform the below commands:

```
# sudo docker exec -it portsip-pbx /bin/bash
# supervisorctl stop nginx
# supervisorctl stop gateway
# supervisorctl stop callmanager
# supervisorctl start nginx
# supervisorctl start gateway
# supervisorctl start callmanager
```

After restarted, you can sign in PortSIP PBX Web Portal by URL [https://yourdomain.com:8887](https://yourdomain.com:8887)

If you don't use trusted certificate files for the WSS transport, you will get the browser warning and blocked when you use the WebRTC client.<br>
