# PortSIP PBX Beta Testing

## Important Notes Before You Begin

* **Beta Stage:** Version 22.0 is currently in its beta phase. It is not recommended for use in a production environment.
* **Operating System Compatibility:** The beta version is only available for Linux. We recommend using Debian 11/12 or Ubuntu 22.04/24.04.
* **Fresh Installation Required:** Since this is a beta release, it cannot be seamlessly upgraded from the current v16.x installation. You will need to perform a fresh installation on a new server.
* **No Backward Compatibility:** Beta versions do not support backward compatibility. This means that if you want to install a new beta version, you will need to uninstall and delete the previous beta version.
* **Future Upgrades:** Once the v22.0 is officially released, it will support seamless upgrades from v16.x installations.

{% hint style="danger" %}
The beta version is intended for testing purposes only and should not be used in a production environment. We do not guarantee the stability of features or the integrity of data in this beta release.
{% endhint %}

## Supported Linux OS

The PortSIP PBX only supports the following 64-bit Linux OS:

* Ubuntu 22.04, 24.04
* Debian 11.x, 12.x

## Minimal Hardware Requirements

The PortSIP PBX requires a minimum of 2 CPU cores, 4GB of memory, and 30GB of storage. With this configuration, and without enabling call recording, the system can support up to 1,000 online users and handle 300–400 simultaneous calls.

## Preparing the Linux Host Machine for Installation

Tasks that MUST be completed before installing PortSIP PBX

* **Ensure the server date-time is synced correctly**.
* Use the `sudo` to perform the installation is recommended.
* If the Linux on which PBX will be installed is located on a LAN, assign &#x61;**`static private IP address`**&#x74;o the PBX server; if it's on a public network, assign &#x61;**`static public IP address`** and a **`static private IP`** to the PBX server.&#x20;
* Install all available updates and service packs before installing PortSIP PBX.
* Do not install **PostgreSQL** on your PortSIP PBX Server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or other similar software on the host machine.
* The PortSIP PBX must not be installed on a host that is a DNS or DHCP server.
* The below ports must be permitted by your firewall.
  * UDP: 5060, 5066, 25000-34999, 45000–65000
  * TCP: 5061, 5063, 5065, 5067, 8882, 8883, 8887, 8888, 8889, 10443. please also ensure the above ports have not been used by other applications.

{% hint style="danger" %}
If the PBX runs on a cloud platform such as AWS and the cloud platform has its own firewall, you **must** also open the ports on the cloud platform's firewall.
{% endhint %}

