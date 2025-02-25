# 31 Configuring Email Notifications

Email notifications are used to send alerts about specific activities occurring in your PBX. For example, you can configure notifications to inform users when a new voicemail is received.

These notifications can be customized by setting various custom parameters.

## System Administrator Level

### Setting Up Mail Servers

In order to send email notifications for system events to the system administrator, the mail server must be correctly configured.&#x20;

1. Sign in as System Administrator
2. Select the menu **Advanced > System Notifications,** and click the **Mail Server** tab.

From here, you can set the mail server settings for different email service providers.

#### Generic

For other generic SMTP providers that are not Google or Microsoft, please set it up as per the provider's instructions. For some users, SMTP authentication is done by IP address rather than by username and password. In this case, select **NONE** for the **Authentication Mode** field.

<figure><img src="../../.gitbook/assets/mai-server-admin-1.png" alt=""><figcaption></figcaption></figure>

#### Google Gmail

You can choose to use Google Gmail as the email service provider. Enter the username (usually your email address), the sender’s email, and the recipient emails.

Since Google has disabled third-party apps from sending emails using a username and password, after saving the settings, you will need to complete the [Google Integration](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/29-integrations/google-workspace-integration) process to enable the PBX to send emails via Gmail using OAuth.

<figure><img src="../../.gitbook/assets/mai-server-admin-2.png" alt=""><figcaption></figcaption></figure>

#### Microsoft 365

You can choose to use Microsoft 365 as the email service provider. Enter the username (usually your email address), the sender’s email, and the recipient emails.

Since Microsoft has disabled third-party apps from sending emails using a username and password, after saving the settings, you will need to complete the [Microsoft 365 Integration](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/29-integrations/microsoft-365-integration) process to enable the PBX to send emails via Microsoft 365 using OAuth.

<figure><img src="../../.gitbook/assets/mai-server-admin-3.png" alt=""><figcaption></figcaption></figure>

#### Apply Mail Settings to Tenants

There is an option "**Apply the mail server settings to all tenants.".** When it is enabled, the PBX will use the System Administrator’s email settings to send notification emails to tenants who have not configured their own mail server.

### Configure Notifications

Select the menu **Advanced > System Notifications,** and click the **Notifications** tab, where you can decide how to send the notifications.

Emails containing the quota reports and other events reported in the following functional areas are sent to the recipients:

* Hard Disk Threshold - The hard disk usage threshold in percentage, 0 means don't monitor hard disk usage.
* CPU Threshold - The CPU usage threshold in percentage, 0 means don't monitor CPU usage.
* Memory Threshold - The memory usage threshold percentage, 0 means don't monitor memory usage.
* When specified hard disk threshold is exceeded - Turn on/off to enable email notification for the hard disk usage.
* When specified CPU threshold is exceeded - Turn on/off to enable email notification for the CPU usage.
* When specified memory threshold is exceeded - Turn on/off to enable email notification for memory usage.
* An IP has been blacklisted - Turn on/off to enable email notification once an IP was blocked.
* The license limit is reached - Turn on/off to enable email notification once the license limit is reached.
* PBX Services are stopped or fail  - Turn on/off to enable email notification once one of the PBX services is stopped or fails.

## Tenant Level

PortSIP PBX also supports email notifications for the tenant administrator, different from the system events, the tenant events are more related to the calls.

Each tenant needs to set up its own SMTP server if wants to receive email notifications.

Sign in to the Web Portal as the tenant Admin user or [manage the tenant](3-tenant-management.md#3.4-managing-tenant) from the system administrator workspace, the Web Portal will pop-ups a Window to ask to configure the SMTP server for this tenant if the SMTP server is not set up yet.&#x20;

<figure><img src="../../.gitbook/assets/tenant_smtp.png" alt=""><figcaption></figcaption></figure>

You can also click the **Settings** button to set up it. You can also go to this page by selecting the menu **Advanced > Notifications** and clicking the **Mail Server** page.

* Enter your SMTP server domain/address and port.
* **Reply Email Address**: the email of the sender.
* Enter the username and password.
* **Recipients**: enter the recipients' email addresses, and separate them by a semicolon.
* **Enable TLS/SSL**: turn on this option if your SMTP requires TLS/SSL.

### Configure Notifications

Select the menu **Advanced > System Notifications,** and click the **Notifications** tab, where you can decide how to send the notifications.

Emails containing the quota reports and other events reported in the following functional areas are sent to the recipients:

* **A new user was created** - Turn on/off to send the welcome email which includes the user extension number, password, PBX domain and IP address for logging in, and the QR code.
* **When queue SLA time has been breached** - Turn on/off to send the email notification if the SLA time has been breached in a queue.
* When queue callback has been made - Turn on/off to send the email notifications if the callback is made in a queue.
* When queue callback fails - Turn on/off to send the email notifications if the queue callback failed.
* **When a queue call is lost** - Turn on/off to send an email notification if there have an abandoned call in a queue.

## Custom Email Notification Template

One of the features of the PortSIP PBX is the ability to customize Email Templates. In the **Advanced > Email Templates** menu, you can completely reinvent the email templates within the PortSIP PBX.

For instance, you can take the default Welcome Email Template (the email that is sent out when a new extension is created) and enter some of your own text, including some of your own links and even add an email from your name and the first and last name of the user, as configured in their extension settings.

<figure><img src="../../.gitbook/assets/email_template.png" alt=""><figcaption></figcaption></figure>

