# 6 Transport Management

After completing the **Setup Wizard**, you may manage your PortSIP PBX in the Web Portal.

The PortSIP PBX supports a wide range of transports, including UDP, TCP, and TLS for SIP signaling. You need to configure the transport and set the ports to use when listening for SIP signaling.

{% hint style="info" %}
Only System Administrators are allowed to create or delete SIP transport. When deleting, at least one transport must be left around.
{% endhint %}

The default transport has been configured with the **Setup Wizard**. To make changes, you need to sign in to the PBX Web Portal as the **System Admin** and select the **Call Manager > Transports** menu. Click the **Add** button to add a new transport.

{% hint style="danger" %}
Once you add a new transport, you have to change the firewall rule to allow that transport port. The IP Phone client app will connect to the PBX via transport and port.

If the PBX runs on a cloud platform such as AWS and the cloud platform has its own firewall, you **must** also open the ports on the cloud platform's firewall.
{% endhint %}



## Add UDP/TCP transport

To add UDP/TLS/TCP transport:

* Click the **Add** button, and choose UDP/TLS/TCP in the **Transport protocol** box. The default transport port for UDP/TLS/TCP is 5060/5061/5063. You may specify another port as you like, but the port must not be in use by other applications
* Click the **OK** button to add the transport.

## Add TLS transport

First of all, prepare the certificate files. Please refer to "[Preparing TLS Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/)" to purchase a certificate file from a trusted third-party provider for your PBX **Web Domain** (assuming you purchased the certificate for the domain **uc.portsip.cc**).

* Click the **Add** button and choose **TLS** in the **Transport protocol** box. The default transport port for TLS is 5061. You may specify another port as you like, but the port must not be in use by other applications.
* Click the **OK** button to add the transport.

## Configure Firewall Rule

If you created your own transport rather than using the default transport, you must open the transport port on the firewall with the commands listed below.

For example, if you created a **UDP** transport on port **5066**, **TCP** transport on port **5071**, **TLS** transport on port **5072**, and **WSS** transport on port **5075**, please execute the below commands.

```bash
firewall-cmd --permanent --service=portsip-pbx --add-port=5066/udp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5071/tcp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5072/tcp --set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5075/tcp --set-description="PortSIP PBX"
firewall-cmd --reload
```

{% hint style="danger" %}
If the PBX runs on a cloud platform such as AWS and the cloud platform has its own firewall, you **must** also open the ports on the cloud platform's firewall.
{% endhint %}



