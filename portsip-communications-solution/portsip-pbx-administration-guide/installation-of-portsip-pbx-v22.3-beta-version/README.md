# Installation of PortSIP PBX v22.3 Beta Version

### Attention

This guide is intended **exclusively for installing the PortSIP PBX v22.3 Beta version**.\
**Do not use this release in a production environment.**

If you plan to deploy PortSIP PBX for production use, please refer instead to the [installation guide for **version 22.2.x**](../1-installation-of-the-portsip-pbx/).

#### Important Beta Limitations

* Upgrading from any previous **v22.x** release to **v22.3 Beta** is **not supported**.
* Systems installed with **v22.3 Beta** cannot be upgraded to the official **v22.3 release** once it is released.

### Summary of Changes — PortSIP PBX v22.3 Beta

#### New Services and Features

* **Introduced the Data Flow Service**\
  Enables advanced analytics and reporting through real-time data processing.\
  Includes:
  * All-new Call Reports
  * Enhanced CDR with additional filters
  * Redesigned Dashboards and Wallboards for queue and agent metrics
* **CRM Integration** _(initially supports Zoho and HubSpot; more CRMs to follow)_\
  Provides seamless synchronization between PortSIP PBX and CRM systems:
  * Automatic contact synchronization
  * Call logging and note-taking directly within the CRM
  * Ability to create and edit call notes
  * Unified interaction tracking from both the **PortSIP PBX** and **PortSIP ONE App**
* **CRM Contacts Category**\
  Introduced a new _CRM Contacts_ section that categorizes contacts synchronized from connected CRM platforms.
* **AI Transcription Service**
  * Integrated with AWS and Microsoft Azure to provide automatic transcription for calls and voicemails.
  * Transcriptions are viewable in both the PBX Web Portal and the PortSIP ONE App.

***

#### Administration and Access Control

* **Multiple System Administrators**\
  Support for creating multiple system administrator accounts.
* **New Administrator Roles**\
  Introduced two new predefined roles with limited permissions:
  * Operations Admin
  * Site Admin
* **Customizable Administrator Roles**\
  Administrators can now create custom roles to fine-tune access permissions, enhancing role-based access control and operational flexibility.
* **User Access Limits**\
  System administrators can set per-tenant limits on how many users can access:
  * The PortSIP ONE App
  * The Teams Phone App

***

#### Security and Compliance Enhancements

* **Recording and Voicemail File Protection**\
  Strengthened access control for recordings and voicemail files:
  * Each file now includes both a Public Link and a Private Link.
  * Private Links require credential verification and role-based permission validation.
  * Tenant admins can choose whether to push Public or Private Links to the CRM.
  * When a CRM user clicks a Private Link, credentials are required to verify access.
  * All accesses to recordings and voicemails are now logged in the Audit Log.
* **Enhanced Audit Logging**\
  Added additional filters and detailed tracking for administrator and user activities.

***

#### Telephony and Call Handling

* **Virtual Receptionist Security**\
  Added the option to block direct extension dialling from the Virtual Receptionist, preventing callers from bypassing menu options.
* **Queue and Ring Group Enhancements**
  * Added support for Night Mode per IVR, Queue, or Ring Group.
  * Introduced Queue Exit Options for callers.
  * Added configurable Agent Wrap-Up Time after each call.
  * Added support for Periodic Announcements during queue waiting.
  * Introduced Agent Pause Codes for more accurate reporting.
* **Trunk Enhancements**
  * Added support for the `tel:` URI scheme in trunk configurations.
  * Added configuration for maximum call duration per trunk.
  * Enhanced outbound rule configuration to support SMS routing.
* **Voicemail Improvements**
  * Minimum PIN length increased to **4 digits** for better security.
* **Call Routing Enhancement**
  * If an extension declines a call, it will now follow the Busy Forwarding Rule.

***

#### Device and App Updates

* **New App Releases**
  * Released the PortSIP Teams Phone App.
  * Released PortSIP ONE for macOS.
* **Device Support**
  * Added support for Fanvil W620W, V50P, and V60P phones.
  * Added support for Yealink T7x and T8x phones.
  * Added support for Gigaset IP and DECT phones.
  * Added support for SNOM headsets in the PortSIP ONE app.
  * DECT phone handset names can now be automatically set to the Extension Name.
* **Power Optimization**
  * Phones provisioned by PortSIP PBX now automatically disable power-saving mode to prevent missed calls.
* **Codec Configuration**
  * Added the ability to enable or disable codecs for IP phones during auto-provisioning.

***

#### Connectivity and System Improvements

* **SMS Integration**
  * Integrated with **SMSGlobal** for outbound and inbound SMS support.
* **IPv6 Support**
  * The system now automatically adapts to IPv6 environments — no manual configuration required.
* **WebSocket Interface (WSI)**
  * The WSI now supports subscribing to global queue events within a tenant.

***

#### Bug Fixes

* Fixed issues related to the **Diversion Header**.
* Other stability and performance enhancements across PBX core services.

### About This Beta Release

To provide the powerful features, PortSIP PBX introduces two new components that require separate installation:

* **PortSIP IM Server** – provides instant messaging and presence functionality.
* **PortSIP Data Flow Server** – enables real-time analytics and reporting services.

Please follow the installation sequence below to ensure proper deployment:

1. [Install PortSIP PBX](install-portsip-pbx.md)
2. [Install PortSIP IM Server](install-portsip-im-server.md)
3. [Install PortSIP Data Flow Server](install-portsip-dataflow-server.md)<br>

