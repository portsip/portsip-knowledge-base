# Zero Touch Provisioning Phones

### Zero Touch Provisioning (ZTP) Overview

**Zero Touch Provisioning (ZTP)** enables IP phones and devices to **self-configure out of the box**, eliminating the need for manual setup.

When IP phones are shipped directly from the factory, each device has a **unique MAC address**. **PortSIP PBX** uses this MAC address to associate the phone with its **configuration file URL**, which is stored on the **vendor’s Remote Provisioning Server (RPS)**.

Once the phone is plugged in and connected to the internet, it automatically:

* Retrieves the configuration URL from the vendor’s RPS server
* Connects to PortSIP PBX
* Downloads its configuration
* Registers to the PBX without any user interaction

***

### How Zero Touch Provisioning Works

#### Step 1: Order Placement

* The PBX service provider orders IP phones from the vendor or reseller.
* The seller ships the phones directly to end users (for example, via DHL or FedEx).
* The seller provides the **MAC addresses** of the phones to the service provider.

***

#### Step 2: Configuration Setup

* The service provider creates user extension accounts.
* Phone MAC addresses are associated with users by **importing a CSV file**.
* PortSIP PBX automatically writes each phone’s **configuration URL** to the vendor’s **RPS server**.

***

#### Step 3: Automatic Provisioning

* When users receive and connect their IP phones:
  * The phone contacts the vendor’s RPS server
  * Retrieves its configuration file URL
  * Downloads the configuration from PortSIP PBX
  * Registers automatically

With ZTP, large-scale IP phone deployments become **fast, reliable, and error-free**.

***

### Prerequisites

In this example, we demonstrate **Zero Touch Provisioning (ZTP)** using **five IP phones of the Fanvil X303 model**.

* Phones are shipped directly to end users
* The MAC addresses are already known

**MAC addresses used in this example:**

* `CC-5E-F8-41-B7-A1`
* `CC-5E-F8-41-B7-A2`
* `CC-5E-F8-41-B7-A3`
* `CC-5E-F8-41-B7-A4`
* `CC-5E-F8-41-B7-A5`

***

### Exporting a Template File

#### Create a User with Phone Provisioning Information

1. Sign in to the **PortSIP PBX Web Portal**.
2. Navigate to **Call Manager > Users** and click **Add**.
3. Enter:
   * Username
   * Password
   * Email
   * Extension number
   * Extension password
4. Open the **PHONE PROVISIONING** tab.
5. Click **Add Phone** and configure:
   * **Phone model:** Fanvil X303
   * **MAC address:** `CC-5E-F8-41-B7-A1`
6. Configure the required parameters:
   * **Network interface**
     * If the PBX is internet-facing, choose **Web Domain** or **Public IPv4**
     * If an SBC is deployed, choose the **SBC base domain or IP**
   * **Transport protocol** used for SIP registration
7. Ensure **Save to RPS** is enabled.
8. Click **OK** to save the configuration.

***

#### Export the User as a Template File

1. Go to **Call Manager > Users**.
2. Click **Export**.
3. Download the generated **CSV file**, which contains user and phone provisioning data.

***

### Editing the Template File

#### Open the CSV File

* Locate and open the exported CSV file.
* Identify the row corresponding to the user you created.

***

#### Modify Details for Additional Users

For the **second user**, update the following fields:

* Username
* Password
* Extension number
* Extension password
* IP phone MAC address (for example, `CC-5E-F8-41-B7-A2`)

***

#### Add Remaining Users

1. Copy the existing row.
2. Paste it to create rows for the remaining users.
3. Update the following fields for each new user:
   * Username
   * Password
   * Extension number
   * Extension password
   * IP phone MAC address

> ❗ **Important**\
> Do **not** include the user and MAC address `CC-5E-F8-41-B7-A1`, as it is already configured in the PBX.

***

#### Save the CSV File

* After completing all changes, save the CSV file.
* The file is now ready for import.

<figure><img src="../../../.gitbook/assets/zoro-touch-user.png" alt=""><figcaption><p>Copy users and change the name, password, extension number, extension password, and phone MAC address</p></figcaption></figure>

> ❗ **Security Recommendation**\
> Although the same password and extension password can be reused, this is **not recommended**.\
> Use **unique credentials** for each user to reduce security risks.

***

### Importing Users with Phone Information

#### Import Users

1. Navigate to **Call Manager > Users**.
2. Click **Import**.
3. Upload the edited CSV file to import users and their phone provisioning data.

***

### Configuration URL Registration

After a successful import:

* PortSIP PBX automatically writes each IP phone’s **configuration URL** to the vendor’s **RPS server**
* No manual interaction with the RPS platform is required

***

### Automatic Phone Provisioning

Once users receive their IP phones and connect them to the internet:

* Phones contact the vendor’s RPS server
* Retrieve their configuration file URLs
* Download the configuration from PortSIP PBX
* Register automatically to the system

This seamless process ensures **fast, consistent, and large-scale deployments** with minimal operational effort.

***

### Additional Reference

For more information, please also refer to: [Bulk Importing Users and Auto Provisioning IP Phones](bulk-importing-users-and-auto-provisioning-ip-phones.md).



