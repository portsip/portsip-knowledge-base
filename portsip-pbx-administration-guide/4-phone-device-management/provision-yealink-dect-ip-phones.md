# Provision Yealink DECT IP Phones

From the v16.2 version, PortSIP supports the Yealink DECT IP Phones.

## Supported Yealink DECT IP Phone Models

This guide applies to the following models:

* W60B
* W70B
* W80B/DM
* W90B/DM

## Upgrade the DECT Base and Handsets to the Latest Firmware <a href="#h.i1ns9ummsm0r" id="h.i1ns9ummsm0r"></a>

Ensure that the DECT device and handsets are running on the latest firmware. To check which firmware the phones are running:

<figure><img src="../../.gitbook/assets/yealink-dectp-1.png" alt=""><figcaption></figcaption></figure>

You can follow the below steps to upgrade the firmware:

1. Access the [official Yealink website](https://www.yealink.com/en/solution-detail/resource-for-3cx) and then click **Software**. Select your phone model and download the latest Firmware image file.
2. Sign in to your Yealink DECT Manager web portal in the browser.
3. Navigate to **Settings > Upgrade > Select and Upgrade Firmware**.
4. Click **Browse** choose the firmware image file saved from the Yealink website, and press **Upgrade**.
5. If using the Yealink W80 or W90 repeat this step for all the Yealink Base Stations you will connect to the DECT Manager.

You can also upgrade the handset firmware in the same steps.

<figure><img src="../../.gitbook/assets/yealink-dectp-2.png" alt=""><figcaption></figcaption></figure>

## Factory Reset the Yealink DECT System <a href="#h.7dibl1nchwtg" id="h.7dibl1nchwtg"></a>

1. Hold the device button for at least 20 seconds.
2. Release the button.
3. The device will now proceed to reset and reboot.

You can also click the Reset To Factory Settings button to reset the system by navigating to the menu **Settings > Upgrade > Select and Upgrade Firmware.**

## Add a DECT Phone in PortSIP PBX

Please follow the below steps to add the DECT phone to the PortSIP PBX.

<figure><img src="../../.gitbook/assets/yealink-dect-3.png" alt=""><figcaption></figcaption></figure>

1. Sign in to the PortSIP PBX web portal and navigate to **Call Manager > DECT Phones** from the menu.
2. Click on the **Add** button. A popup window will appear.
3. In this window, select your phone model and enter the MAC address of the phone, then click the **OK** button.
4. Provide a user-friendly name for this DECT Phone.
5. In the **Network** field, select the network interface that the DECT Phone will use.
6. Choose the transport protocol that the phone will use to send and receive SIP messages with the PBX.
7. If your PBX has internet access, please enable the **Save to RPS** option.

## Assign Users to the handsets <a href="#h.ipuczchjqkl4" id="h.ipuczchjqkl4"></a>















