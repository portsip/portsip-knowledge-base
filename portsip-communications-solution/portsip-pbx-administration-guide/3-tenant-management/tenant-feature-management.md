# Tenant Feature Management

**T**enant Feature Management allows the PBX Administrator to centrally activate or deactivate specific features for individual tenants. This provides the PBX owner with a flexible and scalable way to manage tenant capabilities, control resource usage, and offer value-added features as paid options to increase revenue.

By enabling or disabling features at the tenant level, service providers can easily implement service tiers, add-on features, or usage-based charging models.

***

### Configurable Tenant Features

The PBX Administrator can activate or deactivate the following features for each tenant.

***

#### AI Transcription

Controls tenant access to the **AI transcription** feature and its usage limits.

* **Activate / Deactivate**\
  When deactivated, the tenant cannot use AI transcription under any circumstances.
* **Daily Quota Limit**\
  When activated, a daily transcription quota can be assigned to the tenant.\
  Once the tenant reaches the daily quota, AI transcription is automatically suspended until the **next day**, when the quota resets.

This allows precise cost control and supports usage-based billing models.





***

#### App Usage

Controls how many users within a tenant can register using the **PortSIP ONE UC App** and **PortSIP ONE Teams Phone App**.

* If **disabled** or the maximum user count is set to **0**, no extensions under the tenant can register using these applications.
* If a maximum value (for example, **100**) is configured, only that number of extension users are allowed to register using the PortSIP ONE apps.

This feature is commonly used to enforce **per-user licensing limits**.

***

#### Billing

Controls access to **billing and charging features** for the tenant.

* When **deactivated**, the tenant cannot configure or access any billing-related settings.
* When **activated**, billing features become available to the tenant administrator.

***

#### Call Statistics and Data Analytics

Controls tenant access to **call statistics and analytics features**, including:

* Call Reports
* Call Detail Records (CDRs)
* Call Recordings
* Data Analytics dashboards

When this option is deactivated, the tenant cannot access any analytics or reporting data.

***

#### Contact Center

Controls access to **Contact Center features**, such as:

* Wallboards
* Queue monitoring
* Contact Center analytics

When deactivated, all Contact Center–related features are unavailable to the tenant.

***

#### Message Channels

Controls tenant access to **messaging channels**, including:

* SMS configuration
* WhatsApp integration

When this option is deactivated, the tenant cannot configure or use messaging features.

***

#### Trunks

Controls whether a tenant can manage its **own SIP trunks**.

* When deactivated, the tenant cannot access trunk configuration or create tenant-level trunks.
* When activated, the tenant can configure and manage its own trunk settings.

This is commonly used when trunks are **centrally managed by the service provider**.

***

#### Microsoft Teams

Controls access to **Microsoft Teams Direct Routing** configuration.

* When deactivated, the tenant cannot configure Microsoft Teams Direct Routing.
* When activated, Teams integration options become available.

***

#### CRM Integrations

Controls access to **CRM integration features**.

* When deactivated, the tenant cannot configure or use CRM integrations.
* When activated, supported CRM platforms can be configured and used.

***

### Best Practice Recommendation

> It is recommended to enable only the features required by each tenant and offer advanced capabilities as **optional, chargeable add-ons**. This approach improves security, simplifies tenant management, and maximizes service revenue.
