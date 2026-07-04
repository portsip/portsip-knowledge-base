# 17 Roles and Permissions

### Roles and Permissions Overview

PortSIP Roles and Permissions provide role-based control over the features and functions that users can access in the PortSIP PBX.

Each role includes a predefined set of permissions that determines the scope of access available to a user. By assigning roles, administrators can efficiently control who can view, configure, and manage different parts of the system.

PortSIP PBX includes **ready-to-use standard roles** for common positions, such as administrators and end users, with appropriate permissions already configured. For more specialized requirements, you can also create **custom roles** and define exactly which permissions they include. Both standard and custom roles can be assigned to users.

***

### System Administrators

System Administrator roles provide access to system-level configuration and management.&#x20;

For details about System Administrator roles and permissions, see [Administrator Management](2-portsip-pbx-management/administrator-management.md).

***

{% hint style="info" %}
The following guides are only available for Tenant-level management and settings.
{% endhint %}

### Tenant User Roles

**Tenant user roles control access within a specific tenant.** Permissions are **limited to tenant-level** settings and assigned user functions.

**Admin**

Full administrative access within the tenant. This role allows the user to manage tenant settings, users, and features.

**Queue Supervisor (Queue Manager)**

Access limited to queue management. This role is intended for users responsible for monitoring and managing call queues.

**Standard (International)**

Access to personal user settings and international calling.

**Standard**

Access to personal user settings and domestic calling.

***

### Custom Roles

Custom roles allow you to define permission sets for specific business functions, departments, or jurisdictions.

When creating a custom role, select the exact permissions you want to grant, such as access to phone settings while restricting access to administrative features. You can continue adding permissions until the role includes only the intended scope of access.

Once created, the role can be assigned to any user who requires the same permissions, eliminating the need to repeatedly configure permissions for individual users.

***

### Predefined Roles and Permissions

PortSIP PBX also includes **predefined roles** with built-in permission sets that can be assigned by administrators.

Permissions for predefined roles **cannot be modified**.

#### View Predefined Roles

**Option 1: Sign in as System Administrator**

1. Sign in to the PortSIP PBX Web Portal as a **System Administrator**.
2. Navigate to **Tenants**.
3. Select the desired tenant and click **Manage** to switch to that tenant’s administration context.

**Option 2: Sign in as Tenant Administrator**

* Sign in directly as a **Tenant Administrator** to manage the tenant.

After signing in to the web portal, please now go to the menu **Advanced > Roles**; you will see the roles list.

***

### Permissions Overview

Permissions control access to features and configuration options within the PBX Web Portal.

Administrators grant permissions to allow users to view or modify specific features. In some cases, permissions can be assigned to enable management of individual system components.

You may need to adjust permissions when, for example:

* A user needs to manage other users.
* A user needs to configure or manage SIP trunks.

***

### Access and Manage Permissions

**Option 1: Sign in as System Administrator**

1. Sign in to the PortSIP PBX Web Portal as a **System Administrator**.
2. Navigate to **Tenants**.
3. Select the desired tenant and click **Manage** to switch to that tenant’s administration context.

**Option 2: Sign in as Tenant Administrator**

1. Sign in directly as a **Tenant Administrator** to manage the tenant.

After signing in to the web portal, please:&#x20;

1. Go to the menu **Advanced > Roles**.
2. Double-click a role to view its permissions.

Some permissions depend on others and cannot be disabled until the related permissions are disabled first.

<table><thead><tr><th width="286">Role</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>SystemAdmin</td><td>Full system access. On Initial setup, the PBX maintainer.</td><td></td></tr><tr><td>Admin</td><td><ul><li>Full Virtual PBX (for a tenant) access</li><li>All settings of the tenant</li><li>Plus Standard International</li></ul></td><td></td></tr><tr><td>QueueManager</td><td><ul><li>Managing and confining queues</li><li>View trunks</li><li>User management functions for all Users</li><li>Company reporting functionality</li><li>Plus Standard International</li><li>CDR log and Company reporting functionality</li></ul></td><td></td></tr><tr><td>StandardInternationalUser</td><td>User-level access with international dialing access. </td><td></td></tr><tr><td>StandardUser</td><td>User-level access without international dialing access, but has domestic calls. This is the default role assigned to new Users.</td><td></td></tr></tbody></table>

***

### User Permissions

This section describes each user permission, including its access level and functional scope.

***

#### User Voicemail

**View Only**\
Allows viewing the extension user’s voicemail.

**Full Access**\
Allows configuring and deleting the extension user’s voicemail.

***

#### User Meetings

**View Only**\
Allows viewing information about meetings created from the PortSIP ONE app.

**Full Access**\
Allows viewing, editing, and deleting meetings created from the PortSIP ONE app.

***

#### User Call Log

**View Only**\
Allows viewing the extension user’s call log.

**Full Access**\
Allows viewing and managing the extension user’s call log.

***

#### User Recordings

**View Only**\
Allows viewing the extension user’s call recordings.

**Full Access**\
Allows managing the extension user’s call recordings.

***

#### Features

**Audit**\
Allows viewing audit logs.

**Events**\
Allows viewing event logs.

***

#### Trunk

**View Only**\
Allows viewing trunk information.

**Full Access**\
Allows viewing and managing trunk settings.

***

#### Integrations

**View Only**\
Allows viewing integrations such as CRM, Microsoft 365, and Google Workspace.

**Full Access**\
Allows configuring integrations such as CRM, Microsoft 365, and Google Workspace.

***

#### CRM Contacts

**View Only**\
Allows viewing and searching CRM contacts.

**Full Access**\
Allows viewing, searching, creating, and editing CRM contacts.

***

#### Company (Tenant)

**View Only**\
Allows viewing tenant information.

**Full Access**\
Allows managing and configuring tenant settings.

***

#### Analytics

**View Only**\
Allows viewing analytics data, including reports, call history, call recordings, and external message history.

**Full Access**\
Allows managing analytics data, including reports, call history, call recordings, and external message history.

***

#### Company Recordings

**View Only**\
Allows viewing all call recordings within the tenant.

**Full Access**\
Allows managing all call recordings within the tenant.

***

#### Company Call Sessions

**View Only**\
Allows viewing active call sessions within the tenant.

**Full Access**\
Allows managing active call sessions within the tenant, such as ending calls.

***

#### Company Contacts

**View Only**\
Allows viewing company contacts within the tenant.

**Full Access**\
Allows viewing, searching, creating, and editing company contacts within the tenant.

***

#### Phone System

**View Only**\
Allows viewing phone system settings.

**Full Access**\
Allows managing and configuring phone system settings.

***

#### Call Policies

**Domestic Calls**\
Allows making domestic calls.

**Internal Calls**\
Allows making internal calls.

**International Calls**\
Allows making international calls.

***

#### Billing

**View Only**\
Allows viewing billing settings.

**Full Access**\
Allows managing and configuring billing settings.

***

#### Roles

**View Only**\
Allows viewing roles.

**Full Access**\
Allows creating, editing, configuring, and deleting roles.

***

#### Users

**View Only**\
Allows viewing extension users.

**Full Access**\
Allows creating, editing, and deleting extension users.

