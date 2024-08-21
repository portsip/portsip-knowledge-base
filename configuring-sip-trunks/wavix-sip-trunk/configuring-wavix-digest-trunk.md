# Configuring Wavix Digest Trunk

Before proceeding with the next steps, you need to [purchase a DID on the Wavix platform](purchase-a-did-on-wavix-platform.md).

## Create a SIP Trunk on the Wavix platform

To create a new SIP trunk on the Wavix platform

1. Select **Trunks** under **Numbers & trunks** in the top menu
2. Click the **Create new** button
3. Select **Digest** under the **Authentication method**
4. Specify the SIP trunk name, set SIP trunk password, and select one of the DIDs on your account as Caller ID.

<figure><img src="../../.gitbook/assets/fig2.png" alt="" width="563"><figcaption></figcaption></figure>

5. Optionally you can set max outbound call duration, max number of simultaneous calls via the SIPtrunk, and max call cost. If these parameters are not set, global account limits apply.
6. Click **Create**

After the SIP trunk is successfully created, it will appear on the list of SIP trunks on your account.

<figure><img src="../../.gitbook/assets/wavix-fig3.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
Please be advised that your 5-digit SIP trunk username is generated automatically and displayed in the SIP trunk ID column.
{% endhint %}

## Configure Digest Trunk in PortSIP PBX

The Digest Wavix trunk corresponds to the **Register-Based Trunk** in PortSIP PBX. You can configure the **Register-Based Trunk** at either the PortSIP PBX system administrator level or the Tenant Admin level:

* If configured at the system administrator level, you can share this trunk with tenants.
* If configured at the tenant admin level, this trunk can only be used by the tenant itself

Please follow the below steps:

1. Sign in to the PortSIP PBX Web Portal as a System Administrator or Tenant Admin. Navigate to the left menu and select **Call Manager > Trunks**.&#x20;
2. Click the **Add** button to open a menu. From the menu, choose **Register-Based Trunk**.

<figure><img src="../../.gitbook/assets/wavix-fig13.png" alt="" width="563"><figcaption><p>Configure Wavix Trunk in PortSIP PBX</p></figcaption></figure>

3. Enter the trunk name and choose the brand:
   * **Name**: Enter a friendly name for the trunk.
   * **Brand**: Select one of the Wavix trunks from the Brand field, such as **Wavix - US**.

<figure><img src="../../.gitbook/assets/wavix-fig14.png" alt=""><figcaption></figcaption></figure>

4. This step only for if you at the **tenant admin level** to configure this **Register-Based Trunk**, you will need to set up the DID pool for this trunk. The DID pool can consist of a single number, a range of numbers, or a combination of both. For example:
   * `16468097065`
   * `16468097065;16468097066`
   * `16468097065-16468097066`
   * `16468097065-16468097066;16468097069`&#x20;
   * `16468097065-16468097066;16468097070-16468097080`
5. Click the **Next** button, provide **SIP trunk ID** and **password** set up for the SIP  trunk on the Wavix platform.&#x20;

<figure><img src="../../.gitbook/assets/wavix-fig18.png" alt=""><figcaption></figcaption></figure>

6. Click the **Next** button, you can adjust the options for the trunk, we recommend using the default settings except for the **Max Concurrent Calls,** this field is limited to the maximum calls that PortSIP established with this trunk, you can change it to an appropriate value.

<figure><img src="../../.gitbook/assets/wavix-fig16.png" alt=""><figcaption></figcaption></figure>

5. This step is only available when configuring the **Register-Based Trunk** at the System Administrator level. Click the **Next** button to assign this trunk to the tenants and provide them with the DID Pool (DID numbers).&#x20;

A tenant assigned to this trunk can only use the DID numbers within the DID pool range to create inbound and outbound rules, and configure the outbound caller ID for extensions.

<figure><img src="../../.gitbook/assets/wavix-fig17.png" alt=""><figcaption></figcaption></figure>

* DID Pool: The DID pool can consist of a single number, a range of numbers, or a combination of both. For example:
  * `16468097065`
  * `16468097065;16468097066`
  * `16468097065-16468097066`
  * `16468097065-16468097066;16468097069`&#x20;
  * `16468097065-16468097066;16468097070-16468097080`

Click the **OK** button to save the changes, the trunk configuration is completed.

Once the PortSIP PBX successfully registered this trunk to Wavix platform, in the trunk list you will see the status displayed as **Regisetered**.

<figure><img src="../../.gitbook/assets/wavix-fig19.png" alt=""><figcaption></figcaption></figure>

Now you can follow the article to [configuring inbound and outbound calls](configuring-inbound-and-outbound-calls.md).
