# Purchase a DID on Bandwidth Platform

Before placing or receiving calls, you must have **at least one active DID** (or multiple DIDs) on your Bandwidth account.

> **Note**\
> If you already have active phone numbers on your Bandwidth account, you can skip the DID purchase steps and proceed directly to trunk configuration.

***

### Create and Configure Your Bandwidth Account

#### Step 1: Create Your Bandwidth Account

* Sign in to the **Bandwidth Portal** using the credentials provided during the sign-up process.
* If you do not yet have an account, contact Bandwidth to request account creation.

***

#### Step 2: Create a Sub-Account and Location

1. After signing in, click **View Account**.
2. From the top menu, select **Sub-Accounts**.
3. Click **Create Sub-Account** and complete the required information.
4. From the top menu, select **Locations**.
5. Create a **new location** for the sub-account.

> **Best Practice**\
> Bandwidth uses **sub-accounts and locations** to control number assignment and routing. Each SIP trunk should be associated with the correct location.

***

#### Step 3: Configure SIP Settings

1. Navigate to the **Voice** menu.
2. Scroll to the **Origination Settings** section.
3. In **Voice IP Addresses / DNS Hosts**, enter:
   * Your **static public IP address**
   * The **SIP port** used by your PortSIP PBX
4. Contact your Bandwidth account administrator to add your public IP address to the **Termination Settings**.

> **Note**\
> You can continue with other configuration steps while waiting for Bandwidth to complete the termination IP update.

***

#### Step 4: Configure and Purchase Phone Numbers (DIDs)

1. Click **Numbers** in the top menu.
2. Select the desired **number type** (for example, local or toll-free).
3. During checkout:
   * Assign the phone numbers to the correct **sub-account**
   * Assign them to the appropriate **location**

**Expected Result**

* The purchased DIDs are active and associated with the correct sub-account and location.

***

#### Step 5: Finalize Setup

* Once your SIP trunk configuration is approved, Bandwidth will provide a pair of IP addresses for trunk connectivity.
* Record these IP addresses, you will need them when configuring the SIP trunk in PortSIP PBX.

***

### Next Step

After purchasing and preparing your DIDs, proceed to configure the SIP trunk in PortSIP PBX:

* [Configuring Bandwidth IP Authentication Trunk](configuring-bandwidth-ip-authentication-trunk.md)







