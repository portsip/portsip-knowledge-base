# Troubleshooting Call Issues

This article provides solutions for common PortSIP PBX issues with calls.

## Outbound Calls

&#x20;When you make a call from the app or an IP Phone, if the call fails, you will receive a status code indicating the reason. Below, we explain the status codes and their meanings:

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

## Inbound Calls

Sometimes, after configuring the trunk and inbound rule in the PortSIP PBX, calls to the DID number may not be received. To troubleshoot this issue, please follow the steps below:





Feel free to contact the PortSIP support team at [support@portsip.com](mailto:support@portsip.com) or [submit a ticket](https://portsip.atlassian.net/servicedesk/customer/portals). Our team will help you resolve any issues.





