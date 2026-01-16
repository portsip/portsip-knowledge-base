# Supported Template Variable Parameters

### Available Email Template Variables

Below is a complete list of template variables that can be used when customizing email templates in PortSIP PBX.

You can configure email templates in the PortSIP PBX Web Portal under:\
**Advanced > Email Templates**

***

### Global Variables

These variables are available in all email notification templates.

**%%TENANT\_NAME%%**\
The name of the tenant that is sending the email.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

***

### Reset Password

Variables used in password reset email notifications.

**%%DISPLAY\_NAME%%**\
The display name of the extension user who receives the password reset email.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

**%%RESET\_PASSWORD\_LINK%%**\
A secure link that the user clicks to reset their password.

***

### 2FA Verification

Variables used in Two-Factor Authentication (2FA) verification emails.

**%%DISPLAY\_NAME%%**\
The display name of the extension user.

**%%EXTENSION\_NUMBER%%**\
The extension number of the user.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

**%%AUTH\_CODE%%**\
A one-time authentication code used for 2FA verification.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

***

### Call Reports

Variables used in call report generation emails.

**%%CALLREPORT\_TYPE%%**\
The type of call report generated.

**%%CALLREPORT\_NAME%%**\
The name of the generated call report.

**%%CALLREPORT\_LINK%%**\
A download link for the generated call report.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Meeting Invitation

Variables used in meeting invitation emails.

**%%TOPIC%%**\
The meeting subject or topic.

**%%TIME%%**\
The scheduled meeting time.

**%%PASSWORD%%**\
The meeting access password.

**%%JOIN\_LINK%%**\
The URL used to join the meeting.

**%%DIAL\_NUMBER%%**\
The dial-in phone number for the meeting.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Low Disk Space Alert

Variables used in disk usage warning emails.

**%%DATE\_TIME%%**\
The date and time when the alert was triggered.

**%%USED\_SIZE%%**\
The amount of disk space currently in use.

**%%FREE\_SIZE%%**\
The remaining available disk space.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Emergency Call Made

Variables used in emergency call notification emails.

**%%DATE\_TIME%%**\
The date and time when the emergency call was placed.

**%%CALLER\_NUMBER%%**\
The number that initiated the emergency call.

**%%CALLEE\_NUMBER%%**\
The emergency destination number that was called.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### License Limit Reached

Variables used when a license limit is reached.

**%%DATE\_TIME%%**\
The date and time when the license limit was reached.

**%%LICENSE\_TYPE%%**\
The type of license that has reached its limit.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Queue Callback Scheduled

Variables used in queue callback success notifications.

**%%DATE\_TIME%%**\
The date and time when the callback was scheduled.

**%%CALL\_BACK\_NUMBER%%**\
The phone number used for the callback.

**%%QUEUE\_NAME%%**\
The name of the queue.

**%%QUEUE\_NUMBER%%**\
The queue extension number.

**%%CALLER\_NUMBER%%**\
The original caller’s phone number.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Failed Queue Callback

Variables used in queue callback failure notifications.

**%%DATE\_TIME%%**\
The date and time when the callback failed.

**%%CALL\_BACK\_NUMBER%%**\
The callback phone number.

**%%QUEUE\_NAME%%**\
The name of the queue.

**%%QUEUE\_NUMBER%%**\
The queue extension number.

**%%CALLER\_NUMBER%%**\
The original caller’s phone number.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Lost Queue Call

Variables used when a queue call is abandoned.

**%%DATE\_TIME%%**\
The date and time when the call was lost.

**%%QUEUE\_NAME%%**\
The name of the queue.

**%%QUEUE\_NUMBER%%**\
The queue extension number.

**%%CALLER\_NUMBER%%**\
The caller’s phone number for the abandoned call.

**%%PBX\_WEB\_DOMAIN%%**\
The Fully Qualified Domain Name (FQDN) of the PBX web portal.

***

### Queue SLA Breach

Variables used in queue SLA breach notification emails.

**%%DATE\_TIME%%**\
The date and time when the SLA breach occurred.

