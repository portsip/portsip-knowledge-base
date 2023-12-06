# Setup PortSIP PBX for Linux

PortSIP PBX Linux edition is migrated to Docker environment, which does not support RPM and Deb installer.&#x20;

### **Supported Linux OS**

* CentOS: 7.9
* Ubuntu: 18.04, 20.04
* Debian: 10.x
* It only supports 64bit OS

### Preparing the Linux Host Machine for Installation&#x20;

Tasks that MUST be completed before installing PortSIP PBX.

* If the Linux on which PBX will be installed is located in LAN, assign a static LAN IP address; if it's in a public network, please assign a static IP address for the public network&#x20;
* Install all available updates & service packs before installing PortSIP PBX&#x20;
* Do not install PostgreSQL on your PortSIP PBX Server
* Ensure that all power-saving options for your System and Network adapters are disabled (by setting the system to High Performance)&#x20;
* Do not install TeamViewer, VPN, and other similar software on the host machine&#x20;
* PortSIP PBX must not be installed on a host which is a DNS or DHCP server&#x20;
* Below ports must be permitted by your firewall.&#x20;
  * UDP: 45000– 65000, 25000- 35000 TCP: 8899– 8900、8887-8888、8881-8885&#x20;
* Make sure that the below ports have not been used by other programs:&#x20;
  * UDP: 45000– 65000, 25000- 35000 TCP: 8899– 8900、8887-8888、8881-8885&#x20;

{% hint style="info" %}
&#x20;**Imortant**: If the PBX running on a cloud platform such as AWS, and the cloud platform has the firewall itself, you MUST open the ports on the cloud platform firewall too.
{% endhint %}

###

### Installing a fresh PortSIP PBX for Linux

* Ensure server date-time is synced correctly
* Must perform all Linux commands by the **root** user, please **su root** first

#### **Step 1. Perform below command**

```bash
# curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v12.6.x/install_pbx_docker.sh|bash
```

#### **Step 2: Create and run the PortSIP PBX Docker container instance**

Performing the below command will launch the PortSIP PBX docker instance on a Linux server for which the IP is 66.175.222.20.

* The POSTGRES\_PASSWORD is used to specify the PortSIP DB password. In this case, we will use 123456, you can change it by yourself. Suggest using alphanumeric only, special character maybe causes problems.
* The IP\_ADDRESS is the IP address of your PBX server (Linux Server). In this case, _it is 66.175.222.20, you will need to change it by yourself._

```bash
# docker container run -d --name portsip-pbx \
         --restart=always --cap-add=SYS_PTRACE \
         --network=host -v /var/lib/portsip:/var/lib/portsip \
         -v /etc/localtime:/etc/localtime:ro \
         -e POSTGRES_PASSWORD="123456" \
         -e POSTGRES_LISTEN_ADDRESSES="*" \
         -e IP_ADDRESS="66.175.222.20" portsip/pbx:12
```

In the future, once created the new transport in the PortSIP PBX, a new firewall rule must be added to enable the transport port.

&#x20;For example: if created the UDP transport on port 5060, add the below new firewall rule to enable the UDP port 5060.

```bash
# firewall-cmd --permanent --service=portsip-pbx \
               --add-port=5060/udp \
               --set-description="PortSIP PBX"
# firewall-cmd --permanent --add-service=portsip-pbx
# firewall-cmd --reload
```

If there created TCP transport on port 5063 and WSS transport on port 5065, add the below new rules to enable TCP ports 5063 and 5065.

```bash
# firewall-cmd --permanent --service=portsip-pbx \
               --add-port=5063/tcp \
               --add-port=5065/tcp \
               --set-description="PortSIP PBX"
# firewall-cmd --permanent --add-service=portsip-pbx
# firewall-cmd --reload
```

{% hint style="info" %}
&#x20;**Important:** If the PBX running on a cloud platform such as AWS, and the cloud platform has the firewall itself, **MUST** open the ports on the cloud platform firewall too. For more details please read the [PBX User Guide](https://www.portsip.com/portsip-pbx-user-guide/).
{% endhint %}

#### **Step 3: C**onfiguring the PortSIP PBX

Sign in to the PortSIP PBX Web Portal to configure the PBX by opening the below URL. For more details please follow the [PBX User Guide](https://www.portsip.com/portsip-pbx-user-guide/).

> [http://66.175.222.20:8888](http://66.175.222.20:8888)&#x20;
>
> [https://66.175.222.20:8887](https://66.175.222.20:8887)
