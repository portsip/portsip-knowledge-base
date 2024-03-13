# 3 Tenant Management

After completing the Configuration Wizard, you may manage PortSIP PBX in the Web Portal, such as tenant, user(extension), call routing, call forwarding, feature access code, SIP trunk, queue, ring group, and so on.

## Creating Tenant

To create a new tenant, select the left menu **Tenants** and click **Add**.

When creating a tenant, you can specify the tenant profile details such as **Name**, **SIP Domain**, **Office Hours,** and **Storage**.&#x20;

A tenant's profile can be modified after the **System Admin**(admin) signs into the Web Portal. You can also limit the resources the tenant uses by clicking the **General** page. The **Capability** section under this tab allows you to set the maximum extensions, maximum concurrent calls, maximum ring groups, etc.

The **General** page allows you to adjust the following features.

* **Name**: The name to identify that tenant, for example, "**ABC Company"**.
* **SIP Domain**: The SIP domain for this tenant should be unique and can be a dummy domain that does not require real existence. It is the host part of the extension SIP URI used to distinguish multiple tenants. For example, if you set "**test1.com**" as the SIP domain for tenant A and "**test2.com**" for tenant B, then extension 101 of tenant A has the SIP URI: `sip:101@test1.com`; and extension 101 of tenant B has the SIP URI: `sip:101@test2.com`.
* **Capability**: Used to set the capabilities for the tenant

{% hint style="warning" %}
The tenant SIP domain can be any domain, even if it doesn’t actually exist or is unresolvable. It can simply be a dummy domain. It's used to provide a unique SIP address for an extension, thereby distinguishing extensions among different tenants.
{% endhint %}

The **Options** page allows adjusting the following features.

* **Enable this Tenant**: If this option is deselected, this tenant will be disabled, and all extensions of this tenant will no longer be valid.
* **Allow concurrent logins**: If this option is selected, the tenant can sign in to the Web Portal from multiple devices. If deselected, once the tenant signs in, the login in another PC/mobile phone will be invalid.
* **Country**: The country of the tenant
* **Timezone**: The timezone for the tenant
* **Currency**: The currency of the tenant
* **Enable extension audio recording**: If selected, all extension audio calls will be recorded even if the extension has not enabled recording in its profile.
* **Enable extension video recording**: If selected, all extension video calls will be recorded even if the extension has not enabled recording in its profile.

The **Storage** page allows adjusting the storage quotas for Recording files, Voice Mails, and Call Reports:

* **Disk quota (MB)**: The maximum disk quota allowed for this tenant.

You can set the method for deleting recording files and voice mails in the **Auto Cleaning** section.

## Deactivating Tenant

To deactivate an existing tenant, select the left menu option **Tenants**, and all tenants will be listed. Click the **ON/OFF** button in the **Status** column to deactivate/activate that tenant.

## Deleting Tenant

To delete an existing tenant, select the left menu option **Tenants**, and all tenants will be listed. Select a tenant, then click the **Delete** button to delete it.

{% hint style="info" %}
Given that a tenant may utilize a significant amount of resources, the process of deleting all resources associated with that tenant could take some time. During this period, the tenant being deleted will continue to appear in the tenant list until the deletion is fully completed.
{% endhint %}

## Managing Tenant

The System Administrator of PortSIP PBX has the ability to manage tenants, their settings, and extension users. To do this, sign into the PBX Web Portal and navigate to the **Tenants** menu. Select the tenant you wish to manage and click the **Manage** button. This action will allow the System Administrator to assume the role of the tenant’s admin, enabling them to set up or modify the tenant’s settings and manage its extensions.

Once these tasks are completed, the System Administrator can revert to their original role by clicking on the profile picture located at the top right of the page. This will display a menu where they can select **Switch to Administrator**. This action allows the System Administrator to return to their original role without the need to log out of the tenant account and re-login to the administrator account.
