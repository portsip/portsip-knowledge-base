# Release Notes of PortSIP PBX v22.X

## 1. Latest Versions

* **PortSIP PBX**: v22.0.42
* **PortSIP SBC**: v11.1
* **PortSIP ONE (Softphone) for Windows**: v10.2.0
* **PortSIP ONE (Softphone) for iOS**: v10.2.1
* **PortSIP ONE (Softphone) for Android**: v10.2

## 2. Release Notes

* Release Note for [**v22.0.0**](https://www.portsip.com/2024/12/12/portsip-pbx-v22-0-0-release/)
* Release Note for [**v22.0.39**](https://www.portsip.com/2025/01/02/portsip-pbx-v22-0-39-release/)
* Release Note for [**v22.0.42**](https://www.portsip.com/2025/01/16/portsip-pbx-v22-0-42-release/)
* [Summary of Changes](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/summary-of-changes)

## 3. Download the PortSIP ONE App

{% hint style="warning" %}
Please note: **PortSIP ONE** is not compatible with **PortSIP PBX** version 16.x.
{% endhint %}

{% hint style="info" %}
If you have previously installed a beta version of **PortSIP ONE** for testing purposes, ensure that you uninstall the old version and remove all files and folders from the directory `C:\ProgramData\PortSIP\portsip1`.
{% endhint %}

### **PortSIP ONE for Windows**

* [Download Link](https://www.portsip.com/download-portsip-one/)
* [User Guide](https://support.portsip.com/apps-guides/portsip-one-desktop-app)

### **PortSIP ONE for Mobile**

* [Apple App Store](https://apps.apple.com/us/app/portsip-one/id6504066513)
* [Google Play](https://play.google.com/store/apps/details?id=com.portsip.one1)
* [User Guide](https://support.portsip.com/apps-guides/portsip-one-mobile-app)

## 4. Upgrading

### **4.1 Upgrade Prerequisites**

* **Upgrading to v22.x on Linux**
  * **Upgrading from v16.x to v22.x**: Follow the [Prerequisites for Upgrading from v16.x](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/upgrade-to-the-latest-v22.x-on-linux#prerequisites-for-upgrading-from-v16.x).
  * **Upgrading within v22.x**: Follow the [Prerequisites for Upgrading within v22.x](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/upgrade-to-the-latest-v22.x-on-linux#prerequisites-for-upgrading-within-v22.x).

### **4.2 Upgrading the PBX**

Follow the [same steps as a fresh PBX installation](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-pbx-on-linux) to set up a new PBX Docker instance to complete the upgrading.&#x20;

The installer will automatically manage the data upgrade process, ensuring that all your data and configurations are retained.

### **4.3 Upgrading the SBC**

Follow the[ same steps as a fresh SBC installation](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/9-configuring-portsip-sbc/installation-portsip-sbc-v11.x) to set up a new SBC Docker instance to complete the upgrading.

The installer will automatically manage the data upgrade process, preserving all your data and configurations.

### **4.4 Installing/Upgrading the IM Service**

For installation or upgrading the PortSIP IM Server on Linux, please refer to the [Install PortSIP IM Server on Linux documentation](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-im-server-on-linux).

Congratulations! You are now ready to enjoy the enhanced communications experience with PortSIP.

