# Provision Fanvil DECT IP Phones

From the v22 version, PortSIP supports the Fanvil LINKVIL DECT IP Phones.

## Supported Fanvil DECT IP Phone Models

This guide applies to the following models:

* W710D
* W710H

## Factory Reset the Fanvil DECT System <a href="#h.7dibl1nchwtg" id="h.7dibl1nchwtg"></a>

1. Hold the device button for at least 10 seconds.
2. Release the button.
3. The device will now proceed to reset and reboot.

You can also click the Reset To Factory Settings button to reset the system by navigating to the menu **System > Configurations > Reset Devices > Reset.**

## Upgrade the DECT Base and Handsets to the Latest Firmware <a href="#h.i1ns9ummsm0r" id="h.i1ns9ummsm0r"></a>

Ensure that the DECT device and handsets are running on the latest firmware. To check which firmware the phones are running:

<figure><img src="../../../.gitbook/assets/fanvil-dectp-1.png" alt=""><figcaption></figcaption></figure>

You can follow the below steps to upgrade the firmware:

1. Access the [official Fanvil website](https://www.fanvil.com/service/doc/index.html) and then click **Software**. Select your phone model and download the latest Firmware image file.
2. Sign in to your Fanvil DECT Manager web portal in the browser.
3. Navigate to **System > Upgrade > Software Upgrade > System Image File**.
4. Click **Select** choose the firmware image file saved from the Fanvil website, and press **Upload**.
5. If using the Fanvil W710H repeat this step for all the Fanvil Base Stations you will connect to the DECT Manager.

You can also upgrade the handset firmware in the same steps.

<figure><img src="../../../.gitbook/assets/fanvil-dect-2.png" alt=""><figcaption></figcaption></figure>

## Add a DECT Phone in PortSIP PBX

Please follow the below steps to add the DECT phone to the PortSIP PBX.

1. Sign in to the PortSIP PBX web portal and navigate to **Call Manager > DECT Phones** from the menu.
2. Click on the **Add** button. A popup window will appear.
3. In this window, select your phone model and enter the MAC address of the phone, then click the **OK** button.
4. Provide a user-friendly name for this DECT phone.
5. In the **Network** field, select the network interface that the DECT Phone will use.
6. In the **Country/Region** field, select the specified country and region (the handle needs to be consistent with the base station).
7. Choose the transport protocol that the phone will use to send and receive SIP messages with the PBX.
8. If your PBX has internet access, please enable the **Save to RPS** option.

<figure><img src="../../../.gitbook/assets/fanvil-dect-3.png" alt=""><figcaption></figcaption></figure>

## Assign Users to the Handsets <a href="#h.ipuczchjqkl4" id="h.ipuczchjqkl4"></a>

Next, you'll need to assign users to the handsets. Here's how you can do it:

1. Click on the **Users** tab.
2. For each handset, select the users you want to assign.
3. If you want to restrict a user to a specific handset, enter the handset’s IPUI/IPEI in the provided field. If you don’t want to impose any restrictions, leave this field empty.

Remember, assigning users to specific handsets can help manage calls more effectively. However, be sure to double-check the IPUI/IPEI to avoid any mix-ups.

Next, you'll need to assign users to the handsets. Here's how you can do it:

<figure><img src="../../../.gitbook/assets/fanvil-dect-4.png" alt=""><figcaption></figcaption></figure>

## Auto Provision Handsets by RPS

If your PBX installation is in the cloud and you have turned on the **Save to RPS** to configure the DECT phone in the above step&#x73;**,** the DECT phone will download the configuration file and provision handsets, the handsets of this DECT phone station will register to the PortSIP PBX automatically.

## Provision Handsets Manually

If your PBX installation is on-premise without internet access or you turned off the **Save to RPS** option, you will need to follow the below steps to provision handsets:

1. Click the menu **Call Manager > DECT Phones**, and double the DECT phone.
2. Copy the provisioning link.

<figure><img src="../../../.gitbook/assets/fanvil-dect-5.png" alt=""><figcaption></figcaption></figure>

3. Enter the DECT base station's IP address in the web browser and open it.
4. In the menu **System > Auto Provision > Static Provisioning Server**, enter the provisioning link copied in **Server Address** and enter {mac}.cfg in **Configuration File Name**.
5. Press the **Apply** button to save the link and then press the button **Auto Provision Now**.

## Pair the Handset to the DECT Base Station

1. **Open the DECT Base Station’s Web Interface:**
   * Enter the DECT Base Station's IP address in your web browser to access the interface.
   * In the menu, go to **Handset > Maintenance**, then click **Add**.
2. **Enter Handset Details:**
   * Input the **IPUI** of the handset and assign a friendly name.
   * Select the corresponding extension number for the handset.
3. **Ensure Consistent Country and Region Settings:**
   * The country and region selected for the controller must match the settings of the base station. Once the selection is made, the system will restart.
4. **Connect to the Base Station:**
   * Navigate to **Menu > Networks > Available Networks**.
   * Click **Scan** to search for available networks, then connect to the specified base station.
   * Enter the password and click **Confirm** to complete the pairing.

<figure><img src="../../../.gitbook/assets/fanvil-dect-6..png" alt=""><figcaption></figcaption></figure>

