# Supported Template Variable Parameters

Below is a list of all available **template variables** that can be used when customizing each email template.

You can find the email template configuration in the **PortSIP PBX Web Portal** under:

**Advanced > Email Templates**

***

### Global Variables

These variables are applicable to all email notification templates.

* %%TENANT\_NAME%%: Refer to the tenant's name

***

### Rest Password

* %%DISPLAY\_NAME%%: The display name of the extension user who received this email notification for password reset.
* %%PBX\_WEB\_DOMAIN%%: The PBX server domain(FQDN ) used to access the web portal.
* %%REST\_PASSWORD\_LINK%%: The link that the extension user can click on it to reset the password.

***

### 2FA Verification

* %%DISPLAY\_NAME%%: The display name of the extension user who received this email notification for 2FA verification.
*





















