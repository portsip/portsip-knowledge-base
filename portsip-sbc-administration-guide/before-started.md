# Before Started

{% hint style="danger" %}
The PortSIP SBC can't be installed with the **PortSIP PBX v12.x** on the same server. It's can be installed with PortSIP PBX v16.x on the same server.
{% endhint %}



## Prerequisite Knowledge for Linux

Deploying PortSIP SBC in a Linux environment requires planning and knowledge of session initiation protocol (SIP), audio and video calls, presence, and Instant Messaging (IM) administration.

You should also have knowledge of the following Linux infrastructures, a popular Linux distribution:

* CentOS 7.9 (64-bit)
* Debian 10.x, 11.x (64-bit)
* Ubuntu 18.04, 20.04, or 22.04 (64-bit)
* Docker 20.10 or higher.
* IPv4/IPv6
* Systemd
* IP tables
* Firewalld
* HTTP

This document assumes that the Linux OS is already deployed and administrators have been granted **root permission** on Linux.

If your Linux server has installed **PortSIP PBX v12.x**, please follow the below steps before installing PortSIP SBC. These steps will delete all your PBX data. You can back up the `/var/lib/portsip` directory first.

```
sudo docker stop portsip-pbx
sudo docker rm portsip-pbxcd 
cd /var/lib/ && sudo rm -rf portsip
```

The above commands deleted the PBX `docker instance`. Now let us list the PortSIP PBX v12.x `docker image` .

```
// This command will list all docker images
sudo docker image list

// You can use the docker image rm command to delete the PortSIP PBX image, the "1a0c"
// is the first 4 characters of the image tag id.
sudo docker image rm 1a0c
```

###

## Prerequisite knowledge of Windows

Deploying PortSIP SBC in a Windows environment requires planning and knowledge of session initiation protocol (SIP), audio and video calls, presence, and Instant Messaging (IM) administration. You should also have knowledge of the following Windows infrastructure.

A Windows desktop or Windows server OS:

* Windows 10/11 (64-bit)
* Windows Server 2016/2019/2020/2022
* IPv4/IPv6
* Windows firewall

This document assumes that the Windows OS is already deployed and administrators have been granted administrator permission to Windows.

If your Windows server has installed **PortSIP PBX v12.x**, please follow the below steps before installing PortSIP SBC. These steps will delete all your PBX data. You can back up the directory first:`C:\ProgramData\PortSIP` .

1. Uninstall the PortSIP PBX v12.x
2. Delete the `C:\ProgramData\PortSIP` directory
3. Delete the `C:\Program Files\PortSIP` directory

## Cloud and Virtualization Environment Supported

To build a high-availability communication solution to help clients reduce cost and improve communication performance, PortSIP SBC commits support to cloud services and has confirmed compatibility with the following cloud and virtualized environments:

* VMware ESX 5.X and above.
* Linux HyperV
* Microsoft HyperV 2016 R2 and above
* Microsoft AZURE
* Amazon AWS
* Google Could
* Digital Ocean
* ALI Cloud



## Other Requirements

* Latest Firefox, Google Chrome, Edge browser
* Microsoft .NET Framework version 4.5 or higher
* Knowledge of Linux and Linux Internet administration
* Knowledge of Windows and Windows Internet administration
* A constant Internet connection to stun4.l.google.com on port 19302
* Ensure server date time is synced correctly.

## FQDN Support

Although PortSIP SBC is designed to be able to run on servers without FQDN specified, we recommend specifying FQGN with the following advantages:

* Easier access to Web Portal for PortSIP SBC
* Support the Microsoft Teams Direct Routing
* Convenient access to HTTPS when accessing Web Portal
* Avoid browser warnings when accessing the WebRTC Client

The FQDN you are using must be able to be resolved correctly into the server with PortSIP SBCinstalled in LAN. If PortSIP SBC is installed on the public network, FQDN must be resolved correctly into the public network address for the server with PBX installed.

## Prepare the SSL certificate

When there is a need for additional security of the SIP traffic, Transport Layer Security (TLS) is used to secure the device's SIP signaling connections. In TLS protocol, the data is encrypted and protected. TLS communication requires the certificate to authenticate the recipient of the secured data.

PortSIP PBX and SBC use certificates to deliver secured SIP traffic and services and support WebRTC and Microsoft Teams Direct Routing. In addition, certificates are used to provide secure access to the device, and Web (HTTPS) sessions.

Please read [this guide](../portsip-pbx-administration-guide/certificates-for-tls-https-webrtc/) to prepare a trusted SSL certificate.

## Getting Help and Support Resources

[PortSIP Support Center](https://support.portsip.com/)&#x20;

[Submit a request](https://portsip.zendesk.com/hc/en-us/requests/new)

[Email support](mailto:support@portsip.com)
