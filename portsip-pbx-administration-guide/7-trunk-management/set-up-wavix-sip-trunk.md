# Set Up Wavix SIP Trunk

{% hint style="info" %}
This guide is only applicable for the PortSIP PBX v22 or higher.
{% endhint %}

Wavix is a SIP trunk provider, it provides a robust set of calling, SMS/MMS features, and reliability that enhances your communications.

One of the primary advantages of trunking with Wavix is its greatly expanded geographic reach. Wavix provides connectivity across 170+ countries, allowing you to make and receive calls around the world. Your callers and customers worldwide can connect with your business at local rates.

PortSIP PBX integrated the Wavix SIP trunking that allows you to set up easily.

## Purchase a DID on the Wavix platform

Before placing or receiving a call, you need to have an active DID or a dozen DID numbers on your Wavix account. If you already have an active number on your Wavix account, the below steps are optional.

To purchase a DID on your Wavix account:

1. Log in to your account
2. Click on **Buy** under **Numbers & trunks** in the top menu
3. Select a country and region you wish to purchase a DID in
4. Choose a specific number or numbers and click the **Buy Now** button
5. You will be redirected to the Cart where you can confirm your choice and check out the DID(s).

<figure><img src="../../.gitbook/assets/wavix-fig1.png" alt=""><figcaption><p>Search and buy a number</p></figcaption></figure>

{% hint style="warning" %}
Some DIDs may require proof of local address and other documents before they can be activated. To enable these DIDs to receive inbound calls, upload the documents required, and wait until they are approved by the Wavix Number Provisioning team.
{% endhint %}

## Create a SIP Trunk on the Wavix platform

To create a new SIP trunk on the Wavix platform

1. Select **Trunks** under **Numbers & trunks** in the top menu
2. Click the **Create new** button
3. Select **Digest** or **IP Authentication** under the **Authentication method**

### Digest

If selected **Digest**, specify the SIP trunk name, set SIP trunk password, and select one of the DIDs on your account as Caller ID.

<figure><img src="../../.gitbook/assets/wavix-fig2.png" alt=""><figcaption><p>Configure SIP trunk</p></figcaption></figure>

### IP Authentication

If select **IP Authentication**, please follow the below steps:

1. Enter your PortSIP PBX static IP and click **Submit**
2. Click **Save** to apply changes

<figure><img src="../../.gitbook/assets/wavix-fig12.png" alt=""><figcaption><p>Wavix SIP trunk IP authentication</p></figcaption></figure>

Once the Wavix ops team approves your request, IP authentication will be activated on your Wavix SIP trunk.

{% hint style="info" %}
After submitting the IP authentication request, you’ll not be able to update your IP address or change the authentication method.
{% endhint %}

{% hint style="info" %}
By default, an IP address can only be mapped to a single SIP trunk. If you need to have several Wavix SIP trunks sharing the same IP address, please get in touch with support@wavix.com
{% endhint %}

After the SIP trunk is successfully created, it will appear on the list of SIP trunks on your account.

<figure><img src="../../.gitbook/assets/wavix-fig3.png" alt=""><figcaption><p>List of SIP trunks and SIP trunk ID</p></figcaption></figure>

{% hint style="danger" %}
Please be advised that your 5-digit SIP trunk username is generated automatically and displayed in the SIP trunk ID column.
{% endhint %}

## Configure SIP trunk in PortSIP PBX

To configure inbound and outbound calls on your PortSIP PBX, log in to the PortSIP PBX Web Portal, go to the left menu **Call Manager > Trunks, and** click the **Add** button, in the popups menu, if you select the **Authentication method** is **Digest** when you [create the Wavix trunk](set-up-wavix-sip-trunk.md#digest)**,** please choose **Register Based Trunk**; For the IP Authentication, choose **IP Based Trunk**.&#x20;

<figure><img src="../../.gitbook/assets/wavix-fig13.png" alt=""><figcaption><p>Configure Wavix Trunk in PortSIP PBX</p></figcaption></figure>

{% hint style="info" %}
If your Wavix trunk Authentication method is IP Authentication, you must configure it at the PortSIP PBX system Administrator level, then assign it to the tenants, and specify the DID numbers to the tenant.

If your Wavix trunk Authentication method is Digest, you can configure it at the PortSIP PBX System Administrator level or a tenant level.&#x20;
{% endhint %}









