# Password and Sign-In Security

This section allows administrators to configure **password policies** and **sign-in security settings** for extension users within a tenant. These controls help protect user accounts, reduce the risk of unauthorized access, and enforce consistent security standards across the organization.

Configurable options include password length, complexity requirements, and two-factor authentication (2FA).

***

### Password Policy

To configure the password policy:

1. Sign in to the **PBX Web Portal**.
2. Navigate to **Company** from the left-hand menu.
3. Select the **Password Policy** tab.

<figure><img src="../../../.gitbook/assets/tenant-security-1.png" alt=""><figcaption></figcaption></figure>

From here, you can customize the tenant’s password requirements, including:

* **Minimum password length**
* **Maximum password length**
* **Password complexity rules** (for example, uppercase letters, lowercase letters, numbers, and special characters)

These settings apply to all extension users within the tenant and help enforce strong password practices.

***

### Two-Factor Authentication (2FA)

**PortSIP PBX** supports **two-factor authentication (2FA)** for extension users by sending a one-time verification code via email.

#### Enabling 2FA

When the **Enable two-step verification** option is enabled:

* All extension users under the tenant must provide:
  1. Their username and password
  2. A verification code sent to their registered email address

This additional verification step significantly enhances account security.

#### Mail Server Requirement

Because 2FA relies on email delivery, it is **critical** that the **Mail Server settings** are correctly configured and fully operational.

> **Important**\
> If the mail server is not properly configured, users will not receive the verification code and will be unable to sign in.

#### Verification Warning

After enabling 2FA and clicking **OK** to save the configuration, the PBX Web Portal will display a **warning message** prompting you to verify the mail server settings. This ensures administrators confirm email delivery before enforcing two-factor authentication for users.

<figure><img src="../../../.gitbook/assets/tenant-security-2.png" alt=""><figcaption></figcaption></figure>



