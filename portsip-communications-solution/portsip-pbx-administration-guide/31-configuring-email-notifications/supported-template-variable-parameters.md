# Supported Template Variable Parameters

Below is a list of all available template variables that can be used when customizing email templates.

You can access email template configuration in the PortSIP PBX Web Portal under:

**Advanced > Email Templates**

***

### Global Variables

These variables are available in all email notification templates.

* `%%TENANT_NAME%%`\
  The name of the tenant sending the email.

***

### Reset Password

Variables used in password reset email notifications:

* `%%DISPLAY_NAME%%`\
  Display name of the extension user who receives the password reset email.
* `%%PBX_WEB_DOMAIN%%`\
  Fully Qualified Domain Name (FQDN) of the PBX web portal.
* `%%RESET_PASSWORD_LINK%%`\
  Secure link the user clicks to reset their password.

***

### 2FA Verification

Variables used in Two-Factor Authentication (2FA) emails:

* `%%DISPLAY_NAME%%`\
  Display name of the extension user.
* `%%EXTENSION_NUMBER%%`\
  Extension number of the user.
* `%%PBX_WEB_DOMAIN%%`\
  PBX web portal domain (FQDN).
* `%%AUTH_CODE%%`\
  One-time authentication code for 2FA verification.

***

### Call Reports

Variables used in call report generation emails:

* `%%CALLREPORT_TYPE%%`\
  Type of call report.
* `%%CALLREPORT_NAME%%`\
  Name of the generated call report.
* `%%CALLREPORT_LINK%%`\
  Download link for the generated call report.

***

### Meeting Invitation

Variables used in meeting invitation emails:

* `%%TOPIC%%`\
  Meeting subject or topic.
* `%%TIME%%`\
  Scheduled meeting time.
* `%%PASSWORD%%`\
  Meeting access password.
* `%%JOIN_LINK%%`\
  URL used to join the meeting.
* `%%DIAL_NUMBER%%`\
  Dial-in phone number for the meeting.

***

### Low Disk Space Alert

Variables used in disk usage warning emails:

* `%%DATE_TIME%%`\
  Date and time when the alert was triggered.
* `%%USED_SIZE%%`\
  Amount of disk space currently in use.
* `%%FREE_SIZE%%`\
  Remaining available disk space.

***

### Emergency Call Made

Variables used in emergency call notifications:

* `%%DATE_TIME%%`\
  Date and time when the emergency call was placed.
* `%%CALLER_NUMBER%%`\
  The number that initiated the emergency call.
* `%%CALLEE_NUMBER%%`\
  Emergency destination number.

***

### License Limit Reached

Variables used when a license limit is reached:

* `%%DATE_TIME%%`\
  Date and time the license limit was reached.
* `%%LICENSE_TYPE%%`\
  Type of license that has reached its limit.

***

### Queue Callback Scheduled

Variables used in queue callback success notifications:

* `%%DATE_TIME%%`\
  Date and time of the callback were scheduled.
* `%%CALL_BACK_NUMBER%%`\
  Phone number used for the callback.
* `%%QUEUE_NAME%%`\
  Name of the queue.
* `%%QUEUE_NUMBER%%`\
  Queue extension number.
* `%%CALLER_NUMBER%%`\
  Original caller number.

***

### Failed Queue Callback

Variables used in queue callback failure notifications:

* `%%DATE_TIME%%`\
  Date and time when the callback failed.
* `%%CALL_BACK_NUMBER%%`\
  Callback phone number.
* `%%QUEUE_NAME%%`\
  Queue name.
* `%%QUEUE_NUMBER%%`\
  Queue extension number.
* `%%CALLER_NUMBER%%`\
  Original caller number.

***

### Lost Queue Call

Variables used when a queue call is abandoned:

* `%%DATE_TIME%%`\
  Date and time when the call was lost.
* `%%QUEUE_NAME%%`\
  Queue name.
* `%%QUEUE_NUMBER%%`\
  Queue extension number.
* `%%CALLER_NUMBER%%`\
  Caller number of the abandoned call.

***

### Queue SLA Breach

Variables used in queue SLA breach notifications:

