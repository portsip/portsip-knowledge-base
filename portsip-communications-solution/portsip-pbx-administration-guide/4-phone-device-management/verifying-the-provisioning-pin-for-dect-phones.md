# Verifying the Provisioning PIN for DECT Phones

Some DECT base stations do not have a built-in screen. In addition, some DECT phone brands and models do not support entering the provisioning password from the handset screen.

This guide explains how to verify the provisioning PIN when auto-provisioning a DECT phone.

***

### Prerequisites

Before you begin, enable the PIN verification feature for IP phone auto-provisioning.

For details, see [PIN Verification for IP Phone Auto-Provisioning](pin-verification-for-ip-phone-auto-provisioning.md).

***

### Configuring the Provisioning PIN for a DECT Phone

Use the following steps to configure the provisioning PIN for a DECT phone in PortSIP PBX.

#### To configure the provisioning PIN

1. Sign in to the PortSIP PBX web portal as a **System Administrator** and select the tenant you want to manage.\
   Alternatively, sign in directly as the **Tenant Administrator**.
2. Go to **Call Manager > DECT Phones**.
3. Click **Add** to configure a new DECT phone, or select an existing DECT phone to edit it.
4. In the **Provisioning PIN** field, enter the PIN that will be used to verify the provisioning request.
5. Save the changes.

<figure><img src="../../../.gitbook/assets/dect-phone-pin-provsioning-1.png" alt=""><figcaption></figcaption></figure>

**Expected outcome**

The DECT phone has a provisioning PIN assigned. This PIN must be provided by the phone during auto-provisioning.

***

### Verifying the PIN for Fanvil LINKVIL DECT Phones

After you configure a Fanvil LINKVIL DECT phone in PortSIP PBX, use the following steps to configure PIN verification on the phone.

#### To verify the provisioning PIN on a Fanvil LINKVIL DECT phone

In the PortSIP PBX web portal, copy the phone provisioning URL.\
See the screenshot below for reference.



<figure><img src="../../../.gitbook/assets/fanvil-dect-phone-pin-provsioning-2.png" alt=""><figcaption></figcaption></figure>

1. Sign in to the Fanvil LINKVIL web portal.
2. Go to **System > Auto Provision > Static Provisioning Server**.
3. Paste the copied provisioning URL into the **Server Address** field.
4.  In the **Configuration File Name** field, enter the configuration file name in the following format:

    ```
    {MAC}.cfg
    ```

    For example:

    ```
    0c383e5fdcf8.cfg
    ```

See the screenshot below:

<figure><img src="../../../.gitbook/assets/fanvil-dect-phone-pin-provsioning-4.png" alt=""><figcaption></figcaption></figure>

5. Go to **System > Auto Provision > Basic Settings**.
6. In the **Authentication Name** field, enter any text value for it.
7. In the authentication password field, enter the **Provisioning PIN** that you configured in PortSIP PBX.
8. Click **Apply** to save the changes.
9. Provision the Fanvil LINKVIL DECT phone by following the guide [Provision Fanvil DECT IP Phones](provision-fanvil-dect-ip-phones.md).

<figure><img src="../../../.gitbook/assets/fanvil-dect-phone-pin-provsioning-5.png" alt=""><figcaption></figcaption></figure>

**Expected outcome**

The Fanvil LINKVIL DECT phone sends the provisioning PIN during auto-provisioning. PortSIP PBX verifies the PIN before allowing the phone to download its provisioning configuration.

***



