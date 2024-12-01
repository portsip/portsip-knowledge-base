# 17 Roles and Permissions

Use PortSIP Roles and Permissions to provide role-based control over the functions accessible to your users. Each role includes a set of permissions that defines how much of the PortSIP PBX a user can access.&#x20;

PortSIP PBX offers four ready-to-use, standard roles for common positions, such as administrator, and user that come with the appropriate permissions already defined. For more specialized needs, you can easily create your own custom roles and specify the permissions you want. You can use both standard and custom roles with your users.

## Benefits of Roles and Permissions

### Onboard new users faster, and with fewer errors

* Save time and effort by associating new users with roles from the start. With roles, you no longer have to set up each new user one by one, manually applying the same permission set over and over to each user as you add them into the system. Roles can be created for functions or positions in the company with all the appropriate permissions built in.
* Four standard, ready-to-use roles make it a cinch to quickly grant the right level of system access to many users at the same time, virtually eliminating the errors that can happen when permissions are set individually.
* Use bulk upload options and templates to conveniently update and assign roles to users across the entire organization.

### Incorporate stronger security and controls over user access to the system

* Role-based access controls act like an extra layer of security to help you enforce company security policies by giving you complete oversight into which permissions are in use. With roles, the same level of access is unilaterally given to every user assigned to that role, greatly reducing the chances of producing outlier users with unauthorized levels of access.
* Custom roles support countless permission combinations, extending your range of granular control over how users can access PortSIP PBX features. For each role, you can select the precise permissions you want to grant, and update your selections at any time.
* Roles give you the flexibility to respond quickly to business needs. You can remove or add roles as you need them or modify existing ones to match new requirements.

### Easily delegate administrator activities with admin roles

* Use admin roles to distribute administrative responsibilities among select users across your team or organization.
* Assign admin roles for specific functions, like billing tasks or tenant management, while restricting administrator-level changes to other parts of your PortSIP PBX.
* Custom admin roles, constructed with more granular permissions, can help you easily delegate specialized authority or jurisdiction to other admins.

## What are the four standard roles?

There are two admin roles and two user roles. Each role has appropriate permissions already included. The roles are:

* System Admin: Access to all PortSIP features and apps, with the highest permissions
* Tenant Admin: Access to tenant scope settings and user settings only
* Standard (International): Access to their own user settings and international calling
* Standard: Access to their own user settings and domestic calling

## How do custom roles work?

* You can create custom roles for every business need, from company-level positions to specialized functions or jurisdictions. For each role, select the permissions you want the role to have, such as access to phone settings but not to others. Keep adding permissions until you reach the limit of features you want the role to access. Now, whenever you have a user who requires the same set of permissions, you can assign that role to the user, without having to select the permissions all over again.

## Predefined User Roles and Permissions

There are 5 **predefined roles** — those with a built-in set of permissions that can be readily assigned by administrators. Permissions granted to these roles cannot be modified.\


{% hint style="info" %}
Standard is assigned to new users by default.
{% endhint %}



To view the predefined roles, sign in to the PBX Web portal, then go to **Advanced > Roles**.

<table><thead><tr><th width="286">Role</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>SystemAdmin</td><td>Full system access. On Initial setup, the PBX maintainer.</td><td></td></tr><tr><td>Admin</td><td><ul><li>Full Virtual PBX (for a tenant) access</li><li>Billing functions plus Manager functions</li><li>User management functions for all Users</li><li>Company reporting functionality</li><li>Plus Standard International</li></ul></td><td></td></tr><tr><td>QueueManager</td><td><ul><li>Managing and confining queues</li><li>View trunks</li><li>User management functions for all Users</li><li>Company reporting functionality</li><li>Plus Standard International</li><li>CDR log and Company reporting functionality</li></ul></td><td></td></tr><tr><td>StandardInternationalUser</td><td>User-level access with international dialing access. </td><td></td></tr><tr><td>StandardUser</td><td>User-level access without international dialing access but has domestic calls. This is the default role assigned to new Users.</td><td></td></tr></tbody></table>

## List of User Permissions <a href="#content-section--0-0--list-title" id="content-section--0-0--list-title"></a>

Permissions determine access and the ability to modify functions and features in products such as the Web Portal. Administrators may allow users to enable changes to these products by granting them permissions. You may also grant permissions to users to change a specific feature. You may want to modify permissions if, for example:

* A user needs to access or modify user information for other users.
* A user needs to manage the SIP trunk.

\
To access permissions:

1. Go to the  Web portal and sign in.
2. Click on the menu **Advanced > Roes**, then double-click a role. Permissions are listed in the page.

Some permissions depend on other permissions, and can’t be disabled until other permissions have been disabled first.

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



