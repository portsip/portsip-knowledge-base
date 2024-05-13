# Configuring SIP Trunk

VoIP service providers host phone lines and replace traditional telco lines to provide the trunk service. Providers can assign local numbers in one or more cities or countries and route these to you. In most cases, they also support number porting.

Service providers are able to offer better call rates because they may have an international network or have negotiated better rates. Therefore, using VoIP providers can reduce call costs.

PortSIP PBX supports these trunk types.

* **Register Based Trunk**: this trunk requires the PBX to register with the trunk by using an authentication ID and password. **System Admin** can configure a **Register Based** trunk and assign it to tenants, as well as assign a DID Pool to each tenant. The tenants share this trunk but have different DID number pools. **Tenant Admin** can also configure a **Register Based** trunk, however, this trunk cannot be shared with other tenants, and its hostname and authentication ID cannot be the same as those of other trunks.
* **Accept Register Trunk**: The trunk registers to PBX with the predefined authentication ID and password. **System Admin** can configure an **Accept Register** trunk and assign it to tenants, as well as assign a DID Pool to each tenant. **Tenant Admin** can also configure an **Accept Register** trunk, however, this trunk cannot be shared with other tenants, and its hostname and authentication ID cannot be the same as those of other trunks.
* **IP Based Trunk**: IP Based trunk does not generally require the PBX to register with the trunk. The IP address of the PBX needs to be configured on the trunk side so that it knows where calls to your number should be routed. The **IP based** trunk can only be configured by **System Admin**, and an IP-based trunk can only be added once. If the **System Admin** wishes to allow more than one tenant to use this trunk, the **System Admin** can assign this trunk to the tenants and assign each tenant a DID Pool.
* **Microsoft Teams**: Set up Microsoft Teams Direct Routing as a trunk in the PortSIP PBX. This Teams trunk can only be configured by the **Tenant Admin** and cannot be shared with other tenants.

The **Tenant Admin** has the ability to view all trunks that the **System Admin** allocated to him and create the inbound/outbound rule on its basis, but he cannot change such trunks.

## Configuring Trunk

First, you need to have an account with a VoIP service provider. PortSIP PBX supports most of the popular SIP-based VoIP service providers.

After you get the trunk account from the VoIP service provider, you will need to configure the account in PortSIP PBX.

<figure><img src="../../.gitbook/assets/turnk_1.png" alt=""><figcaption></figcaption></figure>

### **DID Pool**

Since the PortSIP PBX is a multi-tenant PBX, if more than one tenant set up the trunk from the same trunk provider in the PBX, and sets the same DID number for the inbound rule, when a call is arriving at the PBX, the PBX does not know which tenant should route the call to; and when an extension of a tenant makes the call to the trunk, set the outbound caller ID which belongs to another tenant, will cause problems there.

To avoid the problems above, PortSIP PBX introduced the DID pool concept.

Once a tenant was assigned with the trunk by the **System Admin**, the **System Admin** must set up a DID pool for that tenant, the DID pool number cannot overlap with other tenants' DID pools. When a tenant creates the inbound rule based on this assigned trunk, the tenant can only use the DID number from the DID pool.

If a **Tenant Admin** adds a trunk for himself, a DID pool must be specified for that trunk. When creating the inbound rule for this tenant based on this trunk after it was added, the DID number used must be in this DID pool range. The DID pool number cannot overlap with the same trunk provider.

Tenant A, for example, adds a trunk of the **trunk provider XYZ** and sets the DID Pool to 1000-2000; Tenant B also adds a trunk of the same **trunk provider XYZ** and sets the DID Pool to 2000-3000; This will fail because the DID Pool number of the same trunk is overlapping; when a call to number 2000 is incoming, the PBX does not know which tenant should route the call to.

The DID pool allows a single number or a number range as the below.

* 1000-2000
* 282556000-282556900
* 101;203;300-450

{% hint style="info" %}
The DID number and DID number range cannot begin with "**+"**, "**0"**, or "**00"** when setting into the DID Pool for a tenant; if your DID number or DID number range begins with "**+"**, "**0"**, or "**00"**, please remove it before entering.
{% endhint %}

### **Add the Trunk by System Admin**

