# Before Started

## Prerequisite Knowledge for Linux

Deploying PortSIP PBX in a Linux environment requires planning and knowledge of session initiation protocol (SIP), audio and video calls, presence, and Instant Messaging (IM) administration.

You should also have knowledge of the following Linux infrastructures and popular Linux distributions:

* Debian 11.x, 12.x (64-bit)
* Ubuntu 22.04, or 24.04 (64-bit)
* Docker 20.10 or higher.
* IPv4/IPv6
* Systemd
* IP tables
* Firewalld
* HTTP

This document assumes that the Linux OS has already been installed and that administrators of PortSIP PBX have been granted root permission to use Linux.

## Remove the PBX v12.x Installation

{% hint style="danger" %}
Please ignore this section if you haven't installed the PortSIP PBX v12.x version.
{% endhint %}

If your Linux server has installed **PortSIP PBX v12.x**, please follow the below steps before installing v16.0. These steps will delete all your PBX data. You can back up the `/var/lib/portsip` directory first.

```
sudo docker stop portsip-pbx
sudo docker rm portsip-pbx 
cd /var/lib/ && sudo rm -rf portsip
```

The above commands deleted the PBX v12.x `docker instance`. Now let us list the PortSIP PBX v12.x `docker image` .

```
// This command will list all docker images
sudo docker image list

// You can use the docker image rm command to delete the PortSIP PBX image, the "1a0c"
// is the first 4 characters of the image tag id.
sudo docker image rm 1a0c
```



## Prerequisite Knowledge of Windows

Deploying PortSIP PBX in a Windows environment requires planning and knowledge of session initiation protocol (SIP), audio, video call, presence, and Instant Messaging (IM) administration. You should also have knowledge of the following Windows infrastructure.

A Windows desktop or Windows server OS:

* Windows 10 1903/19H1 or higher, Windows 11
* Windows Server 2022
* IPv4/IPv6
* Windows firewall

This document assumes that the Windows OS has already been deployed and administrators of PortSIP PBX have been granted administrator permission to access Windows.

## Remove the PBX v12.x Installation

{% hint style="danger" %}
Please ignore this section if you haven't installed the PortSIP PBX v12.x version.
{% endhint %}

If your Windows server has installed **PortSIP PBX v12.x**, please follow the below steps before installing v16.0. These steps will delete all your PBX data. You can back up the `C:\ProgramData\PortSIP` directory first.

1. Uninstall the PortSIP PBX v12.x
2. Delete the `C:\ProgramData\PortSIP` directory
3. Delete the `C:\Program Files\PortSIP` directory

## Cloud and Virtualization Environment Supported

To build a high-availability communication solution to help clients reduce cost and improve communication performance, PortSIP PBX commits support to cloud services and has confirmed compatibility with the following cloud and virtualized environments:

* VMware ESX 5.X and above.
* Linux HyperV
* Microsoft HyperV 2016 R2 and above
* Microsoft AZURE
* Amazon AWS
* Google Could
* Digital Ocean
* Linode

## System Performance Depends On the Following Key Factors

* Maximum simultaneous calls needed for PBX
* Maximum online users needed for PBX
* Maximum users to chat and share files in chat
* Maximum chat groups
* Recordings for calls
* Recording audio only or both audio and video
* Maximum online users for audio/video conferences on PBX
* Maximum IVRs (Virtual Receptionists) on PBX
* Maximum Call Queues on PBX
* Maximum Ring Groups on PBX

Depending on the key features listed above, PortSIP PBX is able to run on PCs and servers with various CPUs ranging from Intel i3 CPUs to Xeon.

## Other Requirements

* Latest Firefox, Google Chrome, and Edge browser
* Microsoft.NET Framework version 4.5 or higher
* Knowledge of Linux and Linux Internet administration
* Knowledge of Windows and Windows Internet administration
* A constant Internet connection to stun4.l.google.com on port 19302
* Ensure server date time is synced correctly.

## FQDN Support

Although PortSIP PBX is designed to be able to run on servers without FQDN specified, we recommend specifying FQGN with the following advantages:

* Easier access to Web Portal for PortSIP PBX
* Easier management of IP phones and clients after IP address change for PBX
* Convenient access to HTTPS when accessing the Web Portal
* Avoid browser warnings when accessing the WebRTC Client

The FQDN you are using must be resolved correctly on the server with PortSIP PBX installed in the LAN. If PortSIP PBX is installed on the public network, FQDN must be resolved correctly into the public network address for the server with PBX installed.

## Prepare the TLS certificate

When there is a need for additional security of the SIP traffic, Transport Layer Security (TLS) is used to secure the device's SIP signaling connections. In TLS protocol, the data is encrypted and protected. TLS communication requires the certificate to authenticate the recipient of the secured data.

PortSIP PBX and SBC use certificates to deliver secured SIP traffic and services and support WebRTC and Microsoft Teams Direct Routing. In addition, certificates are used to provide secure access to the device and Web (HTTPS) sessions.

Please read [this topic](../tutorials/certificates-for-tls-https-webrtc/) to prepare a trusted SSL certificate.

## Getting Help and Support Resources

[PortSIP Support Center](https://support.portsip.com/)&#x20;

[Submit a request](https://portsip.zendesk.com/hc/en-us/requests/new)

[Email support](mailto:support@portsip.com)

