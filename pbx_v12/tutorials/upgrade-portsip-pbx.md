# Upgrade PortSIP PBX

## Start upgrading

### **Windows**

1. Ensure your current installation **is v12.6.x/12.7.x/v12.8.x.**
2. Download the [PortSIP PBX v12.8.2 installer](https://www.portsip.com/download-portsip-pbx/) for Windows
3. After downloading the v12.8.2 installer for Windows, you only need to double-click the installer, which will guide you through the upgrade process.



### **Linux**

Please execute the below commands to upgrade.

> **Note:**
>
> * The **IP\_ADDRESS** is the IP address of your PBX server. In this case it is 66.175.222.20, you will need to change it by yourself, if your server is on public internet network, this IP should be the public IP.
> * The **POSTGRES\_PASSWORD** is used to specify the PortSIP DB password. In this case we will use 123456, you can change it by yourself.

> **The OS required:**
>
> * CentOS: 7.9
> * Ubuntu: 18.04, 20.04
> * Debian: 10.x
> * Only supports 64bit OS



{% hint style="info" %}
From v12.6.1, the PortSIP PBX requires running with the above Linux OS versions. If there already installed the PortSIP PBX which is less than v12.6.1, and wish to upgrade to v12.6.1 or a later version, must upgrade the Linux OS to the above version before upgrading the PortSIP PBX.
{% endhint %}

{% hint style="danger" %}
Please replace the IP 66.175.222.20 with your PBX IP; feel free to choose a string for the POSTGRES\_PASSWORD
{% endhint %}

For CentOS/Ubuntu/Debian, use the below commands to perform the upgrade.

```
# su root
# docker stop -t 120 portsip-pbx
# docker rm -f portsip-pbx
# sudo curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v12.6.x/install_pbx_docker.sh|bash
# docker pull portsip/pbx:12
# docker container run -d --name portsip-pbx \
         --restart=always --cap-add=SYS_PTRACE \
         --network=host -v /var/lib/portsip:/var/lib/portsip \
         -v /etc/localtime:/etc/localtime:ro \
         -e POSTGRES_PASSWORD="123456" \
         -e POSTGRES_LISTEN_ADDRESSES="*" \
         -e IP_ADDRESS="66.175.222.20" portsip/pbx:12
```

