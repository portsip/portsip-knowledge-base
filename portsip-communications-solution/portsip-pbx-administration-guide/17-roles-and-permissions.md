# 17 Roles and Permissions

### Roles and Permissions Overview

PortSIP Roles and Permissions provide role-based control over the features and functions that users can access in the PortSIP PBX.

Each role includes a predefined set of permissions that determines the scope of access available to a user. By assigning roles, administrators can efficiently control who can view, configure, and manage different parts of the system.

PortSIP PBX includes **ready-to-use standard roles** for common positions, such as administrators and end users, with appropriate permissions already configured. For more specialized requirements, you can also create **custom roles** and define exactly which permissions they include. Both standard and custom roles can be assigned to users.

***

### Benefits of Roles and Permissions

#### Faster, Error-Free User Onboarding

Roles simplify user onboarding by allowing administrators to assign a predefined permission set when creating users.

Instead of configuring permissions individually for each new user, you can assign a role that already includes the required access. This approach reduces manual effort and minimizes configuration errors.

Standard roles make it easy to grant the correct level of system access to multiple users at once, significantly reducing the risk of inconsistent or incorrect permissions.

You can also use bulk upload options and templates to efficiently assign roles to users across the organization.

***

#### Stronger Security and Access Control

Role-based access control (RBAC) adds an extra layer of security by ensuring users only have access to the features required for their responsibilities.

All users assigned to the same role receive an identical permission set, reducing the risk of unauthorized or inconsistent access.

Custom roles allow granular control over system access. You can define precise permission combinations and update them at any time to match evolving security or operational requirements.

Roles also make it easier to adapt to organizational changes by allowing you to add, remove, or modify roles as business needs change.

***

#### Delegated Administration with Admin Roles

Admin roles allow you to distribute administrative responsibilities across your organization without granting full system access.

You can assign admin roles for specific functions, such as billing or tenant management, while restricting access to other administrative areas.

Custom admin roles provide fine-grained control, enabling you to delegate specialized administrative authority while maintaining overall system security.

***

### Standard Roles in PortSIP PBX

PortSIP PBX includes a set of **standard roles**, consisting of **three system administrator roles** and **three tenant user roles**. Each role includes a predefined set of permissions.

***

#### System Administrators

System Administrator roles provide access to system-level configuration and management.

For details about System Administrator roles and permissions, see [Administrator Management](2-portsip-pbx-management/administrator-management.md).

***

**Tenant User Roles**

Tenant user roles control access within a specific tenant. Permissions are limited to tenant-level settings and assigned user functions.

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

1. Sign in to the PBX Web Portal.
2. Go to **Advanced > Roles**.

***

### Permissions Overview

Permissions control access to features and configuration options within the PBX Web Portal.

Administrators grant permissions to allow users to view or modify specific features. In some cases, permissions can be assigned to enable management of individual system components.

You may need to adjust permissions when, for example:

* A user needs to manage other users.
* A user needs to configure or manage SIP trunks.

***

### Access and Manage Permissions

1. Sign in to the PBX Web Portal.
2. Go to **Advanced > Roles**.
3. Double-click a role to view its permissions.

Some permissions depend on others and cannot be disabled until the related permissions are disabled first.

<table><thead><tr><th width="286">Role</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>SystemAdmin</td><td>Full system access. On Initial setup, the PBX maintainer.</td><td></td></tr><tr><td>Admin</td><td><ul><li>Full Virtual PBX (for a tenant) access</li><li>All settings of the tenant</li><li>Plus Standard International</li></ul></td><td></td></tr><tr><td>QueueManager</td><td><ul><li>Managing and confining queues</li><li>View trunks</li><li>User management functions for all Users</li><li>Company reporting functionality</li><li>Plus Standard International</li><li>CDR log and Company reporting functionality</li></ul></td><td></td></tr><tr><td>StandardInternationalUser</td><td>User-level access with international dialing access. </td><td></td></tr><tr><td>StandardUser</td><td>User-level access without international dialing access, but has domestic calls. This is the default role assigned to new Users.</td><td></td></tr></tbody></table>

***

### List of User Permissions <a href="#content-section--0-0--list-title" id="content-section--0-0--list-title"></a>

The following section provides a **detailed explanation of each user permission**, including access levels and functional scope.

#### User Voicemail

* **View Only**\
  Allows access to view the extension user's voicemail.
* **Full Access**\
  Allows configuring, deleting the extension user's voicemail.

***

#### User Meeting

* **View Only**\
  Allows access to dashboards and reports for monitoring system usage, call statistics, and performance metrics.
* **Full Access**\
  Allows configuring analytics settings, creating or modifying reports, dashboards, filters, and exporting data.

***

#### User Call Log

* **View Only**\
  Allows viewing dealers and associated information without making changes.
