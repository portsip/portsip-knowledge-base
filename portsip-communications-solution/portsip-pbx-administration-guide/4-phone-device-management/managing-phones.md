# Managing Phones

PortSIP PBX provides a centralized and efficient way to monitor, manage, and secure IP phones and softphones across your network.

The Call **Manager > Phones** menu in the PortSIP PBX Web Portal allows administrators to:

* View all phones on the network, including IP addresses and MAC addresses
* View all PortSIP clients connected in softphone mode
* Check the firmware version currently running on each phone
* Remotely reboot one or multiple phones
* Re-provision phones to apply updated configurations
* Launch the phone’s web-based administration interface
* Monitor the security strength of extension passwords and PINs

> ❗ **Important**\
> Weak extension passwords and PINs are one of the **most common causes of PBX security breaches**.\
> Regularly review phone and extension security settings to reduce the risk of unauthorized access and toll fraud.

***

### Adding Phones

You can add phones to PortSIP PBX using the following methods:

* **Plug and Play (PnP)**\
  Simply connect the IP phone to the local LAN. The PBX will automatically detect and provision the device.
* **Remote Provisioning Service (RPS)**\
  Use RPS to provision phones that are located **outside the local network**, such as remote or home-office devices.

***

### Changing Phone Settings

Configuration changes made under the **User**, **Extension**, or **Phone Provisioning** tabs are not applied to IP phones immediately. To force a phone to retrieve the updated configuration, you must **re-provision** the device.

#### Re-provisioning a Phone

Use re-provisioning when configuration changes have been made and need to take effect immediately:

1. Navigate to **Call Manager > Phones**.
2. Select the provisioned phone you want to update.
3. Click **Reprovision**.

* If a reboot is required, the phone will **restart automatically**.
* The updated configuration will be **downloaded and applied automatically**.

> ❗ **Important**\
> Re-provisioning ensures configuration consistency and is recommended after changes to **extensions, codecs, SIP credentials, or security settings**.



