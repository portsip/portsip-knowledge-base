# Troubleshooting Call Issues

This article provides solutions for common PortSIP PBX issues when making calls. When you make a call from the app or an IP Phone, if the call fails, you will receive a status code indicating the reason. Below, we explain the status codes and their meanings:

**480 - Temporarily Unavailable**

This status code usually means the callee is not online or unavailable to answer your call.

**408 - Request Timeout**

This status code indicates the callee can’t be reached, possibly due to a lost network connection.

**503 - Service Unavailable**

This code may appear for the following reasons:

* The PortSIP PBX can’t provide service at this moment.
* The app/IP Phone failed to resolve the PBX domain DNS.
* The SIP trunk can’t provide service at this moment if the call is sent to the trunk.
* The PortSIP PBX failed to resolve the trunk domain DNS.

**403 - Forbidden**

This code may appear for the following reasons:

* The current usage exceeds the PortSIP PBX license.
* If you make a call to the SIP trunk and the called number starts with a **+**, it means this is an international call. In this case, if the extension caller role is a **standard user**, they will receive this error since standard users do not have permission to make international calls, please change the extension's role to avoid this issue.
* If the extension caller has permission to make international calls, but the country code is disallowed, you will need to allow the country code in the menu: **Blacklist and Codes > Codec and E164.** Click the tab **ALLOWED COUNTRY CODE** and select the country code to enable it.
* The current established calls have reached the maximum call limit of the trunk.
* The callee number is in the PBX number blacklist. You can manage the blacklist in the menu: **Blacklist and Codes > Number Blacklist**.

**402 - Payment Required**

This code appears if the tenant admin has enabled online billing, but the extension caller does not have enough balance.

**404 - Not Found**

This code may appear for the following reasons:

* The called extension number does not exist.
* The called PSTN number does not exist.
* The outbound call doesn’t match any outbound rule.
* The outbound call matched a trunk that was not available.
* The outbound rule matched, but it is currently outside office hours or during holidays.

Feel free to contact the PortSIP support team at [support@portsip.com](mailto:support@portsip.com) or [submit a ticket](https://portsip.atlassian.net/servicedesk/customer/portals). Our team will help you resolve any issues.

