# Supported Template Variable Parameters

PortSIP PBX notification templates support dynamic variables. These variables are replaced with actual values when the notification email is generated.

You can configure email templates in the PortSIP PBX Web Portal under:\
**Advanced > Email Templates**

Variables use the following format:

`%%VARIABLE_NAME%%`

For example, `%%PRODUCT_NAME%%` is replaced with the product name, and `%%TENANT_NAME%%` is replaced with the tenant name.

This section lists the variables used in the **System Notification Templates** and **Tenant Notification Templates**, including both TXT and HTML versions.

> **Note**\
> This section classifies variables based on the uploaded template contents.\
> A **Global Variable** means the variable is used across all templates in that category.\
> A **Common Variable** means the variable is used by multiple notification types, but not by all templates.

> **Note**\
> TXT and HTML versions generally use the same variables. The only detected difference is that the **New User Account Created** HTML template includes `%%QR_CODE%%`, while the TXT version does not.

***

### System Notification Template Variables

System notification templates are used for system-level events, such as license limits, service status changes, resource usage alerts, blocked IP addresses, and administrator authentication emails.

#### System Global Variables

The following variable is used across all system notification templates.

| Variable           | Description                                                   |
| ------------------ | ------------------------------------------------------------- |
| `%%PRODUCT_NAME%%` | The product name or brand name displayed in the notification. |

#### System Common Variables

The following variables are used by multiple system notification types. However, they are not used by all system templates and are only available for the related notification scenarios.

| Variable             | Description                                     | Typical Usage                                                                                                                                                                                                         |
| -------------------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `%%PBX_WEB_DOMAIN%%` | The PBX web domain.                             | Authentication, password reset, resource usage alerts, service status notifications, tenant disk quota notifications, SIP trunk status notifications, push certificate notifications, and webhook queue notifications |
| `%%DATE_TIME%%`      | The date and time when the event occurred.      | Event-based notifications                                                                                                                                                                                             |
| `%%DISPLAY_NAME%%`   | The display name of the notification recipient. | Authentication and password reset notifications                                                                                                                                                                       |
| `%%FREE_SIZE%%`      | The remaining free resource size.               | Disk and memory usage notifications                                                                                                                                                                                   |
| `%%USED_SIZE%%`      | The used resource size or usage value.          | CPU, disk, and memory usage notifications                                                                                                                                                                             |
| `%%TRUNK_NAME%%`     | The SIP trunk name.                             | SIP trunk-related notifications                                                                                                                                                                                       |

#### Variables by System Notification Type

| Notification Type                                        | Variables                                                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Two-factor authentication verification code notification | `%%AUTH_CODE%%`, `%%DISPLAY_NAME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`                     |
| CPU usage threshold exceeded notification                | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%USED_SIZE%%`                        |
| Low disk space warning notification                      | `%%DATE_TIME%%`, `%%FREE_SIZE%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%USED_SIZE%%`       |
| Blocked IP address notification                          | `%%DATE_TIME%%`, `%%IP_ADDRESS%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`                       |
| License limit reached notification                       | `%%LICENSE_TYPE%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`                                      |
| Memory usage threshold exceeded notification             | `%%DATE_TIME%%`, `%%FREE_SIZE%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%USED_SIZE%%`       |
| Password reset notification                              | `%%DISPLAY_NAME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%RESET_PASSWORD_LINK%%`           |
| Service connected notification                           | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%SERVICE_NAME%%`, `%%SERVICE_TYPE%%` |
| Service disconnected notification                        | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%SERVICE_NAME%%`, `%%SERVICE_TYPE%%` |
| Tenant disk quota limit reached notification             | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%TENANT_NAME%%`                      |
| Trunk concurrent call limit reached notification         | `%%PRODUCT_NAME%%`, `%%TRUNK_MAX_CONCURRENT_CALLS%%`, `%%TRUNK_NAME%%`                            |
| SIP trunk connected notification                         | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%TRUNK_NAME%%`                       |
| SIP trunk disconnected notification                      | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%STATUS_CODE%%`, `%%TRUNK_NAME%%`    |
| Push certificate update failed notification              | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`                                         |
| Webhook queue size exceeded notification                 | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%PRODUCT_NAME%%`, `%%QUEUE_SIZE%%`                       |