1. Select **Call Manager > Trunks** menu, and click the arrow button to choose the trunk type that you will need to add.
2. Enter a friendly name for this trunk, and fill in the **Host Domain or IP**, **Port**, **Outbound Proxy Server**, and **Outbound Proxy Server port** fields as the details that you received from your trunk service provider.
3. Transport. The transport used for the PBX communicates with the trunk, you should consult your trunk provider and choose the appropriate transport, that currently supports UDP, TCP, and TLS. The transport must be added in PBX before adding the trunk. For example, if your provider requires the TCP, you should add the TCP transport in PBX, please refer to the section [Transport Management](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/6-transport-management).
4. Associated IPs of the trunk. For some trunk providers, it may send the INVITE message to PBX from multiple IPs rather than from the **Host Domain or IP** only. You need to click the **Add** button and enter each IP here to add the associated IP.

If the trunk type is **Register based**, click the **Next** button to fill in the username/Authentication name, password, and register time as the account details that you get from the trunk provider.

Click the **Next** button to set up more parameters.

1. Use the private IP address to communicate with this trunk: Enable this option if the PBX uses the private IP address for the trunk connection, otherwise, disable it then the PBX will use the public IP address to connect this trunk.
2. Rewrite the host IP of **Via** header by the PBX server public IP when sending the request to the trunk: if this option is enabled and the PBX has a public IP, the PBX will change the host IP of the **Via** header by the PBX public IP when sending SIP message to the trunk. Unless the trunk provider is required, keep this option as the default setting.
3. Verify the port when receiving SIP messages from the trunk: when the PBX receives a SIP message from the trunk and tries to recognize the trunk by matching the IP and port. The port will be ignored if this option is turned off.  This value was suggested leaving it as the default.
4. This trunk only accepts a single Via SIP header: if this option is turned on, the PBX will just keep a single Via header when sending a SIP message to the trunk.
5. Send OPTIONS message for keep alive: when enabled, the PBX sends keep-alive messages (SIP OPTIONS) to the trunk to determine its connectivity status (offline or online). The PBX marks this trunk offline if not receive the 200 OK of the OPTIONS.
6. Send OPTIONS message interval(seconds): how often messages are sent and when a destination is considered unavailable. The default is 360 seconds.

Click the **Next** button to set up more parameters.

Since the trunk is added by the **System Admin**, the **System Admin** will need to choose one or more tenants to allow them access to this trunk.

Once a tenant is assigned the trunk, must set up the DID pool for the tenant, the DID pool number cannot overlap if assigned a trunk for multiple tenants.

When the tenant creates the inbound rule based on this assigned trunk, the tenant can only use the DID number from the DID pool.