* **Full Access**\
  Allows creating, editing, disabling, or deleting dealers and managing dealer-related settings.

***

#### User.Recordings

* **View Only**\
  Allows viewing tenant information and configuration details without modification.
* **Full Access**\
  Allows creating, editing, disabling, and deleting tenants, as well as managing tenant-level settings.

***

#### Features

* **View Only**\
  Allows viewing configured integrations and their status.
* **Full Access**\
  Allows configuring, modifying, enabling, or disabling integrations with external systems and services.

***

#### Trunk

* **View Only**\
  Allows viewing of the phone device templates.
* **Full Access**\
  Allows adding and editing phone device templates.

***

#### Trunks

* **View Only**\
  Allows viewing trunk configurations, status, and usage information.
* **Full Access**\
  Allows creating, editing, enabling, disabling, and deleting trunks settings.

***

#### Integrations

* **View Only**\
  Allows viewing system features such as Audit Trail, Events.
* **Full Access**\
  Allows managing system features such as Audit Trail, Events.

***

#### System Settings

* **View Only**\
  Allows viewing global system configuration and parameters.
* **Full Access**\
  Allows modifying global system settings that affect platform behavior and services.

***

#### CRM Contacts

* **View Only**\
  Allows viewing existing roles and their assigned permissions.
* **Full Access**\
  Allows creating, editing, and deleting roles and configuring permission assignments.

***

#### Company

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Analytics

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Company Recordings

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Company Call Sessions

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Company Contacts

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Phone System

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.

***

#### Company Call Sessions

* **View Only**\
  Allows viewing of administrators and related information.
* **Full Access**\
  Allows creating, editing, disabling, and deleting administrators, and managing their settings.













































### General

The voicemail access permissions.

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Voicemail View Only</td><td>View and listen/play voicemail</td><td></td></tr><tr><td>Voicemail Full Access</td><td>View, delete, and listen/play voicemail</td><td></td></tr></tbody></table>

### Trunk

Let the user manage the SIP trunk.

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>View Only</td><td>View the trunks, and create inbound &#x26; outbound rules based on the trunk</td><td></td></tr><tr><td>Full Access</td><td>View the trunks and create inbound and outbound rules based on them. You can also add, modify, or delete trunks.</td><td></td></tr></tbody></table>

### Company

The permissions for managing the tenant.

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>View Only</td><td>View the setings in tenant scope .</td><td></td></tr><tr><td>Full Access</td><td>View the settings within the tenant scope and make changes to them as needed.</td><td></td></tr></tbody></table>

### Company Call Log

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>CDR View Only</td><td>View all CDR of the tenant</td><td></td></tr><tr><td>CDR Full Access</td><td>Full access the CDR of the tenant</td><td></td></tr><tr><td>Recording View Only</td><td>View all recordings of the tenant</td><td></td></tr><tr><td>Recording Full Access</td><td>You have full access to the recordings of the tenant, including the ability to download, play and delete them.</td><td></td></tr></tbody></table>

### Features

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Audit Trail</td><td>Access the audit logs</td><td></td></tr><tr><td>Events</td><td>Access the events information</td><td></td></tr></tbody></table>

### Phone System

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>View Only</td><td>View the call-related settings, including forwarding rules and FAC (Feature Access Codes), among others.</td><td></td></tr><tr><td>Full Access</td><td>View the call-related settings, including forwarding rules and FAC (Feature Access Codes), among others. You also have the ability to make changes to these settings</td><td></td></tr></tbody></table>

### User Management

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Roles</td><td>Manage the roles</td><td></td></tr><tr><td>Users</td><td>Manage the users</td><td></td></tr></tbody></table>

### User Call Log

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>CDR View Only</td><td>View all CDR of a user</td><td></td></tr><tr><td>CDR Full Access</td><td>Full access the CDR of a user</td><td></td></tr><tr><td>Recording View Only</td><td>View all recordings of a user</td><td></td></tr><tr><td>Recording Full Access</td><td>Full access the recordings of a user</td><td></td></tr></tbody></table>

### Call Policies

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Domestic Calls</td><td>Has ability to make domestic calls</td><td></td></tr><tr><td>Internal Calls</td><td>Has ability to make internal calls</td><td></td></tr><tr><td>International Calls</td><td>Has ability to make international calls</td><td></td></tr></tbody></table>

### Analytics

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>Call Report View Only</td><td>View and download the call reports</td><td></td></tr><tr><td>Call Report Full Access</td><td>Generate, view, and download the call reports</td><td></td></tr></tbody></table>

### Billing

<table><thead><tr><th>Permission</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>View Only</td><td>View the billing settings, billing rules</td><td></td></tr><tr><td>Full Access</td><td>View the billing settings and rules. You also have the ability to change these settings and create, modify, or delete billing rules</td><td></td></tr></tbody></table>



