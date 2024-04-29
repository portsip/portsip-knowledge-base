# Upgrade PortSIP PBX v12.x to the v12.8.7

As Google announced, they will [stop to support the legacy push method in Jun 2024](https://firebase.google.com/docs/cloud-messaging/migrate-v1).&#x20;

The PortSIP PBX v12.x uses the legacy push method to send push notifications, and the PortSIP PBX v12.x was EOL about 1 year ago (Mar 30, 2023). Until today, we see there still have some customers are still staying with PortSIP PBX v12.x and can't upgrade to the v16.x soon.&#x20;

Therefore, we released the PortSIP PBX v12.8.7 to support the new push method for the Android push notifications even though the v12.x is EOL.

This is the last update for the v12.x since it was already EOL long ago. All v12.x customers must need to upgrade to the v16.x ASAP to get better features and support.

## Start Upgrading

### **Windows**

1. Ensure your current installation **is v12.6.x/12.7.x/v12.8.x.**
2. Download the [PortSIP PBX v12.8.7 installer for Windows](https://www.portsip.com/downloads/pbx/v12/portsip-pbx-12.8.7.2683.exe).
3. After downloading the v12.8.7 installer for Windows, you only need to double-click the installer, which will guide you through the upgrade process.

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

## Change push settings for Android

If you are using the PortSIP softphone app, you do not need to do anything, the upgraded PBX v12.8.7 will work with the PortSIP softphone app seamlessly.

### Get a new push profile JSON file

* If you used the Android app that the PortSIP rebranded, please get in touch with us to obtain the new push setting JSON file.
* If you used the Android app that you developed on your own, please follow the below steps to get the new push setting JSON file. Read the guide [Android Push Notifications](https://docs.apppresser.com/article/301-android-push-notifications) and follow **steps 1 - step 3** (no need to follow other steps) to download the push setting JSON file

Assume the push setting JSON file is:&#x20;

### Upload push setting JSON file

Now we need to upload the push setting JSON file to the PortSIP PBX server.

* Linux: Upload that file to `/var/lib/portsip` the folder.
* Windows: Upload that file to `C:\ProgramData\PortSIP` folder.

### Adjust settings

Sign in to the PortSIP PBX v12.8.7 Web portal, select the menu **Advanced > Mobile Push**, and double-click the app to change the settings.

Enter the folder path in which you placed the push setting JSON file to the Google Server Key field.

<figure><img src="../../.gitbook/assets/android_push_path.png" alt=""><figcaption></figcaption></figure>