* `%%DATE_TIME%%`\
  Date and time the SLA breach occurred.
* `%%CALLER_NUMBER%%`\
  Caller number associated with the breach.
* `%%QUEUE_NAME%%`\
  Queue name.
* `%%QUEUE_NUMBER%%`\
  Queue extension number.
* `%%BREACHSLANO%%`\
  Total number of calls when the SLA breach occurred.
* `%%WAITSESSIONNO%%`\
  Number of callers waiting in the queue.
* `%%SLA_TIME%%`\
  Configured SLA time threshold for the queue.

***

### User Recharge

Variables used in extension user recharge or payment notifications:

* `%%DATE_TIME%%`\
  Date and time of the recharge.
* `%%PAYMENT_AMOUNT%%`\
  Recharge or payment amount.
* `%%DISPLAY_NAME%%`\
  Display name of the extension user.

***

### New Shared Voicemail Received

Variables used when a shared voicemail is received:

* `%%DATE_TIME%%`\
  Date and time the voicemail was received.
* `%%CALLER_EXTENSION_NUMBER%%`\
  Caller phone number.
* `%%VOICEMAIL_NAME%%`\
  Name of the shared voicemail box.

***

### Trunk Max Concurrent Calls Reached

Variables used when a trunk reaches its concurrency limit:

* `%%DATE_TIME%%`\
  Date and time the limit was reached.
* `%%TENANT_NAME%%`\
  Tenant name.
* `%%TRUNK_NAME%%`\
  Trunk name.
* `%%TRUNK_MAX_CONCURRENT_CALLS%%`\
  Configured maximum concurrent call limit.

***

### SIP Trunk Connected

Variables used when a SIP trunk connects successfully:

* `%%DATE_TIME%%`\
  Date and time of the connection.
* `%%TRUNK_NAME%%`\
  Trunk name.
* `%%PBX_WEB_DOMAIN%%`\
  PBX web portal domain (FQDN).

***

### SIP Trunk Disconnected

Variables used when a SIP trunk disconnects:

* `%%DATE_TIME%%`\
  Date and time of the disconnection.
* `%%TRUNK_NAME%%`\
  Trunk name.
* `%%PBX_WEB_DOMAIN%%`\
  PBX web portal domain (FQDN).
* `%%STATUS_CODE%%`\
  SIP response or error code.

***

### Extension Information for New User Created

Variables used in the welcome email for a newly created extension:

* `%%DISPLAY_NAME%%`\
  User display name.
* `%%USER_NAME%%`\
  Login username.
* `%%USER_PASSWORD%%`\
  User login password.
* `%%EXTENSION_NUMBER%%`\
  Extension number.
* `%%EXTENSION_PASSWORD%%`\
  Extension SIP password.
* `%%VOICEMAIL_PIN%%`\
  Voicemail PIN.
* `%%EMAIL_ADDRESS%%`\
  The user's email adress.
* `%%DOMAIN%%`\
  Tenant SIP domain.
* `%%PBX_WEB_DOMAIN%%`\
  PBX web portal domain (FQDN).
* `%%SIP_PUBLIC_IP%%`\
  SIP server public IPv4 address.
* `%%SIP_PRIVATE_IP%%`\
  SIP server private IPv4 address.
* `%%SIP_PUBLIC_IPV6%%`\
  SIP server public IPv6 address.
* `%%SIP_PRIVATE_IPV6%%`\
  SIP server private IPv6 address.
* `%%UDP_PORT%%`\
  SIP UDP port.
* `%%TCP_PORT%%`\
  SIP TCP port.
* `%%TLS_PORT%%`\
  SIP TLS port.
* `%%QR_CODE_URL%%`\
  QR code URL for client app login.

***

### New Voicemail Received

Variables used when a new voicemail is received:

* `%%DATE_TIME%%`\
  Date and time the voicemail was received.
* `%%CALLER_EXTENSION_NUMBER%%`\
  Caller phone number.
* `%%VOICEMAIL_NAME%%`\
  Voicemail box name.
* `%%VOICEMAIL_DOWNLOAD_URL%%`\
  Direct download link for the voicemail file.





