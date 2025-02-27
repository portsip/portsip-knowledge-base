# Troubleshooting Call Issues

This article provides solutions for common PortSIP PBX issues with calls.

## 1. Troubleshooting Outbound Calls Issues

When you make a call from the app or an IP Phone, if the call fails, you will receive a status code indicating the reason. Below, we explain the status codes and their meanings:

### Check if the Error Originates from the PBX or the Trunk

When an outbound call fails, you can determine whether the error occurs in the PBX or is returned by the trunk by following these steps:

First, navigate to **Call Statistics > CDR**.

* If no record of your call appears, it means the PBX did not receive the call. In this case, check if your app or IP phone can reach the PortSIP PBX.
* If a record for the call exists, double-click it:
  * If no pop-up window appears, it indicates the call failed within the PBX (e.g., no outbound rule matched).
  * If a window appears displaying the call details and the **Destination** field is not empty, it indicates the call was sent to the trunk, but the trunk returned an error. See the example screenshot below for reference.

<figure><img src="../../.gitbook/assets/cdr-trouble-shoot.png" alt=""><figcaption></figcaption></figure>

### **480 - Temporarily Unavailable**

This status code usually means the callee is not online or unavailable to answer your call.

### **408 - Request Timeout**

This status code indicates the callee can’t be reached, possibly due to a lost network connection.

### **503 - Service Unavailable**

This code may appear for the following reasons:

* The PortSIP PBX can’t provide service at this moment.
* The app/IP Phone failed to resolve the PBX domain DNS.
* The SIP trunk can’t provide service at this moment if the call is sent to the trunk.
* The PortSIP PBX failed to resolve the trunk domain DNS.

### **403 - Forbidden**

This code may appear for the following reasons:

* The current usage exceeds the PortSIP PBX license.
* If you make a call to the SIP trunk and the called number starts with a **+**, it means this is an international call. In this case, if the extension caller role is a **standard user**, they will receive this error since standard users do not have permission to make international calls, please change the extension's role to avoid this issue.
* If the extension caller has permission to make international calls, but the country code is disallowed, you will need to allow the country code in the menu: **Blacklist and Codes > Codec and E164.** Click the tab **ALLOWED COUNTRY CODE** and select the country code to enable it.
* The current established calls have reached the maximum call limit of the trunk.
* The callee number is in the PBX number blacklist. You can manage the blacklist in the menu: **Blacklist and Codes > Number Blacklist**.
* The trunk received the call from the PortSIP PBX but returned an error. Please contact your trunk support team for further assistance.

### **402 - Payment Required**

This code appears if the tenant admin has enabled online billing, but the extension caller does not have enough balance.

### **404 - Not Found**

This code may appear for the following reasons:

* The called extension number does not exist.
* The called PSTN number does not exist, so the trunk replied 404.
* The outbound call doesn’t match any outbound rule.
* The outbound call matched a trunk that was not available.
* The outbound rule matched, but it is currently outside office hours or during holidays, click the Office Hours tab of the outbound rule to check.

### Other Errors

Usually, trunks require the Outbound Caller ID (CLI) to be included in the **From** header of the INVITE message sent to the trunk. The caller extension must configure the **Outbound Caller ID** or must configure an **Outbound Caller ID** at the tenant level. For more details, please refer to the section on [Handling Outbound Calls Through SIP Trunk](../portsip-pbx-administration-guide/7-trunk-management/handle-outbound-calls-through-sip-trunk.md#outboundcallerid).

Note: Some trunks also require the **Outbound Caller ID** to start with a '**+**'. Please confirm this requirement with your trunk provider.

## 2. Troubleshooting Inbound Calls Issues

Sometimes, after configuring the trunk and inbound rule in the PortSIP PBX, calls to the DID number may not be received. To troubleshoot this issue, please follow the steps below.

### Check the Trunk IP Address

If the trunk sends calls to the PortSIP PBX from an IP address that does not match the IP host or the **Associated IP Addresses** configured for the trunk in the PortSIP PBX, the PBX will respond with a 407 error.

It’s important to contact your trunk provider to obtain the IP addresses they use to send the INVITE messages to the PortSIP PBX. Once you have this information, add the IP addresses to the **Trunk Associated IP Addresses** as shown in the screenshot below.

**Note**: Some preconfigured trunks already have these associated IP addresses.

<figure><img src="../../.gitbook/assets/trunk-asscociated-ip.png" alt=""><figcaption></figcaption></figure>

The trunk also includes an option in the PortSIP PBX: **"Verify the port when receiving SIP messages from the trunk"**. This option is **disabled** by default. When enabled, the PBX will verify that the source port of incoming SIP messages matches the port configured in the PortSIP PBX. If the ports do not match, the PBX will respond with a **407** error.

We recommend keeping this option **disabled** by default unless you specifically require it for your configuration.

### Check the DID Number

When the PortSIP PBX identifies that the call is coming from a configured trunk, it will check if the dialed number falls within the trunk's DID pool range. If the dialed number is not within the DID pool for a tenant’s configured trunk, the PBX will respond with a **407** error to the trunk.

In many scenarios, customers configure the DID pool without including the **country code**, but the trunk sends the INVITE message to the PortSIP PBX with the country code prefixed. For example, if a tenant’s DID pool is set to **620012861** or **620012861-6200128618**, but the trunk sends the DID as **33620012861** or **+33620012861**, the PortSIP PBX will return a 407 error because the dialed number is outside the DID pool range. In this case, the DID pool should be configured as **33620012861** or **33620012861-633200128618**.

**Note:** You do not need to include the "+" prefix in the DID pool configuration; the PortSIP PBX will automatically handle this scenario.

### Check the Inbound Rules

Once the PortSIP PBX verifies that the call is coming from the trunk, it will proceed to match the tenant's inbound rules. You must ensure that the dialed number matches at least one inbound rule.

For the inbound rule, fill in the **"DID/DDI Number or Number Range"** field with the dialed number you want to route to an extension or system extension.&#x20;

Typically, the **Caller Number Mask** field does not need to be filled in. For more details, please refer to the guide on [**Configuring Inbound Rules**](../portsip-pbx-administration-guide/8-call-route-management/configuring-inbound-rule.md).

<figure><img src="../../.gitbook/assets/inbound-rule-did.png" alt=""><figcaption></figcaption></figure>

## 3. Troubleshooting No Response from the Trunk

In some cases, the trunk may not respond to the PortSIP PBX when it sends REGISTER, INVITE, or other SIP messages. To troubleshoot this issue, please follow the steps below.

### Check the Whitelist on the Trunk Side

Some trunk providers require you to whitelist the PortSIP PBX’s IP address. Please contact your trunk provider to confirm this requirement.

### Change the User-Agent

Some trunk providers host their services based on PortSIP competitors' platforms, and they may block the PortSIP PBX **User-Agent**.  So if the trunk recognizes that the SIP message’s **User-Agent** header includes the "**PortSIP**" string, it may silently discard the message without sending any response to the PortSIP PBX.

To resolve this, sign in to the PortSIP PBX web portal as a **System Administrator**, navigate to the menu **Advanced > Settings**, change the **User-Agent**, save the changes, and then try again.

**Note:** This is an unfortunate behavior that wastes both customer and support team time during troubleshooting.

<figure><img src="../../.gitbook/assets/portsip-pbx-user-agent.png" alt=""><figcaption></figcaption></figure>

Feel free to contact the PortSIP support team at [support@portsip.com](mailto:support@portsip.com) or [submit a ticket](https://portsip.atlassian.net/servicedesk/customer/portals). Our team will help you resolve any issues.

