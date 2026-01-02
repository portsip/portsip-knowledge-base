# Configuring SIPTRUNK IP Authentication Trunk

Before proceeding with trunk configuration, ensure that you have purchased at least one DID on the [SIPTRUNK ](https://www.siptrunk.com)platform.

***

### SIPTRUNK Setup Guide

To enable inbound calling, you must first complete the required configuration on the SIPTRUNK platform.

1. Sign in to your [SIPTRUNK ](https://login.siptrunk.com/)account.
2.  &#x20;Your SIP trunk will also need to be configured via the customer portal. Refer to this article on [How to Set Up IP Auth](https://support.siptrunk.com/hc/en-us/articles/38765554523547) for complete details.&#x20;

    &#x20;

    **IMPORTANT**: You must implement the following whitelisting requirements detailed in the article [Interconnecting with SIPTRUNK](https://support.siptrunk.com/hc/en-us/articles/38762945316379).

    &#x20;

Once completed, continue with the trunk configuration in PortSIP PBX.

***

### Configure a SIPTRUNK IP Authentication Trunk in PortSIP PBX

You can configure SIPTRUNK as an **IP-Based Trunk** at the **system administrator** level. After configuration, this trunk can be shared with one or more tenants.

***

#### Step 1: Create the IP-Based Trunk

1. Sign in to the PortSIP PBX Web Portal as a System Administrator.
2. Navigate to **Call Manager > Trunks**.
3. Click **Add**, then select **IP Based Trunk**.

<figure><img src="../../../.gitbook/assets/add-ip-trunk.png" alt=""><figcaption></figcaption></figure>

***

#### Step 2: Enter Trunk Details

Configure the trunk with the following settings:

* **Name**: Enter a friendly name for the trunk (for example, `SIPTRUNK Trunk`)
* **Brand**: Select **SIPTRUNK**

Click **Next** to continue.

<figure><img src="../../../.gitbook/assets/SIPTRUNK.com_1.png" alt=""><figcaption></figcaption></figure>

***

#### Step 3: Configure Trunk Options

* **Max Concurrent Calls**\
  Set the maximum number of concurrent calls allowed on this trunk. Adjust this value based on your SIPTRUNK subscription and expected traffic.

> ❗**Recommendation**\
> Keep the default values for other options unless you have specific requirements.

Click **Next** to continue.

<figure><img src="../../../.gitbook/assets/ip-trunk-options.png" alt=""><figcaption></figcaption></figure>

***

#### Step 4: Assign the Trunk to Tenants and Configure the DID Pool

1. Assign the trunk to one or more **tenants**.
2. Configure the **DID Pool**(DID numbers) using the SIPTRUNK DIDs you purchased.

> ❗**Important**
>
> A DID number can be assigned to **only one tenant**.

A tenant assigned to this trunk can:

* Use **only** the DID numbers defined in the **DID Pool**
* Create **inbound and outbound routing rules** based on the assigned DID numbers
* Configure **outbound caller IDs** for extensions using the assigned DID numbers

***

**DID Pool Format Examples**

The DID Pool can include a single number, multiple numbers, or number ranges, separated by semicolons.

For example:

```
16468097065
16468097065;16468097066
16468097065-16468097066;16468097069
16468097065-16468097066;16468097070-16468097080
```

***

#### Step 5: Save the Configuration

1. Click **OK** to save the trunk configuration.

<figure><img src="../../../.gitbook/assets/SIPTRUNK.com_2.png" alt=""><figcaption></figcaption></figure>

***

### Expected Result

* The [SIPTURNK ](https://www.siptrunk.com)P-based trunk is successfully created.
* The trunk status displays **Registered**\
  &#xNAN;_(IP-Based Trunks always show “Registered” in PortSIP PBX)_.
* Assigned tenants can immediately use the configured DIDs for inbound and outbound calls.

***

> ❗**Note**\
> Ensure that SIPTRUNK allows traffic from your PBX IP address when using IP authentication.

Now you can follow the article on [Configuring inbound and outbound calls](configuring-outbound-and-inbound-calls.md).

<br>
