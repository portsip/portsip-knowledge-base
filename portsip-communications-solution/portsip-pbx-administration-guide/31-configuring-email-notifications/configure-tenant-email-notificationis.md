# Configure Tenant Email Notificationis

Please follow the steps below to configure tenant-level email notifications.

### Set up the mail server

PortSIP PBX supports email notifications for tenant administrators, primarily for call-related events, which are different from system-level events.

Each tenant can configure its own mail server to receive email notifications. Alternatively, if permitted by the System Administrator, tenants may use the system-level mail server settings. For more information, see [Apply Mail Settings to All Tenants](configure-system-email-notifications.md#apply-mail-settings-to-all-tenants).

#### Configure the tenant mail server

1. Sign in to the **PBX Web Portal** as a **Tenant Administrator**, or switch to the tenant from the **System Administrator** workspace.
2. If the SMTP server has not been configured, the Web Portal will display a pop-up prompting you to set it up.
3. Click **Settings** to configure the mail server.

<figure><img src="../../../.gitbook/assets/tenant_smtp.png" alt=""><figcaption></figcaption></figure>

You can also configure the mail server manually by navigating to:

**Advanced > Notifications > Mail Server**

***

#### Using the system administrator’s mail server

If the PBX System Administrator has enabled [Apply Mail Settings to All Tenants](configure-system-email-notifications.md#apply-mail-settings-to-all-tenants), you will see the following message displayed in green:

> **“You can now use the system administrator's mail server settings to send email notifications.”**

<figure><img src="../../../.gitbook/assets/tenant-mail-server-1.png" alt=""><figcaption></figcaption></figure>

This indicates that you do **not** need to configure your own mail server settings. The tenant will use the system administrator’s mail server to send email notifications.

> **Note:**\
> You must still specify at least one recipient email address in the **Emails of Recipients** field to define where notifications will be sent.

***

#### Overwrite system mail server settings (optional)

If you prefer to use your own mail server, follow the steps in [Set up the mail server](configure-system-email-notifications.md#set-up-the-mail-server) to configure it at the tenant level.

Once configured, the PBX will use the tenant’s mail server settings to send email notifications, even if [Apply Mail Settings to All Tenants](configure-system-email-notifications.md#apply-mail-settings-to-all-tenants) is enabled at the system level.

***

### Configure notification events

1. Navigate to **Advanced > Notifications**.
2. Open the **Notifications** tab.
3. Enable or disable notifications for the desired events.

The following tenant-level email notifications are supported and will be sent to the configured recipients:

* **A new user was created**\
  Sends a welcome email containing the user’s extension number, password, PBX domain, login IP address, and a QR code.
* **When queue SLA time has been breached**\
  Sends an email notification when a queue’s SLA threshold is exceeded.
* **When a queue callback has been made**\
  Sends an email notification when a callback is successfully completed.
* **When the queue callback is unsuccessful**\
  Sends an email notification if a queue callback attempt fails.
* **When a queue call is lost**\
  Sends an email notification when a call is abandoned in a queue.

***



## Custom Email Notification Template

PortSIP PBX offers the ability to customize email templates. From the **Advanced > Email Templates** menu, you can fully modify the email templates used within the system.

For example, you can customize the default **Welcome Email Template** (the email sent when a new extension is created). You can add your own text, include personalized links, and even modify the sender’s name to display your own name, or the first and last name of the user, as configured in their extension settings.

<figure><img src="../../../.gitbook/assets/email_template.png" alt=""><figcaption></figcaption></figure>



