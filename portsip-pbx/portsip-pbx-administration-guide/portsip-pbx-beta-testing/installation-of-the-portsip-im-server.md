# Installation of the PortSIP IM Server

This article is for installing the PortSIP IM Server for the PortSIP PBX, please ensure you have completed the step of [Installation of the PortSIP PBX v22.0 beta](installation-of-the-portsip-pbx-beta.md).

## Install Instant Messaging Service

Follow these steps to install the IM server:

**1. Generate Token for the IM Server**

1. Log in as the **System Administrator** to the PortSIP PBX Web portal.
2. Navigate to **Servers > IM Servers**.
3. Select the default server and click the **Generate Token** button.
4. Copy the generated token.

<figure><img src="../../../.gitbook/assets/portsip-pbx-v22-im-token.png" alt=""><figcaption></figcaption></figure>

### Create and Run Instant Messaging Docker Instance

Follow these steps to create the IM service Docker instance:

1. Navigate to the **/opt/portsip** directory by running the following command:

```sh
cd /opt/portsip
```

2. Use the command below to create the Instant Messaging service Docker instance. Replace the placeholders with your actual values:

* **-p**: Specifies the path for storing the IM service data.
* **-a**: Specifies the IP address of the server.
* **-i**: Specifies the PBX Docker image version.
* **-t**: Specifies the token generated in the previous step.

{% code overflow="wrap" %}
```sh
sudo /bin/sh im_ctl.sh run -p /var/lib/portsip/ -i portsip/pbx:22.0.33.1354-beta \
-t MJC4NZBLYTGTZTJJNS0ZMWZHLWIXZDCTZJLLMDEWZJHKZTAY
```
{% endcode %}

## Step 6: Reboot to Apply the Certificate

If you uploaded a trusted SSL certificate in **Step 2: SSL Certificate** (instead of using the default self-signed certificate), you need to restart the PBX to apply the changes. Use the following commands to reboot the PBX:

```sh
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh restart
sudo /bin/sh im_ctl.sh restart
```

Now the PortSIP PBX is successfully installed.