***

### Tenant Notification Template Variables

Tenant notification templates are used for tenant-level events, such as extension account creation, voicemail notifications, queue events, emergency calls, SIP trunk status changes, password reset emails, and meeting invitations.

#### Tenant Global Variables

The following variable is used across all tenant notification templates.

| Variable          | Description                                    |
| ----------------- | ---------------------------------------------- |
| `%%TENANT_NAME%%` | The tenant name displayed in the notification. |

#### Tenant Common Variables

The following variables are used by multiple tenant notification types. However, they are not used by all tenant templates and are only available for the related notification scenarios.

| Variable                     | Description                                             | Typical Usage                                                                        |
| ---------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `%%PBX_WEB_DOMAIN%%`         | The PBX web domain.                                     | Login, password reset, license, emergency call, and SIP trunk notifications          |
| `%%DATE_TIME%%`              | The date and time when the event occurred.              | Event-based notifications                                                            |
| `%%DISPLAY_NAME%%`           | The display name of the notification recipient.         | Authentication, password reset, recharge, user creation, and voicemail notifications |
| `%%CALLER_NAME%%`            | The caller name.                                        | Queue, voicemail, and emergency call notifications                                   |
| `%%CALLER_NUMBER%%`          | The caller number.                                      | Queue, voicemail, and emergency call notifications                                   |
| `%%QUEUE_NAME%%`             | The queue name.                                         | Queue-related notifications                                                          |
| `%%QUEUE_NUMBER%%`           | The queue number.                                       | Queue-related notifications                                                          |
| `%%TRUNK_NAME%%`             | The SIP trunk name.                                     | SIP trunk-related notifications                                                      |
| `%%CALL_BACK_NUMBER%%`       | The callback number entered or requested by the caller. | Queue callback notifications                                                         |
| `%%VOICEMAIL_DOWNLOAD_URL%%` | The URL used to download the voicemail message.         | Voicemail notifications                                                              |
| `%%VOICEMAIL_NAME%%`         | The voicemail message name.                             | Voicemail notifications                                                              |

#### Variables by Tenant Notification Type

