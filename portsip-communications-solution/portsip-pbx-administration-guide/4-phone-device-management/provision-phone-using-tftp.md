# Provision Phone Using TFTP

### What Is TFTP?

**TFTP (Trivial File Transfer Protocol)** is a lightweight file transfer protocol that operates over **UDP port 69**. It is primarily used for **automated file transfers** between devices on a local network.

In a **VoIP environment**, TFTP is commonly used to upload or download **firmware files**, configuration files, and other resources for devices such as:

* IP Phones
* VoIP Gateways
* Routers and other network hardware

***

### Installing a TFTP Server

There are many free and open-source TFTP server applications available, such as:

* **PumpKIN**

Download and install a TFTP server of your choice according to your operating system and requirements.

***

### Configuring PumpKIN

To configure PumpKIN as a TFTP server:

1. Launch PumpKIN.
2. From the PumpKIN menu, navigate to:\
   **Options > Server > TFTP file system root**
3.  Set the TFTP root directory to the **PortSIP PBX provisioning folder**.\
    By default, this path is:

    ```
    C:\ProgramData\PortSIP\pbx\provision\R2Xu8aVV20Jka
    ```

    > `R2Xu8aVV20Jka` is a **randomly generated provisioning folder name** created by PortSIP PBX.

    This directory is used to **store files that devices upload to or download from the PBX via TFTP**.
4. Configure additional options as needed, such as:
   * **Read and Write Request Behavior** (for prompting before file uploads or downloads)
   * **Network port**
   * **Timeout values**

With the TFTP server installed, you can now begin updating IP phones, gateways, and routers as required.

<figure><img src="../../../.gitbook/assets/portsip_tftp_phone.png" alt=""><figcaption></figcaption></figure>

> ⚠️ **Security Note**\
> For security reasons, you must **disable Auto-Provisioning Security Enhancement** before provisioning phones using TFP as well.\
> Refer to the article "[**DHCP Option 66 and Auto-Provisioning in PortSIP PBX**](auto-provisioning-security.md#dhcp-option-66-and-auto-provisioning-in-portsip-pbx)**"** for details.