For more details please refer to [DID Pool](configuring-sip-trunk.md#did-pool).

<figure><img src="../../.gitbook/assets/trunk_did_pool.png" alt=""><figcaption></figcaption></figure>

### **Add the Trunk by Tenant Admin**

When a **Tenant Admin** logs into the Web Portal, he has the ability to create trunks for this tenant. The **Tenant Admin** can only add the trunk types listed below.

* Register Based: PBX registers to the trunk
* Accept Register: The trunk register to PBX
* Microsoft Teams: The Microsoft Teams Direct Routing

The **IP Based** trunk can only be added by the **System Admin**.

1. Select **Call Manager > Trunks** menu, and click the arrow button to choose the trunk type that you will need to add.
2. Enter a friendly name for this trunk.
3. DID Pool: A DID pool must be specified for the tenant. When creating the inbound rule for this tenant based on this trunk, the DID number used in the inbound rule must be in the DID pool range. For more details, please refer to [DID Pool](configuring-sip-trunk.md#did-pool).
4. Fill in the **Host Domain or IP**, **Port**, **Outbound Proxy Server**, and **Outbound Proxy Server port** fields as the details you received from the trunk service provider.
5. Transport. The transport used for the PBX communicates with the trunk, you should consult your trunk provider and choose the appropriate transport, that currently supports UDP, TCP, and TLS. The transport must be added in PBX before adding the trunk. For example, if your provider requires the TCP, you should add the TCP transport in PBX, please refer to the section [Transport Management](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/6-transport-management).
6. Associated IPs of the trunk. For some trunk providers, it may send the INVITE message to PBX from multiple IPs rather than from the **Host Domain or IP** only. You need to click the **Add** button and enter each IP here to add the associated IP.

If the trunk type is  **Register Based**, click the **Next** button to fill in the username/Authentication name, password, and Reregister time as the account details that you get from the trunk provider.

Click the **Next** button to set up more parameters.

1. Rewrite the host IP of **Via** header by the PBX server public IP when sending the request to the trunk: if this option is enabled and the PBX has a public IP, the PBX will change the host IP of the Via header by the PBX public IP when sending SIP message to the trunk. Unless the trunk provider is required, keep this option as the default setting.&#x20;
2. Verify the port when receiving SIP messages from the trunk: when the PBX receives a SIP message from the trunk and tries to recognize the trunk by matching the IP and port. The port will be ignored if this option is turned off. This value was suggested leaving it as the default.&#x20;
3. The trunk is located on the same LAN as PBX: if this trunk is not located on the public internet, please turn this option on.
4. This trunk only accepts a single Via SIP header: if this option is turned on, the PBX will just keep a single Via header when sending a SIP message to the trunk.
5. Send OPTIONS message for keep alive: when enabled, the PBX sends keep-alive messages (SIP OPTIONS) to the trunk to determine its connectivity status (offline or online). The PBX marks this trunk offline if not receive the 200 OK of the OPTIONS.&#x20;
6. Send OPTIONS message interval(seconds): how often messages are sent and when a destination is considered unavailable. The default is 360 seconds

### **Configure E1/T1 Gateway Register to PortSIP PBX**

Consider we deployed the PortSIP PBX on a cloud platform such as AWS, AZURE, GCE, and wish to configure the E1/T1 gateway which is located in local LAN as a trunk for the PortSIP PBX, but the E1/T1 without static public IP, we can't configure the **Authentication mode** to **IP Based** and **Register Based.**

For this scenario, we can configure that E1/T1 is registering to the cloud PortSIP PBX from the local LAN, and then the E1/T1 can act as the Trunk works with PortSIP PBX for make & receive calls.

Please follow the below steps to config the E1/T1 register to the cloud PortSIP PBX.

1. Select **Call Manager > Trunks**, click the arrow button, and choose **Accept Register**.
2. Enter a friendly name for this trunk.
3. DID Pool: A DID pool must be specified for the trunk. When creating the inbound rule for this tenant based on this trunk after it was added, the DID number used must be in this DID pool range. For more details please refer to DID Pool&#x20;
4. Enter a domain for the **Host Domain or IP**, this domain doesn't require an existing domain, you can enter any domain here, for example, **portspitrunk1.io**.&#x20;

{% hint style="danger" %}
Ensure this domain does not equal any tenant's SIP domain.
{% endhint %}

Click the **Next** button to set up more parameters.

1. For the **Authorization Name**, you can enter any number here, for example, "**123456**", the E1/T1 gateway will use this for the authorization when it registers to PortSIP PBX.
2. For the password, you can enter any password here, the E1/T1 gateway will use this for the authorization when it registers to PortSIP PBX.
3. Other settings are the same as in the previous section for configuring the **IP Based** and **Register Based** Trunk.
4. After successfully adding the trunk, now you can configure the E1/T1 gateway to let it register to the cloud PortSIP PBX.&#x20;
5. In the E1/T1 settings, set up the trunk **Host Domain or IP** as **SIP Domain/SIP Server**, in the case is "**portsiptrunk1.io**"; set up the cloud PBX public static IP as **Outbound Proxy Server**, set up the PortSIP PBX transport port as the **Outbound Proxy Server port**, set up the trunk **Authorization name** and **Password** as the **username/auth ID/auth name** and **password**, then the E1/T1 gateway can register to cloud PortSIP PBX.

## Outbound Parameters and Inbound Parameters

After completing the setup for the trunk, you could also go to **Call Manager > Trunks** select a trunk, and click the **Edit** button to change the Inbound/Outbound Parameters for the trunk.

* On the **Outbound Parameters** page, you could set some rules to make changes for headers of INVITE messages to be sent to the trunk. For example, the **user** part of the **From** SIP header could be set to the **Outbound Caller ID** of the extension who made the call. You can set up the **Outbound caller ID** of the extension on the **General** page of the user settings, see section [5.2 General](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/5-extension-management#5.2-general).
* On the **Inbound Parameters** page,  the user could set rules to make changes to the field values of SIP messages for incoming calls from the trunk.

For more details, please read this topic: [Handle Outbound Calls Through SIP Trunk](handle-outbound-calls-through-sip-trunk.md).

{% hint style="danger" %}
Both inbound and outbound parameters are advanced options. It’s recommended to use default values.
{% endhint %}

## Deleting Trunk

The trunk cannot be deleted if there are still inbound or outbound rules based on it.

* If you want to delete a trunk, you must change the trunk in the inbound and outbound rules to another trunk.
* Before deleting a trunk, simply delete the inbound and outbound rules that were created based on that trunk.

