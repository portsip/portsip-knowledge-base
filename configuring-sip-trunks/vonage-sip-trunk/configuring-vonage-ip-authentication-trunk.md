# Configuring Vonage IP Authentication Trunk

Before proceeding with the next steps, you need to [purchase a DID on the Vonage platform](purchase-a-did-on-vonage-platform.md).

## Create a SIP Trunk on the Vonage platform

To create a new SIP trunk on the Vonage platform:

1. Navigate to the menu **Build & Manage > SIP**.&#x20;
2. By default, Vonage has created a SIP trunk for you. You can click the settings icon to adjust its settings or click **Create New** to set up a new SIP trunk. In this guide, we will demonstrate how to create a new trunk.
3. Click **Create New** and enter a domain for your trunk, such as “portsip,” and click **Create**.

<figure><img src="../../.gitbook/assets/vonage-fig10.png" alt=""><figcaption></figcaption></figure>

4. Once the SIP trunk is successfully created, you will automatically be directed to the trunk details page.

<figure><img src="../../.gitbook/assets/vonage-fig11.png" alt=""><figcaption></figcaption></figure>

5. Click **Add Authentication** under the **Outbound Calling** section you will be redirected to the **Authentication** page, under **Access Control List(ACL),** input your PortSIP PBX static public IP Address here for the I**P Address** field, and enter 32 for the **Range** field.&#x20;

<figure><img src="../../.gitbook/assets/vonage-fig23.png" alt=""><figcaption></figcaption></figure>

6. After adding the IP Address, the **Access Control List(ACL)** section will display as enabled in green, indicating that the IP authentication is enabled.&#x20;

<figure><img src="../../.gitbook/assets/vonage-fig24.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
If you have added the Access Control List (ACL) for authentication, you must not add the User Key and Secret, as they cannot be enabled simultaneously.
{% endhint %}

7. Click **Back**. On the trunk details page, you will see that Outbound Calling is displayed as ready. Please copy and note the Vonage SIP trunk's domain, in this case, they are below, you can pick up one near your PortSIP PBX location:
   * portsip.sip-us.vonage.com
   * portsip.sip-eu.vonage.com
   * portsip.sip-ap.vonage.com

<figure><img src="../../.gitbook/assets/vonage-fig25.png" alt=""><figcaption></figcaption></figure>

8. Under the **Inbound Calling** section, add the URI so that the Vonage SIP trunk knows how to route inbound calls to your PortSIP PBX. Simply input the priority(0-100), such as 1, and enter the URI, which is your PortSIP PBX IP address or domain (e.g., 44.242.60.185 or pbx.portsip.com).

<figure><img src="../../.gitbook/assets/vonage-fig15.png" alt=""><figcaption></figcaption></figure>

9. Click the **+** button to add the URI. Once added, the URI will be successfully displayed.

<figure><img src="../../.gitbook/assets/vonage-fig16.png" alt=""><figcaption></figcaption></figure>

10. Add one or multiple numbers to this SIP Trunk by clicking the **Link all...** or **Link** button Link numbers section. If there are no numbers, you can buy them, and it will redirect you to the buy number portal. In the numbers dashboard, you can easily filter, link, and unlink multiple numbers.

<figure><img src="../../.gitbook/assets/vonage-fig17.png" alt=""><figcaption></figcaption></figure>

11. Once the number is successfully linked, Vonage will display the information indicating that you are ready to receive calls.

<figure><img src="../../.gitbook/assets/vonage-fig18.png" alt=""><figcaption></figcaption></figure>

## Configure Register Based Trunk in PortSIP PBX

The **IP Registration** trunk refers to the **IP Based Trunk** in PortSIP PBX. You can configure the Register Based Trunk at either the PortSIP PBX **system administrator level** or the **Tenant Admin level**:

* If configured at the system administrator level, you can share this trunk with tenants.
* If configured at the tenant admin level, this trunk can only be used by the tenant itself.

Please follow the below steps:

1. Sign in to the PortSIP PBX Web Portal as a System Administrator or Tenant Admin. Navigate to the left menu and select **Call Manager > Trunks**.&#x20;
2. Click the **Add** button to open a menu. From the menu, choose **IP Based Trunk**.

<figure><img src="../../.gitbook/assets/add-ip-trunk.png" alt=""><figcaption></figcaption></figure>

3. Enter the trunk name and choose the brand:
   * **Name**: Enter a friendly name for the trunk.
   * **Brand**: Select **Vonage** for this field.
   * **Hostname or IP Address**: Paste the Vonage SIP trunk domain that you copied in previous steps

<figure><img src="../../.gitbook/assets/vonage-fig26.png" alt=""><figcaption></figcaption></figure>

4. Click the **Next** button, you can adjust the options for the trunk.
   * &#x20;**Max Concurrent Calls:** This field sets the maximum number of calls that PortSIP can establish with this trunk. You can adjust it to an appropriate value.
   * We recommend keeping the default settings for other options unless you have specific requirements.

<figure><img src="../../.gitbook/assets/registration-trunk-options.png" alt=""><figcaption></figcaption></figure>

5. This step is only available when configuring the Register-Based Trunk at the _**System Administrator Level**_. Click the **Next** button to assign this trunk to the tenants and provide your Vonage DIDs/Numbers to them with the DID Pool (DID numbers).  A DID can be only assigned to one tenant.
   * A tenant assigned to this trunk can only use the DID numbers within the DID pool range to create inbound and outbound rules and configure the outbound caller ID for extensions.
   * DID Pool: The DID pool can consist of a single number, a range of numbers, or a combination of both. For example:
     * `12057494879`
     * `12057494879-12057494880`
     * `12057494879-12057494880;12057494885`&#x20;
     * `12057494879-12057494880;12057494890-12057494899`

<figure><img src="../../.gitbook/assets/vonage-fig21.png" alt=""><figcaption></figcaption></figure>

Click the **OK** button to save the changes, the trunk configuration is completed.

In the trunk list, you will see the status displayed as **Registered**. (For IP-based trunks, it always displays as **Registered**.)

<figure><img src="../../.gitbook/assets/vonage-fig22.png" alt=""><figcaption></figcaption></figure>

Now you can follow the article to [Configuring Outbound & Inbound Calls](configuring-outbound-and-inbound-calls.md).

