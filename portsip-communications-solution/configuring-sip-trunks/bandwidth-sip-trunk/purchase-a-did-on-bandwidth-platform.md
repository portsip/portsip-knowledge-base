# Purchase a DID on Bandwidth Platform

Before placing or receiving a call, you need to have an active DID or a dozen DID numbers on your VoIP Bandwidth account. If you already have an active number on your Bandwidth account, the below steps are optional.

## Creating Your Bandwidth Account

### **1. Create Your Bandwidth Account**

**Sign in** to your Bandwidth portal using the credentials provided during the sign-up process. If you don't have an account, you'll need to contact Bandwidth to create one.

### **2. Set Up Sub-Account and Location**

* After signing in, click on **“View Account”** and select **“Sub-Accounts”** from the top menu.
* Click **“Create Sub-Account”** and complete the required information to set up your sub-account.
* Navigate to **“Locations”** from the top menu to create a new location for your sub-account.

### **3. Configure SIP Settings**

* Under the **“Voice”** menu, scroll down to the **“Origination Settings”** section.
* In the **“Voice IP Addresses / DNS Hosts”** field, enter your static public IP address and SIP port.
* Contact your account administrator to add your public IP address to the **“Termination Settings”**. You can proceed with other configurations while waiting for this update.

### **4. Configure Phone Numbers**

* Click on **“Numbers”** in the top menu, and select the desired type of numbers based on your preferences.
* During the checkout process, make sure to assign the phone numbers to your sub-account and location.

### **5. Finalizing Setup**

Once your SIP trunk is ready, Bandwidth will provide you with a **pair of IP addresses**. Make sure to note them down for future reference.

After purchasing the DID, you can follow one of the guides to configure the trunk with PortSIP PBX.

* [Configuring Bandwidth IP Authentication Trunk](configuring-bandwidth-ip-authentication-trunk.md)

