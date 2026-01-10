# 26 Hot Desking

### Overview

**Hot desking** is a workplace model where employees use any available desk or phone rather than having a permanently assigned workspace. Introduced in the 1990s, hot desking has become increasingly practical with modern communications technology.

With hot desking:

* Users can log in to **any desk phone** on the network using their credentials
* Personal extension settings, voicemail, and call handling follow the user
* Cloud-based VoIP systems enable flexible and remote working environments

Some organizations deploy hot desking company-wide, while others apply it to specific teams, locations, or roles.

***

### Key Features and Benefits

Hot desking in PortSIP PBX provides the following advantages:

* Employees working across multiple offices can share desk phones while retaining their own extension profiles and voicemail
* Reduces hardware and facility costs by allowing shared phone usage
* Improves productivity without requiring dedicated devices
* Supports a wide range of IP phone models and deployment scenarios

***

### Setting Up a Device for Hot Desking

PortSIP PBX supports two provisioning methods for hot desking devices:

* **PnP (Plug and Play) Provisioning** – typically used for on-premises PBX deployments
* **RPS (Redirect/Remote Provisioning Service)** – typically used for cloud deployments

<figure><img src="../../.gitbook/assets/hdk-2.png" alt=""><figcaption></figcaption></figure>

***

### Setting Up Hot Desking via PnP Provisioning

This method is commonly used when the PBX is deployed on-premises.

#### Steps

1. Sign in to the **PortSIP PBX Web Portal** as a **Tenant Administrator**.
2. Plug the IP phone into the network.
3. The phone sends a multicast discovery message on the LAN.
4. The PBX detects the phone, which appears under **Call Manager > Phones** as a new device.
5. Copy the IP phone’s **MAC address**.
6. Navigate to **Advanced Services > Hot Desking** and click **Add**.
7. Enter the phone details:
   * **MAC Address**
   * **Provisioning Method**
   * **Network** (select **SBC** if an SBC is used)
8. Enter:
   * A friendly **Display Name**
   * A unique **Extension Number** for the hot desking phone
9. Click **OK** to create the hot desking extension.

After saving:

* The PBX generates the configuration file
* A **NOTIFY** message is sent to the phone with the provisioning URL
* The phone downloads the configuration, provisions itself, and registers the hot desking extension

***

### Setting Up Hot Desking via RPS Provisioning

This method is typically used when the PBX is deployed in the cloud.

#### Steps

1. Sign in to the **PortSIP PBX Web Portal** as a **Tenant Administrator**.
2. Copy the IP phone’s **MAC address**.
3. Navigate to **Advanced Services > Hot Desking** and click **Add**.
4. Enter the phone details:
   * **MAC Address**
   * **Provisioning Method**
   * **Network** (select **SBC** if applicable)
5. Enter:
   * A friendly **Display Name**
   * A unique **Extension Number** for the hot desking phone
6. Ensure **Save to RPS** is enabled.
7. Click **OK** to create the hot desking extension.

After saving:

* The PBX writes the provisioning URL to the phone vendor’s RPS
* When powered on, the phone retrieves the provisioning link using its MAC address
* The phone downloads the configuration, provisions itself, and registers the hot desking extension automatically

***

### Logging In to a Hot Desking Phone

Once the hot desking phone is registered, users can log in to it with their own extension.

#### Enable Hot Desking for a User

1. Navigate to **Call Manager > Users**.
2. Double-click the user (for example, extension **101**).
3. Open the **Extension** tab.
4. Enable **Hot Desking**.
5. Save the changes.

#### User Login Procedure

1. From the hot desking phone, dial **\*70** followed by the user’s extension number
   * Example: **\*70101**
2. The PBX answers and prompts for the user’s **voicemail PIN**.
3. After successful authentication:
   * A confirmation prompt is played
   * The phone is re-provisioned with the user’s extension profile

The device now behaves exactly as the user’s own phone.

***

### Logging Out of a Hot Desking Phone

To log out from a hot desking phone:

1. Dial **\*71** from the phone.
2. The PBX confirms that the user has been logged out.
3. The phone is automatically re-provisioned back to the hot desking extension.





