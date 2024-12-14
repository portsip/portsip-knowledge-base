# PortSIP ONE Desktop App

## Downloading PortSIP ONE for Windows Desktop

To download the latest version of PortSIP ONE for Windows desktop, visit the [PortSIP website](https://www.portsip.com/download-portsip-one/). The application is provided as an MSI installer. Simply double-click the downloaded file to start the installation process.

## Logging into the PortSIP ONE App via QR Code

The PortSIP ONE app makes logging in effortless by using a QR code. Please follow the below steps to process.

### 1. Obtaining a QR Code

If the SMTP server has been configured in PortSIP PBX by the tenant manager, the PBX will automatically email the account information and QR code to the extension user upon successful extension creation.

As a tenant manager, you can also manually access and manage QR codes by following these steps:

1. Sign in to the PBX web portal as the tenant manager.
2. Navigate to **Call Manager > Users** to list all users.
3. Select a user by either clicking **Edit** or double-clicking their name.
4. The user’s QR code will be displayed. To refresh the QR code, click on it.

<figure><img src="../../.gitbook/assets/user_qr1.png" alt=""><figcaption></figcaption></figure>



Within the **Extension** tab of a user’s settings, there are two QR code configuration options:

1. **Preferred Transport for QR Code:**
   * This dropdown menu lets you specify the preferred transport protocol (e.g., TCP, UDP) for the QR code. When scanning the QR code to register with the PBX, the PortSIP ONE app will prioritize this setting.
2. **Generate a QR Code with the Network Interface Below:**
   * Use this option to designate the Outbound Proxy Server for the PortSIP ONE app when scanning the QR code to log in to the PBX.

**Important Note:** If you modify the PBX IP address or adjust either of the above options, you must refresh the QR code by clicking on it to ensure the updated configuration is applied.

### 2. Logging into the PortSIP ONE App

The PortSIP ONE app makes logging in effortless by using a QR code. On the PortSIP ONE login window, click the QR code icon located at the bottom-right corner. Select the QR code option, and then scan the QR code provided by the PBX. The app will log in automatically.

<figure><img src="../../.gitbook/assets/portsip-one-desktop-qrcode.png" alt=""><figcaption></figcaption></figure>