| Notification Type                                        | Variables                                                                                                                                                                                                                                                                                    |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Password reset notification                              | `%%DISPLAY_NAME%%`, `%%PBX_WEB_DOMAIN%%`, `%%RESET_PASSWORD_LINK%%`, `%%TENANT_NAME%%`                                                                                                                                                                                                       |
| Two-factor authentication verification code notification | `%%AUTH_CODE%%`, `%%DISPLAY_NAME%%`, `%%EXTENSION_NUMBER%%`, `%%PBX_WEB_DOMAIN%%`, `%%TENANT_NAME%%`                                                                                                                                                                                         |
| Call report ready notification                           | `%%CALLREPORT_LINK%%`, `%%CALLREPORT_NAME%%`, `%%CALLREPORT_TYPE%%`, `%%TENANT_NAME%%`                                                                                                                                                                                                       |
| Meeting invitation notification                          | `%%DIAL_NUMBER%%`, `%%JOIN_LINK%%`, `%%PASSWORD%%`, `%%TENANT_NAME%%`, `%%TIME%%`, `%%TOPIC%%`                                                                                                                                                                                               |
| Low disk space warning notification                      | `%%DATE_TIME%%`, `%%FREE_SIZE%%`, `%%TENANT_NAME%%`, `%%USED_SIZE%%`                                                                                                                                                                                                                         |
| Emergency call placed notification                       | `%%CALLEE_NUMBER%%`, `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%TENANT_NAME%%`                                                                                                                                                                        |
| License limit reached notification                       | `%%LICENSE_TYPE%%`, `%%PBX_WEB_DOMAIN%%`, `%%TENANT_NAME%%`                                                                                                                                                                                                                                  |
| Queue callback request notification                      | `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%CALL_BACK_NUMBER%%`, `%%DATE_TIME%%`, `%%QUEUE_NAME%%`, `%%QUEUE_NUMBER%%`, `%%TENANT_NAME%%`                                                                                                                                                     |
| Queue callback failed notification                       | `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%CALL_BACK_NUMBER%%`, `%%DATE_TIME%%`, `%%QUEUE_NAME%%`, `%%QUEUE_NUMBER%%`, `%%TENANT_NAME%%`                                                                                                                                                     |
| Queue abandoned call notification                        | `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%DATE_TIME%%`, `%%QUEUE_NAME%%`, `%%QUEUE_NUMBER%%`, `%%TENANT_NAME%%`                                                                                                                                                                             |
| Queue SLA breach notification                            | `%%BREACHSLANO%%`, `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%DATE_TIME%%`, `%%QUEUE_NAME%%`, `%%QUEUE_NUMBER%%`, `%%SLA_TIME%%`, `%%TENANT_NAME%%`, `%%WAITSESSIONNO%%`                                                                                                                     |
| Account recharge notification                            | `%%DATE_TIME%%`, `%%DISPLAY_NAME%%`, `%%PAYMENT_AMOUNT%%`, `%%TENANT_NAME%%`                                                                                                                                                                                                                 |
| Shared voicemail received notification                   | `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%DATE_TIME%%`, `%%TENANT_NAME%%`, `%%VOICEMAIL_DOWNLOAD_URL%%`, `%%VOICEMAIL_NAME%%`                                                                                                                                                               |
| Trunk concurrent call limit reached notification         | `%%DATE_TIME%%`, `%%TENANT_NAME%%`, `%%TRUNK_MAX_CONCURRENT_CALLS%%`, `%%TRUNK_NAME%%`                                                                                                                                                                                                       |
| SIP trunk connected notification                         | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%TENANT_NAME%%`, `%%TRUNK_NAME%%`                                                                                                                                                                                                                   |
| SIP trunk disconnected notification                      | `%%DATE_TIME%%`, `%%PBX_WEB_DOMAIN%%`, `%%STATUS_CODE%%`, `%%TENANT_NAME%%`, `%%TRUNK_NAME%%`                                                                                                                                                                                                |
| New user account created notification                    | `%%DISPLAY_NAME%%`, `%%DOMAIN%%`, `%%EXTENSION_NUMBER%%`, `%%EXTENSION_PASSWORD%%`, `%%QR_CODE%%`, `%%SIP_PRIVATE_IP%%`, `%%SIP_PUBLIC_IP%%`, `%%TCP_PORT%%`, `%%TENANT_NAME%%`, `%%TLS_PORT%%`, `%%UDP_PORT%%`, `%%USER_EMAIL%%`, `%%USER_NAME%%`, `%%USER_PASSWORD%%`, `%%VOICEMAIL_PIN%%` |
| New voicemail received notification                      | `%%CALLER_NAME%%`, `%%CALLER_NUMBER%%`, `%%DATE_TIME%%`, `%%DISPLAY_NAME%%`, `%%TENANT_NAME%%`, `%%VOICEMAIL_DOWNLOAD_URL%%`, `%%VOICEMAIL_NAME%%`                                                                                                                                           |

***

### Template Format Notes

When editing notification templates, observe the following rules:

1.  Keep the variable name unchanged, including the leading and trailing double percent signs.

    Correct example:\
    `%%TENANT_NAME%%`

    Incorrect examples:\
    `%TENANT_NAME%`\
    `TENANT_NAME`\
    `%%TENANT NAME%%`
2.  Do not translate variable names.

    For example, `%%CALLER_NUMBER%%` must remain unchanged in all language versions.
3.  Variables are case-sensitive.

    For example, `%%USER_NAME%%` and `%%user_name%%` are not the same variable.
4.  HTML templates may include variables used for visual content.

    For example, the **New User Account Created** HTML template includes `%%QR_CODE%%` to display the QR code. This variable is not used in the TXT version.
5. Before saving a customized template, make sure all required variables for that notification type are still present.

Removing required variables may cause important information to be missing from the notification email.