**%%CALLER\_NUMBER%%**\
The caller’s phone number associated with the breach.

**%%QUEUE\_NAME%%**\
The name of the queue.

**%%QUEUE\_NUMBER%%**\
The queue extension number.

**%%BREACHSLANO%%**\
The total number of calls when the SLA breach occurred.

**%%WAITSESSIONNO%%**\
The number of callers waiting in the queue at the time of the breach.

**%%SLA\_TIME%%**\
The configured SLA time threshold for the queue.

***

### User Recharge

Variables used in extension user recharge or payment notification emails.

**%%DATE\_TIME%%**\
The date and time of the recharge or payment.

**%%PAYMENT\_AMOUNT%%**\
The recharge or payment amount.

**%%DISPLAY\_NAME%%**\
The display name of the extension user.

***

### New Shared Voicemail Received

Variables used when a new shared voicemail is received.

**%%DATE\_TIME%%**\
The date and time when the voicemail was received.

**%%CALLER\_EXTENSION\_NUMBER%%**\
The caller’s phone number.

**%%VOICEMAIL\_NAME%%**\
The name of the shared voicemail box.

**%%VOICEMAIL\_DOWNLOAD\_URL%%**\
A direct download link for the voicemail file.

***

### Trunk Max Concurrent Calls Reached

Variables used when a trunk reaches its maximum concurrent call limit.

**%%DATE\_TIME%%**\
The date and time when the limit was reached.

**%%TENANT\_NAME%%**\
The tenant name.

**%%TRUNK\_NAME%%**\
The trunk name.

**%%TRUNK\_MAX\_CONCURRENT\_CALLS%%**\
The configured maximum number of concurrent calls for the trunk.

***

### SIP Trunk Connected

Variables used when a SIP trunk successfully connects.

**%%DATE\_TIME%%**\
The date and time of the connection.

**%%TRUNK\_NAME%%**\
The trunk name.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

***

### SIP Trunk Disconnected

Variables used when a SIP trunk disconnects.

**%%DATE\_TIME%%**\
The date and time of the disconnection.

**%%TRUNK\_NAME%%**\
The trunk name.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

**%%STATUS\_CODE%%**\
The SIP response or error code indicating the disconnection reason.

***

### Extension Information for New User Created

Variables used in the Welcome Email sent when a new extension is created.

**%%DISPLAY\_NAME%%**\
The user’s display name.

**%%USER\_NAME%%**\
The login username.

**%%USER\_PASSWORD%%**\
The user’s login password.

**%%EXTENSION\_NUMBER%%**\
The extension number.

**%%EXTENSION\_PASSWORD%%**\
The SIP password for the extension.

**%%VOICEMAIL\_PIN%%**\
The voicemail PIN.

**%%EMAIL\_ADDRESS%%**\
The user’s email address.

**%%DOMAIN%%**\
The tenant SIP domain.

**%%PBX\_WEB\_DOMAIN%%**\
The PBX web portal domain (FQDN).

**%%SIP\_PUBLIC\_IP%%**\
The public IPv4 address of the SIP server.

**%%SIP\_PRIVATE\_IP%%**\
The private IPv4 address of the SIP server.

**%%SIP\_PUBLIC\_IPV6%%**\
The public IPv6 address of the SIP server.

**%%SIP\_PRIVATE\_IPV6%%**\
The private IPv6 address of the SIP server.

**%%UDP\_PORT%%**\
The SIP UDP listening port.

**%%TCP\_PORT%%**\
The SIP TCP listening port.

**%%TLS\_PORT%%**\
The SIP TLS listening port.

**%%QR\_CODE%%**\
The QR code is used for client app login.

***

### New Voicemail Received

Variables used when a new voicemail is received.

**%%DATE\_TIME%%**\
The date and time when the voicemail was received.

**%%CALLER\_EXTENSION\_NUMBER%%**\
The caller’s phone number.

**%%VOICEMAIL\_NAME%%**\
The voicemail box name.

**%%VOICEMAIL\_DOWNLOAD\_URL%%**\
A direct download link for the voicemail file.





