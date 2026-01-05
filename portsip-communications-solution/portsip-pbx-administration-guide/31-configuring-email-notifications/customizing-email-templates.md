# Customizing Email Templates

### Introduction to Email Templates

PortSIP PBX allows you to **customize email notification templates**, giving you full control over the content and appearance of system-generated emails.

With this powerful feature, you can tailor every email template in PortSIP PBX to match your organization’s needs and branding. You can add your own logos or branding text, include links to important resources, and personalize messages using recipient-specific information and other dynamic fields.

For example, you can customize the default **Welcome Email Template,** the email sent when a new extension is created, to include your own messaging, custom links, and even modify the **From (Display Name)** or dynamically insert the recipient’s **first and last name** based on their extension settings.

In the following example, we’ll walk through how to modify the Welcome Email Template to:

* Set a custom sender name, and
* Automatically include the recipient’s first and last name in the email content.

***

### Add a Display Name to Your Email

You can configure a custom sender display name for outgoing emails by editing the email template.

Follow the steps below:

1. Navigate to **Advanced > Email Templates**.
2. Select the email template you want to modify.
3. In the **Mail From** field, enter the desired sender display name.
4. Click **Save**.

The value entered in **Mail From** appears as the **From** name in the Welcome Email and other emails that use this template.

#### Using a dynamic sender name

You can use the variable:

* `%%TENANT_NAME%%`

When this variable is used in the **Mail From** field, the PBX automatically replaces it with the tenant’s name, allowing the sender's display name to match the tenant dynamically.

<figure><img src="../../../.gitbook/assets/custom_email_template.png" alt=""><figcaption></figcaption></figure>

***

### Customize the Welcome Email Sent to an Extension

By default, PortSIP PBX sends email notifications in **plain-text format**.

You can freely edit the content in the template editor—add or remove text, reorder sections, and adjust any of the supported **template variables** to suit your requirements.

A full list of supported template variables is available here:

**Supported Template Variable Parameters**

***

### Customize Email Notifications Using HTML Templates

PortSIP PBX supports sending email notifications in **HTML format**, allowing you to create more visually appealing and professional-looking emails.

You can create your own HTML page that includes the supported **template variables**, then copy and paste the HTML source code into the template editor. You are free to modify the layout, styling, and content as needed.

<figure><img src="../../../.gitbook/assets/custom_email_template_1.png" alt=""><figcaption></figcaption></figure>

To verify and preview your HTML email before using it, you can paste the HTML source code into an online editor such as:

[HTML Code Editor – Instant Preview](https://htmlcodeeditor.com/)

***

#### Default HTML templates

PortSIP provides a set of **default HTML email templates** that you can use as a starting point.

You are welcome to download these templates and customize them to suit your branding and notification requirements.











